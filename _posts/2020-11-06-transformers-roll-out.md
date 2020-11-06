---
layout: post
title: Transformers, Roll Out!
tags: [papers, OpenAI Scholars]
image:
---
I've spent a good majority of my time (when not constantly refreshing for election results) thinking about transformers. There are already many articles describing transformers and implementing them, so I won't go into too much detail about that here. Instead, I want to share some questions I've had while playing around with transformers and LSTMs on small datasets this past week. I tried to get [models of the same size](https://gist.github.com/christinakim/26c5ae3b22eb599b4fb5575918ff7a3b) for these experiments roughly. When tested on variable context lengths from training in my small datasets experiments, LSTMs performed better than transformers. The intuition here is that LSTMs generalize better on context length due to their recurrence.

There are transformer architectures that try to add recurrence to the models, like the Universal Transformer. It's interesting to note which tasks the Universal Transformer performed well on and didn't perform well on. For example, on machine translation, the Universal Transformer performed a bit worse. The Universal Transformer is computationally more expensive than the traditional transformer architecture. In the case of machine translation, my mentor suggested that it might just essentially need to do a lookup into the weights for the correct word (aka memorization). However, the added recurrence did help for other tasks. So, what is the actual trade-off between compute and performance?

Besides adding some idea of recurrence in the Universal Transformer, many other transformer architectures try to improve on the original implementation and performance. I'm curious about evaluating and understanding the different architecture trade-offs in the many transformers' papers. I hypothesize that autoregressive transformer models can learn some positional information, so they might benefit more from other types of positional encodings than non-autoregressive transformer models. It'd also be interesting to compare different attention mechanisms, such as the Reformer's v.s. self-attention. I'm interested in learning the answers to these questions since I think it'll hopefully elucidate what "skills" are essential and what degree of natural language understanding.

Below is my self-attention function that uses [einsum](https://pytorch.org/docs/stable/generated/torch.einsum.html), which has been really handy!
```
def self_attention(key, query, value):
   # scores = dotprod of key + query
   # b here stands for batch, l = length, k= nu pixels, num words etc)
   scores = torch.einsum('bkl,bql->bqk', key, query)

   # note that the dimensions of key query and value
  dimensions_k = key.dim()

   # this helps to create a more stable gradient (you could probably normalize via other constants)
   scores = scores/np.sqrt(dimensions_k)

   # turn scores into probabilities from 0,1
   attention = torch.softmax(scores)

   # dotprod of value vector, and attention vector
   attended_values = torch.einsum('bdl,bad->bal', value, attention)

   return attended_values
```

and here is an obligatory photo of my favorite transformer
![bumblebee]( https://hips.hearstapps.com/digitalspyuk.cdnds.net/17/25/1498134404-transformers-dark-of-the-moon-bumblebee-poster.jpg?resize=980:*)