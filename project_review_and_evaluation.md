# English Markdown Cells for `ECEUoP_SP_HW_GR_1.ipynb`

Below you will find every Markdown cell of the notebook translated into English, including both the **assignment instructions** and your **completed answers**, so you can copy-paste them directly into your `.ipynb` file before pushing to GitHub. At the end of this file, English versions of the `print` statements and plot titles are also provided for completeness.

## Cell 1 — Introduction (`conv-student-000`)

```
# Homework 1: 2D Convolution and Output Verification

In this assignment, we will connect a small hand calculation with the output of a program and explore how to design effective verification tests.

The steps are:

- **Step A:** Find the output size and compute one output value by hand.
- **Step B:** Complete a minimal implementation of 2D convolution and compare it against SciPy.
- **Step C:** Identify and fix a bug in a candidate implementation.
- **Step D:** Formulate our own research question and conduct a small computational investigation.
- Optional feedback at the end.

**Related material:** Worksheet 3, Exercises 1.1, 1.2, and 5.1. You may also consult the solved recitation exercises.

```

## Cell 2 — Setup Instructions (`conv-student-001`)

```
## Before We Start

This assignment uses the NumPy, SciPy, and Matplotlib libraries.
To install them, run `%pip install numpy scipy matplotlib` in a separate cell and restart the kernel.

The initial notebook executes even with the blanks left unfilled. `None` indicates "no answer has been provided yet." `np.nan` is used here as a placeholder for a missing number. Helper functions are provided. Fill in your code between `### TODO: ADD YOUR CODE HERE` and `### END of TODO`.

```

## Cell 5 — Step A Header (`conv-student-002`)

```
## Step A

We use the input array and kernel from Exercise 1.1. Indexing starts at $0$. In `X[m, n]`, the first index selects the row and the second selects the column.

```

## Cell 7 — Step A1 & A2 Instructions (`conv-student-004-2`)

```
$X$ is the input matrix and $H$ is the kernel, i.e., the filter weights. $y[m,n]$ is a single output value. For this specific kernel:

$$y[m,n]=\sum_{i=0}^{1}\sum_{j=0}^{1}H[i,j]X[m-i,n-j].$$

The indices $m,n$ select the output position. The indices $i,j$ iterate over the rows and columns of the kernel. The two summations mean that we add four products. Any value of $X$ outside the given matrix is assumed to be zero (zero-padding).

### A1. Output Size
If the input has $M$ rows and $N$ columns and the kernel has $M_h$ rows and $N_h$ columns, the full convolution output has shape:

$$(M+M_h-1,\ N+N_h-1).$$

The subscript $h$ distinguishes the dimensions of the kernel from those of the input. The shape is written as `(rows, columns)`. This notation is called a **tuple** and stores both values together. For example, a matrix with $2$ rows and $5$ columns has shape `(2, 5)`.

Find the output shape for the matrices above and fill in `shape_by_hand` in the same format.

### A2. One Value by Hand
In class, we calculated $y[1,1]=5$. Now find **$y[1,2]$**, which corresponds to the second row and third column of the output. First complete the table in the separate answer cell below without calling SciPy.

Add the four products and assign the result to `value_by_hand`.

```

## Cell 8 — Answer A2 Table (`answer-alpha2-table`)

```
**Answer A2**

| $(i,j)$ | $H[i,j]$ | Position $X[1-i,2-j]$ | Input Value | Product |
|---|---:|---|---:|---:|
| $(0,0)$ | $1$ | $X[1,2]$ | $-1$ | $-1$ |
| $(0,1)$ | $2$ | $X[1,1]$ | $1$ | $2$ |
| $(1,0)$ | $-1$ | $X[0,2]$ | $0$ | $0$ |
| $(1,1)$ | $0$ | $X[0,1]$ | $2$ | $0$ |

```

## Cell 10 — Step B1 Instructions (`conv-student-006-2`)

```
## Step B
### B1. Two-Dimensional Convolution

The function `conv2d_full(x, h)` computes the full convolution of two non-empty, real-valued, two-dimensional arrays. The lowercase `x, h` are the parameters of the function. When we call it with `X, H`, it will use the matrices from Step A.

Complete **the two TODO regions**:

1. The input indices `mi, nj` corresponding to `m, n, i, j`.
2. The product added to `y[m, n]`.

The loops, output size initialization, boundary checks, and `return` statement are already provided.

In NumPy, negative indices read elements from the end of the array. The boundary check ensures that only samples within the input bounds participate in the calculation, consistent with zero-padding.

Follow the formula from Step A. In that formula, kernel flipping is expressed through the indices of the input.

```

## Cell 12 — Step B2 Instructions (`conv-student-010-2`)

```
### B2. Convolution Using SciPy

SciPy provides a built-in implementation. Using `mode="full"`, we retain the entire convolution output. Using `boundary="fill", fillvalue=0`, we specify that values outside the input boundaries are zero. SciPy already handles the convolution flipping internally, so we pass the original kernel directly.

The function `compare_outputs` displays the shapes of both results. If they match, it computes the maximum absolute difference, i.e., the largest distance between any two corresponding values. `np.allclose` returns `True` when all corresponding values agree within a small numerical tolerance.

A value very close to zero indicates numerical agreement on the given example. Note that tiny differences, such as $10^{-15}$, may arise due to the order of floating-point operations.

As long as `np.nan` placeholders remain in the function's TODO blocks, the difference is displayed as `nan` and numerical agreement as `False`.

```

## Cell 14 — Step B3 Instructions (`conv-student-012-2`)

```
### B3. Correctness Verification

To verify the correctness of our algorithm, we can design small tests ("unit tests") aimed at checking our solution across different types of input. This allows us to evaluate the implementation under broader conditions, such as corner cases and non-square dimensions.

By testing diverse inputs, we can catch bugs that remain hidden in the initial example and build greater confidence in our implementation. Here, we test the following condition: in the first example, both the $3 \times 3$ input and the $2 \times 2$ kernel are square. If we accidentally swapped the number of rows with the number of columns, the bug might go unnoticed because the two dimensions are equal. Let us see what happens when we use a $4 \times 6$ input and a $3 \times 2$ kernel, where the dimensions differ. The full output has shape `(6, 7)`, allowing us to verify distinct row and column counts in the result as well.

```

## Cell 16 — Step B4 Instructions (`beta3-own-check-instructions`)

```
### B4. Your Own Verification Check

Select one additional small input and kernel to test a different case of `conv2d_full`. Briefly explain why you chose these data and what you expect to happen. Fill in the two arrays in the code cell and compare the output against SciPy. Then update your written answer to state whether the result matches your prediction.

```

## Cell 17 — Step B4 Completed Answer (`beta3-own-check-answer`)

```
**Test Case Selection:** We selected a single-row input vector of shape $1 \times 3$ (`x_check = [[1, 2, 3]]`) and a single-column kernel vector of shape $2 \times 1$ (`h_check = [[2], [-1]]`). This tests the corner case where one dimension of each 2D array is equal to $1$ (row vector convolved with a column vector, i.e., an outer-product-like structure).

**Prediction:** According to the spatial dimension formula $(M+M_h-1,\ N+N_h-1)$, we expect the output shape to be $(1+2-1,\ 3+1-1) = (2, 3)$. Specifically, the first row of the output should be the input multiplied by $2$, yielding `[2, 4, 6]`, and the second row should be the input multiplied by $-1$, yielding `[-1, -2, -3]`.

**Conclusion:** After execution, `conv2d_full` produces an output of shape `(2, 3)` with values `[[2., 4., 6.], [-1., -2., -3.]]`. The maximum absolute difference against SciPy is `0.0` (`np.allclose` returns `True`), confirming both our analytical prediction and the correctness of the implementation on degenerate 1D-like shapes.

```

## Cell 19 — Step C1 Instructions (`conv-student-014-2`)

```
## Step C
### C1. Bug Localization

The function `conv2d_candidate` contains a computational bug. It executes without syntax errors and returns the correct output shape. **First, observe its outputs.**

We will compare this candidate function against SciPy, first using a symmetric kernel whose weights are all equal to $1$, and then using the non-symmetric kernel `H` from Step A. Predict in which case an indexing/flipping error will be easier to detect.

Do not modify the function yet.

```

## Cell 22 — Step C1.1 Instructions (`conv-student-019`)

```
#### C1.1. The First Kernel

Does the kernel with all coefficients equal to $1$ change when rotated by $180^\circ$? Fill in your answer in the next code cell. Then briefly explain how this affects our ability to detect an erroneous kernel reversal.

```

## Cell 24 — Step C1.1 Completed Answer (`conv-student-021`)

```
**Justification C1.1**

A kernel with all coefficients equal to $1$ is completely symmetric across both axes and remains invariant when rotated by $180^\circ$ (flipped horizontally and vertically). Consequently, using such a kernel masks any bug related to index reversal: the unflipped and flipped kernels are identical, producing the exact same numerical output and preventing us from detecting the error.

```

## Cell 25 — Step C1.2 Instructions (`gamma1-factor-question`)

```
#### C1.2. Identifying the Faulty Term

In the line that updates `y[m, n]`, which term reads the kernel with reversed indices? Write inside the quotes the exact `h[...]` term found in the faulty function. We will fix the function in Step C3.

```

## Cell 27 — Step C1.3 Instructions (`gamma1-shape-question`)

```
#### C1.3. Checking the Output Shape

Is having the correct output shape sufficient to confirm that the function computes the convolution properly? Fill in your answer and justify it based on the results observed above.

```

## Cell 29 — Step C1.3 Completed Answer (`gamma1-shape-reason`)

```
**Justification C1.3**

No, verifying the output shape alone is not sufficient. As demonstrated in the test with the non-symmetric kernel `H`, `conv2d_candidate` returns the expected `(4, 4)` output shape, yet its numerical values are incorrect (yielding a maximum absolute difference of `7.0` compared to SciPy and `False` for numerical agreement). Output dimensions only depend on array allocation, not on the accuracy of the inner-loop arithmetic.

```

## Cell 30 — Step C2 Instructions (`conv-student-022`)

```
### C2. Unit Impulse Test

A useful test case is one whose outcome we can easily predict. Here we use the array `[[1.0]]` as input, with zero values everywhere outside it. This input represents the 2D discrete unit impulse (Kronecker delta) at the origin of both axes.

Convolving the unit impulse with the kernel `H` must return `H` itself (the impulse response). Since the single non-zero input sample has value $1$, it reproduces the kernel elements at the corresponding output positions.

Run the next cell and compare the two matrices:

1. Does `conv2d_candidate` return the expected matrix `H`?
2. Describe how the returned matrix differs from `H`.

```

## Cell 33 — Step C2 Completed Answer (`answer-gamma-2`)

```
**Description of Difference C2**

The matrix returned by `conv2d_candidate` (`[[0., -1.], [2., 1.]]`) is the original kernel `H` (`[[1., 2.], [-1., 0.]]`) rotated by $180^\circ$—that is, flipped both vertically (along rows) and horizontally (along columns). Because the input indices `m - i` and `n - j` already account for the convolution flip, reversing the kernel indices a second time via `h[Mh - 1 - i, Nh - 1 - j]` cancels out the flip and computes cross-correlation instead of convolution.

```

## Cell 34 — Step C3 Instructions (`conv-student-023-2`)

```
### C3. Bug Fix

The next cell contains a second copy of the function (`conv2d_repaired`) so that we preserve the original faulty implementation as a reference example. Fix the line between the TODO comments. Then re-run the function definition and the two comparison tests that follow.

```

## Cell 37 & 38 — Step D Instructions (`conv-student-026` & `delta-deliverables`)

```
## Step D: Open Investigation

Choose **one question related to the material covered so far in the lectures and recitations (Lectures 1–3)**. Design a small computational experiment to help you better understand the topic you selected.

### Topic Selection

Pick something that interests you or challenged you and to which you would like to dedicate some extra time. You may start from an exercise seen in class, from the course worksheets, or from your own question.

Indicative directions for investigation include:

- **Visualization:** Create a visual representation that aids in understanding a mathematical concept or method.
- **Experimentation:** Modify a known exercise or vary the parameters of a problem and examine how the outcome is affected.
- **Implementation:** Explore alternative ways of implementing a method or analyze the behavior of an algorithm.
- **Verification:** Test the validity of a mathematical property, hypothesis, or computational result.

### Experiment Design

The goal is not merely to reproduce a known solution or plot a chart, but to investigate a specific question and explain what you observe.

Design a **small test, comparison, or visualization** that answers your question. Consider which quantities you vary, which you keep constant, and what you expect to observe.

### Tools

Your investigation must include **executable Python code in the notebook**. You may use any library you wish, as well as AI tools such as ChatGPT, Claude, or Gemini. Using AI is permitted and encouraged as an aid for understanding concepts, designing experiments, writing code, or interpreting results. If you use AI, state the model and the prompts used.

---

### Deliverables

```

## Cell 39 & 40 — Step D1 Question & Completed Answer (`delta1-instructions` & `delta1-answer`)

```
#### D1. Research Question and Initial Prediction

What question did you choose, why does it interest you, and what did you expect to happen?

```

```
**Research Question:** How does increasing the spatial dimension $K \times K$ of a normalized uniform averaging kernel (box filter, where every weight equals $1/K^2$) affect its 2D convolution with a bright square pulse in terms of:
1. Edge sharpness and spatial spread,
2. Peak (maximum) output intensity, and
3. Conservation of the total sum of array elements?

**Motivation:** In Step C, we examined a symmetric kernel filled with ones. In image and signal processing, normalized uniform kernels are widely used as low-pass smoothing filters (box blur). We want to understand both visually and quantitatively how the kernel scale $K$ interacts with the characteristic size of an object in the input signal.

**Initial Prediction:**
1. As $K$ increases ($K = 2, 4, 6$), the full output dimensions will expand according to $(M+K-1,\ N+K-1)$, and the sharp boundaries of the square pulse will become progressively blurrier (wider transition band).
2. As long as the kernel size $K$ is less than or equal to the width of the bright square ($4 \times 4$), the maximum value at the center of the output will remain $1.0$, because the kernel can fit entirely inside the region of ones. Once $K$ exceeds $4$ (i.e., $K=6$), the peak value will drop below $1.0$ because the kernel footprint will inevitably overlap with surrounding zeros.
3. The total sum of all elements in the output array will remain invariant and equal to the sum of the input array ($16.0$), because the kernel weights sum to $1.0$.

```

## Cell 41 & 42 — Step D2 Method & Completed Answer (`delta2-instructions` & `delta2-answer`)

```
#### D2. Methodology and Tools

Describe what you varied, what you kept constant, and how you tested your question. If you used AI, report the model and prompts.

```

```
**Constant Parameters:**
- The input array $X$ of shape $10 \times 10$, containing a centered $4 \times 4$ square pulse of ones ($1.0$) surrounded by zeros ($0.0$). The total sum of the input elements is $\sum X = 16.0$.
- The normalization constraint of the smoothing kernel $H_K$, where all weights are set to $1/K^2$ so that $\sum_{i,j} H_K[i,j] = 1.0$.

**Varied Parameter:**
- The spatial dimension $K$ of the square kernel $H_K$ ($K \times K$), tested across three values: $K = 2$, $K = 4$, and $K = 6$.

**Methodology and Tools:**
We implemented the experiment in Python using `NumPy` and `Matplotlib`, utilizing our verified `conv2d_full` function from Step B. For each value of $K$, we compute the full 2D convolution, log the output shape, maximum intensity, and total element sum in a summary table, and visualize the resulting 2D arrays side by side as heatmaps (`viridis` colormap with fixed range $[0, 1]$).

**Use of AI:**
We used **Google Gemini** to help structure the experimental design, format the visualization code, and translate the documentation into English.
- *Prompts used:* 
  1. *"Design and explain step by step an experiment for Step D in Python using NumPy and Matplotlib where we vary a single kernel parameter while keeping the 2D input signal constant."*
  2. *"Translate all the notebook Markdown cells and explanations into English and prepare a README and project review for GitHub."*

```

## Cell 43, 45 & 46 — Step D3 & D4 Completed Answer (`delta3-instructions`, `delta4-instructions` & `delta4-answer`)

```
#### D3. Experiment Code

Include your executable Python code in the next code cell.

```

*(Note: See the English code snippet at the bottom of this file for Cell D3 so that your table headers and plot titles render in English!)*

```
#### D4. Results and Observations

Present your results using code output, tables, plots, or any other suitable format. You may refer directly to the output of D3. Clearly show what is being compared or verified. Explain your conclusions, whether the results match your prediction, and whether they answer your initial question. Add what you would like to investigate next, if applicable.

```

```
**Results and Observations (referencing D3 output):**
1. **Output Shape and Edge Smoothing:** Both the printed table and the heatmaps confirm that the output dimensions grow from `(10, 10)` in the input to `(11, 11)`, `(13, 13)`, and `(15, 15)` for $K = 2, 4, 6$, respectively, matching $(10+K-1,\ 10+K-1)$. Visually, as $K$ increases, the sharp step transition at the boundary of the square turns into a broader, smoother gradient ramp.
2. **Peak Intensity Attenuation:** For $K=2$ and $K=4$, the maximum output value remains exactly `1.0000` because the $2 \times 2$ and $4 \times 4$ kernels fit completely inside the $4 \times 4$ square pulse at the center position. However, when $K=6$, the maximum value drops to `0.4444` ($16 / 36 = 4/9$), since even when centered, the $6 \times 6$ kernel averages all $16$ ones together with $20$ surrounding zeros.
3. **Sum Preservation (Conservation Property):** Across all three kernels ($K=2, 4, 6$), the total sum of the output elements remains constant at `16.0000`, matching the input sum $\sum X = 16.0$. This numerically verifies the fundamental convolution property $\sum_{m,n} (X * H)[m,n] = \left(\sum_{m,n} X[m,n]\right)\left(\sum_{i,j} H[i,j]\right)$.

**Conclusion:** The experimental findings completely confirm our initial predictions and clearly illustrate the spatial and energetic effects of box-filter scale in 2D convolution.

**Future Work:** As a next step, it would be interesting to corrupt the input square pulse with additive white Gaussian noise (AWGN) and measure the Signal-to-Noise Ratio (SNR) improvement as a function of $K$, quantifying the fundamental trade-off between noise suppression and edge blurring.

```

## Cell 47–51 — Final Checklist & Optional Feedback (`conv-student-033` to `9e36311f`)

```
## Before Submitting the Assignment

- Run **Restart Kernel and Run All** and save the notebook **with all cell outputs visible**, including tables, plots, and printouts from Step D.
- The erroneous responses of `conv2d_candidate` must remain visible as part of the diagnostic example.
- Ensure your name and student ID are filled in at the top of the notebook.

```

```
### Optional: Give Us Your Feedback!

How much did each part of the recitation/assignment help your understanding?
- **A.** Analytical exercises by hand
- **B.** Connecting analytical exercises with Python implementation
- **C.** Open experimental investigation

```

```
* bridging the analytical hand calculations with unit testing and visual experimentation in Python made the mechanics of 2D convolution and index flipping very intuitive.*

```

## Bonus: English Print Statements & Plot Labels for Code Cells

If you also want the code outputs in your notebook to appear in English on GitHub, you can use these English print labels in **Cell D3**:

```
### TODO: ADD YOUR CODE HERE
# 1. Define constant input X (10x10) with a centered 4x4 square pulse
X_exp = np.zeros((10, 10), dtype=float)
X_exp[3:7, 3:7] = 1.0

# Values of the varied parameter K (kernel size KxK)
K_values = [2, 4, 6]

print(f"Input X: Shape = {X_exp.shape}, Max Value = {np.max(X_exp):.4f}, Sum of Elements = {np.sum(X_exp):.4f}\n")
print(f"{'Kernel Size (KxK)':<20} | {'Output Shape':<14} | {'Max Value':<12} | {'Output Sum':<12}")
print("-" * 66)

# 2. Set up figure (1 row, 4 columns: Input + 3 Outputs)
fig, axes = plt.subplots(1, 4, figsize=(14, 3.5))

im0 = axes[0].imshow(X_exp, cmap="viridis", vmin=0, vmax=1)
axes[0].set_title("Input X (10x10)")
plt.colorbar(im0, ax=axes[0], fraction=0.046, pad=0.04)

# 3. Run experiment for each K
for idx, K in enumerate(K_values, start=1):
    # Normalized uniform averaging kernel KxK (weights sum to 1)
    H_k = np.ones((K, K), dtype=float) / (K ** 2)
    
    # Compute full 2D convolution using our function from Step B
    Y_k = conv2d_full(X_exp, H_k)
    
    # Print metrics
    print(f"{f'{K}x{K}':<20} | {str(Y_k.shape):<14} | {np.max(Y_k):<12.4f} | {np.sum(Y_k):<12.4f}")
    
    # Plot heatmap
    im = axes[idx].imshow(Y_k, cmap="viridis", vmin=0, vmax=1)
    axes[idx].set_title(f"Output K={K} {Y_k.shape}")
    plt.colorbar(im, ax=axes[idx], fraction=0.046, pad=0.04)

plt.tight_layout()
plt.show()
### END of TODO

```