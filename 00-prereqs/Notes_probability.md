## 25-05-2026 - Phase 0 - Session 7

What I did:

- Watched video on Histogram, probability distribution, normal distribution

What clicked:

- A `distribution` describes how probability is spread across the values of a random variable.
- Stacking values of random variable in different bins (a range of values) is called `Histogram`. Too many and too less bins gives us no idea how data is distributed.
- Normal (Gaussian) distribution, also called the bell curve, is a **symmetric** distribution that **peaks at the mean** and tapers off smoothly on both sides. It is fully described by two parameters: the **mean (μ)** which sets the center, and the **standard deviation (σ)** which sets the spread. A larger standard deviation gives a wider & shorter curve; a smaller standard deviation gives a narrower & taller curve (because the total area under the curve must always equal 1).
- The parameter defines how a distribution fits population data are called `population parameters`.
- `Mean` is the average of all collected values.
- `Variance` is the average of all the squared difference between mean and that value. $$\text{Var}(X) = \frac{1}{n}\sum_{i=1}^{n}(x_i - \mu)^2$$
- `Standard deviation` is the square root of Variance, $\sigma = \sqrt{\text{Var}(X)}$
- Calculated vs Estimated mean, variance & standard deviation
  - When we have the complete data for a population, mean, variance, and standard deviation are calculated directly (population parameters: $\mu$, $\sigma^2$, $\sigma$). When we only have a sample, we estimate them (sample statistics: $\bar{x}$, $s^2$, $s$). The variance formula changes slightly — instead of dividing by $n$, we divide by $n-1$ (Bessel's correction) to compensate for the fact that we're measuring deviations around the estimated mean $\bar{x}$ rather than the true mean $\mu$. Standard deviation follows as the square root of variance.
- `Bayes' theorem` — tells you how to **update belief** about a hypothesis when new evidence arrives. It flips the direction of conditioning: you usually know $P(E \mid H)$ (forward) but want $P(H \mid E)$ (reverse).
  - **Formula:** $$P(H \mid E) = \frac{P(E \mid H) \cdot P(H)}{P(E)}$$
  - **Names of each piece:**
    - $P(H)$ — **prior**: belief about $H$ before seeing evidence
    - $P(E \mid H)$ — **likelihood**: how likely the evidence is, _if_ $H$ were true
    - $P(E)$ — **evidence / marginal**: total probability of seeing $E$ across all hypotheses
    - $P(H \mid E)$ — **posterior**: updated belief about $H$ after seeing $E$
  - **The mantra:** $\text{posterior} \propto \text{likelihood} \times \text{prior}$. The denominator $P(E)$ is just a normalizer that makes all posteriors sum to 1.
  - **Computing the denominator (law of total probability):** $$P(E) = \sum_{i} P(E \mid H_i) \cdot P(H_i)$$ summed over all possible hypotheses (e.g. for binary case: $P(E) = P(E \mid H) P(H) + P(E \mid \neg H) P(\neg H)$).
- `Expected value` is the long-run average outcome of a random variable i.e. the sum (or integral) of each possible outcome weighted by its probability. In a bet, it's the average payoff per play if you repeated the bet many times. Note: it's an average **value**, not a probability. It can be negative and is not bounded in $[0, 1]$.
  - **Discrete random variable:** $$E[X] = \sum_{i} x_i \cdot P(x_i)$$
  - **Continuous random variable:** $$E[X] = \int_{-\infty}^{\infty} x \cdot f(x) \, dx$$ where $f(x)$ is the probability density function.
- `Probability` and `likelihood` come from the same curve (the Probability Density Function, or PDF), but they ask different questions depending on what's held fixed.
  - **Probability** — parameters are fixed (you know the mean and standard deviation), and you ask: _"What's the chance the data falls in this range?"_ It's the **area under the PDF curve** over a range of $x$ values.
  - **Likelihood** — the data is fixed (you've observed some values), and you ask: _"How plausible are these parameters?"_ It's the **PDF value treated as a function of the parameters**, not of $x$.
  - Probability adds up to 1 across all possible data; likelihood does not add up to 1 across parameters. In everyday speech the two words mean the same thing, but in statistics and ML they're distinct.
- `Maximum Likelihood Estimation (MLE)`, which is how neural networks are trained, works by finding the parameters that make the observed data most likely for a selected distribution.
- `Log Likelihood` — The log likelihood is just the natural logarithm of the likelihood function:
  $$\ell(\theta) = \log L(\theta) = \log \prod_{i=1}^{n} f(x_i \mid \theta) = \sum_{i=1}^{n} \log f(x_i \mid \theta)$$
  We use it for two reasons:
  - **Numerical stability:** the raw likelihood is a product of many small PDF values and underflows to 0 on a computer. Taking the log keeps it in a usable range.
  - **Easier optimization:** $\log$ turns the product into a sum, so gradients add cleanly across data points — which is what makes gradient descent on a batch work.

  Since $\log$ is monotonic, maximizing $\ell(\theta)$ gives the same $\theta$ as maximizing $L(\theta)$.

- `ArgMax` vs `SoftMax`
  - `ArgMax` operates on a vector (in ML, the output layer logits) and returns the **index** of the largest value — i.e., the predicted winner class. Used at **inference / evaluation time**. Cannot be used in backpropagation because it's discontinuous and its derivative is zero almost everywhere — no gradients flow through it.
    $$\text{argmax}(z) = i^* \quad \text{such that} \quad z_{i^*} \geq z_j \;\; \forall\, j \in \{1, \dots, K\}$$
  - `SoftMax` also operates on a vector (the output layer logits) but converts them into a **probability distribution** — values between 0 and 1 that **sum to 1**, expressing how confident the model is in each class. Used during **training** because it's smooth and differentiable, so gradients flow through it during backpropagation.
    $$\text{softmax}(z)_i = \frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}$$
  - Taking argmax of the softmax output gives the same winner as argmax on the raw logits, since softmax preserves the ranking.
- `Cross entropy` is a loss function that measures how different two probability distributions are. In ML, those two distributions are:
  - $y$ — the **true** distribution (the correct label, usually one-hot encoded: e.g. $[0, 0, 1, 0]$ for class 2)
  - $\hat{y}$ — the **predicted** distribution (the softmax output of your model: e.g. $[0.1, 0.2, 0.6, 0.1]$)

  **Formula (general form):**
  $$H(y, \hat{y}) = -\sum_{i=1}^{K} y_i \log \hat{y}_i$$

  For one-hot labels (only one $y_i = 1$, rest are 0), the sum collapses to a single term — the log probability the model assigned to the correct class:
  $$H(y, \hat{y}) = -\log \hat{y}_{\text{correct class}}$$

What is still fuzzy/questions to chase:

- Bessel's correction
- Binomial distribution
- KL Divergence

Time spent: ~4hrs
