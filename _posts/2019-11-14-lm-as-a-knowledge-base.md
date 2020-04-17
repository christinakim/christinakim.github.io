---
layout: post
title: 'Language Models as a Knowledge Base?'
tags: [paper-summaries]
image:
---
paper summary of [Language Models as Knowledge Bases?]('https://arxiv.org/pdf/1909.01066.pdf')
<!--end_excerpt-->

This paper explores using language models as a knowledge base. Structured knowledge bases are pretty restrictive and hard to manage. For that reason, there hasn't been a lot of research interest in structured knowledge bases since the 80s since it appeared to be impractical for Q/A other fact-based NLP tasks.

Language models make an attractive substitute for structured knowledge bases since they don't run into the same issues structured knowledge bases had. They don't require schema engineering, allow queries about an open class of relations, can easily extend to more data and require no human supervision to train.

The authors propose LAMA (LAnguage Model Analysis), which is a probe for analyzing the factual and commonsense knowledge contained in pretrained language models. They used this method to evaluate BERT. They found that BERT-large performs on par to a knowledge base from a text. Their experiments found that pretrained BERT-large was able to recall knowledge better than its other pretrained LM competitors and at a level remarkably competitive with non-neural and supervised alternatives.