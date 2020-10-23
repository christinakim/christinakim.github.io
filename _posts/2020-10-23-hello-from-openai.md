---
layout: post
title: 'Hello from OpenAI'
tags: [papers, OpenAI Scholars]
image:
---
I'm excited to be joining the Fall 2020 cohort of OpenAI's Scholars program. I'll be writing semi-regularly as a log of what I'm learning and thinking about.

I'm excited to be part of the scholars' program since I find learning in a group motivating and useful, especially now when everyone is more isolated. It's also been beneficial to ask questions and learn from people who have already been thinking about my research interests.

One of the high-level goals I'd like to work on is developing "taste" or "aesthetic" for deep learning research throughout this experience.

For the past two weeks, I've been reading about generalization and language models. I've also been working on reimplementing the smaller transformers from the [Scaling Laws for Neural Languages](https://arxiv.org/pdf/2001.08361.pdf).

### Scaling Laws for Neural Languages
The paper uses a decoder only transformer for most of its experiments, in addition to LSTM models and the universal transformer. For now, I'll focus on reproducing the smaller-scale experiments with the transformer architecture. To understand the architecture for the decoder only transformer better, I read the [original GPT paper](https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf). It's surprising to remember that this paper is only ~2 years old. I plan to use datasets available via [HuggingFace's Dataset library](https://huggingface.co/docs/datasets/) for training initially, and look into this [WebText scraper](https://github.com/jcpeterson/openwebtext) later.

I found these resources really useful for understanding and implementing the transformer architecture.
- [The Illustrated Transformer – Jay Alammar](https://jalammar.github.io/illustrated-transformer/)
- [Illustrated Guide to Transformers](https://towardsdatascience.com/illustrated-guide-to-transformers-step-by-step-explanation-f74876522bc0)
- [The Annotated Transformer](http://nlp.seas.harvard.edu/2018/04/01/attention.html#position-wise-feed-forward-networks)

Coincidentally, Jared Kaplan, one of the paper's authors, gave a talk on scaling laws this past Wednesday. The slides and the video from the talk can be accessed here on the [Physics ∩ ML website](http://www.physicsmeetsml.org/posts/sem_2020_10_21/).

Below are papers suggested by my mentor for other relevant language model papers to read:
- [Transformer](https://arxiv.org/abs/1706.03762)
- [Transformer XL](https://arxiv.org/abs/1901.02860)
- [Universal Transformer](https://arxiv.org/abs/1807.03819)
- [GPT-1](https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf)
- [GPT-2](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf)
- [GPT-3](https://arxiv.org/abs/2005.14165)
- [Reformer](https://arxiv.org/abs/2001.04451)
- [RL from human feedback](https://arxiv.org/abs/2009.01325)
- [Critical batch size paper](https://arxiv.org/abs/1812.06162v1)
- [T5](https://arxiv.org/abs/1910.10683)
- [BERT](https://arxiv.org/abs/1810.04805)
- [ULMFiT](https://arxiv.org/abs/1801.06146)
- [BabyAI](https://arxiv.org/abs/1810.08272v1)
- [XLNet](https://arxiv.org/abs/1906.08237)

### Generalization
I've also been thinking about model generalization this week. I've been thinking about some questions: what are the differences between generalization and memorization for some of these larger models with smaller datasets? what is the minimum amount of data required to generalize? what are other factors that allow models to generalize quickly? are there similar scaling law-esque properties for model generalization? what does it look like for a model to generalize well on out of distribution data?

Some papers I've been reading about generalization:
- [Leveraging Procedural Generation to Benchmark Reinforcement Learning](https://arxiv.org/pdf/1912.01588.pdf)
- [LSTMS Compose—and Learn—Bottom-Up](https://arxiv.org/pdf/2010.04650.pdf)
-[Solving Rubik's Cube with a Robot Hand
](https://arxiv.org/abs/1910.07113)
- [Exploring Generalization in Deep Learning](https://papers.nips.cc/paper/7176-exploring-generalization-in-deep-learning.pdf)
- [Stronger generalization bounds for deep nets via a compression
approach](https://arxiv.org/pdf/1802.05296.pdf)