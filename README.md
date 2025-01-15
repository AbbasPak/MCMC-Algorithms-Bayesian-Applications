# **📊 MCMC Algorithms with Bayesian Applications**

This repository delves into **Markov Chain Monte Carlo (MCMC)** algorithms and their applications in **Bayesian statistics**. It provides:
- 📂 Explanations of key MCMC methods.
- 🔍 Practical, real-world examples in Bayesian inference.
- 🛠️ Implementation guides in R.

---

## ⚠️ Repository Under Preparation

Please note that this repository is currently under development and will be completed in the next month. Stay tuned for updates, including code examples, detailed explanations, and real-world use cases. Watch this repository to get notified of new additions!

---


## **📑 Table of Contents**
1. 🔍 **Introduction**
   - 🌟 What is MCMC?

The Markov Chain Monte Carlo (MCMC) algorithm is a class of methods used in statistical computing to sample from probability distributions. These methods are particularly valuable for estimating distributions that are difficult to compute directly, especially in high-dimensional spaces. MCMC algorithms create a Markov chain that has the desired distribution as its equilibrium distribution, allowing us to generate samples from this distribution by simulating the chain over time.

**Key Concepts:**

Markov Chain: A sequence of random variables where the probability of moving to the next state depends only on the current state (the Markov property). The chain transitions between states in a way that eventually converges to the target distribution.

Monte Carlo: Refers to the use of random sampling to estimate numerical results. In the context of MCMC, it involves using samples generated by the Markov chain to estimate properties of the target distribution.

   - 🔗 Why MCMC is vital in Bayesian inference.

     Markov Chain Monte Carlo (MCMC) methods are extensively used in Bayesian statistics to perform posterior inference. Bayesian statistics involves updating our beliefs about parameters using data, resulting in posterior distributions. In many cases, these posterior distributions are complex and do not have closed-form solutions, making direct sampling or analytical integration intractable. MCMC provides a way to generate samples from these complex posterior distributions, allowing us to estimate various properties of the parameters.

Here’s a detailed explanation of how MCMC is used in Bayesian statistics:

### Key Steps in Bayesian Inference Using MCMC

1. **Specify the Bayesian Model**:
   - Define the prior distribution $\pi(\theta)$ for the parameters $\theta$.
   - Define the likelihood function $L(\theta | \mathbf{y})$ based on the observed data $\mathbf{y}$.

2. **Posterior Distribution**:
   - The posterior distribution is given by Bayes' theorem:
     $$\pi(\theta | \mathbf{y}) = \frac{L(\theta | \mathbf{y}) \pi(\theta)}{p(\mathbf{y})}$$
   - Here, $p(\mathbf{y})$ is the marginal likelihood or evidence, which is often difficult to compute directly.

3. **Constructing the Markov Chain**:
   - Use an MCMC algorithm to construct a Markov chain whose stationary distribution is the posterior distribution $\pi(\theta | \mathbf{y})$.

---

🧩 **Popular MCMC methods in Bayesian statistical models**

 
🔄 **Gibbs Sampling**

**Gibbs Sampling** is a Markov Chain Monte Carlo (MCMC) technique that is particularly useful for sampling from complex, high-dimensional probability distributions when direct sampling is challenging. Gibbs sampling is widely used in Bayesian statistics, especially for models where the joint distribution of parameters is known but difficult to sample from directly.

### Key Idea
The main idea behind Gibbs sampling is to sample each parameter in a model sequentially from its *conditional distribution*, given the current values of all other parameters. This allows us to break down a high-dimensional joint distribution into simpler conditional distributions, which are often easier to sample from.

### How Gibbs Sampling Works

Assume we have a set of parameters $\theta = (\theta_1, \theta_2, \ldots, \theta_d)$ that we want to sample from a joint posterior distribution $p(\theta | \text{data})$. The steps are as follows:

1. **Initialize Parameters**: Start with initial values for all parameters, $\theta^{(0)} = (\theta_1^{(0)}, \theta_2^{(0)}, \ldots, \theta_d^{(0)})$.

2. **Iterate Over Parameters**:
   - For each iteration $t$, sample each parameter $\theta_i$ from its conditional distribution given the current values of the other parameters:
     $$\theta_1^{(t)} \sim p(\theta_1 | \theta_2^{(t-1)}, \theta_3^{(t-1)}, \ldots, \theta_d^{(t-1)}, \text{data})$$, 
     $$\theta_2^{(t)} \sim p(\theta_2 | \theta_1^{(t)}, \theta_3^{(t-1)}, \ldots, \theta_d^{(t-1)}, \text{data})$$, $\ldots$ , 
     $$\theta_d^{(t)} \sim p(\theta_d | \theta_1^{(t)}, \theta_2^{(t-1)}, \ldots, \theta_{d-1}^{(t-1)}, \text{data})$$
     
   
3. **Repeat**: Perform multiple iterations to allow the samples to converge to the target distribution.

4. **Burn-In Period and Sampling**:
   - Discard the initial set of iterations (the “burn-in” period) to reduce the impact of initial values. Afterward, use the remaining samples to make inferences about the posterior distribution.

### Why Gibbs Sampling Works

Gibbs sampling works because each sequential update moves the parameter values closer to their distribution under the joint posterior. Under certain conditions, this chain of parameter updates converges to the joint distribution $p(\theta | \text{data})$, allowing us to estimate posterior summaries (e.g., means, variances, and quantiles). Gibbs sampling is often combined with the Metropolis-Hastings algorithm when conditional distributions are not straightforward to sample from directly. 

---

- 🌀 **Metropolis-Hastings Algorithm**
The **Metropolis-Hastings algorithm** is a general MCMC technique used to generate a sequence of samples from a probability distribution when direct sampling is difficult. This algorithm is particularly valuable in Bayesian statistics, where we often work with complicated posterior distributions. The goal of MH is to construct a Markov chain whose stationary distribution is the target distribution.

### Key Idea

The main idea behind Metropolis-Hastings is to use a *proposal distribution* to suggest new sample points, which are then either accepted or rejected based on an *acceptance probability*. This acceptance step ensures that, over time, the samples generated by the algorithm represent the target distribution.

### Steps of the Metropolis-Hastings Algorithm

Suppose we want to sample from a target distribution \( p(x) \) for which we can compute the value of the pdf (up to a normalizing constant) but cannot sample from directly.

1. **Initialize the Chain**: Start with an initial value \( x^{(0)} \).

2. **Proposal Step**: For each iteration \( t \):
   - Use a proposal distribution \( q(y | x^{(t-1)}) \) to propose a new sample \( y \), given the current sample \( x^{(t-1)} \).
     - The proposal distribution \( q(y | x) \) could be, for instance, a normal distribution centered at \( x \): \( q(y | x) = N(x, \sigma^2) \).
   
3. **Acceptance Step**:
   - Calculate the *acceptance probability* \( A(x^{(t-1)}, y) \):
     \[     A(x^{(t-1)}, y) = \min \left( 1, \frac{p(y) \, q(x^{(t-1)} | y)}{p(x^{(t-1)}) \, q(y | x^{(t-1)})} \right)     \]     This ratio compares the probabilities of moving from \( x^{(t-1)} \) to \( y \) and vice versa, weighted by the proposal distribution.
   - Generate a random number \( u \sim \text{Uniform}(0, 1) \).
   - **If** \( u < A(x^{(t-1)}, y) \), accept the proposal and set \( x^{(t)} = y \).
   - **Otherwise**, reject the proposal and set \( x^{(t)} = x^{(t-1)} \) (i.e., stay at the current value).

4. **Repeat**: Continue this process for a large number of iterations to allow the chain to converge to the target distribution.

### Why It Works
The acceptance probability \( A(x^{(t-1)}, y) \) is carefully designed so that the Markov chain formed by the MH algorithm has the target distribution \( p(x) \) as its stationary distribution. Over time, this means that the samples generated by the algorithm approximate the target distribution.

### Choosing the Proposal Distribution \( q(y | x) \)
- The choice of proposal distribution \( q(y | x) \) can significantly affect the efficiency of the algorithm. Common choices include:
  - **Random walk**: \( q(y | x) = N(x, \sigma^2) \), where \( y \) is sampled from a normal distribution centered at \( x \).
  - **Independent proposals**: \( q(y) \) independent of \( x \), useful when you have a good approximation of the target distribution.

### Advantages of Metropolis-Hastings

- **Flexibility**: The algorithm can handle any target distribution as long as it’s proportional to a known function.
- **Simple Setup**: You only need the form of the target distribution up to a constant, making it ideal for Bayesian problems where normalizing constants are intractable.

### Limitations

- **Tuning Required**: The proposal distribution needs careful tuning. A proposal with a high variance will result in many rejections, slowing down convergence, while a low variance may lead to slow exploration.
- **Convergence Time**: The algorithm can be slow to converge, especially in high-dimensional spaces or when the target distribution has complex shapes (e.g., multiple modes).



---

When applying MCMC methods in Bayesian inference, several challenges and limitations can arise. Here are some common ones:

---
1. 📈 **Applications in Bayesian Statistics**
   - 📐 Bayesian inference 
     

2. 💻 **Code Implementation**
   - 🧮 R: MCMC implementation, MCMC Chain Diagnostics & Visualization
     

3. 🌍 **Real-World Applications**
   - 📊 **Case Studies**: Detailed real-world projects.

---

