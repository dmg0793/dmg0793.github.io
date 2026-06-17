---
title: "Theory of Statistics I — Chapter 1.1: Statistical Models"
date: 2026-03-19
categories:
- Mathematics
- Theory of Statistics
tags:
- statistics
- decision-theory
- statistical-model
- identifiability
toc: true
toc_label: "Contents"
mathjax: true
---

> This series is a running set of notes for **Theory of Statistics I**.
> Content will be updated periodically.

---

## Notation

| Symbol | Meaning |
|--------|---------|
| $(\Omega, \mathcal{F}, \mathbb{P})$ | A probability space |
| $X : \Omega \to \mathbb{R}^n$ | A random vector |
| $\mathcal{P} = \{P_\theta : \theta \in \Theta\}$ | A statistical model (family of distributions) |
| $\Theta$ | The parameter space |
| $\mathcal{X}$ | Sample space |
| $\mathcal{X}^{(n)}$ | The $n$-fold sample space |
| $\mathcal{C}$ | Collection of CDFs on $\mathbb{R}$ |
| $\phi(\cdot)$ | Standard normal PDF |
| $\mathbb{I}(\cdot)$ | Indicator function |

---

## 1. Statistical Models

We begin by fixing the objects that the whole theory is built on: a probability space, a random vector observed from it, and a family of candidate distributions for that random vector.

<div class="math-box box-definition">
<span class="math-box-title">Definition 1.1 — Probability space and random vector</span>

A **probability space** is a triple $(\Omega, \mathcal{F}, \mathbb{P})$. A **random vector** is a measurable map

$$X : \Omega \to \mathbb{R}^n.$$

</div>

<div class="math-box box-definition">
<span class="math-box-title">Definition 1.2 — Statistical model</span>

A **statistical model** $\mathcal{P}$ is the family of candidate distributions of $X$. When the family is indexed by a parameter $\theta$, we write

$$\mathcal{P} = \{P_\theta : \theta \in \Theta\}, \qquad \theta \longmapsto P_\theta,$$

where $\theta$ is the **parameter** and $\Theta$ is the **parameter space**.

</div>

<div class="math-box box-example">
<span class="math-box-title">Example 1.3 — Location model</span>

Let $X_i = \mu + \varepsilon_i$ with $\varepsilon_i \overset{iid}{\sim} F$ for $i \in [n]$. The goal is to learn $\mu \in \mathbb{R}$. Depending on what we assume about $F$, the model changes:

**(i)** $F \sim \mathcal{N}(\mu, 1)$:

$$\mathcal{P} = \left\{ \prod_{i=1}^{n} \phi(x_i - \mu) : \mu \in \mathbb{R} \right\}.$$

By convention we write $\mathcal{P} = \{ \phi(\cdot - \mu) : \mu \in \mathbb{R} \}$, with slight abuse of notation, since the iid condition is understood.

**(ii)** $F \sim \mathcal{N}(\mu, \sigma^2)$ with $\sigma^2 \in (0,\infty)$:

$$\mathcal{P} = \left\{ \frac{1}{\sigma}\, \phi\!\left( \frac{\,\cdot\, - \mu}{\sigma} \right) : (\mu, \sigma^2) \in \mathbb{R} \times \mathbb{R}^+ \right\}, \qquad \Theta = \mathbb{R} \times \mathbb{R}^+.$$

**(iii)** $F$ symmetric about zero:

$$\mathcal{P} = \left\{ F(\cdot - \mu) : F \text{ symmetric about } 0 \text{ on } \mathbb{R},\ \mu \in \mathbb{R} \right\}, \qquad \theta := (\mu, F).$$

</div>

<div class="math-box box-definition">
<span class="math-box-title">Definition 1.4 — Parametric, nonparametric, semiparametric</span>

- **Parametric:** $\Theta$ is finite dimensional; the parameter space is a subset of $\mathbb{R}^k$.
- **Nonparametric:** $\Theta$ is infinite dimensional.
- **Semiparametric:** nonparametric, but $\Theta$ has a finite dimensional parameter of interest.

</div>

<div class="math-box box-definition">
<span class="math-box-title">Definition 1.5 — Identifiability</span>

If the map $\theta \mapsto P_\theta$ is injective, the model is **identifiable**. Otherwise it is **unidentifiable**.

</div>

<div class="math-box box-definition">
<span class="math-box-title">Definition 1.6 — Parameter of interest and nuisance parameter</span>

- The **parameter of interest** is the parameter we want to estimate (the primary target).
- A **nuisance parameter** is a parameter that is not of primary interest.

</div>

<div class="math-box box-definition">
<span class="math-box-title">Definition 1.7 — Statistic</span>

A **statistic** $T$ is a function of the sample $X_n$.

</div>

<div class="math-box box-example">
<span class="math-box-title">Example 1.8 — Empirical CDF</span>

The **empirical CDF** is

$$T(x)(t) = \frac{1}{n}\sum_{i=1}^{n} \mathbb{I}(X_i \le t),$$

where $T : \mathcal{X}^{(n)} \to \mathcal{C}$ maps a sample into the collection $\mathcal{C}$ of CDFs on $\mathbb{R}$.

</div>

<div class="math-box box-example">
<span class="math-box-title">Example 1.9 — Regression model</span>

The data are $\{ (Y_i, X_i) : Y_i \in \mathbb{R},\ X_i \in \mathbb{R}^p \}$, where $Y_i$ is the **response** and $X_i$ the **covariate**. Conditionally,

$$\mathcal{P}_{\mathrm{cond}} := \left\{ \prod_{i=1}^{n} P_{x_i} : P_{x_i} \text{ a distribution on } \mathbb{R} \text{ given } x_i \right\}.$$

Three nested specifications illustrate the parametric–nonparametric spectrum:

**(a)** No restriction on $P_{x_i}$.

**(b)** $Y_i = g(X_i) + \varepsilon_i$ with $\varepsilon_i \overset{iid}{\sim} F$, so that

$$\mathcal{P} := \{ g : \mathbb{R}^p \to \mathbb{R} \} \times \{ F \text{ unspecified} \}, \quad \text{or} \quad \{ g \} \times \{ F \sim \mathcal{N}(\mu, \sigma^2),\ \sigma^2 > 0 \}.$$

**(c)** Linear model:

$$\mathcal{P} := \{ g : \mathbb{R}^p \to \mathbb{R} \mid g(x) = \langle x, \beta \rangle,\ \beta \in \mathbb{R}^p \} \times \{ F \}.$$

</div>
