# Project Review & Technical Evaluation

**Course:** Signal Processing (ECE-Y523) — University of Patras  
**Assignment:** Homework 1 — 2D Discrete Convolution & Verification  
**Overall Assessment:** **Outstanding (10/10)**

---

## 1. Executive Summary

This project presents a thorough, well-documented, and mathematically rigorous exploration of 2D discrete convolution. It transitions seamlessly from manual index arithmetic (Step A) to a clean reference implementation and unit-testing suite (Step B), systematic debugging of orientation errors (Step C), and an insightful experimental study on spatial smoothing filters (Step D). Both the code and the accompanying analytical explanations meet the highest academic and software engineering standards.

---

## 2. Step-by-Step Evaluation Rubric

| Component | Criteria | Status | Evaluation Notes |
| :--- | :--- | :---: | :--- |
| **Step A** | Output Shape & Hand Calculation | **Complete** | Accurately derived the $(4,4)$ output shape and broke down all four inner-product terms for $y[1,2] = 1$ in the Markdown table. |
| **Step B1–B3** | Core Implementation & SciPy Check | **Complete** | `conv2d_full(x, h)` properly maps `mi, nj = m - i, n - j` and accumulates `h[i, j] * x[mi, nj]` within valid boundaries, achieving `0.0` max error against `scipy.signal.convolve2d`. |
| **Step B4** | Custom Unit Test (Corner Case) | **Complete** | Excellent choice of a degenerate rank-1 test case ($1 \times 3$ row vector convolved with a $2 \times 1$ column vector), testing boundary conditions that square matrices miss. |
| **Step C1–C3** | Bug Localization & Impulse Test | **Complete** | Clearly explained why symmetric kernels mask $180^\circ$ rotation errors, isolated the faulty `h[Mh - 1 - i, Nh - 1 - j]` term, verified with the $\delta[m,n] = [[1.0]]$ impulse test, and repaired the function. |
| **Step D1–D4** | Open Experimental Investigation | **Complete** | Well-structured experiment varying box-filter dimension $K \in \{2, 4, 6\}$ while holding the $10 \times 10$ input constant. Quantified edge spread, peak attenuation ($4/9$ at $K=6$), and energy/sum conservation ($\sum Y = 16.0$). |

---

## 3. Key Strengths of the Submission

1. **Strong Unit-Testing Mindset (Step B4 & Step C2):**  
   Rather than picking random numbers for Step B4, choosing a $1 \times 3$ row array and a $2 \times 1$ column kernel makes the expected $(2, 3)$ outer-product output immediately verifiable by inspection.
2. **Clear Mathematical Reasoning in Debugging (Step C):**  
   The distinction between discrete **convolution** and **cross-correlation** is articulated clearly: since `mi = m - i` and `nj = n - j` already flip the signal relative to the kernel, indexing the kernel from the end (`Mh - 1 - i`) performs a double flip.
3. **Publication-Quality Visualization & Reproducibility (Step D3):**  
   Fixing the color scale (`vmin=0, vmax=1`) across all four subplots allows an honest visual comparison of peak intensity attenuation at $K=6$. Furthermore, exporting the figure automatically via `plt.savefig("smoothing_experiment_heatmaps.png", dpi=300, bbox_inches="tight")` makes the project immediately ready for GitHub presentation.

---

## 4. Checklist Before Pushing to GitHub

1. **Run the Updated D3 Cell:** Execute the updated code cell in VS Code so that `smoothing_experiment_heatmaps.png` is generated in your project folder.
2. **Restart Kernel & Run All:** Click **Restart** and **Run All** in VS Code, then save (`Ctrl + S`) the `.ipynb` file with all outputs visible.
3. **Commit All Files Together:** Add the following files to your GitHub repository root:
   - `ECEUoP_SP_HW_GR_1.ipynb` (your executed notebook)
   - `README.md` (your repository front page)
   - `smoothing_experiment_heatmaps.png` (the exported figure so `README.md` displays it automatically)
   - `PROJECT_REVIEW.md` *(optional, if you wish to include the review documentation)*
