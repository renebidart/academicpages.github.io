---
title:  "VAEs-Explained"
date:   2018-03-13
author: Rene Bidart
---

The two motivations for Variational Autoecoders (VAE). When I first saw example code for the VAE, it looked like someone had tweaked the standard autoencoder in order to make it work as a generative model. It turns out this was not the original motivation, and that in addition to the making it work peerspective, we can see the VAE as ?? statistical/variational inference.

## Make autoencoders better 


An autoencoder performs compression. Just like you can compress and uncompress an image with jpeg, you can use an autoencoder to compress an image to a small latent vector, $$z$$, and to decompress it. The difference is here we use a neural network for the compression (encoding) and decompression (decoding).

| ![autoencoder_1d.png](/images/post_imgs/vae/autoencoder_1d.jpg) |
|:--:| 
| *The standard autoencoder, where an input image x is encoded/compressed into a smaller dimensional latent vector z* |


But what if we want to generate new images, not just compress existing ones? Can we plug random latent values into the decoder and hope it creates a decent image? 

There are a few reasons why this won't work. 
Not all $z$ values correspond to an image, and we don't know how to sample those that do. Also the latent space is not structured, which is something that would be a useful property to have for interpolation.

Solve the sampling problem with KL, and the problem of ensuring all z correspond to some image by adding noise. When noise is added to this, it both forces nearbly points to produce similair images, and enforces an information bottleneck, where the model must take advantage of the full latent space, not just putting images within epsilon of each other. Maximally spread the images out in the latent space, but not in any way because of the KLD.


#### 1. Not all $z$ values correspond to an image, and we don't know how to sample those that do.
It would be nice if we could sample z from some distribution, so we could ensure we get reasonably “normal” images most of the time, with some variance so unusual ones appear occasionally. To do this, we can add a loss team to the original Autoencoder, forcing the latent space to be close to some distribution.

For simplicity, we will force the latent space to be standard normal, and use the KL divergence as loss function:

\begin{equation}
Loss = KL(z || N(0, 1)]
\end{equation}

Now we should have some way to sample from the latent space, but is it structured in a sensible way?

#### 2. This latent space has no structure
It would be great if nearby points in z represented similar images, so we could attempt to interpolate between images, or have a more principled way to sample similar ones. In addition, there is a danger that the latent space may still contain $z$ values that do not correspond to sensible images. The deoc

To solve this, we could add noise to the latent vectors. This will force points near a given latent vector to be decoded to similar images, to reduce the reconstruction loss. To keep things simple, we can add Gaussian noise.

If we do this, we will get something resembling a VAE:

| ![vae_1d_fake_v2.png](/images/post_imgs/vae/vae_1d_fake_v2.jpg) |
|:--:| 
| *The result of trying to make an autoencoder a generative model. The addition of another loss function is explicitly indicated here. This is NOT a VAE* |



Now, if we add in a learnable sigma, we’re left with the true VAE!


| ![vae_1d.png](/images/post_imgs/vae/vae_1d.jpg) |
|:--:| 
| *The actual VAE. Note here that both the mean and the variance of the noise distribution are learned.* |

This model is written in this indirect way because it isn't possible to directly output the parameters and train them with backprop. For this the *reparamatrization trick* is used to allow us use backprop to train the parameters of this distribution. This is a simple concept, and just relies on the fact that we can get a normal distribution of any mean or variance by multiplying the standard normal by a variance and adding in the mean. Now everything is nice and differentiable! ??? Reareead stuff to make sure I'm right about this. Maybe even add in picture of what it would look like without this (either in vae or just showing the graphical model part.)



This seems resonable, but look at all the equations in [original VAE paper](https://arxiv.org/pdf/1312.6114.pdf). Clearly the above explaination isn't whole truth. So what is the other motivation for the VAE?

## Statistics?? varinf??


Equation time. So we assume we’d like to create a generative model


