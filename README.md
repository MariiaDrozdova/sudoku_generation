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

This repository contains a small personal research project inspired by the MIT course **Generative AI with Stochastic Differential Equations (6.S184)** by Peter Holderrieth and Ezra Erives.

Course website: https://diffusion.csail.mit.edu/

A full technical report (PDF) with theoretical background, methodology, and detailed analysis
is included directly in this repository.

---

## Overview

This project explores the use of **continuous generative models**-including flow matching,
score matching, and diffusion-inspired samplers. Our goal is to generate and solve **Sudoku puzzles**.

Sudoku serves as a challenging testbed due to its highly structured, discrete nature:
valid solutions form an extremely sparse subset of the continuous relaxation space.
Rather than framing Sudoku as a search or constraint satisfaction problem, we treat it as
a **sampling problem** from a learned continuous distribution.

The goal of this work is not efficiency or optimal solving, but to investigate whether
continuous-time generative models can assign **non-zero probability mass** to valid Sudoku
configurations, and how different probability paths and sampling regimes behave under
strong global constraints.

---

## Methods

We study a unified Gaussian probability path framework of the form

$$x_t = \alpha(t) x_0 + \beta(t)\varepsilon, \qquad \varepsilon \sim \mathcal{N}(0,I),$$

and explore multiple learning and sampling strategies derived from this formulation:

- **Flow matching (ODE sampling)**
- **Score matching (SDE sampling)**
- **Hybrid flow + score SDEs**
- **Discrete diffusion samplers (DDPM / DDIM)**
- **Custom SDE samplers using $\beta(t)$ as diffusion coefficient**

Both **linear** and **cosine** schedules for $(\alpha,\beta)$ are considered, allowing direct
comparison between continuous-time SDE samplers and discrete diffusion models.

A key empirical finding of this project is that using the path-dependent coefficient
$\beta(t)$ directly as the diffusion strength in SDE sampling, rather than a constant
$\sigma$, leads to substantially improved stability and success rates, despite not
corresponding to a classical diffusion forward process for linear paths.

---

## Constraint-Aware Sudoku Solving

In addition to unconditional generation, the repository includes a **guided reverse-time SDE
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

The project is organized into four fully standalone notebooks:

- **01_linear_path_sudoku_generation.ipynb**  
  Linear Gaussian paths, flow matching, score matching, and entropy analysis.

- **02_cosine_path_ddpm_sde_sudoku_generation.ipynb**  
  Cosine paths, DDPM/DDIM discretizations, and comparison to continuous SDE samplers.

- **03_linear_path_sudoku_solver_trainer.ipynb**  
  Training of score and flow models used for Sudoku solving.

- **04_linear_path_sudoku_solver.ipynb**  
  Guided reverse SDE sampling for solving given Sudoku puzzles.

Each notebook can be run independently without external dependencies beyond standard
PyTorch and scientific Python libraries.

---

## Key Findings (High-Level)

- Stochastic SDE samplers consistently outperform deterministic ODE sampling.
- Score-based SDEs achieve the highest unconditional success rates.
- Using $\beta(t)$ as diffusion strength improves stability and robustness across settings.
- Cosine schedules outperform linear schedules for *unconditional generation* and diffusion-style sampling.
- Discrete DDPM/DDIM samplers underperform continuous SDEs in this highly constrained setting.

Importantly, for the **Sudoku solving task with given constraints**, the most effective configuration
in our experiments is a **score-based SDE with a linear Gaussian path and $\beta(t)$ used as the diffusion coefficient**,
which consistently balances stochastic exploration with constraint preservation.

All successful regimes demonstrate **non-zero probability mass** on valid Sudoku grids.

---

## Scope and Limitations

This project is exploratory and research-oriented.
It is **not** intended to compete with classical Sudoku solvers or symbolic methods.

The emphasis is on understanding:
- how continuous generative models behave under strong combinatorial constraints,
- how different sampling regimes affect convergence,
- and where diffusion-style methods succeed or fail on discrete tasks.

---

## Acknowledgements

This work is heavily inspired by the MIT **6.S184** course materials and lectures.

---

## Contact

If you find this project interesting or would like to discuss related ideas, please send a mail to mariia.drozdova(at)unige.ch
