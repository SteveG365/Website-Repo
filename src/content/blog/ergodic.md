---
title: "Why the Ergodic Theorem is so Important"
description: ""
pubDate: 2025-05-30
---

Ergodic theory is a branch of Mathematics which concerns itself with the long-term statistical properties of a system. The ergodic theorem lies at the intersection of all the fields I am most passionate about, statistics, data science, and physics. The ergodic theory originates from statistical physics and was proved in its general form originally by Birkhoff in 1931 [1], and it asserts that for a wide array of measure preserving systems (we will refer to these as ergodic), the long-time average of the system will converge to the ensemble average of the system. This has been a monumental result in computational science, and has revolutionised subfields of physics, statistics, and data science.

**Definition:** Consider a measure-preserving dynamical system represented by $\bigl(X ,\mu,  \mathcal{M}, T\bigr)$. $X$ represents the state space, $\mathcal{M}$ the $\sigma$-algebra of X (ie. the collection of subsets $A \subset X$). $\mu$ is the probability measure, and $\mu(A)$. 
For this to be a measure preserving system it is necessary that for every $A \in \mathcal{M}$ it is true that $\mu(A) = \mu(T^{-1}A)$. T is an evolution operator $X \rightarrow X$, which in our setting will be called the time evolution operator.

**Ergodic Theorem:** Let $f : X \mapsto \mathbb{R}$ be an integrable function. We define the time average of $f$ as $\bar{f}(x) = \sum_{i=0}^{N-1} f\bigl( T^i x \bigr)$. It follows that:

1. The limit $\tilde{f} \equiv \lim_{N \rightarrow \infty} \bar{f}(x)$ exists for almost all $x \in X$

2. The limit $\tilde{f}$ is invariant under transformations of $T$, i.e. $\tilde{f}(Tx) = \tilde{f}(x)$ for almost all $x \in X$

3. For any $T$-invariant set $A \in \mathcal{M}$:
   $$
   \int_{A} \tilde{f}(x)\, d\mu = \int_{A} f(x)\, d\mu
   $$

## Proof Sketch:
The proof is outlined by Damon Binder [2] in three main steps:

**1. Maximal Inequality.**  
A maximal ergodic lemma provides control over the oscillations of the time averages, ensuring they cannot diverge too wildly.

**2. Existence of the Limit.**  
From this control, one shows that the $\limsup$ and $\liminf$ of the averages coincide almost everywhere, so the time averages converge to a limit function $\tilde f$.

**3. Identification of the Limit.** 
The limit $\tilde f$ is $T$-invariant. In general, it may vary with $x$, but in the ergodic case every invariant function is constant, forcing

$$
\tilde f(x) = \int f \, d\mu \quad \text{almost everwhere.}
$$

Thus, time averages exist almost surely, and for ergodic systems they equal ensemble averages.

I initially encountered the ergodic theorem in the physics course "Simulation and Inference in Stochastic Systems", where I was introduced to simulating the $2$-dimensional Ising model using Markov Chain Monte Carlo (MCMC). The Ising model in $2$D contains $N$ lattice sites, with each containing a spin value $s_{ij} \in \{-1, 1\}$ which represents magnetic alignment in the north or south direction. Define the entire lattice of all spin sites as $\mathbf{s}$, then the probability distribution is given by the Boltzmann distribution:

$$
p(\mathbf{s}) = \frac{1}{Z}e^{-\beta \,E(\mathbf{s})}.
$$

The $\beta$ term represents the inverse temperature, $E(\mathbf{s}) = h\sum_{i=1}^{N}s_i \,+\, J \sum_{\langle i,j \rangle}s_i s_j $ is the energy of the configuration which can be feasibly calculated, and Z is the normalising "partition function". We found in the course that in order for this distribution to be normalised: $Z = \sum_{\mathbf{s}} e^{-\beta \, E(\mathbf{s})}$, which sums over every possible configuration. It is not hard to see that computing this normalising constant very quickly becomes infeasible, as there are $2^N$ possible spin configurations (which grows exponentially with N). So, if one was to compute an expectation relative to this distribution, for example the magnetisation $M$:

$$
\langle M \rangle = \mathbb{E}_p[M] = \sum_{\mathbf{s}}M(\mathbf{s}) \,p(\mathbf{s}) =  \sum_{\mathbf{s}}M(\mathbf{s})\frac{1}{Z}e^{-\beta \,E(\mathbf{s})} 
$$

Computing this ensemble average is clearly intractable due to the partition function. However, we can use the ergodic theorem here by sampling points from an ergodic Markov chain.
Rather than computing the ensemble average explicitly, the system is evolved under an ergodic sampling process (such as MCMC with the Metropolis-Hastings algorithm) and compute the magnetisation at each time step. The time average of $M(\mathbf{s})$ along a single trajectory will converge to the ensemble average as the number of samples grows. This makes the problem tractable, and we can now analyse thermodynamic properties of the system.


Although the ergodic theorem originated from statistical physics, it has proven itself to be extremely useful in data science. Bayesian inference has generally only been feasible for conjugate likelihood/prior pairs, such that the resulting posterior distribution takes on a known distribution's kernel, and one can infer the normalising constant. However, for more complex Bayesian inference where the prior and likelihood do not combine to form a known distribution, one can still perform Bayesian inference by using an MCMC sampling approach. In the course "Stochastic Simulation" I took in my Honours year, we learnt how to use the Metropolis-Hastings sampler, as well as the Gibbs sampler to efficiently simulate samples from intractable posterior distributions. My project for this course was investigated how the R-package "NIMBLE" works, and how they efficiently simulate posterior samples for any prior/likelihood combination the users wish to use. I then used this framework to infer what the $\beta$ parameter was for a given set of observed Ising spin configurations using a non-informative prior, and a pseudo-likelihood term.

Additionally, in reinforcement learning, the performance of a policy is evaluated by its expected reward in the long run. Again, the ensemble averages are inaccessible, however, due to the ergodic theorem one can observe the trajectory of one agent over a long period of time. The ergodic theorem ensures that if the underlying Markov decision process is ergodic, then the empirical time average reward along this trajectory will converge to the true expectation value.


It is important to understand the ergodic theorem's limitations. The ergodic theorem guarantees convergence "almost surely", which in practice means that assessing whether the sampling process has converged is not trivial. I witnessed this first hand with my project on "NIMBLE", as the chain would initialise far away from the true value, and then eventually appear to converge to drawing samples centred around the true value. Assessing convergence can be done both qualitatively through examining the trace plot, or quantitatively. A significant body of research is dedicated to investigating convergence assessment, for example through diagnostic statistics such as the Gelman-Rubin statistic, effective sample sizes, and visualisations through trace-plots and density overlays.

In conclusion, the ergodic theorem establishes a mathematically rigorous bridge between ensemble averages and time averages. In practice, it is common for the ensemble averages to be computationally infeasible to calculate exactly, and the ergodic theorem allows us to track the evolution of a single trajectory over time and therefore approximate the ensemble average using an empirical time average. Throughout my undergraduate degree,  I have observed ergodic Markov Chains be applied to a variety of dynamical physical systems like the Ising model, as well as simulating samples from Bayesian posterior distributions that otherwise would be intractable. Looking forward, I am fascinated by its use in estimating policy rewards in reinforcement learning, a fascinating area of research in data science.


## References

[1] George D. Birkhoff. Proof of the ergodic theorem. Proceedings of the National Academy of Sciences, 17(12):656–660, 1931.
[2] DJ Binder. The Ergodic Theorem. https://djbinder.com/documents/ErgodicTheorem.pdf.