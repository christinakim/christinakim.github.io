---
layout: post
title: Scaling Laws
tags: [research, OpenAI Scholars]
image:
---

My research direction for the OpenAI Scholar's program is heavily influenced by the scaling laws paper by OpenAI on [language models](https://arxiv.org/abs/2001.08361), and [autoregressive models](https://arxiv.org/abs/2010.14701) published last year. Scaling laws exist for cross-entropy loss in five domains: language modeling, generative image modeling, video modeling, multimodal image to text models, and mathematical problem solving (Henighan et al 2020). They have also been discovered for a few specific problems within those domains. Scaling laws’ applicability across a wide range of scales and surprisingly exact curves indicate they are an important piece to understanding neural networks and their performance.

Scaling laws exist throughout nature and it's particularly interesting to find them in deep learning as well.

In [Scaling Laws for Neural Language Models](https://arxiv.org/abs/2001.08361), the authors found that the loss of LM scales as a power-law with model size, dataset size, and the amount of compute used for training up to seven order of magnitudes.

Scaling laws flavored AI research can help with our understanding of model improvements and get past the benchmark optimization phase of AI research. Instead, research can be focused scaling law optimization and scaling law tradeoff understanding. Experiments designed with scaling laws in mind can be smaller, and less compute heavy experiments as long as findings can extrapolate into trends.