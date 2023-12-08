# Fund Replication

<p align="center">
  <img src="Replication Chart.png" alt="Replication of the DAX with global Smart Beta / Factor ETFs" style="width:100%">
</p>
<p align="center">
  <i>Replication of the DAX with global Smart Beta / Factor ETFs</i>
</p>

Oft stellen Publikums-Fonds und andere Finanz-Produkte eine nachteilige, d.h. bspw. zu teure oder kapitalineffiziente Neuverpackung günstigerer elemetarer Portfoliobausteine / Faktoren dar. Um das zu überprüfen, habe ich dieses Jupyter Notebook zusammengestellt.

## Optimizer / Customizable Linear Regression

Zunächst habe ich eine Funktion `linear_regression` definiert, welche einen Paramater-Vektor $\beta $, in diesem Fall die Gewichtungen der Portfoliobausteine, welche zur Replikation genutzt werden sollen, ermittellt. Dabei lassen sich folgende Paramater spezifizieren:

### Loss / Objective Function

Hier stehen zwei gängige Verlust-Funktionen zur Verfügungen zur Verfügung:

- For Mean Squared Error (`mse`):

$$ \min_{\boldsymbol{\beta}} \frac{1}{n} \sum_{i=1}^{n} (y_i - X_i \boldsymbol{\beta})^2 $$

- For Mean Absolute Error (`mae`):

$$ \min_{\boldsymbol{\beta}} \frac{1}{n} \sum_{i=1}^{n} |y_i - X_i \boldsymbol{\beta}| $$

### Constraints

Hier lassen sich jeweils selbsterklärend folgende Optimierungs-Beschränkungen spezifizieren

- Long-Only Constraint (if `long_only_constraint` is `True`):

  $$\beta_j \geq 0 \quad \forall j \in\{1,2, \ldots, m\}$$

  is the contraint

- Leverage Constraint (if `leverage_constraint` is `True`):

  $$0 \leq \sum_{j=1}^m \beta_j \leq 1$$  

### Optimization Method

The optimization is performed using the *Sequential Least Squares Programming* (`SLSQP`) method or any other multivariate method from the [`scipy`](https://docs.scipy.org/doc/scipy/reference/optimize.html#local-multivariate-optimization) library specified by the `optimizer` parameter.

## Application

Hier können nun die jeweiligen Ticker `assets` und `target` der Wertpapiere von Interesse, wie sie auch auf *Yahoo Finance* geführt werden, speifiziert und die oben definierte Funktion angewandt werden. Das Resultat sind *Portfolio Gewichtungen* $\beta $, der *Determinations-Koeffizient* der linearen Optimierung, die *Korrelation* des replizierenden Portfolios zum Fund und der *Trackking Error*, welcher die Sample-Standardabweichung der Differenz-Renditen darstellt.

Falls der Fonds nicht eindeutig repliziert werden kann, ist im Notebook sowohl ein Bootstrapping-Test der Fund Sharpe Ratio in Relation zur Sharpe Ratio des replizierendes Portfolios und eine CAPM-Regression, zur Bestimmung des Alphas, angewähnt. So kann untersucht werden, wie statistisch signifikant die Unterschiede sind.

Der Bootstrapping Test gibt folgenden Plot und einen *p-Wert* für die rechtsseitige Alternativ-Hypothese $H_1 : SR_{Fund} > SR_{Portfolio} $ aus:

<p align="center">
  <img src="Null Distribution of Replication Portfolio Sharpe Ratios.png" alt="Replication of the DAX with global Smart Beta / Factor ETFs" style="width:100%">
</p>
<p align="center">
  <i>Null Distribution of Replication Portfolio Sharpe Ratios</i>
</p>
