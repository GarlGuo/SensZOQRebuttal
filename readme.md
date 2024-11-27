This anonymized repository contains additional experiment figures for **Zeroth-Order Fine-Tuning of LLMs with Transferable Static Sparsity**.

### Figures

- #### Convergence curves

We include the training loss of SensZOQ vs. ZO Full FT (MeZO) in [OPT-13B training loss, same learning rate](https://anonymous.4open.science/r/SensZOQRebuttal-9EE4/Figures/opt-13b-loss-same-learning-rate.png), and [OPT-13B training loss, best learning rate of both parties](https://anonymous.4open.science/r/SensZOQRebuttal-9EE4/Figures/opt-13b-loss-best-learning-rate-of-both-side.png).

Note that the `Sensitive ZO (C4 mask, 4 bit)` is our `SensZOQ` method, and the `Sensitive ZO (C4 mask, 16 bit)` will leave the dense weights unquantized (still in FP16) as it more aligns with our theory.

1. [OPT-13B training loss, same learning rate](https://anonymous.4open.science/r/SensZOQRebuttal-9EE4/Figures/opt-13b-loss-same-learning-rate.png): best learning rate from ZO Full FT (MeZO), also used for SensZOQ and the unquantized version

Summary of the figure: For RTE and Wiki2, SensZOQ has much faster convergence while for WiC and COPA, the convergence is about the same.

For these 4 figures, we first search the best learning rate for ZO Full FT (MeZO) that reaches the lowest training loss in [1e-7, 2e-7, 5e-7, 1e-6] grid. We then change to Sensitive ZO with C4 gradient masks (4 bit as our SensZOQ method, and 16 bit as an ablation study) with **an identical learning rate** (as well as other hyperparameters as batch size $B$, ZO perturbation constant $\epsilon$). 

2. [OPT-13B training loss, best learning rate of both parties](https://anonymous.4open.science/r/SensZOQRebuttal-9EE4/Figures/opt-13b-loss-best-learning-rate-of-both-side.png): best learning rate for both ZO Full FT and SensZOQ

Summary of the figure: For all of these 4 tasks (RTE, WiC, COPA, Wiki2), SensZOQ has much faster convergence.

We conduct a learning rate grid search for both ZO Full FT and SensZOQ separately (the other hyperparams $B$ and $\epsilon$ are still the same), and make these 4 figures. 

- #### Sparsity of sensitive parameters in reasoning and math tasks

  We include
- #### C4 gradient mask's transferability in reasoning and math tasks
