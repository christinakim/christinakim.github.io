---
layout: post
title: Experimenting in an Infinite Data Regime
tags: [research, OpenAI Scholars, streaming dataset, dataloader, dataset, pytorch]
image:
---
Most machine learning tutorials gear toward defined datasets that can fit in the memory of most machines. These datasets are great for benchmarking new algorithms and for learning. However, newer SOTA models have many more parameters, and they train in an infinite data regime.

I ran into quite a few bugs while setting up an experiment with OpenWebText2, a clone of WebText which contains over 40GB of data. In this blog post, I want to share some differences to consider when working in an infinite data regime and how to prevent common bugs.

Working in an infinite data regime means you won't have overfitting issues. You won't need to be worried about having enough samples for training while saving enough for testing and evaluating. Instead of setting a max number of epochs, you'll be setting the max number of steps in an infinite data regime since you shouldn't need to see all of the samples (aka an entire epoch) to reach the lowest possible loss.

In an infinite data regime, it makes sense to prepare, tokenize, batch on the fly. In contrast, an indexable finite dataset can transform and fit easily in a GPU's memory. Understanding how to work in an infinite data regime will only become more critical for machine learning researchers and practitioners.

Below is how I use PyTorch's IterableDataset to stream from multiple files to create batches. You can use this dataset class with PyTorch's DataLoader class. It's important to remember that all shuffling and batching should be handled within your IterableDataset, (batch_size for your DataLoader should be set to None to let the DataLoader know that your dataset is batching).

To handle the multiple transformations from the raw text, to a batched output, I use generators for each transformation in the process.

```
import random
from itertools import chain
from itertools import cycle

import torch
from torch.utils.data.dataset import IterableDataset
from transformers import GPT2Tokenizer


class FileIterator:
    def __init__(self, dataset_paths):
        self.dataset_paths = dataset_paths

    def get_file(self, path):
        with open(path, "r", encoding="utf-8") as f:
            yield from f.readlines()

    def __iter__(self):
        for path in self.dataset_paths:
            yield from self.get_file(path)


class TokenizerIterator:
    def __init__(self, seq_len, tokenizer, dataset_path):
        self.seq_len = seq_len
        self.tokenizer = tokenizer
        self.data_iter = FileIterator(dataset_path)

    def tokenize_data(self, x):
        tokenized = self.tokenizer(text=x, truncation=True).input_ids
        # adding end of sequence to the beginning and end of the document
        tokenized.append(self.tokenizer.eos_token_id)

        tokenized.insert(0, self.tokenizer.eos_token_id)
        if len(tokenized) >= self.seq_len:
            for i in range(len(tokenized) - self.seq_len):
                yield tokenized[i: i + self.seq_len], tokenized[i + 1: i + 1 + self.seq_len], len(
                    tokenized[i: i + self.seq_len]
                )
        else:
            pass

    def __iter__(self):
        for x in self.data_iter:
            yield from self.tokenize_data(x)


class BatchIterator:
    def __init__(self, seq_len, batch_size, drop_last, tokenizer, dataset_paths):

        self.dataset_paths = dataset_paths
        self.batch_size = batch_size
        self.drop_last = drop_last
        self.seq_len = seq_len
        self.tokenizer = tokenizer

    def process_file(self, file):
        tokenizer_iter = TokenizerIterator(self.seq_len, self.tokenizer, file)
        for x in tokenizer_iter:
            yield x

    def shuffled_file_list(self, i):
        split = len(self.dataset_paths) // self.batch_size
        dataset_paths = self.dataset_paths[(i * split):((i + 1) * split)]
        return random.sample(dataset_paths, len(dataset_paths))

    def get_stream(self, file_list):
        return chain.from_iterable(map(self.process_file, cycle(file_list)))

    def get_streams(self):
        return zip(*[self.get_stream(self.shuffled_file_list(i)) for i in range(self.batch_size)])

    def __iter__(self):
        return self.get_streams()


class StreamingIterableDataset(IterableDataset):
    def __init__(self, batch_size, drop_last, dataset_paths, seq_len, tokenizer=None):
        if tokenizer is None:
            tokenizer = GPT2Tokenizer.from_pretrained("gpt2")
        self.seq_len = seq_len
        self.dataset_paths = dataset_paths
        self.batch_iter = BatchIterator(
            seq_len=seq_len,
            batch_size=batch_size,
            drop_last=drop_last,
            tokenizer=tokenizer,
            dataset_paths=dataset_paths,
        )

    def __iter__(self):
        try:
            for x in self.batch_iter:
                yield self.collate_fn(x)
        except StopIteration:
            return

    def collate_fn(self, batch):
        data_list, label_list, seq_len_list = [], [], []
        for _data, _label, _seq in batch:
            data_list.append(_data)
            label_list.append(_label)
            seq_len_list.append(_seq)
        # adding the permute to make the batch be the first dimension
        return (
            torch.LongTensor(data_list).permute(1, 0),
            torch.LongTensor(label_list).permute(1, 0),
            torch.LongTensor(seq_len_list),
        )

```



