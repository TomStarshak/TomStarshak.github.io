---
layout: post
title: Two Improvements to Causal Distillation
---

# Causal Distillation

Distillation is a well known technique in Machine Learning where a student network is trained on the outputs of a teacher network. Generally the student is a much smaller model and is cheaper/faster/more memory efficient to use in inference. A specific type of distillation, [distillation interchange intervention training objective](https://arxiv.org/abs/2112.02505) (DIITO), adds another objective. First, create a mapping between layers in the student model and the teacher model e.g. layer 2 in the student might map to layers 3 and 4 in the teacher. Second, perform an *interchange intervention* on both the student and teacher models. That is, feed a copy of both models a different input, copy the activations at the mapped layers, and swap the activations from the model copies to the base copy. This will, of course, change the output of the student and teacher models. The auxilarly objective is to match the change in output from student to teacher. The intuition is that if a change in state causes the same change in output, the student must be a *causal abstraction* of the teacher (same reasoning). 

<center><img src='/public/diito.PNG'></center>

# Improvements
## In context intervention
The original DIITO paper found these interchange interventions by randomizing the order of examples in the mini-batch. The copies then have different internal states because they see completely different sentences. This works, but is unsatisfying because the internal states of the two networks are unrelated and one wouldn't expect the internal reasoning to match. An alternative is to feed the same sentences to both the base and copy networks, but randomly swap out some proportion of tokens to different words (synonyms or antonyms didn't make a differnce). Keeping the context the same showed improvements in accuracy on the GLUE benchmark.

## Mixup intervention
While using a copy of the base networks to generate interventions works and makes intuitive sense, it is quite expensive. Every intervention requires an additional forward pass to both the student and teacher networks. This eats a lot of extra compute. What might be a better way to create interventions? Adding random noise to the internal states of both networks. 

<center><img src='/public/mixup.PNG'></center>

## Results
<center><img src='/public/sst2.PNG'></center>

I tried different strategies on 10% of the SST-2 benchmark. In-context interventions worked best and nearly matched the performance of the model on the full dataset. Interestingly, the mixup intervention outperformed all the attempts (synonym/antontm/etc) to make the word swapping more intelligible, albeit worse than the baseline and in-context settings. I suspect this is because the pool of words that can be swapped that still make sense is significantly smaller than all words in the vocabulary.

# Conclusion and Potential Further Work

A full writeup of my experiments can be found [here](https://web.stanford.edu/class/cs224n/final-reports/final-report-169343837.pdf). 

I explored several strategies for improving the performance of student models when using the DIITO and a small amount of training data. Through experiments on the Stanford Sentiment Treebank (SST-2) I found that keeping interventions in context worked better than using alternative inputs to the copied models. I also found that restricting the counterfacual interventions by using related tokens is counter productive. Finally I discovered that it is possible to perform an activation intervention without a counterfactual example at all, eliminating the need to store two extra networks or compute a forward pass on them. 

Further potential avenues of investifation are: 1) are there better intervention embeddings than random vectors and 2) is there an effective way to modify the embedding to take into account the teacher model having multiple intervention layers. 

