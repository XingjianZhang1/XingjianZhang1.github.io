---
title: 'Nested Sampling from Scratch'
date: 2024-05-06
permalink: /posts/2024/05/nested-sampling/
tags:
  - Statistics
  - Nested Sampling
  - Self-taught Records
---

By chance, I learned about the Nested Sampling method. Noting its potential in dealing with multimodal problems and the fact that there seemed to be little information about it on the Chinese Internet, I decided to reproduce this method from scratch, both to record my own learning process and to write a full Chinese introduction to this method in the future.

Nested sampling (NS) is a Monte Carlo algorithm first developed for computing Bayesian evidence, but it can also provide posterior samples, which makes it different from other Monte Carlo algorithms that can only provide posterior samples. Under normal circumstances, integration in high-dimensional space is often plagued by the "curse of dimensionality" and is therefore not feasible, which is also one of the main obstacles to Bayesian inference. MCMC algorithms bypass the marginal likelihood method for sampling, while NS solves this problem through the exponential behavior of volume compression.

Introduction to Nested Sampling
======

Bayesian Inference
------

Back to Bayesian statistics, let \\(M\\) denotes the statistical model we selected, \\( \Theta \\) denotes model parameters (can be a vector for multidemensional case) and \\(D\\) denotes the data, the posterior distribution is:

$$
P(\Theta | D, M) = \frac{P(D | \Theta, M) P(\Theta | M)}{\int P(D | \Theta, M) P(\Theta| M) d \Theta} = \frac{L(\Theta) \pi(\Theta)}{Z}
$$

where \\(L(\Theta) \\) is the likelihood, \\(\pi(\Theta) \\) is the prior distribution and \\(Z\\) is marginal likelihood, or evidence.

So it can be noted that evidence \\(Z \\) is an important part of Bayesian inference. On the one hand, only by calculating it can we get the analytical expression of the posterior distribution, and on the other hand, it also shows how reasonable the model we choose is for the data. For two models \\(M_0\\) and \\(M_1\\), the Bayes factor:

$$
B_{10} = \frac{Z_1}{Z_0}
$$

tells us how we must update the relative plausibility of two models in light of data.

Therefore, we need methods to compute the integral on parameter space to get the evidence.

The General Principle of Nested Sampling
------

For evidence \\(Z \\), use the Riemann integral to calculate：

$$
Z = \int L(\Theta) \pi(\Theta) d \Theta = \sum L(\Theta_i) \pi(\Theta_i) \Delta \Theta_i
$$

where \\( \Delta \Theta_i \\) denotes the \\(i\\)-th hypercube in parameter space. However, due to the curse of demensionalty, the number of hypercubes required will increase exponentially. Similar increases in computational cost also occur in simple methods such as reject sampling. Therefore we need methods to overcome it.

(Starting here is my personal understanding, which may not be correct. Please do not refer to it.)

NS circumvents the problem of high-dimensional parameter space. Note that the likelihood function \\( L(\Theta) \\) and the prior \\( \pi(\Theta_i) \\) are one-dimensional regardless of the parameter dimensions (i.e., the probability of data appearing under a certain parameter and the probability of a certain parameter appearing in our prior knowledge). Therefore, directly integrating them avoids the curse of dimensionality.

Using ideas similar to the Lebesgue integral, we no longer need to decompose the hyperspace into small hypercubes, but can decompose it into any shape needed, as follow:

$$
Z = \int L(\Theta) \pi(\Theta) d \Theta \\
= \sum L(\Delta V_i) \Delta V_i
$$

where \\( V_i \\) denotes the i-th sub-hyperspace small enough that share the same likelihood value \\( L(\Delta V_i) \\), and \\( \Delta V_i \\) denotes the volume of this sub-hyperspace.

Therefore we only need to get a one-dimensional integral, even if the parameters \\( \Theta \\) may be high-dimensional. (I might put a visual example here in the future)

Algorithm of Nested Sampling in more detail
------

