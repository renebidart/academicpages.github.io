---
title:  "Open Research 3: Mistakes"
date:   2020-2-16
author: Rene Bidart
---
## Distractions and Mistakes
The emotional roller coaster of research continues. Previously I was working on a paper where the results were mediocre, but I thought there was something useful there. I wanted to finish some kind of paper, even if it would never make it to a top conference.

After some wasted time because of Pytorch lighting and the differences between training on CIFAR10 vs. Imagenet, a few experiments convinced me to had found something good. I was getting unexpectedly good results, where it looked like I'd actually found a better way to prune networks. This quickly came crashing down when I realized a coding error had invalidated every promising result. Basically I missed a downsampling layer when implementing weight sharing, and this guaranteed results would look favorable for my method. All my good results were gone, as well as my pointless theorizing about why this was happening. What went wrong?

_______

### Two sides of research
There are two sides of research - trying to prove you’ve made something good, and trying prove it wrong. This is the scientific method. My issue was I was that I found it difficult to switch between these two modes, so I would do this too slowly. I had done multiple incorrect tests, and not noticed the mistakes until much later. It was carelessness, I knew I had to do test these things, this, but I put it off doing it for too long.

In general, **iteration speed is key in research, and fast iteration speed means quickly generating and testing hypothesis**. Effective testing is uncomfortable, because you must try to disprove your work.  The key here is quickly switching between these ways of looking at it - I always wanted to work on it for weeks before attacking it in any way - but this can end in tons of wasted work. I had this tendency to not properly dig into things that are either difficult or might destroy my plan. An even better way is asking others to pick apart your ideas.


