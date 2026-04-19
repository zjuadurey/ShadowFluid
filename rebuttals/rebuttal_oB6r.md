# Response to Reviewer oB6r

We thank the reviewer for the thoughtful feedback and for recognizing the problem motivation, operator-first formulation, error analysis, and related-work discussion. We address the three main concerns below.

## 1. On the current scope and the role of linear Schr\"odinger dynamics

We agree that the current paper is restricted to **linear Schr\"odinger-type dynamics**, and we will sharpen this boundary in the revision. At the same time, this does **not** mean the framework is limited to trivial irrotational flows: in the Schr\"odinger-flow formulation, finite vorticity can be introduced through a **two-component wavefunction**, as also demonstrated by Meng et al. [6].

More importantly, nonlinearity is a genuine field-wide bottleneck rather than a paper-specific omission. Even Meng et al. [6] handle the vortex example by transforming nonlinear vortex dynamics into a **linear two-component Schr\"odinger equation** for demonstration, and explicitly note that more general body forces-especially Newtonian-fluid dissipation-lead back to nonlinear equations that remain open for efficient quantum treatment.

For this reason, our goal here is narrower: to establish an **error-certified reduced simulation framework** in a regime where closure, truncation, and task-level error can be analyzed rigorously, while making the regime boundary explicit rather than overstating nonlinear-fluid applicability.

## 2. On the use of Fourier-sparse cosine potentials and the generality of the coupling-graph construction

We agree that the original experiments should better show what happens when the Hamiltonian becomes less structured. To address this point, we added a **structured multi-component stress test** that progressively densifies the Fourier coupling graph.

In the fixed-total-coupling setting $(\alpha_{\mathrm{tot}}=0.5, K_0=5, t=0.5, R_{\mathrm{hops}}=1)$, increasing the number of Fourier components from $J=1$ to $J=8$ enlarges the reference set from $|R|=3$ to $17$ and the reduced size from $243$ to $1377$, while the density error remains near $3\times 10^{-2}$:

| Fourier components $J$ | R size | Reduced size | Density error | $\Delta Z_F$ | Leakage $\ell_{\mathrm{rms}}$ |
|---|---:|---:|---:|---:|---:|
| 1 | 3 | 243 | 0.029049 | 0.007733 | 0.242161 |
| 4 | 9 | 729 | 0.030072 | 0.001924 | 0.137493 |
| 8 | 17 | 1377 | 0.029789 | 0.003565 | 0.101099 |

We also quantified the closure-depth tradeoff on the densest structured case $(J=8)$:

| Closure depth $R_{\mathrm{hops}}$ | R size | Reduced size | Density error | $\Delta Z_F$ | Leakage $\ell_{\mathrm{rms}}$ |
|---|---:|---:|---:|---:|---:|
| 1 | 17 | 1377 | 0.029789 | 0.003565 | 0.101099 |
| 3 | 115 | 9315 | 0.029789 | 0.000478 | 0.077749 |

These results clarify the regime boundary: denser couplings do shrink the dimensionality advantage by enlarging $R$, but do not automatically destroy approximation quality in the tested regime. Deeper closure improves operator fidelity, but at a substantial cost in reduced size. In the revision, we will state more explicitly that the current construction is validated for **structured Fourier-coupled regimes**.

## 3. On KDD/ML relevance and the relation to learning-based AI4Science workflows

We agree that the current paper is not a learned-surrogate or data-mining paper in the narrow sense. Our intended contribution is an **error-certified reduced simulation framework** for scientific modeling. At the same time, its AI4Science relevance is more concrete than a generic future-work claim: ShadowFluid produces a compact, task-aligned, and physically grounded reduced representation that can interface naturally with downstream learning.

Following the reviewer’s suggestion in a lightweight form, we added a **small downstream-learning probe** in the tested single-cosine regime. Using the same learner for both feature families-PCA to $16$ dimensions followed by Ridge regression-we compared using the reduced coherence representation $Z(t)$ produced by ShadowFluid as features versus using reconstructed task-level low-pass density features for predicting the next-step unresolved high-frequency energy $E_{\mathrm{HF}}(t+\Delta t)$.

| Train size | Low-pass MSE | Shadow $Z$ MSE | Gain |
|---|---:|---:|---:|
| 16 | $2.369252\times 10^{-7}$ | $2.015528\times 10^{-7}$ | 14.9\% |
| 32 | $2.316244\times 10^{-7}$ | $1.676996\times 10^{-7}$ | 27.6\% |
| 64 | $2.596946\times 10^{-7}$ | $1.509508\times 10^{-7}$ | 41.9\% |

Across all $3$ random splits and all reported train sizes, using $Z(t)$ as features consistently yields lower test MSE than using reconstructed task-level low-pass density features.

Thus, in the tested regime, the reduced representation produced by ShadowFluid is directly useful as a downstream learning feature layer. This is **not** a full ML benchmark or neural PDE surrogate comparison, but it shows that ShadowFluid can serve as a **physically constrained feature extractor**.