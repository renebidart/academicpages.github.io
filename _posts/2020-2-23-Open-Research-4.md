---
title:  "Open Research 4: Abandoning Weight Sharing, Bitter Lessons and Reasonable Ambition
date:   2020-2-23
author: Rene Bidart
---
## Disentangled Representations, Bitter Lessons, and the Edge of Ability
For a long time I've been obsessed with the idea of learning disentangled representations of the world. The general problem of learning disentangled representations is [impossible without assumptions](https://ai.googleblog.com/2019/04/evaluating-unsupervised-learning-of.html), just like how there is [no classsifier to rule them all](https://www.mitpressjournals.org/doi/abs/10.1162/neco.1996.8.7.1341). Luckily just like we can construct useful classifiers by making some general assumptions about the structure of the world, we can make assumptions to learn disentangled representations. 

I previously worked on learning disentangled representations for rotations on 2d images, but I'm more interested in the general problem, creating 3d disentangled representations from a 2d image. This is much harder, but luckily there's a nice intermediate problem, which is creating a disentangled representation using a 3d image. I'll focus on this first, it should be a straightforward extension of the previous work.

I started off creating a 3d VAE, which was surprisingly easy. Not much more that changing nn.Conv2d to nn.Conv2d, and it worked reasonably well. It can encode some objects from the modelnet dataset resobably well, so its time to start a prototype. I'll aim to learn to disentangle the orientation of an object, so the model which is fed images at all orientations should learn to encode a single one, plus the orientation angle.

*The [Kaolin library](https://github.com/NVIDIAGameWorks/kaolin) is awesome for 3D data, but unfortunately it can require changing NVIDIA drivers, which is always surprisingly painful*

#### Generality and the Bitter Lesson
The real dream is unsupervised learning with less assumptions, where the unsupervised objective is determined only by performance on some downstream tasks. This would be learned self-supervised learning, but right now this is too difficult. Naively attacking this will have a double loop optimization problem - one to train an unsupervised objective, and another to train the supervised objective on top of it. Instead, I'll make more assumptions and attack a particular problem.

I always worry about working on a problem that will be killed by a more general approach, like in [Richard Sutton's bitter lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html). Learned features beat out hand designed features for image classification once there was enough compute, data and and some algorithmic advances. You don't want to be the one working on better hand designed features in 2012. This is always what I'm trying to balance, being general enough that the result will scale, and specific enough that the problem is approachable now. More specific assumptions make it more likely to work, but less likely to scale up and create a paradigm shift in the field.

---

Choosing the generality of an approach in deep learning is a specific case of a common choice we make, which is how ambitious we should be?

Working on something too easy or too hard are both signs of weakness. It is obvious with a problem that is too easy, because you have no chance of failing, but this is also true with problems that are too hard because there is no expectation of success. You have no fear of failing when working on time travel, and no chance of failing trying to get SOTA on a useless dataset with a pre-trained model.

It is easy to convince ourselves we aren't as good as we really are, and work on something too easy, or be delusional and do something way too difficult,  where we won't be held accountable for the eventual failure. We have to be self-aware enough to know how good we are, and confident enough to work just at the edge of our ability.

---


#### Abandoning Weight Sharing
Also, this week I put the final nail in the coffin in a project that should have been finished long ago. I did a few more experiments to show weight sharing within convolution layers was not useful for image classification, unlike [NLP where this was useful](https://arxiv.org/pdf/1901.10430.pdf). I investigated both adding this weight sharing in standard CNNs (ResNet18, ResNet50), as well as in depthwise separable convolutions (Xception) to investigate if the result was better using the spatial or channel wise convolutions. I found it is much better in spatial convolutions, but because these have so few parameters, it doesn't shrink the network in a significant way. The only case where this might be useful is in regularizing a neural network that is way larger than should be used for a given task, but if this is the case you're better off using a smaller network directly.


