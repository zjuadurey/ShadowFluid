# Response to Reviewer wCX6

We thank the reviewer for the thoughtful feedback and for the helpful comments on the paper’s positioning, the scope of the current validation and measurement-reduction discussion, and the need to distinguish ShadowFluid more clearly from prior shadow Hamiltonian simulation. We address these points below.

## 1. On the paper’s AI4Science positioning and KDD-track relevance

We agree that the submitted version did not include a trained AI component, and that this boundary was not stated sharply enough. Our main contribution remains an **error-certified reduced simulation framework** for scientific dynamical systems, rather than a learned surrogate in the narrow sense.

To make the AI4Science connection more concrete, we added a **small downstream-learning probe** in the tested single-cosine regime. In this probe, we use the same learner for both feature families---PCA to $16$ dimensions followed by Ridge regression---and compare **coherence-aware Shadow features** against **task-level low-pass density inputs** for predicting the next-step unresolved high-frequency energy $E_{\mathrm{HF}}(t+\Delta t)$, which is **not** directly available from the task truncation:

Train size|Low-pass density MSE|Shadow coherence $Z$ MSE|Relative gain of Shadow
|---|---:|---:|---:|
16|$2.369252\times 10^{-7}$|$2.015528\times 10^{-7}$|14.9\%
32|$2.316244\times 10^{-7}$|$1.676996\times 10^{-7}$|27.6\%
64|$2.596946\times 10^{-7}$|$1.509508\times 10^{-7}$|41.9\%

Thus, in the tested regime, under the same downstream learner and matched feature dimension, the coherence-aware Shadow representation achieves **lower test MSE** than task-level low-pass inputs on a downstream unresolved scalar quantity. We emphasize that this is **not** a full AI benchmark; rather, it is a concrete hybrid-learning example showing that ShadowFluid can serve as a **compact coherence-aware frontend for downstream learning**.

In the revision, we will sharpen this positioning in the introduction and discussion: the main contribution remains an error-certified reduced simulation framework, while the added probe provides a concrete AI4Science-relevant integration beyond a future-work statement.

## 2. On classical-only validation and the scope of the measurement-reduction claim

We agree that the current paper does not present a hardware-level demonstration of quantum advantage. Our intended claim is narrower: ShadowFluid replaces full-state evolution with reduced operator dynamics under exact closure, and provides controlled truncation with an *a priori* computable leakage certificate under approximate closure. The discussion of measurement reduction is therefore meant as a deployment motivation in the quantum-native regime, rather than as an experimentally demonstrated end-to-end advantage in this paper.

Beyond the classical matrix-form implementation reported in the manuscript, we also implemented a noiseless Qiskit-based circuit realization and used it as an implementation-level cross-check against the classical version. The two agree up to very small numerical discrepancy, providing an implementation-level consistency check. At the same time, this does **not** constitute a hardware-level validation of quantum advantage.

We will revise the abstract, discussion, and limitations sections to state this scope more explicitly.

## 3. On novelty beyond prior shadow Hamiltonian simulation

We agree that the distinction from prior shadow Hamiltonian simulation should have been made more explicit. Our contribution is not simply to apply a general shadow-Hamiltonian idea to a new domain, but to turn it into a task-driven and certifiable methodology for approximate closure in quantum fluid observables. Relative to the prior framework, the present paper contributes:  
(1) a **task-driven multi-reference coherence dictionary** tailored to low-frequency fluid observables in the Fourier basis;  
(2) a **coupling-graph-based construction** of the reference set, tied to PDE-style Fourier-space Hamiltonians;  
(3) an extension from exact closure to the practically relevant **approximate-closure regime**; and  
(4) a **state-independent, \textit{a priori} computable commutator-leakage quantity**, together with empirical validation of the hierarchy between the computable bound, the operator-level discrepancy, and the downstream task error on $2$D Schrödinger-flow benchmarks.

In the revision, we will strengthen the related-work and contribution sections to make this distinction much more explicit.

We thank the reviewer again for the constructive feedback. These clarifications sharpen the intended scope of the paper and better distinguish ShadowFluid from prior shadow Hamiltonian simulation.