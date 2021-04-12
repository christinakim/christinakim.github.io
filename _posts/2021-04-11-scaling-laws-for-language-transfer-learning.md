---
layout: post
title: Scaling Laws for Language Transfer Learning
tags: [research, OpenAI Scholars, scaling laws, transfer learning, fine-tuning, pre-trained models, transformers]
image:
---
###### Building upon OpenAI’s recent work on scaling laws, my project explores how much pre-training on English helps when transferring across different languages. Here, I will discuss scaling laws discovered while fine-tuning across different languages with pre-trained English language models. Specifically, I found that a) pre-trained English models help most when learning German, then Spanish, and finally Chinese and b) transfer from English to Chinese, German, and Spanish scales predictably in terms of parameters, data, and compute.

## Introduction
Historically, the advancement of deep learning capabilities has centered around three levers: improved algorithms, faster and cheaper compute, and larger and higher quality datasets.  Given machine learning’s promise to significantly impact society, deepening our general understanding of machine learning, and how certain levers improve models, is critical for making better predictions for which capabilities will develop next, and when. Recently, researchers have increasingly explored scaling relationships between these three levers.
![image](/images/posts/scaling-laws-for-language-transfer/scaling-autoreg.png)
*Figure from Henighan et al. 2020.*
{: .imgContainer}
My project’s framework for experiments is inspired by the work on scaling laws published by OpenAI in the past year. Scaling laws (Kaplan et al. 2020) can predict machine learning performance as a function of model size, dataset size, and the amount of compute used for training. Kaplan et al. (2020) also found that this relationship holds over several orders of magnitude. Earlier this year, scaling relationships were found for transfer learning from pre-trained english text models to Python (Hernandez et al 2021).

![image](/images/posts/scaling-laws-for-language-transfer/meme.jpg)
{: .imgContainer1}

In my project, I continue the study of transfer between distributions and look at scaling between three other languages and English. Scaling laws for transfer are important because the scaling relationships explain how to work in limited data regimes. In an ideal world, one would have an infinite amount of data for a model to learn from. However, getting a large quantity of high quality data is a nontrivial, if not impossible, task and as a result, most problems exist in the low data regime. Before the Scholars program, I was a machine learning engineer and saw firsthand how costly it was in terms of both time and money to get good quality human labels for our tasks. Exploring the relationships between different languages can provide more insight on how to tackle low-resource languages and how to best leverage pre-trained language models. Given the real world limitations of data, the tradeoffs between budgeting for compute on larger models and budgeting for more fine-tuning data is an important practical relationship to understand.


## Experiment Methodology
Building upon work from Scaling Laws for Transfer (Hernandez et. al. 2021), my experiments focused on exploring the relationships between fine-tuning on non-English languages. My experiments try to answer the questions: *How much does pre-training on English help when transferring across different languages as we vary the dataset size and model size?*

### Pre-training
I first trained English language models in a similar setup to Scaling Laws for Neural Languages.  I pre-trained decoder-only transformers of size 124M, 70M, 51M, 39M, 16M, 3.3M, non-embedding parameters with the same hyperparameters on openwebtext2 (65.86GB), an open-source version of WebText created by Eleuther AI. I used Adam, a batch size of 512, sequences of 1024 tokens and the learning rate schedule was a 500 step warm-up with a cosine decay to 10% of the maximum learning rate. The text was encoded with the same GPT2 tokenizer, a Byte-Level Byte-Pair Encoding tokenizer with a 50K vocab size, and all models were trained for a total of 26 billion tokens with no repeats. The code to reproduce these pre-trained models is available here, including model weights. As seen in the figure below comparing loss and model size, the models exhibit scaling laws as model size increases. However, the relationship isn’t exactly linear, suggesting that maybe the larger models are undertrained or the hyperparameters are not tuned thoroughly.

![image](/images/posts/scaling-laws-for-language-transfer/openwebtext2.png)
{: .imgContainer1}
![image](/images/posts/scaling-laws-for-language-transfer/openwebtext2_compute.png)
{: .imgContainer1}



### Fine-Tuning
For the fine-tuning experiments, dataset size spanned four orders of magnitude, and model size spanned two orders of magnitude trained on three different languages: Spanish, Chinese, and German. Models trained or fine-tuned on Chinese datasets leveraged a 1.2 billion character dataset, Community QA. Community QA (webtext2019zh) is similar to the WebText corpus. Models trained or fine-tuned on Spanish texts and German texts are from Oscar, a multilingual corpus obtained by language classification and filtering of the Common Crawl corpus.

The models fine-tuned on non-English languages with the pre-trained English model weights, and the models trained on non-English languages from-scratch were all trained to convergence to the optimal early stopping point, with a learning rate schedule of a 300 step warm-up with a cosine decay to 10% of the maximum learning rate. The code to replicate these experiments will also be available on GitHub.

## Results
#### Effective Data Transfer
![image](/images/posts/scaling-laws-for-language-transfer/effective-data-transfer-explanation.png)
{: .imgContainer1}

In my experiments, I wanted to find the effective data transferred for models trained on English text to Chinese, Spanish, and German text. The effective data transferred is defined in Scaling Laws for Transfer as the amount of additional fine-tuning data that a model of the same size, trained on only that fine-tuning dataset, would have needed to achieve the same loss as a pre-trained model.

<style type="text/css">
  .imgContainer img {
    max-width: 147% !important;
    margin-left: 50%;
    transform: translateX(-50%);
  }

  .imgContainer1 img {
    margin-left: 50%;
    transform: translateX(-50%);
  }

</style>



![image](/images/posts/scaling-laws-for-language-transfer/effective_data_transfer_16M_transformer.png)
{: .imgContainer}



As seen in the figures above, English to Chinese had a smaller amount of data transferred compared to English to Spanish for the same model size and English to German had the greatest amount of data transferred. Pre-trained English text models help most when learning German, followed by Spanish, and finally, Chinese. I believe these results reflect the degree of linguistic similarities between English and the non-English  languages. English and German are both derived from Proto-Germanic and are linguistically most similar. Although Spanish shares many symbols with the English alphabet, it is a Romance language, and Chinese doesn’t share the same alphabet as English .I want to focus a bit on the distance and shape between the lines, as you can see the effective data transfer is not too much greater for spanish, vs chinese, at the smallest dataset size, 8000 tokens. However, as we increase the dataset size, pre-training continues to help for another order of magnitude until the 100M token dataset size  than the chinese which converges at 10M token dataset size.

#### Comparing the Fraction of Effective Data from Fine-Tuning, $D_f /D_e $

$D_f /D_e $ measures the fraction of effective data from fine-tuning. The smaller this fraction, it means pre-training helps more. I found that as model size increases,  $D_f /D_e $ decreases across all languages pre-training becomes more effective. However, as dataset size increases,  $D_f /D_e $  increases across model sizes pre-training becomes less effective. In the figure above, german has steeper curves, seeing the effective data from fine-tuning decrease the most, which shows English helps the most while Chinese has the flattest curves, showing pre-training helps the least for Chinese.

![image](/images/posts/scaling-laws-for-language-transfer/comparing_1-effective.png)
{: .imgContainer}


I find many of the same trends and relationships found in the Scaling Law for Transfer between text and code, between English and different languages.

In the low data regime, pre-training is helpful across model sizes, but especially in small model sizes. When using pre-trained models, model performance is parameter limited. Model performance is data limited when increasing the number of parameters doesn’t impact the loss. This is evident in the figure below, which shows that as I increase model size with a fixed dataset size of Chinese text to fine-tune on, models trained from scratch on Chinese did not improve while models that were pre-trained on English continue to achieve better performance. The flat lines indicate the performance is data limited, while the sloped lines indicates the performance is more limited by the number of parameters.

Lastly, pre-trained models are more compute efficient than training from-scratch across dataset sizes. This is without accounting for the compute costs for the pre-trained model.
![image](/images/posts/scaling-laws-for-language-transfer/loss_v_compute_60M_transformer_500M_chinese.png)
{: .imgContainer1}


## Limitations
There are important limitations of my work. First, I used the same BBPE GPT-2 tokenizer for all languages. Second, my largest models could have been pre-trained for longer. Additionally, I was only able to do a limited hyperparameter sweep and learning rate schedules due to compute and time. Finally, my finetuning datasets were from different sources.

## Future Work
Some potential future directions include:
- Compare the effective data transfer using pretrained models in Chinese, German, Spanish, then fine-tuned to English
- Fine-tune with low resource languages or other tasks/distributions and find the effective data transfer
- Predict ideal ratio for pre-trained v.s. fine-tune for any given problem, given some budget
- Study the “forgetting” problem in transfer learning in terms of effective data transfer


## Acknowledgements
Thanks to my mentor Jerry Tworek, the Scholars cohort, Danielle Ensign and Kudzo Ahegbebu for sharing compute, everyone that gave me feedback (especially Danny Hernandez and Mohammad Bavarian), Jeromy Johnson for helping me find the Community QA dataset, and program coordinators, Muraya Maigua and Christina Hendrickson, and OpenAI for making this all possible

## References
- Jörg Bornschein et al. Sep 26, 2020. Small Data, Big Decisions: Model Selection in the Small-Data Regime
- Frédéric Branchaud-Charron et al. May 20, 2019. Spectral Metric for Dataset Complexity Assessment
- Tom Henighan et al. Oct 28, 2020. Scaling Laws for Autoregressive Generative Modeling
- Henning Fernau. Jan 24, 2019. Algorithms for Learning Regular Expressions from Positive Data Algorithms
- Pierre Guillou. Jul 3, 2020. Byte-level BPE, an Universal Tokenizer but...
- Danny Hernandez et al. Feb 2, 2021. Scaling Laws for Transfer
- Joel Hestness et al. Dec 1, 2017. Deep Learning Scaling is Predictable, Empiricially.
- Jared Kaplan et al. Jan 23, 2020. Scaling Laws for Neural Language Models
- Ameet A. Rahane et al. Apr 16, 2020.  Measures of Complexity for Large Scale Image Datasets [IEEE Conference Publication]
- Jonathan S. Rosenfeld et al. Sep 25, 2019. A Constructive Prediction of the Generalization Error Across Scales
- Céline Van den Rul. Nov 9, 2019. [NLP] Basics: Measuring The Linguistic Complexity of Text


