---
layout: post
title: Negative Learning Rates in Meta-Learning Algorithms Learn Universal Features
---

# Meta-Learning

Meta-Learning is a field in machine learning where the (meta) objective is actually to learn. In image classification we might build a model, train it on ImageNet, and report the top-1 or top-5 accuracy. This is pretty well studied by now and SOTA models are basically human level (ignoring adversarial examples, etc). Meta-Learning, on the otherhand, might say "Here are 10 different image classification tasks, find a model that can quickly adapt to any of them." This is a more interesting problem, and potentially a more important one. Data collection can be slow, expensive, or dangerous (robotics/autonomous vehicles). It would be useful to have a model that is good at learning and can reach adequate performance with only a small amount of training data.

There are a couple ways of approaching this: black-box meta-learning, optimization based meta-learning, non-parametric meta-learning. A black-box method might be an LSTM where the input is a dataset and the output are the weights of a model that work well with that dataset. A non-parametric approach might learn an embedding such that a classical non-parametric approach like k-nearest neighbors will work well on the embedding. Optimization based approaches explicitly optimize towards quick adaptation to new tasks. I'll discuss them more in the next sections.


# MAML and Meta-SGD

<center><img src='/public/maml.png'></center>

[Model-Agnostic Meta-Learning](https://arxiv.org/abs/1703.03400) (MAML) and [Meta-SGD](https://arxiv.org/abs/1707.09835) are two very similar approaches. Two have two loops. The inner-loop just a normal ML problem, say image classification. Given a set of images, compute a gradient of the loss and update the weights of your model as appropriate. The outer-loop repeats the inner-loop several times and optimizes over the average validation loss that occurred on the inner-loop for the various tasks. Visualed in the above graphic, these algorithms learn a *meta-initialization* such that some small number of gradient steps on a new task can lead to good performance.

Meta-SGD is the same algorithm as MAML except that the inner-loop learning rate per-parameter and learned by the outer-loop. This allows the step-size to be optimized as well as the direction. A more formal explanation of the algorithm is given below.

<center><img src='/public/meta-sgd-algo.png'></center>

# Feature Reuse

So why does this work? This is often phrased as the choosing between [rapid learning or feature reuse](https://arxiv.org/abs/1909.09157). That is, does the model quickly change feature representations, or does it leaern a meta-initialization that is already good for a variety of tasks.

<center><img src='/public/rapid-or-reuse.png'></center>

Some [studies](https://arxiv.org/pdf/2002.06753) have shown that MAML learns a good meta-initialization and reuses features across tasks. Does Meta-SGD's learned inner-loop learning rate change this?


# Negative Learning Rates

<center><img src='/public/meta-sgd-learning-rates.png'></center>

These are the distribution of learning rates that result from training a small convolutional layer on mini-ImageNet. Something odd, is that the mean learning rate for every layer except the output is negative. This means that for a given task, the inner-loop pushes some weights in the direction that will make them perform worse on that task! For this to be valuable, and it must be or they would not learn to be negative, these "worse" weights must have another benefit. As it turns out, negative learning rates on the inner loop cause the model to learn feature representations that are universal. The representations perform worse on the given task, but better for the majority of the possible tasks that the model could be trained on.

<center><img src='/public/final-accuracy.png'></center>

What this graph shows is the test accuracy for two models, both with the same architecture. One was trained with MAML and the other with Meta-SGD. Pre-adaptation is using only the meta-initialization feature representation, on-task is using the feature representation after the model went through the inner loop and was evaluated on that same task, and off-task is after adaptation, but evaluated on a completely new task. 

You might wonder why the accuracy goes down for both models in the on-task case. For Meta-SGD, well, it has negative learning rates. For MAML, it's less clear, but I did strip off the linear classification layer because I only wanted to test the feature representations. The linear layer has been known to change the most during adaptation, and I believe it is do to that. The more interesting part, and the reason for this study/blog post is what happens in the off-task case. MAML gets worse, as expected, but **Meta-SGD gets better**. This shows that *negative learning rates cause models to learn task-agnostic features.*  More details can be found [here](https://github.com/TomStarshak/TomStarshak.github.io/blob/master/public/Negative Learning Rates Learn Universal Features.pdf)

