---
title: "Theory of Statistics I — Chapter 1.3: Prediction"
date: 2026-03-21
categories:
- Mathematics
- Theory of Statistics
tags:
- statistics
- prediction
- conditional-expectation
- best-linear-predictor
- multiple-correlation
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
| $\text{MSPE}(g) = \mathbb{E}[|Y - g(X)|^2]$ | Mean squared prediction error |
| $\mu_L(X)$ | Best linear predictor of $Y$ given $X$ |
| $\rho_{XY}$ | Multiple correlation coefficient |
| $\Sigma_{XX}, \Sigma_{XY}, \Sigma_{YX}, \Sigma_{YY}$ | Block covariance matrices |
| $\Sigma_{Y \cdot X}$ | Conditional (Schur) covariance $\Sigma_{YY} - \Sigma_{YX}\Sigma_{XX}^{-1}\Sigma_{XY}$ |
| $L^2(\Omega, \mathcal{F}, P)$ | Space of square-integrable functions |

---

## 3. Optimal Prediction under Squared Error

Given a pair $(Y, X) \in \mathbb{R} \times \mathbb{R}^k$, we want to predict $Y$ from $X$ as well as possible in mean-square. The natural objective is the mean squared prediction error.

<div class="math-box box-definition">
<span class="math-box-title">Definition 3.1 — Mean squared prediction error</span>

For $(Y, X) \in \mathbb{R} \times \mathbb{R}^k$, the <strong>mean squared prediction error</strong> of a predictor $g$ is

$$\text{MSPE}(g) := \mathbb{E}\!\left[|Y - g(X)|^2\right].$$

We seek $g^* := \arg\min_{g \in \mathcal{G}} \text{MSPE}(g)$ over a class $\mathcal{G}$.

</div>

<div class="math-box box-theorem">
<span class="math-box-title">Theorem 3.2 — Conditional expectation is the optimal predictor</span>

Let $Y \in L^2(\Omega, \mathcal{F}, P)$.

<strong>(i)</strong> $\mathbb{E}[Y] = \arg\min_{c \in \mathbb{R}} \mathbb{E}(Y - c)^2$.

<strong>(ii)</strong> For every $g \in L^2(\mathbb{R}, \mathcal{B}(\mathbb{R}), P_X)$, the residual $Y - \mathbb{E}[Y \mid X]$ is uncorrelated with $g(X)$, i.e. $\operatorname{Cov}(Y - \mathbb{E}[Y\mid X],\, g(X)) = 0$, and consequently

$$\mathbb{E}[Y \mid X] = \arg\min_{g \in L^2} \mathbb{E}(Y - g(X))^2.$$

</div>

<strong>Proof.</strong> Write $g^*(x) = \mathbb{E}[Y \mid X = x]$. The orthogonality $\mathbb{E}[(Y - g^*(X))g(X)] = \mathbb{E}[g(X)\cdot 0] = 0$ follows from the tower property. Then

$$\text{MSPE}(g) = \mathbb{E}[(Y - g^*(X))^2] + \mathbb{E}[(g^*(X) - g(X))^2] \geq \text{MSPE}(g^*),$$

with equality iff $g(X) = g^*(X)$ a.s. $\blacksquare$

<div class="math-box box-example">
<span class="math-box-title">Remark 3.3 — Vector responses</span>

For $Y \in \mathbb{R}^d$, the same orthogonality $\operatorname{Cov}(Y - g^*(X),\, g(X)) = \mathbb{E}[(Y - g^*(X))g(X)^\top] = 0$ holds, so taking traces shows $g^*(X) = \mathbb{E}[Y \mid X]$ minimizes $\mathbb{E}[\|Y - g(X)\|^2]$.

</div>

<div class="math-box box-example">
<span class="math-box-title">Example 3.4 — Multivariate normal</span>

Suppose $\Sigma_{XX} \succ 0$ and

$$\begin{bmatrix} X \\ Y \end{bmatrix} \sim \mathcal{N}\!\left(\begin{bmatrix} \mu_X \\ \mu_Y \end{bmatrix}, \begin{bmatrix} \Sigma_{XX} & \Sigma_{XY} \\ \Sigma_{YX} & \Sigma_{YY} \end{bmatrix}\right).$$

With $A := \Sigma_{YX}\Sigma_{XX}^{-1}$, the residual $Y - AX$ is uncorrelated with $X$, hence (by joint normality) independent of $X$. Therefore

$$Y \mid X \sim \mathcal{N}\!\left(\mu_Y + A(X - \mu_X),\ \Sigma_{Y\cdot X}\right), \qquad \Sigma_{Y\cdot X} = \Sigma_{YY} - \Sigma_{YX}\Sigma_{XX}^{-1}\Sigma_{XY}.$$

<strong>(1) Best predictor.</strong> $\mathbb{E}[Y \mid X] = \mu_Y + A(X - \mu_X)$.

<strong>(2) MSPE.</strong> With $g^*(X) = \mathbb{E}[Y\mid X]$,

$$\mathbb{E}[\|Y - g^*(X)\|^2] = \operatorname{tr}\!\left(\Sigma_{YY} - \Sigma_{YX}\Sigma_{XX}^{-1}\Sigma_{XY}\right).$$

<strong>(3) Multiple correlation.</strong> For scalar $Y$ with $\operatorname{Var}(Y) > 0$,

$$R^2 := \frac{\operatorname{Var}(\mathbb{E}[Y\mid X])}{\operatorname{Var}(Y)} = \frac{\Sigma_{YX}\Sigma_{XX}^{-1}\Sigma_{XY}}{\sigma_Y^2},$$

which reduces to $R^2 = \rho^2$ when $X$ is scalar.

</div>

---

## 3.1 Best Linear Predictor

When we restrict the predictor class to affine functions of $X$, the optimum is determined entirely by first and second moments.

<div class="math-box box-theorem">
<span class="math-box-title">Theorem 3.5 — Best linear predictor</span>

<strong>(i)</strong> If $\mathbb{E}[Y^2] < \infty$ and $0 < \mathbb{E}[X^2] < \infty$, then

$$\beta^* := \arg\min_{b \in \mathbb{R}} \mathbb{E}(Y - bX)^2 = \frac{\mathbb{E}[XY]}{\mathbb{E}[X^2]}.$$

<strong>(ii)</strong> For $Y \in \mathbb{R}^d$, $X \in \mathbb{R}^k$ with $\Sigma_{XX} \succ 0$ and finite second moments,

$$(\beta_0^*, \beta_1^*) = \arg\min_{(\beta_0, \beta_1)} \mathbb{E}\!\left[\|Y - \beta_0 - \beta_1 X\|^2\right] = \left(\mu_Y - \beta_1^* \mu_X,\ \Sigma_{YX}\Sigma_{XX}^{-1}\right).$$

</div>

<strong>Proof.</strong> It suffices to check the normal equations, i.e. that the residual is uncorrelated with every admissible predictor. For (i), $\mathbb{E}[(Y - \beta^* X)X] = 0$ holds iff $\beta^* = \mathbb{E}[XY]/\mathbb{E}[X^2]$. For (ii),

$$\operatorname{Cov}(Y - \beta_0^* - \beta_1^* X,\ \beta_0 + \beta_1 X) = (\Sigma_{YX} - \beta_1^* \Sigma_{XX})\beta_1 = 0 \quad \forall \beta_1,$$

which forces $\beta_1^* = \Sigma_{YX}\Sigma_{XX}^{-1}$, and the intercept follows from matching means. $\blacksquare$

<div class="math-box box-definition">
<span class="math-box-title">Definition 3.6 — Best linear predictor</span>

For $0 < \operatorname{Var}[Y] < \infty$ and $\Sigma_{XX} \succ 0$, the <strong>best linear predictor</strong> is

$$\mu_L(X) := \mu_Y + \langle \beta^*, X - \mu_X \rangle.$$

</div>

<div class="math-box box-definition">
<span class="math-box-title">Definition 3.7 — Multiple correlation coefficient</span>

For $0 < \operatorname{Var}[Y] < \infty$, $\Sigma_{XX} \succ 0$, and $\Sigma_{XY} \neq 0$, the <strong>multiple correlation coefficient</strong> is

$$\rho_{XY} := \operatorname{Cor}\!\left(Y,\, \mu_L(X)\right) = \sqrt{\frac{\Sigma_{YX}\Sigma_{XX}^{-1}\Sigma_{XY}}{\sigma_Y^2}}.$$

It measures the strongest linear association between $Y$ and any linear combination of the components of $X$.

</div>
