---
title:  "Open Research 1: Decisions"
date:   2020-01-17
author: Rene Bidart
---

## What to work on?
I spent the fall doing an internship at a hedge fund (Shell Street Labs) in Hong Kong, which was a great break from this open ended research. But what will I work on now?

What is the most important problem in machine learning right now? I'm excited for what's going to happen in a bunch of fields, like self-supervised learning, time series, 3D modelling, pose estimation, and a more general form of Neural Architecture Search. But which one is right to work on? Excitement alone isn't enough, it better be something I'm able to solve. Here are a few of the topics I'm most interested in:

#### Time Series
This internship introduced me the world of time series data, and the significant limitations we have in the field. Time shouldn't be treated the same as another x or y direction. The structure of a problem changes over time and the model needs to know about this, kind of like the state in a hidden markov model. It isn't the same as a language model, the world really is a different place in 2008 than 2020, and we need a learned way to incorporate this into our models. 

We also don't know how what the correct inductive bias is for these type of models. With infinite data transformers are the best approach, because they will change from the bag of words style model to whatever is the correct way to handle time series during training. On the other hand RNNs start out with a better inductive bias, but lack flexibility. How do we trade this off for different sized datasets? Also, how to handle multiple simultaneous series in a way that extends to hundreds/thousands efficiently? These are more open ended questions, but I'm surprised there isn't more research on this topic.

#### Disentangled Representations
I've worked a on creating disentangled representations for [2D rotations before](https://arxiv.org/abs/1905.05300), but I wasn't sure how to extend the work to 3D. With the release of better libraries like [Kaolin](https://github.com/NVIDIAGameWorks/kaolin), it might be the right time to do this, creating generative models which explicitly represent (lighting, pose, etc.) parameters, even without any labeled training data. We would create a VAE which takes as input the image of an object, but reconstructs the object using the differentiable 3D rendering, with the loss as the projection of this object back to an image. We would optimize pose parameters (rotation, lighting, etc.) for each example during training and pray it is able to learn a disentangled representation like what happened in the 2d case.

#### Self-Supervised
What I've thought about the most over the past few months is self-supervised learning. Self-supervised learning was a cornerstone of NLP for the last couple years and Yann LeCunn is finally getting his unsupervised learning revolution in [computer vision too](https://arxiv.org/pdf/2001.07685.pdf). What I'm most interested in is how we do self supervised learning, and how it is connected to our brain rewarding us with dopamine? Is this our own self-supervised learning, and could this be incorporated into a deep learning algorithm? The dream would be learning self-supervised objectives, but how can we do this in an efficient way? 

An objective can be represented as a neural network, but naively training this is a double loop optimization, where optimizing the self-supervised objective takes a full unsupervised training of the self-supervised and supervised problems, before optimizing the self-supervised objective with some gradient free method. Or maybe there is some more efficient approach by taking the [derivative through the training process](https://eng.uber.com/generative-teaching-networks/) to update the objective?

_______
### Meta: Why Writing?
I watched [You and Your Research](https://www.youtube.com/watch?v=a1zDuOPkMSw) by Richard Hamming last week, and it was the best talk I've heard on . It reinforced my view that its extremely important to pick the right problem, and that so many people don't succeed because they miss this crucial step, and focus on unimportant but familiar problems:
> "The average scientist, so far as I can make out, spends almost all his time working on problems which 
> he believes will not be important and he also doesn't believe that they will lead to important problems."

 Without both picking the right problem, and working very hard, you will not succeed. He also had a comment about how the people who discussed ideas with others were most likely to work on the most important problems:
> But I can say there is a pretty good correlation between those who work with the doors open and those
> who ultimately do important things, although people who work with doors closed often work harder. 

The modern equivalent of the open door is spreading your ideas online. In a way it is even more extreme than the open door policy, because you're open to criticism and input from anyone. It forces you to continually iterate on your ideas until they are good. Like being in a panopticon, this is an uncomfortable thing, but it seems like the right thing to do. There are some more modern takes on [why you should write](https://www.perell.com/blog/why-you-should-write), but this open door is reason enough for me. 

I'll aim to keep an open door by writing about research once per week.
