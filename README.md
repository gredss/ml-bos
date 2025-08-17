# Barrier Option Pricing and Machine Learning

This project explores the intersection of **quantitative finance** and **machine learning** through the construction of a synthetic dataset for barrier option pricing. The workflow combines stochastic simulation, classical pricing engines, and machine learning techniques for approximation and surrogate modeling.

---

## 1. Mathematical Foundations

### Barrier Options

Barrier options are exotic derivatives whose payoff depends not only on the terminal asset price but also on whether the underlying asset has crossed a specified barrier during the option’s life. Common variants include:

* **Knock-in options**: Only activate if the barrier is breached.
* **Knock-out options**: Become void if the barrier is breached.

Mathematically, let:

* $S_t$: underlying asset price at time $t$
* $B$: barrier level
* $K$: strike price
* $T$: maturity
* $r$: risk-free rate
* $q$: dividend yield

The risk-neutral dynamics of the asset price are given by:

$$
 dS_t = (r - q)S_t dt + \sigma S_t dW_t
$$

where $\sigma$ is volatility and $W_t$ is a Wiener process.

### Pricing Approaches

Pricing a barrier option requires considering path dependency. Closed-form solutions exist for some types (e.g., down-and-out call), but numerical methods such as Monte Carlo simulation or finite differences are typically used.

---

## 2. Dataset Construction

The dataset is built by sampling parameters across realistic ranges:

* Spot price ($S$)
* Strike price ($K$)
* Volatility ($\sigma$)
* Risk-free rate ($r$)
* Dividend yield ($q$)
* Barrier level ($B$) relative to spot
* Time-to-maturity ($T$)

For each configuration, the option price and sensitivities (Greeks) are computed using a pricing engine.

---

## 3. Data Quality Challenges

### Numerical Instabilities

Several cases lead to unreliable pricing outputs:

* **Near-zero price**: Extreme conditions (e.g., very far out-of-the-money).
* **Analytic fallback**: Numerical solver fails, requiring a closed-form approximation.
* **Near-barrier**: Asset starts close to the barrier, increasing discontinuity risk.
* **Greek outliers**: Sensitivities explode near maturity or barrier.
* **Very short maturity (T)**: Numerical schemes unstable when $T$ is too small.

### Cleaning Strategy

The dataset is filtered by retaining only a fraction of problematic cases to preserve variety but avoid dominance:

* Retain \~5% of near-zero prices.
* Retain \~20% of analytic fallback cases.
* Preserve a balanced representation of barrier regions (near, mid, far).

---

## 4. Machine Learning Component

The cleaned dataset provides the basis for supervised learning experiments:

* **Inputs**: (S, K, σ, r, q, B, T)
* **Outputs**: Price, Delta, Gamma, Vega, etc.
* **Goal**: Train surrogate ML models (e.g., XGBoost, neural networks) to approximate pricing engines.

The rationale is to significantly reduce pricing runtime for large-scale simulations, portfolio risk aggregation, or real-time applications.

---

## 5. Real-World Analogy

Consider an insurance company selling a flood insurance policy with a clause: coverage is void if water level ever exceeds a certain threshold (the barrier). Pricing such a contract requires not only knowing average rainfall but also simulating extreme weather paths. Similarly, barrier options demand path-aware valuation, which introduces mathematical and numerical complexity.

---

## 6. Project Deliverables

* Synthetic dataset (`barrier_dataset_clean.csv`, `barrier_dataset_full_tagged.csv`)
* Diagnostic reports on tag distributions
* ML training scripts and models for surrogate pricing
* Documentation (this README)

---

## 7. Potential Extensions

* Compare classical closed-form vs. ML approximation accuracy
* Introduce variance reduction in Monte Carlo to improve baseline
* Explore deep learning architectures (LSTM, Transformer) for sequential simulation

---

## 8. Objective

This project is intended as a **portfolio-level demonstration** of combining quantitative finance theory with machine learning practice. It is not designed for publication but as a showcase of domain knowledge, coding ability, and methodological rigor for prospective employers.

