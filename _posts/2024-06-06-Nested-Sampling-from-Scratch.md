---
title: 'Nested Sampling from Scratch'
date: 2024-06-06
permalink: /posts/2024/06/nested-sampling/
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

So it can be noted that evidence \\(Z = P(D | M) \\) is an important part of Bayesian inference. On the one hand, only by calculating it can we get the analytical expression of the posterior distribution, and on the other hand, it also shows how reasonable the model we choose is for the data. For two models \\(M_0\\) and \\(M_1\\), the Bayes factor:

$$
B_{10} = \frac{Z_1}{Z_0}
$$

tells us how we must update the relative plausibility of two models in light of data.

Therefore, we need methods to compute the integral on parameter space to get the evidence.

Nested Sampling
------

For evidence \\(Z \\), use the Riemann integral to calculate：

$$
Z = \int L(\Theta) \pi(\Theta) d \Theta = \sum L(\Theta_i) \pi(\Theta_i) \Delta \Theta_i
$$

where \\( \Delta \Theta_i \\) denotes the \\(i\\)-th hypercube in parameter space. However, 
