---
title: "Using global sensitivity analysis to understand deep learning"
excerpt: "My master's thesis studied the application of Global Sensitivity Analysis (GSA) and Approximate Bayesian Computation (ABC) on Neural Network models. <br/><img src='/images/500x300.png'>"
collection: portfolio
---

Deep learning models such as Deep Neural Networks (DNN) perform well, but their interpretability is low. In response to this, I first used Global Sensitivity Analysis (GSA) to analyze the impact of each input in the fitting model on the output, and then explored the use of Approximate Bayesian Computation (ABC) to train the Bayesian Neural nerworks (BNN) model and the use of ABC to set appropriate informative priors for BNN models.

-This article uses the housing price dataset of Ames, Iowa, USA. DNN is first used for fitting, and then the GSA methods called Sobol' indices is used to analyze the importance of each input feature and simplify the model.

-Improve the original model to BNN and find it difficult to set non-informative priors for the model parameters.This is because 1） we cannot determine the significance of a single BNN model parameter and 2）the model is too complex, resulting in the likelihood being intractable.

-To solve the above problem 1, we choose to set information priors for parameter functions (such as the value of the Sobol' index of a certain input feature, which represents the importance of the factor). Also, because the model is too complex, it is impossible to directly derive the prior distribution of each model parameter from the prior distribution of the Sobol' index.

-Note that the current problems are all due to the model being too complex to get explicit priors/likelihoods. The ABC method essentially circumvents this problem by using a simulator instead of a complex model. 

-<span style="color:red"> Therefore, I developed an ABC-based to set informative priors for complex models and obtain corresponding posterior estimates when the likelihood is intractable. And checked the effectiveness of the algorithm on a simple polynomial model

<a href="/files/dissertation-v2.0.pdf">Get the full paper here.</a>
