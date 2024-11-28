This anonymized repository contains additional experiment figures for **Zeroth-Order Fine-Tuning of LLMs with Transferable Static Sparsity**.

### Figures

- #### OPT-13B: Convergence curves

We include the training loss of SensZOQ vs. ZO Full FT (MeZO) in [OPT-13B training loss, same learning rate](https://anonymous.4open.science/r/SensZOQRebuttal-9EE4/Figures/opt-13b-loss-same-learning-rate.png), and [OPT-13B training loss, best learning rate of both parties](https://anonymous.4open.science/r/SensZOQRebuttal-9EE4/Figures/opt-13b-loss-best-learning-rate-of-both-side.png).

Note that the `Sensitive ZO (C4 mask, 4 bit)` is our `SensZOQ` method, and the `Sensitive ZO (C4 mask, 16 bit)` will leave the dense weights unquantized (still in FP16) as it more aligns with our theory.

- [OPT-13B training loss, same learning rate](https://anonymous.4open.science/r/SensZOQRebuttal-9EE4/Figures/opt-13b-loss-same-learning-rate.png): best learning rate from ZO Full FT (MeZO), also used for SensZOQ and the unquantized version. We first search the best learning rate for ZO Full FT that reaches the lowest training loss in [1e-7, 2e-7, 5e-7, 1e-6] grid. We then change to Sensitive ZO with C4 gradient masks (4 bit as our SensZOQ method, and 16 bit as an ablation study) with **an identical learning rate** (as well as other hyperparameters as batch size $B$, ZO perturbation constant $\epsilon$). 

  - For RTE and Wiki2, SensZOQ has much faster convergence while for WiC and COPA, the convergence is about the same.


- [OPT-13B training loss, best learning rate of both parties](https://anonymous.4open.science/r/SensZOQRebuttal-9EE4/Figures/opt-13b-loss-best-learning-rate-of-both-side.png): best learning rate for both ZO Full FT and SensZOQ. We search for the best learning rate for ZO Full FT and SensZOQ separately (the other hyperparams $B$ and $\epsilon$ are kept the same), and make these 4 figures. 

  - For all of these 4 tasks (`RTE`, `WiC`, `COPA`, `Wiki2`), SensZOQ has much faster convergence.



- #### Mistral 7B: sparsity of sensitive parameters in commonsense reasoning and math tasks

  [Sparsity of Mistral 7B's sensitive parameters in 3 commonsense reasoning and 1 math task](https://anonymous.4open.science/r/SensZOQRebuttal-9EE4/Figures/mistral-7b-sensitive-parameters-sparsity.png): we compute the cumulative sum of gradient square entries during FO-Adam finetuning for 1000 steps. We can see that sensitive parameters are still fairly sparse for these 4 tasks.


  - For 3 commonsense reasoning tasks, the argument that 0.1% of parameters contribute about 50% gradient norm still holds.
  - For the math task, the sensitive parameters tend to be slightly denser, but 1% of parameters can still cover 50% gradient norm (0.1% will cover ~30% gradient norm).


- #### Mistral 7B: C4 gradient mask's transferability in reasoning and math tasks

  [Transferability of C4 gradient mask in 3 commonsense reasoning and 1 math task for Mistral 7B](https://anonymous.4open.science/r/SensZOQRebuttal-9EE4/Figures/mistral-7b-C4-transferability.png): we compute the covered task gradient squares $[\mathcal{F}(w)]_i^2$ by C4 gradient mask in 3 commonsense reasoning and 1 math task. 

  - There are still no solid lines (for all top-($\textcolor{#fe0000}{\textrm{1e-2}}$,$\textcolor{#d1a738}{\textrm{1e-3}}$,$\textcolor{#00ff00}{\textrm{1e-4}}$)) parameters with C4 mask) vanishs to 0, we can see C4 gradient mask still demonstrates useful transferability to these 3 commonsense and 1 math task. For top-$\textcolor{#d1a738}{\textrm{1e-3}}$ C4 gradient mask, the lowest covered gradient norm is ~0.2, while the maximum possible (*task grad, dyn.*) is ~0.6 across tasks. 

  - We still note that for AQuA_Rat (math algebraic word problem task), C4's transferability is weaker than the other 3 commonsense reasoning tasks. We speculate that this is due to the need to learn more math-related knowledge during FT as the covered task gradient squares by task gradient mask before FT mask (*task grad, static*) also declines more during FT. 


- #### Mistral 7B: C4 gradient masks versus other gradients from pretraining text corpuses

  [Overlap ratio in top entries of gradient squares](https://anonymous.4open.science/r/SensZOQRebuttal-9EE4/Figures/mistral-7b-7tasks-top-overlap.png): we compute the overlap ratio of top entries in gradient squares for gradients from causal LM loss in (the number inside the parenthesis refers to the column/row rank for each subfigure)
  
  (1) C4, our choice.

  (2) [OpenWebMath](https://huggingface.co/datasets/open-web-math/open-web-math), a pile of Internet mathematical proofs. We abbreviate it as `Math`.

  (3) [ArXiv](https://huggingface.co/datasets/armanc/scientific_papers), a pile of ArXiv papers. We use the ArXiv articles subset from this dataset.
  
  (4) [Wiki103](https://huggingface.co/datasets/Salesforce/wikitext), a pile of selected Wikipedia articles. We abbreviate it as `Wiki`.
  
  We also include the task gradient as a reference:

  (5) task gradient before FT (start). We abbreviate it as $\nabla \mathcal{F} (w_\mathrm{st})$.

  (6) task gradient after 10% finetuning steps (mid). We abbreviate it as $\nabla \mathcal{F} (w_\mathrm{mid})$.

  (7) task gradient at the end of FT (end). We abbreviate it as $\nabla \mathcal{F} (w_\mathrm{end})$.

  We consider 7 tasks (3 commonsense reasoning tasks `Arc-C`, `HellaSwag`, `PIQA`, 1 math task `AQuA_Rat`, 3 SuperGLUE tasks `RTE`, `WiC`, `COPA`).

  **For a quick overview, we include an [average overlap ratio across these 7 tasks](https://anonymous.4open.science/r/SensZOQRebuttal-9EE4/Figures/mistral-7b-7tasks-top-overlap-avg.png)** (first row). The second row gives an empirical evidence for the "fixed gradient feature" during FT - the top entries in $\nabla \mathcal{F} (w_\mathrm{st})$ (task grad before FT) resemble $\nabla \mathcal{F} (w_\mathrm{mid})$ (task grad during FT) and $\nabla \mathcal{F} (w_\mathrm{end})$ (task grad during FT)

  - **Empirical findings**
  - - The top gradient entries from all 4 pretraining text corpuses still overlap considerably with the task gradient, at least for top-0.1% entries. 

  - - **C4 generally covers more top gradient entries than OpenWebMath, ArXiv, and Wiki103.**

