# Response to Reviewer swV6

We thank the reviewer for the careful and constructive feedback. We agree that the submission should have been clearer on scope, approximate closure, error decomposition, proof presentation, and several experimental details. We address these points below.

## 1. On fit to KDD/data science

The paper contributes an **error-certified reduced simulation framework** for scientific dynamical systems, together with a task-aligned reduced representation that can interface with downstream learning.

To make the AI-facing connection concrete, we added a **small downstream-learning probe** in the single-cosine regime. Using the same learner for both feature families-PCA to $16$ dimensions followed by Ridge regression-we compared using the reduced coherence representation $Z(t)$ produced by ShadowFluid as features versus using reconstructed task-level low-pass density features for predicting the next-step unresolved high-frequency energy $E_{\mathrm{HF}}(t+\Delta t)$.

| Train size | Relative gain of $Z(t)$ features |
|---|---:|
| 16 | 14.9\% |
| 32 | 27.6\% |
| 64 | 41.9\% |

This is a concrete hybrid-learning probe.

## 2. On the positioning of approximate closure

What we study is **Hamiltonian commutator-induced operator-subspace closure**, rather than BBGKY-type, Gaussian, or kinetic-moment closure.

Relative to prior shadow Hamiltonian simulation, Somma et al. [6] assume exact invariance: the operator set is closed under commutation with the Hamiltonian, so the shadow state evolves exactly under a reduced Hamiltonian. Our setting starts where this regime fails for task-selected fluid observables. Here, exact closure means the commutator stays inside the chosen span, while approximate closure means projecting/truncating it onto a task-selected coherence dictionary and quantifying the leakage outside that subspace.

Thus, our method is a **task-driven truncation rule** for low-frequency fluid observables.

## 3. On separating low-pass truncation error from shadow-approximation error

We agree that this separation should have been shown more explicitly. Under the same cutoff $K_0$, we compare: (i) the **full-state** solution, (ii) the **exact full low-pass** solution with the same cutoff but no dictionary approximation, and (iii) the **ShadowFluid** approximation from the reduced dictionary evolution.

The existing $V=0$ sanity check already isolates these effects cleanly: ShadowFluid matches the exact full low-pass baseline up to machine precision, so the total density error is entirely explained by low-pass truncation.

To make the same interpretation explicit in the $V \neq 0$ regime, we added a direct decomposition experiment. Here, $\varepsilon_{\mathrm{cutoff}}$ is the normalized gap between exact full low-pass and full-state, $\varepsilon_{\mathrm{shadow}}$ is the normalized gap between ShadowFluid and exact full low-pass, and $\varepsilon_{\mathrm{total}}$ is the normalized gap between ShadowFluid and full-state.

| $\alpha$ | $K_0$ | $\varepsilon_{\mathrm{cutoff}}$ | $\varepsilon_{\mathrm{shadow}}$ | $\varepsilon_{\mathrm{total}}$ | shadow fraction |
|---|---:|---:|---:|---:|---:|
| 0.2 | 8 | 0.006038 | 0.000079 | 0.006038 | 0.0131 |
| 0.5 | 6 | 0.020021 | 0.000886 | 0.020004 | 0.0443 |
| 1.0 | 4 | 0.055400 | 0.010831 | 0.054553 | 0.1985 |

Note that $\varepsilon_{\mathrm{total}}$ is the norm of the **sum of two error vectors**, not the arithmetic sum $\varepsilon_{\mathrm{cutoff}}+\varepsilon_{\mathrm{shadow}}$. Therefore, when $\varepsilon_{\mathrm{shadow}} \ll \varepsilon_{\mathrm{cutoff}}$, $\varepsilon_{\mathrm{total}}$ can coincide with $\varepsilon_{\mathrm{cutoff}}$ up to the displayed precision.

Across these $V \neq 0$ settings, the **dominant source of density error is the low-pass task truncation**, while the additional shadow/dictionary approximation error remains secondary. Even in the strongest tested case $(\alpha=1, K_0=4)$, the shadow contribution stays below $20\%$ of the total density error.

## 4. On the proof sketch and experimental reporting

We agree that both the theory presentation and the experimental reporting should have been more explicit.

On the theory side, we will expand the Frobenius-norm derivation: starting from the variation-of-constants error dynamics, we will show how unitary conjugation preserves the Frobenius norm and how the residual/leakage term leads to the fully computable bound used in the experiments.

On the experimental side, we will state the numerical precision explicitly for both the full-state baseline and ShadowFluid (`complex128` for complex-valued evolution and `float64` for density-level quantities and scalar metrics), include the full-state / exact full low-pass / ShadowFluid decomposition directly in RQ2, and visualize the benchmark potential for the reported $(q,\alpha)$ choices (for the standard case, $V(x)=\alpha\cos(q\cdot x)$ with $q=(1,0)$).

These changes make the closure argument, proof, and reporting clearer.