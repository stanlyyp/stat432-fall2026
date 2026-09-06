---
id: w02-675646432-aic-bic-feature-selection
title: "Why AIC and BIC Select Different Models"
author: "Runting Chen (675646432)"
---

### Why Do AIC and BIC Often Select Different Models During Feature Selection?

AIC and BIC both compare fitted models by combining a goodness-of-fit term with a penalty for the number of fitted parameters. For a Gaussian linear model with $k$ fitted coefficients, their common forms are

$$
\operatorname{AIC}=-2\log L(\widehat\theta)+2k,
\qquad
\operatorname{BIC}=-2\log L(\widehat\theta)+k\log n.
$$

The difference is the complexity penalty. AIC adds $2$ for every additional parameter, whereas BIC adds $\log n$. When $n>e^2\approx 7.4$, BIC penalizes an extra predictor more heavily than AIC. Therefore, a predictor can improve the likelihood enough to overcome AIC's penalty but not BIC's penalty. In the Week 2 diabetes comparison, adding `s6` improved the fit term by about $3.108$: this exceeds AIC's penalty of $2$, but not BIC's penalty of $\log(370)\approx5.914$. Thus AIC selects Model B and BIC selects Model A.

Their objectives also differ. AIC estimates expected out-of-sample predictive performance by approximately minimizing expected Kullback-Leibler discrepancy. It is prediction-oriented and may retain extra variables when they improve predictive accuracy. BIC is a large-sample approximation to a Bayesian model comparison criterion. Under its assumptions, it is model-selection consistent: if the true finite-dimensional model is among the candidates, BIC selects it with probability approaching one as $n\to\infty$. Its stronger penalty consequently favors a more parsimonious model.

Both criteria require models fit to the same response observations and compare maximized likelihoods on a common likelihood scale. Their usual interpretations additionally rely on independent observations, a correctly specified likelihood family, regular identifiable parameters, and a fixed or suitably controlled candidate-model set. The familiar RSS forms for Gaussian linear regression further assume independent, constant-variance Gaussian errors. BIC's consistency claim needs the stronger condition that one candidate is the true model; this is rarely literally guaranteed in applied work.

Related criteria answer somewhat different questions:

- **Mallows' $C_p$** estimates prediction error for linear regression using a common estimate of the noise variance, usually from a sufficiently rich reference model. With a common variance estimate, it uses a complexity penalty of $2k$ on its scale and often behaves similarly to AIC.
- **$\mathrm{AIC}_c$** is AIC with a finite-sample correction. It is preferable when $n$ is not large relative to $k$ because its extra penalty protects against the small-sample optimism of AIC; it converges to AIC as $n$ grows.
- **Cross-validation** estimates prediction error by repeatedly evaluating predictions on held-out observations. It makes fewer likelihood-model assumptions and targets predictive performance directly, but its selected model can vary with the data split and its computation is more expensive.

No criterion proves that a selected model is the true data-generating mechanism. Each choice is conditional on the candidate set, data, assumptions, and objective. AIC, BIC, $C_p$, $\mathrm{AIC}_c$, and cross-validation can reasonably choose different models because they penalize complexity differently or target different goals.