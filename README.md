# Fund Replication

<p align="center">
  <img src="Replication Chart.png" alt="Replication of the DAX with global Smart Beta / Factor ETFs" style="width:100%">
</p>
<p align="center">
  <i>Replication of the DAX with global Smart Beta / Factor ETFs</i>
</p>

Oft stellen Publikums-Fonds und andere Finanz-Produkte eine nachteilige, d.h. bspw. zu teure oder kapitalineffiziente Neuverpackung günstigerer elemetarer Portfoliobausteine / Faktoren dar. Um das zu überprüfen, habe ich dieses Jupyter Notebook zusammengestellt.

## Ablauf

### Optimizer / Customizable Linear Regression

Zunächst habe ich eine Funktion definiert, welche einen Paramater-Vektor, in diesem Fall die Gewichtungen der Portfoliobausteine, welche zur Replikation genutzt werden sollen,

## Mathematical Formulation of the Optimization Problem

The `linear_regression` function optimizes the weights of a linear model to minimize a specified loss function subject to certain constraints. The mathematical formulation of this optimization problem is as follows:

### Objective Function

- **Long-Only Constraint** (if `long_only_constraint` is `True`):
  Dies stellt sicher, dass kein Shortselling stattfindet, indem es die Koeffizienten auf nichtnegative Werte beschränkt.

  $$ \beta_j \geq 0 \quad \forall j \in \{1, 2, \ldots, m\} $$

- **Leverage Constraint** (if `leverage_constraint` is `True`):
  Dies beschränkt die Summe aller Beta-Koeffizienten auf Werte zwischen 0 und 1, um die Hebelwirkung des Portfolios zu steuern.

  $$ 0 \leq \sum_{j=1}^{m} \beta_j \leq 1 $$


### Optimization Method

- The optimization is performed using the Sequential Least Squares Programming (SLSQP) method or any other method specified by the `optimizer` parameter.
