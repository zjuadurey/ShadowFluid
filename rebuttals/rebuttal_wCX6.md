# Response to Reviewer wCX6

We thank the reviewer for the thoughtful feedback. We appreciate the concerns about the paper’s AI4Science positioning, the scope of the currently demonstrated benefit, and the distinction from prior shadow Hamiltonian simulation. We address these three points below.

## 1. On the paper’s AI4Science positioning and KDD relevance

We agree that this boundary should have been stated more clearly. Our main contribution is an **error-certified reduced simulation framework** for scientific dynamical systems, rather than a learned surrogate in the narrow sense.

ShadowFluid extracts a **compact, task-aligned, physically grounded representation** from a high-dimensional scientific system, while retaining an explicit accuracy-control mechanism. In this sense, it is complementary to learned ROMs and neural PDE solvers: it provides a certified reduced representation for downstream scientific tasks, rather than replacing learning itself.

To make this concrete, we added a **small downstream-learning probe** in the tested single-cosine regime. Using the same learner for both feature families-PCA to $16$ dimensions followed by Ridge regression-we compared the reduced coherence representation $Z(t)$ produced by ShadowFluid against reconstructed task-level low-pass density features for predicting the next-step unresolved high-frequency energy $E_{\mathrm{HF}}(t+\Delta t)$:

| Train size | Low-pass density MSE | Shadow coherence $Z$ MSE | Relative gain |
|-|-:|-:|-:|
| 16 | $2.369252\times 10^{-7}$ | $2.015528\times 10^{-7}$ | 14.9\% |
| 32 | $2.316244\times 10^{-7}$ | $1.676996\times 10^{-7}$ | 27.6\% |
| 64 | $2.596946\times 10^{-7}$ | $1.509508\times 10^{-7}$ | 41.9\% |

Thus, under the same learner and matched feature dimension, using $Z(t)$ as features yields lower test MSE than reconstructed low-pass density features. This is a concrete AI integration beyond the original future-work statement.

## 2. On the concretely demonstrated implementation-level benefit

We agree that the currently demonstrated benefit should be stated precisely. Beyond validating the error-control theory, the current study also shows a concrete **implementation-level gain**: replacing the full target with the reduced target shrinks the synthesized register and compiled circuit.

In the original quantum-fluid encoding, the register width is tied to the 2D grid representation. ShadowFluid changes this dependence by replacing the full grid-driven target with a reduced target whose size is determined by the retained task-relevant modes/coherences. Importantly, both **initial state preparation** and **time evolution** are implemented as quantum circuits, so reductions in target size directly translate into reductions in compiled-circuit complexity.

| Case | Full target | Reduced target | Qubits | Two-qubit gate count | Two-qubit depth |
|---|---|---|---:|---:|---:|
| State preparation | 128 amplitudes | 18 retained amplitudes packed into 32 | $7 \rightarrow 5$ | $268 \rightarrow 55$ | $260 \rightarrow 55$ |
| $V=0$ time evolution | $64\times 64$ active-mode unitary | $9\times 9$ reduced unitary padded to $16\times 16$ | $6 \rightarrow 4$ | $174 \rightarrow 29$ | $164 \rightarrow 28$ |

The practical point is simple: lower **two-qubit gate count** means fewer entangling operations overall, and lower **two-qubit depth** means fewer sequential layers after transpilation. Both are closely related to compiled-circuit cost and runtime. Our method reduces both substantially.

## 3. On novelty beyond prior shadow Hamiltonian simulation

We agree that the distinction from prior shadow Hamiltonian simulation should have been made more explicit. The key novelty is **not** simply applying a general shadow-Hamiltonian idea to fluid dynamics. Rather, prior shadow Hamiltonian simulation provides a reduced evolution principle under the strict operator closure, whereas ShadowFluid turns this principle into a **task-driven and certifiable reduced-simulation method for fluid observables in the practically relevant approximate-closure regime**.

Concretely, ShadowFluid adds:  
(1) a **task-driven multi-reference coherence dictionary** tailored to low-frequency fluid observables in the Fourier basis;  
(2) a **coupling-graph-based dictionary construction** for PDE-style Fourier-space Hamiltonians; and  
(3) a **state-independent, \textit{a priori} computable commutator-leakage certificate** for approximate closure, together with empirical validation of the hierarchy between the computable bound, the operator-level discrepancy, and the downstream task error on 2D Schr\"odinger-flow benchmarks.

In short, if Somma et al. establish the shadow-evolution principle under closure, ShadowFluid contributes the **task construction, approximate-closure treatment, and computable certification machinery** needed to make that principle usable for reduced quantum fluid simulation.