
# 2D Discrete Convolution, Unit Testing & Spatial Smoothing Analysis

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![NumPy](https://img.shields.io/badge/NumPy-Array%20Computing-013243)
![SciPy](https://img.shields.io/badge/SciPy-Signal%20Processing-8CAAE6)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557C)

A complete study and from-scratch Python implementation of **Two-Dimensional (2D) Discrete Convolution**, developed as part of the **Signal Processing (ECE-Y523)** course at the Department of Electrical and Computer Engineering, University of Patras.

This repository bridges analytical paper-and-pencil derivations with numerical implementation, systematic unit testing, bug localization (convolution vs. cross-correlation), and an experimental investigation of spatial box-filter smoothing.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Mathematical Background](#mathematical-background)
- [Repository Structure & Workflow](#repository-structure--workflow)
- [Key Findings & Experimental Results](#key-findings--experimental-results)
- [Getting Started](#getting-started)
- [Acknowledgments](#acknowledgments)

---

## Project Overview

Implementing signal processing algorithms requires more than translating equations into loops; it demands careful handling of boundary conditions, array indexing, and diagnostic test design. This project is structured into four progressive stages:

1. **Step A — Analytical Derivation:** Manual computation of full 2D convolution output dimensions and element-wise products under zero-padded boundary conditions.
2. **Step B — From-Scratch Implementation & Verification:** Implementation of `conv2d_full(x, h)` in pure Python/NumPy and validation against `scipy.signal.convolve2d` across square, rectangular, and rank-1 (row/column vector) corner cases.
3. **Step C — Diagnostic Testing & Debugging:** Analysis of a faulty implementation (`conv2d_candidate`) that inadvertently performs cross-correlation by double-reversing indices. Demonstrates why symmetric kernels and shape-only checks fail to catch orientation bugs, and how the **2D unit impulse test** ($\delta[m,n]$) immediately exposes kernel rotation errors.
4. **Step D — Experimental Investigation:** Quantitative and visual analysis of how the spatial scale $K \times K$ of a normalized uniform averaging filter affects edge blur, peak intensity attenuation, and total sum conservation on a 2D square pulse.

---

## Mathematical Background

Given a 2D input signal $X \in \mathbb{R}^{M \times N}$ and a 2D kernel (filter) $H \in \mathbb{R}^{M_h \times N_h}$, the **full 2D discrete convolution** $Y = X * H$ has spatial dimensions:

$$\text{shape}(Y) = (M + M_h - 1,\ N + N_h - 1)$$

Each output sample $y[m, n]$ is defined as:

$$y[m,n] = \sum_{i=0}^{M_h-1} \sum_{j=0}^{N_h-1} H[i,j] \, X[m-i, n-j]$$

where any sample $X[m-i, n-j]$ falling outside the valid index domain $[0, M-1] \times [0, N-1]$ is treated as zero (**zero-padding**).

---

## Repository Structure & Workflow

| Section | Focus Area | Description |
| :--- | :--- | :--- |
| **Step A** | Hand Calculation | Computes output shape `(4, 4)` and evaluates $y[1,2] = 1$ step by step for a $3 \times 3$ input and $2 \times 2$ kernel. |
| **Step B** | Implementation & Unit Tests | Implements `conv2d_full` and validates numerical agreement (`np.allclose`) on square ($3 \times 3 * 2 \times 2$), rectangular ($4 \times 6 * 3 \times 2$), and vector ($1 \times 3 * 2 \times 1$) inputs. |
| **Step C** | Bug Localization | Diagnoses an index-reversal bug (`h[Mh - 1 - i, Nh - 1 - j]`) using non-symmetric kernels and the 2D unit impulse `[[1.0]]`, then repairs the implementation. |
| **Step D** | Smoothing Experiment | Evaluates normalized $K \times K$ box filters ($K \in \{2, 4, 6\}$) on a $10 \times 10$ image containing a $4 \times 4$ bright square pulse. |

---

## Key Findings & Experimental Results

### 1. Why Symmetric Kernels Hide Orientation Bugs (Step C)
When testing `conv2d_candidate` with a symmetric $2 \times 2$ kernel of ones, the output matched SciPy with `0.0` error because a symmetric kernel is invariant under $180^\circ$ rotation. Passing a **2D unit impulse** $\delta[m,n] = [[1.0]]$ with an asymmetric kernel immediately revealed that the candidate function returned the kernel rotated by $180^\circ$.

### 2. Effect of Box-Filter Scale $K \times K$ (Step D)
Convolving a $10 \times 10$ input $X$ (containing a centered $4 \times 4$ square of ones, $\sum X = 16.0$) with normalized uniform kernels $H_K$ of weight $1/K^2$ yielded the following metrics:

| Kernel Size ($K \times K$) | Output Shape | Max Intensity | Output Sum ($\sum Y$) | Observation |
| :---: | :---: | :---: | :---: | :--- |
| **Input $X$** | `(10, 10)` | `1.0000` | `16.0000` | Sharp binary edges around the $4 \times 4$ square. |
| **$2 \times 2$** | `(11, 11)` | `1.0000` | `16.0000` | Slight edge smoothing; kernel fits inside square. |
| **$4 \times 4$** | `(13, 13)` | `1.0000` | `16.0000` | Critical scale $K=4$; only the center pixel retains `1.0`. |
| **$6 \times 6$** | `(15, 15)` | `0.4444` | `16.0000` | Kernel exceeds object size; peak drops to $16/36 = 4/9$. |

---



## Acknowledgments

- **Course:** Signal Processing (ECE-Y523), Department of Electrical and Computer Engineering, University of Patras.
- **Tools:** Implemented with Python, NumPy, SciPy, and Matplotlib; experimental design and English documentation assisted by Google Gemini.
