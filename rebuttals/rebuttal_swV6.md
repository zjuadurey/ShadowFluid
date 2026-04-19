# Response to Reviewer swV6

We thank the reviewer for the constructive feedback. We agree that the submission should have been clearer on scope, approximate closure, error decomposition, proof presentation, and several experimental details. We address these points below.

## 1. On fit to KDD/data science

We agree that the current paper is not a learned-model or data-mining paper in the narrow sense. Our main contribution remains an **error-certified reduced simulation framework** for scientific dynamical systems.

To make the connection to downstream AI4Science workflows more concrete, we added a **small downstream-learning probe** in the single-cosine regime. Using the same learner for both feature families---PCA to $16$ dimensions followed by Ridge regression---we compared **coherence-aware Shadow features** against **task-level low-pass density inputs** for predicting the next-step unresolved high-frequency energy $E_{\mathrm{HF}}(t+\Delta t)$, which is **not** directly available from the task truncation:

Train size|Low-pass density MSE|Shadow coherence $Z$ MSE|Relative gain of Shadow
|---|---:|---:|---:|
16|$2.369252\times 10^{-7}$|$2.015528\times 10^{-7}$|14.9\%
32|$2.316244\times 10^{-7}$|$1.676996\times 10^{-7}$|27.6\%
64|$2.596946\times 10^{-7}$|$1.509508\times 10^{-7}$|41.9\%

Thus, in the regime studied, under the same downstream learner and matched feature dimension, the coherence-aware Shadow representation achieves **lower test MSE** than task-level low-pass inputs on a downstream unresolved scalar quantity. We stress that this is **not** a full ML benchmark, but it does provide a concrete hybrid-learning example showing that ShadowFluid can serve as a **compact coherence-aware frontend for downstream learning**.

## 2. On the positioning of approximate closure

We agree that this point should have been positioned more carefully. Our setting is narrower than BBGKY-type, Gaussian, or kinetic moment closure. What we study is **operator-subspace closure induced by Hamiltonian commutators**: exact closure means the commutator stays in the dictionary span; approximate closure means projecting/truncating commutator dynamics onto a task-selected operator subspace and quantifying the residual leakage outside that subspace. We will revise the related-work and method sections to make this distinction explicit.

## 3. On separating low-pass truncation error from shadow-approximation error

We agree that this separation should have been shown more explicitly. Our interpretation compares, under the same cutoff $K_0$, (i) the full-state solution, (ii) the exact full low-pass solution, and (iii) the ShadowFluid approximation. The existing $V=0$ sanity check already isolates these effects cleanly: ShadowFluid matches the full low-pass baseline up to machine precision, so the total density error is entirely explained by the spectral truncation induced by the cutoff.

To make this interpretation explicit in the $V \neq 0$ regime, we added a direct decomposition experiment. Here, `eps_cutoff` measures the normalized gap between the full low-pass baseline and the full solution, `eps_shadow` measures the normalized gap between the ShadowFluid result and the full low-pass baseline, and `eps_total` measures the normalized gap between the ShadowFluid result and the full solution.

$\alpha$|$K_0$|$\varepsilon_{\mathrm{cutoff}}$|$\varepsilon_{\mathrm{shadow}}$|$\varepsilon_{\mathrm{total}}$|shadow fraction
|---|---:|---:|---:|---:|---:|
0.2|8|0.006038|0.000079|0.006038|0.0131
0.5|6|0.020021|0.000886|0.020004|0.0443
1.0|4|0.055400|0.010831|0.054553|0.1985

Across all $V \neq 0$ single-cosine settings, the **dominant source of density error is the task cutoff $K_0$**, while the additional shadow approximation error remains secondary. Even in the strongest tested case $(\alpha=1, K_0=4)$, the shadow contribution stays below $20\%$ of the total density error.

## 4. On the proof sketch and experimental reporting

We agree that the theoretical presentation was too compressed. The core argument is that the error dynamics can be written in variation-of-constants form, and unitary conjugation preserves the Frobenius norm; this is why the resulting certificate grows linearly in time rather than exponentially. We will unpack this logic more explicitly and clarify how the leakage term leads to the fully computable bound used in the experiments.

We will also state the numerical precision used in the experiments (`complex128` for state evolution and `float64` for density-level quantities), make the benchmark potential more explicit (for the standard case, $V(x)=\alpha \cos(q\cdot x)$ with $q=(1,0)$), and present the role of the full low-pass baseline more directly.

We thank the reviewer again for the constructive feedback. These clarifications improve the precision of the presentation, better ground the notion of approximate closure, and make the error decomposition more explicit.