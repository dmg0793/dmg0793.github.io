---
title: "Theory of Statistics I — Chapter 1.2: Decision Theory"
date: 2026-03-20
categories:
- Mathematics
- Theory of Statistics
tags:
- statistics
- decision-theory
- risk-function
- bayes-rule
- minimax
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
| $\mathcal{A}$ | Action space |
| $L : \Theta \times \mathcal{A} \to \mathbb{R}_{\geq 0}$ | Loss function |
| $\rho_\tau$ | Check loss with $\tau \in (0,1)$ |
| $\delta : \mathcal{X} \to \mathcal{A}$ | Decision rule |
| $R(\theta, \delta) = \mathbb{E}_\theta[L(\theta, \delta(X))]$ | Risk function |
| $\pi$ | Prior distribution on $\Theta$ |
| $r(\pi, \delta)$ | Bayes risk |
| $q_F(\tau)$ | $\tau$-quantile of $F$ |

---

## 2. Elements of Decision Theory

Decision theory provides a unified language for estimation, testing, and interval construction. The basic ingredients are an observation model, an action space, a loss function, a decision rule, and the risk that results from combining them.

<div class="math-box box-definition">
<span class="math-box-title">Definition 2.1 — Ingredients of a decision problem</span>

A decision problem consists of the following.

<strong>(observation / model)</strong> A family $(\mathcal{X}, \mathcal{F}, (P_\theta)_{\theta \in \Theta})$.

<strong>(action space)</strong> A set $\mathcal{A}$ of possible actions. For example, hypothesis testing has $\mathcal{A} = \{0,1\}$, while estimation has $\mathcal{A} = \Theta$.

<strong>(loss function)</strong> A map $L : \Theta \times \mathcal{A} \to \mathbb{R}_{\geq 0}$.

</div>

<div class="math-box box-example">
<span class="math-box-title">Example 2.2 — Check loss</span>

For $\tau \in (0,1)$, define

$$\rho_\tau(u) := u(\tau - \mathbb{I}(u < 0)) = u\tau + \max(-u, 0) = \max(u(\tau-1),\, u\tau).$$

Setting $L(\theta, a) := \rho_\tau(\theta - a)$ on $\mathcal{A} = \Theta = \mathbb{R}$ gives the loss whose risk minimizer is a quantile.

</div>

<div class="math-box box-example">
<span class="math-box-title">Example 2.3 — Testing and interval losses</span>

For hypothesis testing, the $0$-$1$ loss is

$$L(\theta, H_i) = 1 - \mathbb{I}(\theta \in \Theta_i).$$

For interval estimation with $\mathcal{A}$ a collection of subsets of $\Theta$,

$$L(\theta, a) = \mathbb{I}(\theta \notin a).$$

</div>

<div class="math-box box-definition">
<span class="math-box-title">Definition 2.4 — Decision rule</span>

A (non-randomized) <strong>decision rule</strong> is a map $\delta : \mathcal{X} \to \mathcal{A}$, $x \mapsto \delta(x)$. A <strong>randomized decision rule</strong> is a map $\delta : \mathcal{X} \to \mathcal{P}(\mathcal{A})$ into probability measures on $\mathcal{A}$, $x \mapsto \delta_x(da)$, and its loss is averaged:

$$L(\theta, \delta(x)) := \int_{\mathcal{A}} L(\theta, a)\, \delta_x(da).$$

</div>

<div class="math-box box-definition">
<span class="math-box-title">Definition 2.5 — Risk function</span>

The <strong>risk</strong> of $\delta$ at $\theta$ is the expected loss

$$R(\theta, \delta) = \mathbb{E}_\theta\!\left[L(\theta, \delta(X))\right].$$

</div>

<div class="math-box box-example">
<span class="math-box-title">Example 2.6 — Risk under squared and absolute loss</span>

Under squared loss $L(\theta, \delta) = (\theta - \delta)^2$, the risk decomposes as

$$R(\theta, \delta) = \operatorname{Var}_\theta[\delta(X)] + \operatorname{Bias}_\theta[\delta(X)]^2.$$

For $X_i \overset{iid}{\sim} \mathcal{N}(\mu, \sigma^2)$ and $\delta = \bar{X}$, squared loss gives $R(\mu, \delta) = \sigma^2/n$, whereas absolute loss $L(\mu, a) = |\mu - a|$ gives

$$R(\mu, \delta) = \mathbb{E}_\mu|\mu - \bar{X}| = \sqrt{\tfrac{2}{\pi n}}\,\sigma.$$

</div>

<div class="math-box box-example">
<span class="math-box-title">Example 2.7 — Testing as a decision problem</span>

Let $\delta(X) \in \{0,1\}$ be the probability of rejecting $H_0$, with rejection region $C$. Under $0$-$1$ loss,

$$\theta \in \Theta_0:\quad R(\theta, \delta) = P_\theta(X \in C) \quad(\text{Type-I error}),$$

$$\theta \in \Theta_1:\quad R(\theta, \delta) = 1 - P_\theta(X \in C) \quad(\text{Type-II error}).$$

</div>

<div class="math-box box-example">
<span class="math-box-title">Example 2.8 — Confidence sets</span>

With $\delta(X) \subseteq \Theta$ and $L(\theta, \delta) = \mathbb{I}(\theta \notin \delta(X))$,

$$R(\theta, \delta) = 1 - \underbrace{P_\theta(\theta \in \delta(X))}_{\text{coverage probability}},$$

where we require $\{x : \theta \in \delta(x)\} \in \mathcal{F}$ for all $\theta$.

</div>

<div class="math-box box-theorem">
<span class="math-box-title">Lemma 2.9 — Loss minimizers: mean and quantile</span>

Let $X \sim F$.

<strong>(i)</strong> For $L(a) = \mathbb{E}_F[(X-a)^2]$, either $L \equiv \infty$, or $\arg\min_a L(a) = \mathbb{E}_F[X]$.

<strong>(ii)</strong> For $L(a) = \mathbb{E}_F[\rho_\tau(X-a)]$, either $L \equiv \infty$, or $\arg\min_a L(a) = q_F(\tau)$, the $\tau$-quantile, i.e. any real number with $F(q_F(\tau)-) \leq \tau \leq F(q_F(\tau))$.

</div>

<strong>Proof sketch.</strong> For (i), if $\mathbb{E}[X^2] = \infty$ then $L \equiv \infty$; otherwise, writing $\mu = \mathbb{E}[X]$,

$$L(a) = \mathbb{E}[(X-\mu)^2] + (\mu - a)^2 \geq L(\mu),$$

with equality iff $a = \mu$. For (ii), if $\mathbb{E}\lvert X\rvert = \infty$ then $L \equiv \infty$; otherwise the convexity inequality

$$\rho_\tau(x-b) \geq \rho_\tau(x-a) + \mathbb{I}(x \geq a)\,\tau(a-b) + \mathbb{I}(x < a)(\tau-1)(a-b)$$

(and its variant) yields, after taking expectations and choosing $a = q_\tau$, that $\mathbb{E}[\rho_\tau(X-b)] \geq \mathbb{E}[\rho_\tau(X-q_\tau)]$ for every $b$. $\blacksquare$

---

## 2.1 Comparison of Rules

We say $\delta$ is **better than** $\delta'$ if $R(\theta, \delta) \leq R(\theta, \delta')$ for all $\theta$, with strict inequality for at least one $\theta$.

<div class="math-box box-definition">
<span class="math-box-title">Definition 2.10 — Admissibility</span>

A rule $\delta$ is <strong>admissible</strong> if no rule is better than $\delta$; it is <strong>inadmissible</strong> if some rule is better than $\delta$.

</div>

Because risk functions cannot in general be minimized uniformly in $\theta$, we select optimal rules either by <strong>restricting</strong> the class of rules (symmetry, unbiasedness, a significance level, etc.) or by applying a <strong>global criterion</strong> such as the Bayes or minimax principle.

<div class="math-box box-definition">
<span class="math-box-title">Definition 2.11 — Bayes risk and Bayes rule</span>

Given a prior $\pi$ on $\Theta$, the <strong>Bayes risk</strong> of $\delta$ is

$$r(\pi, \delta) := \mathbb{E}_{\Theta \sim \pi}\!\left[R(\Theta, \delta)\right],$$

and the <strong>Bayes rule</strong> is $\delta_\pi := \arg\min_\delta r(\pi, \delta)$.

</div>

<div class="math-box box-definition">
<span class="math-box-title">Definition 2.12 — Minimax rule</span>

The <strong>minimax rule</strong> minimizes the worst-case risk,

$$\delta^{\text{minimax}} := \arg\min_\delta\, \sup_\theta R(\theta, \delta).$$

</div>

<div class="math-box box-theorem">
<span class="math-box-title">Theorem 2.13 — Complete class theorem</span>

Suppose $\Theta$ is a separable metric space, $\mathcal{A}$ is a compact metric space, and $L(\theta, a)$ is jointly continuous in $(\theta, a)$. Then for every $\delta \in \mathcal{P}(\mathcal{A})$ there exist priors $\{\pi_k\} \subseteq \mathcal{P}(\Theta)$ and a rule $\delta_0 \in \mathcal{P}(\mathcal{A})$ such that

$$\delta_{\pi_k} \rightsquigarrow \delta_0 \qquad \text{and} \qquad R(\theta, \delta_0) \leq R(\theta, \delta) \ \ \forall \theta \in \Theta.$$

In words, Bayes rules and their weak limits form a complete class: any rule is matched or dominated by a limit of Bayes rules.

</div>

<strong>Proof idea.</strong> Compactness of $\mathcal{A}$ makes $\mathcal{P}(\mathcal{A})$ sequentially compact under weak convergence (Prokhorov), and joint continuity of $L$ makes the risk $R$ jointly continuous. On a finite parameter set the risk set is convex, so the separating hyperplane theorem produces a prior against which the minimax rule is Bayes. Exhausting a countable dense subset of $\Theta$ and passing to a weakly convergent subsequence yields the dominating limit $\delta_0$. $\blacksquare$
