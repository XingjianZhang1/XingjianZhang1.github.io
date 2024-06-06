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

Introduction to Nested Sampling
======

Nested sampling (NS) is a Monte Carlo algorithm first developed for computing Bayesian evidence, but it can also provide posterior samples, which makes it different from other Monte Carlo algorithms that can only provide posterior samples. Under normal circumstances, integration in high-dimensional space is often plagued by the "curse of dimensionality" and is therefore not feasible, which is also one of the main obstacles to Bayesian inference. MCMC algorithms bypass the marginal likelihood method for sampling, while NS solves this problem through the exponential behavior of volume compression.

Back to Bayesian statistics, let $M$ denotes the statistical model we selected, $\Theta$ denotes model parameters (can be a vector for multidemensional case) and $D$ denotes the data, the posterior distribution is:

$$
P(\Theta | D, M) = \frac{P(D| \Theta, M) P(\Theta| M)}{\int P(D| \Theta, M) P(\Theta| M) d|Theta} = \frac{L(\Theta) \pi(\Theta)}{Z}
$$

where $L(\Theta)$ is the likelihood, $\pi(\Theta)$ is the prior distribution and $Z$ is marginal likelihood, or evidence.
