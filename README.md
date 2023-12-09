# Fund Replication

<p align="center">
  <img src="Replication Chart.png" alt="Replication of the DAX with global Smart Beta / Factor ETFs" style="width:100%">
</p>
<p align="center">
  <i>Replication of the DAX with global Smart Beta / Factor ETFs</i>
</p>

Often, mutual funds and other financial products represent a disadvantageous, i.e., for example, too expensive or capital-inefficient, repackaging of cheaper, elementary portfolio components. To check this, I have compiled this Jupyter Notebook, which works as follows:

## Optimizer

First, I defined a function `weight_optimization` that determines a parameter vector $\beta $, in this case, the weights of the portfolio components that are to be used for replication. The following parameters can be specified:

### Loss / Objective Function

Here are two common loss functions available:

- Mean Squared Error (`mse`):

$$ \min_{\boldsymbol{\beta}} \frac{1}{n} \sum_{i=1}^{n} (y_i - X_i \boldsymbol{\beta})^2 $$

- Mean Absolute Error (`mae`):

$$ \min_{\boldsymbol{\beta}} \frac{1}{n} \sum_{i=1}^{n} |y_i - X_i \boldsymbol{\beta}| $$

- Tracking Error (`te`):

$$ \min_{\boldsymbol{\beta}} \sqrt{\frac{1}{n - 1} \sum_{i=1}^{n} (y_i - X_i \boldsymbol{\beta})^2} $$

### Constraints

Here, the following self-explanatory optimization constraints can be specified:

- Long-Only Constraint (if `long_only_constraint` is `True`):

  $$\beta_j \geq 0 \quad \forall j \in\{1,2, \ldots, m\}$$

- Leverage Constraint (if `leverage_constraint` is `True`):

  $$0 \leq \sum_{j=1}^m \beta_j \leq 1$$  

### Optimization Method

The optimization is performed using the *Sequential Least Squares Programming* (`SLSQP`) method or any other multivariate method from the [`scipy`](https://docs.scipy.org/doc/scipy/reference/optimize.html#local-multivariate-optimization) library specified by the `optimizer` parameter.

## Application

Here, the respective tickers `assets` and `target` of the securities of interest, as they are listed on [*Yahoo Finance*](https://finance.yahoo.com/), can be specified, and the above-defined function can be applied. The results are *Portfolio Weights* $\beta $, the *Coefficient of Determination* of the linear fitting, the *Correlation* of the replicating portfolio to the fund, and the *Tracking Error*, which represents the sample standard deviation of the differences in returns.

If the fund cannot be well replicated, the notebook includes both a **bootstrap test** of the fund's Sharpe ratio relative to the Sharpe ratio of the replicating portfolio and a **1-factor regression** to determine the *alpha*. This can be used to examine how statistically significant the differences are.

The bootstrap test produces the following plot and a *p-value* for the right-side alternative hypothesis $H_1 : SR_{Fund} > SR_{Portfolio} $:

<p align="center">
  <img src="Null Distribution of Replication Portfolio Sharpe Ratios.png" alt="Replication of the DAX with global Smart Beta / Factor ETFs" style="width:100%">
</p>
<p align="center">
  <i>Null Distribution of Replication Portfolio Sharpe Ratios for the DAX</i>
</p>
