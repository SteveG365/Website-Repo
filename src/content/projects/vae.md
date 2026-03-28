---
title: "Variational Autoencoders"
description: ""
year: "2026"
date: 2026-03-25
tags: ["Deep GenAI", "Image Reconstruction", "Variational Inference"]
github: "https://github.com/SteveG365/Variational-Autoencoder"
report: "/Variational_Autoencoder-2.pdf"
featured: true
---

## Variational Autoencoder on MNIST

This project implements a Variational Autoencoder (VAE) in TensorFlow/Keras to learn a probabilistic latent representation of handwritten digits from the MNIST-digits dataset. The encoder maps images to a Gaussian latent space and learns the parameters of a variational latent posterior on which we perform importance sampling during training, while the decoder reconstructs images from sampled latent variables using the reparameterisation trick for efficient training.

The model is trained by maximising the Evidence Lower Bound (ELBO), balancing reconstruction accuracy with latent space regularisation via a KL divergence term. This results in a structured latent space that enables meaningful sampling and interpolation between digits.

Key results include reconstruction of input images, generation of new digits from the prior, and analysis of latent dimensions via KL divergence, highlighting how the model utilises its latent representation.