---
title:  "Open Research 0: Past"
date:   2020-1-25
author: Rene Bidart
---
## My Deep Learning PhD Journey (so far)
I'm a third year PhD student, and my whole process for this has been a bit of a mess. This is my attempt at a thinking out loud for a better process. 

### 2016-2017 Pre-PhD
After messing around with some MMA fight prediction in my undergrad I became interested in machine learning, and decided the best route was to take a masters in statistics. I went to my Waterloo after not being able to defer my acceptance to UofT, and being scared of paying rent in Toronto. I had never visited Waterloo, or had much of a clue what a masters degree or research might entail, only the dream of doing machine learning research.

To my horror the focus of the master's degree was not research, but taking classes and learning statistics!  Nonetheless I neglected my classes enough that it may as well have been. My first research question was how does optimization behave with fewer parameters like in pruning? I had found networks will still occasionally get good accuracy when trained with few parameters, but mostly fail. I have no idea why I was interested in this, but quickly abandoned it thinking it was a waste of time, and went to search for a real research project.

I was lucky enough to find a great adviser for my research paper, Ali Ghodsi. He introduced me to people working at Sunnybrook Hospital, and I couldn't be more excited. Not only had I found a supervisor who would let me do machine learning, but he was helping me work on a topic I couldn't have been more excited about. I had met people with preventable misdiagnosis before, and obviously deep learning would be the silver bullet to fix this.  We worked on  [histopathology grading](http://www.renebidart.com/files/nuclei.pdf), and managed to get some decent results, even giving a presentation at SPIE medical imaging. Research was a grind, with me wasting time because of weak coding skills, but I was hooked.  I was convinced I was on the right path, and decided I needed to pursue this, and the best way was with a PhD.

Although my supervisor was supportive, and research was going reasonably well, it felt wrong to be in the statistics department, where proofs mattered more than results. Also, statistics was one of the more strict PhD programs, with more mandatory exams, more course requirements, and more teaching assistant work. I felt like I shouldn't be there, so I walked over to the ugly part of campus and asked if I could become an engineer. They were open to another academic change, and I joined a PhD in engineering not knowing what Matlab or a fourier transform was. 


### 2018 Winter-Summer: Starting PhD
My supervisor would be Alexander Wong, who was experienced in machine learning, the algorithmic and hardware side of medical imaging,  and even a medical startup. This was clearly the right choice, no matter what direction medical imaging went, we would either come up with either solid research, or make something useful. Now to find another project!

I wandered around the field, messing around with a few problems, but for the imaging problems in particular, I kept coming back to the same issue. It seemed the technology was already good enough to solve these problems, it was a question of getting enough data and designing a good interface with doctors to make it practically useful. My goal was deep learning research, so I assumed I wasn’t the guy for this task. Of course I didn't immediately switch directions, I spent a month or two not wanting to admit this to my supervisor.

*With hindsight, there were many ways to make this work, there were plenty of unexplored problems in interpretability, multimodal and data efficient ML, but I didn't feel confident to tackle the issue of interfacing with doctors, so I gave up. I wanted to tackle the most important problem, and here I thought it was interface/system design*

### 2018 Fall: Medical Imaging to Adversarial Examples
I decided to look for a research topic in “pure” machine learning, to avoid the situation where data and interface design would be the secret sauce like what I found in healthcare. What was the most important problem to work on?

An obvious one was adversarial examples - imperceptible perturbations that could destroy our best deep learning models. The current best approach, adversarial training,  was also the most simple one. I wasn’t going to tackle this,  smarter people with more GPUs were already grinding away at it. I'd need to do something a little weirder to succeed. My failed experiments with denoising autoencoders reinforced this, so instead I’d use something else for inspiration - our visual system. 

Multiple objects can't exist in the same location, so we shouldn’t have all features active at every location. Maybe sparse activations are the key to adversarial defence? Were adversarial examples somehow sneaking in to activate other feature maps that shouldn’t be active at all?

I tried an explicit constraint on this, only allowing top x percent of activations to be active in a given spatial location. Instead of using a scalar non-linearity, this was vector to vector. It didn’t help the problem of adversarial examples much, but I noticed you could still train networks with this activation, even removing most ReLUs. I had no idea if this could lead anywhere, so abandoned it. Later I ran into people from Numenta at ICML 2019 who had proved that [sparse representations are useful](https://arxiv.org/abs/1903.11257). 

Another idea was that our brain uses concepts, while CNNs only extract discriminative features, with no model of what is being seen. I was a [cool paper using VAEs in this way](https://arxiv.org/abs/1805.09190) , but there wasn't a clear approach forward without really good generative models. I had an idea the way forward was to adding a [hierarchy into the VAE](https://github.com/renebidart/hvae) but the initial experiments were failing, and again I felt I was wandering in the dark.

### 2019 Winter: Adversarial Examples -> VAE / Disentangled Representations
I decided to focus less on adversarial defence, and more on making intuitively appealing models, because it seemed like the issues with CNNs were deep enough that we may need an entirely new approach. 

Neural Nets don't understand rotations, so for a CNN to generalize to rotated images, we normally make it learn the same image at multiple rotations. This seems wrong, and there had been previous work on the topic like [spatial transformer nets](https://arxiv.org/abs/1506.02025), which make a single guess at the correct rotation of the image, or [group equivariant CNNs](http://proceedings.mlr.press/v48/cohenc16.pdf) which explicitly generalize correctly to rotations, but in a way that was unlikely to scale to more complex transforms. I liked the idea that the network would learn some abstract representation, and then perform some "mental rotation" on it,  like what was discussed by Goeff Hinton in ["What is wrong with convolutional neural nets ?"](http://www.youtube.com/watch?v=rTawFwUvnLE&t=19m50s) This isn't exactly the point of his talk, but I liked the idea of a network that would perform this mental rotation, but not using the one-shot method of spatial transformer nets.

I tried doing this with a [VAE on MNIST](https://arxiv.org/abs/1905.05300), where during training there is an inner optimization loop to optimize the rotation the image was encoded at. This meant the model was continually learning the best rotation to encode each image at, and because of limited model size the model learned to encode all digits of the same class in the same orientation, leading to a disentangled representation of rotation and object.

This worked reasonably well for MNIST, but how can it be extended to more complex images? 2D rotations aren't so useful in the real world.


### 2019 Winter/Spring: Comprehensive Exam
It came time for my comprehensive exam, so I needed an important topic I could work on for a couple years, which would become my thesis. The last project on the AVAE wasn't going to work because I had no idea how to extend it, so yet again I went looking for a project to commit to.

Even though there was hype around using algorithms to design neural networks, with NAS, the biggest performance improvements in terms of efficiency and accuracy have come from hand designed components, like convolution, separable convolution, and self-attention. Current NAS methods have not quite lived up to the hype, and have mostly been limited to rearranging existing components of networks, not creating entirely new layers. These assumptions were necessary because current NAS methods are far too inefficient do lower-level search for better architectures. The eventual goal of NAS is to search for an architecture without making any assumptions about its structure, evolving a fully connected one into something better. The problem is we have no way to approach something this difficult. How can we simplify this?

A lot of different layers can be seen as adding sparsity or weight sharing on standard convolution layers. More generally, they are sparsity and weight sharing on self-attention layers. So what if we assume convolution, and find the optimal form of weight sharing and sparsity within this? Maybe this would be tractable?

Because of the need to pass my comprehensive I ended up looking into methods for sparsification and quantization, to come up with a computationally feasible approach to this. Also, for some experiments I directly extended some forms of weight sharing as seen in [dynamic convolutions](https://arxiv.org/abs/1901.10430) to vision problems by forcing multiple convolution filters to share weights, which is better than removing filters directly, but not particularly impressive.

Looking back, I started with the goal of a general NAS algorithm, and ended adding hard-coded weight sharing in convolutions. The need to get actual results changed my problem from something exciting to something I have no particular interest in. The topic is still important, but I'm not sure at this moment I have the right approach for this more general problem.

_______

### Meta: Exploration-Exploitation Trade Off
It is exciting to have the freedom to choose research projects, but there's a lot of pressure. Choosing the wrong topic means months of wasted effort. If you don't do good research you don't get rewarded, unless you're in love with the idea of being a "Dr.", or are doing doing a PhD for immigration purposes.

The issue is the exploration-exploitation trade-off. Do we give up on something, because it doesn't look like a good research direction, or follow through with it because starting over again is so risky?  We both need to pick the right topic, and work on it for a long time to do anything great.

It is hard to implement a simple strategy like epsilon-greedy when the rewards are delayed. I don't have a good intuition if a project is going well, even a month into it. My guess is so far execution has been the limiting factor for me. At the same time I'm conflicted, I don't want to produce low quality research for the hell of it, but by abandoning projects halfway through I've guaranteed that's all I'd do.









