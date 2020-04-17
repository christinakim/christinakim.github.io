---
layout: post
title: Negated LAMA: Birds cannot fly
tags: [frontpage, jekyll, blog, paper-summaries]
image:
---
A paper summary of [Negated LAMA: Birds cannot fly]('https://arxiv.org/abs/1911.03343')
<!--end_excerpt-->
When pre-trained language models, such as GPT-2, came out, I was curious about what they were learning and what applications could an LM, like GPT-2, have. What exactly was the model learning?

In [Language Models as a Knowledge Base?]({{ site.baseurl }}{% link _posts/2019-11-14-lm-as-a-knowledge-base.md %}) the authors have a possible answer to that question. The idea was that LM is learning facts and understanding some things, and could you used them then as a knowledge base? The experiments they ran focused masking words in a cloze sentence to get the LM to predict what the answer would be. A cloze statement "is generated from a subject- relation-object triple from a knowledge base and from a template statement for the relation that contains variables X and Y for subject and object (e.g., "X was born in Y")." For instance, "birds can MASK," would return fly.

The authors of Negated LAMA find that LM is not great with negation. When given "birds cannot MASK," it returns fly as well. This suggests that LM are not actually understanding the text shoveled into it. This does show that LM's are good at answering all questions regardless of accuracy, but the authors suggest that maybe it should not be giving an answer.