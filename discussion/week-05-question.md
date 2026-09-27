---
id: w05-675646432-nonlinear-irrelevant-variables
title: "Handling Irrelevant Variables in Nonlinear Prediction"
author: "Runting Chen (675646432)"
---

**Question:**

In the context of KNN, when dealing with a large number of irrelevant variables, Euclidean distance can be heavily distorted. While we have previously learned to use Lasso and Ridge regression to eliminate irrelevant variables, these methods work well primarily for linear relationships between $Y$ and the predictors. How can we address this issue if the underlying predictive relationship is nonlinear?

**Answer:**

The main idea is to perform **supervised variable selection or dimension reduction using a nonlinear model**, rather than relying only on a linear penalty. A useful strategy is to fit a nonlinear learner that can identify which variables contribute to prediction. For example, random forests and gradient-boosted trees can estimate variable importance through permutation importance or out-of-sample prediction loss. We can then remove variables with little predictive contribution and fit KNN using the retained variables. Because the variables are selected according to their relationship with $Y$, this can preserve nonlinear effects that a linear lasso model might miss.

Another option is to use a nonlinear additive model, such as a generalized additive model (GAM), with a smooth function for each predictor:

$$
g(x) = \beta_0 + \sum_{j=1}^{p} f_j(x_j).
$$

The smooth functions can be regularized so that unimportant functions are shrunk toward zero. This is a nonlinear analogue of sparse regression: a variable can be useful through a curved relationship even when its ordinary linear coefficient would be close to zero. Kernel methods provide another approach. A kernel can represent nonlinear relationships, while variable-selection or metric-learning methods can reduce the influence of predictors that do not help predict the response.

For KNN specifically, we can replace the ordinary Euclidean distance with a weighted distance,

$$
d_w(x_i,x_l) = \left\{\sum_{j=1}^{p} w_j(x_{ij}-x_{lj})^2\right\}^{1/2},
$$

where the weights are estimated from the training data. Irrelevant variables should receive weights close to zero, while informative variables receive larger weights. More flexible metric-learning methods can also learn a transformation of the predictors before calculating neighborhoods. The weights or transformation must be learned using only the training portion of each cross-validation fold.

A practical workflow would be to compare several nonlinear approaches, such as a tree ensemble, a GAM, and KNN with feature weighting. For each approach, variable selection, tuning, and preprocessing should occur inside the training folds. The final method should be chosen using validation error, and its performance should be reported on an untouched test set. This is important because selecting variables using the full data before cross-validation would make the validation error optimistically biased.

There is no guarantee that one method will always identify the correct variables. Tree-based importance can be biased when predictors are correlated, and marginal screening can miss variables that are useful only through interactions. Therefore, the best choice depends on the form of the nonlinear relationship, the amount of data, predictor correlation, and computational cost. The central lesson is that irrelevant-variable removal should be matched to the model class: nonlinear predictive relationships require nonlinear, prediction-based feature selection or a learned distance rather than only linear lasso or ridge penalties.
