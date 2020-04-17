---
layout: post
title: Natural Language Understanding
tags: [atomic-note, survey]
image:
---

<!--end_excerpt-->
Natural Language Understanding has been a focus in AI since the birth of AI as a field was defined. The 60s and 70s saw a burst of knowledge bases, ontologies, math word problem solvers. NLU is considered an AI hard problem (aka when we figure this out we have figured out general artificial intelligence).

The tricky part of building a machine reading-comprehension system is that it requires a mostly-accurate (I say mostly accurate because unclear how specific human's models of the world are) idea of the world, and the ability to generalize to new contexts/situations reasonably well. The ability to reason and make pretty good predictions in new cases given our prior knowledge and understanding of the world is what makes us great general purpose learners (and also helped us to exist till now probably).

These are the current types of tasks and benchmarks that test for commonsense knowledge and reasoning.

**Textual Entailment**
Textual entailment (TE) in natural language processing is a directional relation between text fragments. The relation holds whenever the truth of one text fragment follows from another text. In the TE framework, the entailing and entailed texts are termed text (t) and hypothesis (h), respectively.
examples of tasks:
- RTE Challenges (Dagan, Glickman, & Magnini, 2005),
- Story Cloze Test (Mostafazadeh, Chambers, He, Parikh, Batra, Vanderwende, Kohli, & Allen, 2016)
- SWAG (Zellers, Bisk, Schwartz, & Choi, 2018)

**Question Answering**
- CommonsenseQA (Talmor, Herzig, Lourie  Berant, 2015)
- Winograd Schema Challenge (Levesque, 2011)
- GLUE (Wang, Singh, Michael, Hill, Levy, & Bowman, 2018)
- Event2Mind (Rashkin, Sap, Allaway, Smith, & Choi, 2018b)
- bAbI (Weston, Bordes, Chopra, Rush, van Merriënboer, Joulin, & Mikolov, 2015)
- SuperGLUE(Wang, Pruksachatkun, Nangia, Singh, Michael, Hill, Levy, Bowman, 2019)