---
title:  "Open Research 2: Getting Started - Self-supervised Learning Basics, Pytorch Ignite, Lightning and Fastai"
date:   2020-2-10
author: Rene Bidart
---
## NLP
I want to do self-supervised learning, and I think NLP is better suited to this, even though I've worked on vision previously. There is something special about NLP where simple self-supervised objectives like next word or masked work prediction work well, compared to computer vision where more complex methods are needed. If something works here, I can extend it to vision, and my guess is that the switch to NLP won't be too difficult.

### Frameworks: Fastai, Ignite and Lightning.
Iteration speed has been an issue for me. Too much planning and too slow execution. Slow execution was mainly from simple coding mistakes and being too messy. I only learned the absolute minimum of CS needed to get by, so my coding is at the far end of the research spectrum and my projects are often a dumpster fire after a few months. 

I want to use a library both for iteration speed, and hopefully to learn some good coding practices. I spent time setting up my approach so there would be quick iteration speed, and getting familiar with some NLP basics.
 
**Fastai** was the best course I've taken, and library was great for doing standard experiments quickly. Unfortunately I found it difficult to do non-standard things with, and code becomes a mess as soon as you try to change anything. Something about the way it is written means to change things they didn’t explicitly intend to takes a lot of effort. I loved the good defaults and having less to worry about when running experiments, but it not ideal for weird research. Luckily there were a couple more I'd never tried before, ignite and lightning. 

**Ignite** is used by [huggingface](https://github.com/huggingface/transformers), which probably has the nicest ML code I've seen, but **Lightning** has more GitHub stars and more memes in their medium articles!

I did the Huggingface [transfer learning tutorial](https://github.com/huggingface/naacl_transfer_learning_tutorial) using a different dataset with both ignite to see which one I liked better. It was written using ignite, which should have made it easy, but I didn't like the experience. I didn’t find it intuitive. I couldn't quite figure out why, or if I should take the effort to get used to this, so I moved on to Lightning.

Lightning was more intuitive, but filled with bugs. Even a basic task like mine ran into multiple problems with logging, and lack of documentation. It was annoying because it seemed perfect on the surface. It’s clearly a work in progress but I like the direction, so I’ll stick with it until it becomes limiting. 

I also realized how good huggingface was. Their transfer learning tutorial was amazing, and their code is so clear and concise compared to most I've seen. This makes a big difference in getting up to speed with NLP.

_______
### Meta: How to have a bad career?
I saw David Patterson's talk [How to Have a Bad Career](https://www.youtube.com/watch?v=Rn1w4MRHIhc), which feels like a more practical and modern version of [You and Your Research](https://www.youtube.com/watch?v=a1zDuOPkMSw). I'll make sure to revisit this one.

There's tons of practical advice on making presentations, giving talks, etc, but there were two big things that stood out for me:

**Spend as much time polishing and presenting the research as you did on the original research** If people don't know about your work, or don't understand it, its the same as not having done it. This is totally contrary to the way I used to work, where I would spend the absolute minimum time possible on presentation. To me this was bullshit, I'm not a used car salesman, so my work will speak for itself. But I was wrong, and its time to treat presentation as a central part of my work.

**Frequent feedback is key** Just like in reinforcement learning, delayed rewards are a huge issue with research. The best way to improve research is an another perspective, from someone who will be critical with your work. Even better to occasionally get feedback from people even further from your topic for a wider perspective.
