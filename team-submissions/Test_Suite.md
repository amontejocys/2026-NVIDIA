# Test Suites: LABS Energy and Reverse Symmetry

This section describes the validation and evaluation test suites used in our QAOA-based solution for the **Low Autocorrelation Binary Sequence (LABS)** problem. These tests ensure correctness, physical consistency, and robustness of the quantum–classical optimization pipeline.

---

## Test Suite 1: LABS Energy Evaluation

### Objective
The LABS Energy test suite evaluates the **quality of a candidate bitstring** by computing its Low Autocorrelation Binary Sequence (LABS) energy. This energy function serves as the **optimization objective** minimized by the QAOA circuit.

---

### Mathematical Definition

Given a binary sequence:

\[
\mathbf{s} = (b_0, b_1, \dots, b_{N-1}), \quad b_i \in \{0,1\}
\]

it is mapped to an Ising spin representation:

\[
\sigma_i = 2b_i - 1 \in \{-1, +1\}
\]

The aperiodic autocorrelation for lag \(k\) is defined as:

\[
C_k = \sum_{i=0}^{N-k-1} \sigma_i \sigma_{i+k}
\]

The LABS energy is then computed as:

\[
E(\mathbf{s}) = \sum_{k=1}^{N-1} C_k^2
\]

Lower energy values correspond to sequences with lower autocorrelation, which are optimal solutions.

---

### Role in the Pipeline

- Used to **score all measured bitstrings** obtained from the QAOA circuit.
- Acts as the **classical cost function** for:
  - Selecting the best bitstring per circuit execution.
  - Ranking \((\gamma, \beta)\) parameter pairs during QAOA seeding.
- Enables **hardware-independent validation**, since energy evaluation is performed classically.

---

### Importance

- Accurately captures the combinatorial structure of the LABS problem.
- Squared autocorrelations strongly penalize structured correlations.
- Provides a **reproducible and quantitative metric** for comparing candidate solutions.

---

## Test Suite 2: Reverse Symmetry Validation

### Objective
The Reverse Symmetry test suite verifies that the LABS energy function respects **mirror (reverse) symmetry**, a known invariance of the LABS problem.

---

### Definition

For a given bitstring:

\[
\mathbf{s} = (b_0, b_1, \dots, b_{N-1})
\]

its reversed sequence is defined as:

\[
\mathbf{s}^{\text{rev}} = (b_{N-1}, b_{N-2}, \dots, b_0)
\]

---

### Symmetry Property

The LABS energy is invariant under reversal:

\[
E(\mathbf{s}) = E(\mathbf{s}^{\text{rev}})
\]

This invariance holds because the autocorrelation function depends only on **relative index differences**, not absolute positions.

---

### Role in the Pipeline

- Serves as a **correctness and consistency check** for:
  - Classical energy computation
  - Quantum measurement post-processing
- Ensures that:
  - The cost Hamiltonian introduces no directional bias
  - The interaction graphs \(G_2\) and \(G_4\) preserve symmetry

---

### Importance

- Confirms **physical and mathematical validity** of the implementation.
- Prevents false optima caused by asymmetric encoding errors.
- Particularly useful for **debugging and rapid validation** in a hackathon setting.

---

## Summary

| Test Suite       | Purpose                              | Guarantee                      |
|------------------|--------------------------------------|--------------------------------|
| LABS Energy      | Quantitative evaluation of solutions | Correct optimization objective |
| Reverse Symmetry | Structural validation                | Invariance and correctness     |

Together, these test suites ensure that the QAOA-based LABS solver is **numerically correct**, **physically consistent**, and **faithful to the underlying optimization problem**.
