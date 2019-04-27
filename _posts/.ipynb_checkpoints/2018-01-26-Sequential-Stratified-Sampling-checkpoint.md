---
title:  "Sequential Stratified Sampling"
date:   2018-1-27
author: Rene Bidart
---

## Stratified Sampling
We want to get the estimate the mean of a population, and this population is broken into different groups. As long as these groups are homogeneous, we can produce a better estimate by estimating the means of the groups individually. For example, we could estimate the mean math ability of students in Ontario by dividing it up by schools. By allocating more samples to the schools with higher number of students and variance in ability, we can get a better estimate overall. 

Back in 1934 [Jerzy Neyman](http://www.stat.cmu.edu/~brian/905-2008/papers/neyman-1934-jrss.pdf) proved that the optimal way to allocate the number of samples for each stratum, $$n_h$$ is proportional to its standard deviation, $$\sigma_{yh}$$, and its size, $$W_h$$: 

\begin{equation}
n_h \propto W_h\sigma_{yh}
\end{equation}

In reality we won't know the stratum variances, so alternative methods using auxiliary information or two stage sampling to get an estimate of the variances are used. The two sample method is not optimal, and auxillary information may not be availiable. 

Today there are situations where we can take samples sequentially, which may not have been possible when these methods were first developed, such as with an online survey where it is easy to distribute and quick to analyse results, but may be costly to pay an individual to complete it. Here we will allocate each sample individually, but how do we pick which strata to sample from?


## Sequential Sampling

we will think of sampling over a number of timne steps, taking one sample from a given stratum at each time. The goal is to sample from the stratum that will cause the largest expected decrease in variation of our estimate, $$\bar{y}$$ at each iteration. We would would like to find $$i$$ to maximize: $$V(\bar{y}_{st, i})-V(\bar{y_{st}}) = $$

\begin{equation}
\sum\limits_{h=1}^{H}W_{h, i}^2(1-\frac{n_{h,i}}{N_{h,i}})\frac{s_{yh,i}^2}{n_h{,i}}-\sum\limits_{h=1}^{H}W_h^2(1-\frac{n_h}{N_h})\frac{s_{yh}^2}{n_h}
\end{equation}

This says we would like to maximize the decrease in variance

Here $$\bar{y}_{st, i}$$ is used to denote this after another sample is taken from stratum $$i$$. For simplicity, assume that all strata have the same size, so $$W_h$$ are equal. Then, we can notice that all the terms in this sum are equal, except for those relating to stratum $$i$$:
\begin{equation}
(1-(\frac{n_i+1}{N_i}))\frac{s_i^{*2}}{n_i+1} - (1-(\frac{n_i}{N_i}))\frac{s_i^{2}}{n_i}
\end{equation} 
If we assume $$s_i^{*2}=s_i^{2}$$, after rearranging we obtain the objective as:
\begin{equation}
\max_{i} \frac{s_i^{2}}{n(n+1)}
\end{equation} 

This shows that the expected benefit is increasing in the variance of the sample, as would be expected from Neyman allocation, and is decreasing in the number of samples already taken from that strata, because as more samples are taken from a given strata, the we have a better estimate of its' mean.


## Procedure:
1. Take a small number of samples from all strata, compute variance, and the expected benefit estimates for all strata.
2. Compute the expected benefit for each of the strata
3. Sample from the strata with maximum expected benefit
4. Repeat this until a total of $$n$$ samples are taken

*Implementation Detail:  To make this computationally efficient, we can use the iterative update formulas for the mean and standard deviation. Using this means previous samples don't have to be stored.*

## Conclusion
This is a simple way to take advantage sequental sampling using stratified sampling. Compared to the two-sample method, this will more efficiently allocate samples, so should result in lower variance of the estimator. More details are [here](https://github.com/renebidart/stratified-sampling).




