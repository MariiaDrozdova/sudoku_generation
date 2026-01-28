# Continuous-Time Generative Models for Sudoku: Unconditional Generation and Guided Solving

<table align="center">
  <tr>
    <td align="center">
      <img src="images/linear_proba_path/gifs/entropy_ODE_flow.gif" width="120" /><br/>
      <sub><b>Flow Matching (ODE)</b></sub>
    </td>
    <td align="center">
      <img src="images/linear_proba_path/gifs/entropy_SDE_flow.gif" width="120" /><br/>
      <sub><b>Flow Matching (SDE)</b></sub>
    </td>
    <td align="center">
      <img src="images/linear_proba_path/gifs/entropy_SDE_flow_score.gif" width="120" /><br/>
      <sub><b>Flow + Score Matching (SDE)</b></sub>
    </td>
    <td align="center">
      <img src="images/linear_proba_path/gifs/entropy_SDE_score.gif" width="120" /><br/>
      <sub><b>Score Matching (SDE)</b></sub>
    </td>
  </tr>
</table>

This repository contains a personal research project inspired by the MIT course **Generative AI with Stochastic Differential Equations (6.S184)** by Peter Holderrieth and Ezra Erives.

Course website: https://diffusion.csail.mit.edu/

A full technical report (PDF) with theoretical background, methodology, and detailed analysis
is included directly in this repository.

---

## Overview

This project studies whether **standard continuous-time generative models**
can represent and explore distributions whose support is an extremely sparse,
globally constrained discrete set.

We use **Sudoku** as a controlled testbed. Valid completed Sudoku grids form a
vanishingly small subset of a continuous relaxation space, and “almost correct”
solutions are meaningless because validity requires simultaneously satisfying
row, column, and subgrid constraints.

Rather than framing Sudoku as a search or symbolic reasoning problem, we treat
it as a **sampling problem** from a learned continuous distribution. The goal is
not efficiency or optimal solving performance, but to understand whether
continuous-time generative models can assign **non-zero probability mass** to
valid Sudoku configurations, and how different probability paths and sampling
procedures behave under strong global constraints.

---

## Methods

We study a unified Gaussian probability path of the form

$$
x_t = \alpha(t) x_0 + \beta(t)\varepsilon, \qquad \varepsilon \sim \mathcal{N}(0,I),
$$

and explore multiple learning and sampling strategies derived from this
formulation:

- **Flow matching (deterministic ODE sampling)**
- **Score matching (stochastic SDE sampling)**
- **Hybrid flow + score SDEs**
- **Discrete diffusion samplers (DDPM / DDIM)**
- **Empirical SDE variants with path-dependent diffusion**

Both **linear** and **cosine** schedules for \((\alpha,\beta)\) are considered.
This allows direct comparison between continuous-time ODE/SDE samplers and
discrete diffusion models derived from the same continuous-time training
objective.

An important empirical observation is that using the path-dependent coefficient
\(\beta(t)\) as the diffusion strength in SDE sampling often improves numerical
stability and success rates in constrained settings, despite deviating from
probability-path–consistent diffusion dynamics. Such samplers are therefore
interpreted as **guided stochastic search procedures**, rather than exact
samplers of the target probability path.

---

## Constraint-Aware Sudoku Solving

In addition to unconditional generation, the repository includes a **guided SDE
solver** for Sudoku puzzles with given digits.

Key components include:
- Two-stage constraint enforcement (soft path-consistent → hard clamping)
- Masked stochastic updates to prevent drift of fixed cells
- Early stopping and batched stochastic sampling

This approach allows solving difficult Sudoku instances through repeated probabilistic sampling,
demonstrating the flexibility of continuous generative models in structured domains.
Importantly, this solver operates entirely through probabilistic sampling and early stopping,
without any explicit symbolic reasoning or search, relying solely on the learned geometry of
the solution manifold.

---

## Notebooks

The project is organized into fully standalone notebooks:

- **01_linear_path_sudoku_generation.ipynb**  
  Linear Gaussian paths, flow matching, score matching, and entropy analysis.

- **02_cosine_path_ddpm_sde_sudoku_generation.ipynb**  
  Cosine paths, DDPM/DDIM discretizations, and comparison to continuous SDE samplers.

- **03_linear_path_sudoku_solver_trainer.ipynb**  
  Training of the score model used for Sudoku solving + inference.

- **04_linear_path_sudoku_solver.ipynb**  
  Inference for solving given Sudoku puzzles vie linear probability paths (no need to download the dataset, you can you the weights provided in the repo).

- **05_ddpm_sudoku_solver.ipynb**  
  Inference for solving given Sudoku puzzles vie DDPM sampling (no need to download the dataset, you can you the weights provided in the repo).
  

Each notebook can be run independently without external dependencies beyond standard
PyTorch and scientific Python libraries.

---

## Key Findings (High-Level)

- Deterministic ODE sampling consistently fails on Sudoku; stochasticity is essential.
- Score-based SDE samplers substantially outperform flow-only models.
- Discrete diffusion samplers (DDPM/DDIM) achieve the highest success rates for
  **unconditional Sudoku generation**, exceeding all continuous-time variants.
- Cosine probability paths are most effective for unconditional generation and
  diffusion-style sampling.
- Under hard constraints (Sudoku solving with given digits), the relative
  performance landscape changes: linear probability paths combined with
  score-based SDE sampling and annealed noise remain competitive, despite
  deviating from probability-path–consistent dynamics.
- All successful regimes assign **non-zero probability mass** to valid Sudoku
  grids.

These results highlight a distinction between samplers optimized for faithful
generation of the data distribution and those that are effective for stochastic
exploration under hard constraints.

---

## Scope and Limitations

This project is exploratory and research-oriented.
It is **not** intended to compete with classical Sudoku solvers or symbolic
constraint satisfaction methods.

The emphasis is on understanding:
- how continuous generative models behave under strong combinatorial constraints,
- how probability paths and sampling regimes affect convergence and stability,
- and where diffusion-style methods succeed or fail on discrete structured tasks.

---

## Acknowledgements

This work is heavily inspired by the MIT **6.S184** course materials and lectures.

---

## Contact

If you find this project interesting or would like to discuss related ideas, please send a mail to mariia.drozdova(at)unige.ch
