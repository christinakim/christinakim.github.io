---
layout: post
title: Backpropagation
tags: [atomic-note, math]
image:
---

<!--end_excerpt-->
## Description
Backpropagation is short for backward propagation of errors. It's used to compute gradients for our loss function in machine learning. It does this by computing the partial derivatives of each training example in regards to the weights and bias. Cost can be written as a function of the outputs from the neural network

Another neat feature of backpropagation is that relates the neural network's error and the weights and biases of the neural network's last layer and it does this for the weights and biases of the last layer to the weights and biases of the second to last layer, and continues, using chain  rule.

why is it important?

there's lots of ways you can compute the gradients for our loss function but most of them take too long for deep learning - requiring us to compute the cost in respects to each parameter each .

Backpropagation is way faster - it allows us to simultaneously compute all the partial derivatives using one forward pass through the network, then one backward pass through the network.

## Math

The actual algorithm:
![image](/images/posts/nielson_backprop.png)

Taken from Neural Networks and Deep Learning by Michael Nielson