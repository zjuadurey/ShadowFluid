# Response to Reviewer oB6r

We thank the reviewer for the thoughtful feedback and for recognizing the motivation of the problem, the elegance of the operator-first framework, the value of the error analysis, and the clarity of the related-work discussion. We also appreciate the reviewer’s comments on scope, benchmark simplicity, and KDD/ML relevance. We address these points below.

## 1. On the current scope being limited to linear Schrödinger dynamics

We agree that the current paper is restricted to **linear Schrödinger-type dynamics** and does not claim to solve general nonlinear CFD or Navier--Stokes dynamics. Our goal is narrower: to establish an **error-certified reduced simulation framework** in a setting where closure structure and truncation behavior can be analyzed rigorously. We will sharpen this boundary in the introduction, discussion, and limitations sections.

## 2. On the use of Fourier-sparse cosine potentials and the generality of the coupling-graph construction

We agree that the original experiments should do more to show what happens as the Hamiltonian becomes less structured. To address this point, we added a **structured multi-component stress test** that progressively densifies the coupling graph.

In the fixed-total-coupling setting $(\alpha_{\mathrm{tot}}=0.5, K_0=5, t=0.5, R_{\mathrm{hops}}=1)$, increasing the number of Fourier components from $J=1$ to $J=8$ enlarges the reference set from $|R|=3$ to $17$ and the reduced size from $243$ to $1377$, while the density error remains near $3\times 10^{-2}$:

Fourier components $J$|R size|Reduced size|Density error|$\Delta Z_F$|Leakage $\ell_{\mathrm{rms}}$
|---|---:|---:|---:|---:|---:|
1|3|243|0.029049|0.007733|0.242161
4|9|729|0.030072|0.001924|0.137493
8|17|1377|0.029789|0.003565|0.101099

We also quantified the closure-depth tradeoff on the densest structured case $(J=8)$:

Closure depth $R_{\mathrm{hops}}$|R size|Reduced size|Density error|$\Delta Z_F$|Leakage $\ell_{\mathrm{rms}}$
|---|---:|---:|---:|---:|---:|
1|17|1377|0.029789|0.003565|0.101099
3|115|9315|0.029789|0.000478|0.077749

These added results clarify the regime boundary: denser couplings do shrink the dimensionality advantage by enlarging $R$, but do not automatically destroy approximation quality in the tested regime. In particular, deeper closure improves operator fidelity, but at a clearly visible cost in reduced size. In the revision, we will state more explicitly that the current construction is validated for **structured Fourier-coupled regimes**, rather than claimed to cover fully broadband or non-periodic Hamiltonians.

## 3. On KDD/ML relevance and the relation to learning-based AI4Science pipelines

We agree that the current paper is not a learned-surrogate or data-mining paper in the narrow sense. Our intended contribution is an **error-certified reduced simulation framework** for scientific modeling.

To make the AI/ML connection more concrete, we added a **small downstream-learning probe** in the tested single-cosine regime. Using the same learner (PCA to $16$ dimensions + Ridge), we compared **Shadow coherence $Z$ features** against **task-level low-pass density inputs** for predicting the next-step unresolved high-frequency energy $E_{\mathrm{HF}}(t+\Delta t)$, which is **not** directly available from the task truncation. The probe uses the same single-cosine family with $q=(1,0)$, $N=16$, $K_0=4$, $\alpha\in\{0.2,0.6,1.0\}$, $t\in\{0,0.2,0.4,0.6\}$, and one Gaussian-vortex initial-condition family.

Train size|Low-pass MSE|Shadow MSE|Gain
|---|---:|---:|---:|
16|$2.369252\times 10^{-7}$|$2.015528\times 10^{-7}$|14.9\%
32|$2.316244\times 10^{-7}$|$1.676996\times 10^{-7}$|27.6\%
64|$2.596946\times 10^{-7}$|$1.509508\times 10^{-7}$|41.9\%

The same direction also holds at the split level: across all $3$ random splits and all reported train sizes, the Shadow features consistently achieve lower test MSE than the task-level low-pass inputs.

Thus, in the tested regime, under the same downstream learner and matched feature dimension, the coherence-aware Shadow representation achieves **lower test MSE** than task-level low-pass inputs on this downstream unresolved prediction task. We stress that this is **not** a full ML benchmark, but a concrete hybrid-learning example showing that ShadowFluid can serve as a **compact coherence-aware frontend for downstream learning**.

We thank the reviewer again for the constructive suggestions. These clarifications and additions sharpen the scope of the paper, make the Hamiltonian assumptions more explicit, and better locate the work relative to broader AI4Science and ML directions.