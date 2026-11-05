# Constrained Augmented Lagrangian Training of a Physics-Informed Transformer for Battery RUL Prediction

Paper for MSML 604 (Applied Machine Learning, University of Maryland, College Park), co-authored with Eunsoo Suk.

**Summary:** Battery remaining useful life (RUL) prediction is framed as a constrained optimization problem. Physics-derived monotonicity and temporal-consistency constraints are enforced on a Transformer's predictions using an Augmented Lagrangian method, and the result is compared against a pure-MSE baseline on the CALCE CS2 dataset.

## Contents

- `RUL_Prediction_PINN_Paper.pdf`: the full paper

## My contribution

I defined the optimization objective (constraints, smoothness term, and the constrained program) and wrote the paper. I did not build the model or training code, so no code is included here.
