---
layout: post
title: On NMT Search Errors and Model Errors: Cat Got Your Tongue?
tags: [frontpage, jekyll, blog, paper-summaries]
image:
---
A paper summary of [On NMT Search Errors and Model Errors: Cat Got Your Tongue?]('https://arxiv.org/abs/1908.10090')
<!--end_excerpt-->
Current NMT models may not be working correctly! When mostly given perfect conditions, aka an infinite search space, over half of the time, the models predict no translation (empty sequences).

In this paper, the authors explore what NMT model's predict as the best translation when the beam width is "infinite." When the beam width is infinite, the model can consider all and any possibilities. Their technical contribution was coming up with a way to have models use infinite beam widths when searching. They found that in 51.8% of the cases, the model thought an empty sequence was the best translation! With beam search, longer sequences have lower probabilities, and it appears in many cases lower probabilities than a single EOS token.

This paper is a startling contribution to NMT. It explores and exposes a very troubling bug of current NMT models. This paper is an exciting contribution since many NLP problems are modeled as a sequence to sequence problems, and many of the SOTA language models involve decoding with beam search. These are exciting implications and bring new questions for NLP research.