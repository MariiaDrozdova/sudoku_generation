# Unconditional Sudoku Generation

<table align="center">
  <tr>
    <td align="center">
      <img src="images/entropy_ODE_flow.gif" width="120" /><br/>
      <sub><b>Flow Matching (ODE)</b></sub>
    </td>
    <td align="center">
      <img src="images/entropy_SDE_flow.gif" width="120" /><br/>
      <sub><b>Flow Matching (SDE)</b></sub>
    </td>
    <td align="center">
      <img src="images/entropy_SDE_flow_score.gif" width="120" /><br/>
      <sub><b>Flow + Score Matching (SDE)</b></sub>
    </td>
    <td align="center">
      <img src="images/entropy_SDE_score.gif" width="120" /><br/>
      <sub><b>Score Matching (SDE)</b></sub>
    </td>
  </tr>
</table>

This repository contains a small personal research project inspired by the MIT course  
**Generative AI with Stochastic Differential Equations (6.S184)**  
by Peter Holderrieth and Ezra Erives.

Course website: https://diffusion.csail.mit.edu/

A full technical report (PDF) with theoretical background, methodology, and detailed analysis is included directly in this repository.

---

## Key Results (Highlights)

The goal of this project is **unconditional generation of valid Sudoku grids** using flow matching and score-based generative models.

Because valid Sudokus form an extremely sparse subset of the continuous relaxation space, we adopt a **weak success criterion**:

> A method is considered successful if it produces **at least one valid Sudoku grid among 500 unconditional samples**.

---

## Sampling Regimes

We evaluate four closely related generative sampling configurations:

1. **Flow Matching (ODE)**  
   Deterministic sampling using a learned velocity field.

2. **Flow Matching (SDE)**  
   Stochastic sampling where the score is derived analytically from the learned flow model.  

3. **Flow + Score Matching (SDE)**  
   Stochastic sampling using both a learned flow and a learned score model.  
   *Underperforms in the current implementation.*

4. **Score Matching (SDE)**  
   Stochastic sampling where the flow is derived analytically from the learned score model.


---

### Results

- **Score + SDE (flow derived analytically)** performs best:
  - ≥0.1 valid grid in total, in ~100% each batches of 500 samples has at least 50 valid sudoku grids.
- Different equations have different optinal sigma.

For all regimes batch-level success indicates that the models assign **non-zero probability mass** to valid Sudoku configurations.

A summary table comparing all four sampling regimes is provided in the report.

---

## Notes

- All valid Sudoku solutions were verified **not to appear in the training dataset** and **not to repeat across runs**.
- Some connections between the SDE framework and annealed Langevin dynamics remain heuristic in this implementation and are discussed openly in the report.
- This project is exploratory and intended as a stress test of continuous generative models on a highly structured discrete task.

---

## Acknowledgements

This work is heavily inspired by the MIT **6.S184** course materials and lectures.  
Any mistakes or misinterpretations are entirely my own.

---

## Contact

If you find this project interesting or would like to discuss related ideas, please send a mail to mariia.drozdova at unige.ch
