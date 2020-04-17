---
layout: post
title: 'Shaping representations through communication: community size effect in artificial learning systems'
tags: [paper-summaries]
image:
---
paper summary of [Shaping representations through communication: community size effect in artificial learning systems
]('https://arxiv.org/abs/1912.06208')
<!--end_excerpt-->

This paper is motivated by co-adapation that occurs (information shared between them can become too specific) a) between a single encoder/speaker, and decoder/listener or b) when shared language arises within small groups. Since as long as the encoder, and decoder agree on the information, the learned information doesn't need to be representative, abstractive, or systematic. This paper explores adding more encoders and decoders that are randomly paired up at each training step to encourage the encoders and decoders to learn a more general representation. They find that increasing the community size of encoders/decoders reduces idiosyncrasies in learned code and prevents co-adaption.