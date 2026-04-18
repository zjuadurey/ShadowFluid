# Response to Reviewer wCX6

We thank the reviewer for the thoughtful feedback and for recognizing the theoretical grounding and technical value of ShadowFluid. We also appreciate the reviewer’s comments on the paper’s positioning, the scope of the current validation and measurement-reduction discussion, and the need to more clearly distinguish ShadowFluid from prior shadow Hamiltonian simulation. We address these points below.

## 1. On the paper’s AI4Science positioning and KDD-track relevance

We agree that the current paper does not introduce a trained AI component, and that this boundary was not stated sharply enough in the submitted version.

Our intended contribution is an **error-certified reduced simulation framework** for scientific dynamical systems. Concretely, ShadowFluid develops a task-driven low-dimensional operator representation and a reduced evolution rule for task-relevant fluid observables, so that one can evolve only the observables needed by downstream objectives rather than the full wavefunction. In this sense, we view the paper as contributing to the broader **scientific modeling and reduced-simulation stack** that can complement downstream AI4Science pipelines, rather than itself being a learned-model paper.

In the revision, we will sharpen this positioning in the introduction and discussion, clarify that the present work contributes an error-certified reduced simulation framework for scientific modeling, and make clear that any integration with neural surrogates or hybrid physics-AI pipelines is future work rather than a demonstrated part of the current paper.

## 2. On classical-only validation and the scope of the measurement-reduction claim

We agree that the current paper does not present a hardware-level demonstration of quantum advantage. Our intended claim is narrower: ShadowFluid replaces full-state evolution with reduced operator dynamics under exact closure, and provides controlled truncation with an *a priori* computable leakage certificate under approximate closure. The discussion of measurement reduction is therefore meant as a deployment motivation in the quantum-native regime, rather than as an experimentally demonstrated end-to-end advantage in this paper.

Beyond the classical matrix-form implementation reported in the manuscript, we also implemented a noiseless Qiskit-based circuit realization and used it as an **implementation-level cross-check** against the classical version. The two agree up to very small numerical discrepancy, providing an implementation-level consistency check. At the same time, this does **not** constitute a hardware-level validation of quantum advantage: the current paper does not study device noise, real-hardware behavior, or end-to-end measurement savings on quantum hardware.

We will revise the abstract, discussion, and limitations sections to state this scope more explicitly.

## 3. On novelty beyond prior shadow Hamiltonian simulation

We agree that the distinction from prior shadow Hamiltonian simulation should have been made more explicit.

Our contribution is not simply to apply a general shadow-Hamiltonian idea to a new domain, but to turn it into a task-driven and certifiable methodology for approximate closure in quantum fluid observables. Relative to the prior framework, the present paper contributes:

(i) a task-driven multi-reference coherence dictionary tailored to low-frequency fluid observables in the Fourier basis;

(ii) a coupling-graph-based construction of the reference set, tied to PDE-style Fourier-space Hamiltonians;

(iii) an extension from exact closure to the practically relevant approximate-closure regime; and

(iv) a state-independent, *a priori* computable commutator-leakage quantity, together with empirical validation of the hierarchy between the computable bound, the operator-level discrepancy, and the downstream task error on 2D Schrödinger-flow benchmarks.

In the revision, we will strengthen the related-work and contribution sections to make this distinction much more explicit.

We thank the reviewer again for the constructive feedback. We believe these clarifications will improve the paper by sharpening its scope, making the intended claims more precise, and better distinguishing ShadowFluid from prior shadow Hamiltonian simulation.

# Response to Reviewer oB6r

We thank the reviewer for the thoughtful feedback and for recognizing the motivation of the problem, the elegance of the operator-first framework, the value of the error analysis, and the clarity of the related-work discussion. We also appreciate the reviewer’s comments on the scope of the dynamics studied, the simplicity of the benchmark Hamiltonians, and the paper’s positioning relative to KDD/ML. We address these points below.

## 1. On the current scope being limited to linear Schrödinger dynamics

We agree that the current paper is restricted to **linear Schrödinger-type dynamics** and does not claim to solve general nonlinear CFD or Navier-Stokes dynamics. Our goal is narrower: to establish an error-certified reduced simulation framework in a setting where closure structure and truncation behavior can be analyzed rigorously.

We will sharpen this boundary in the introduction, discussion, and limitations sections, and clarify that the present formulation is most naturally suited to regimes with a Hamiltonian representation and analyzable commutator structure. Nonlinear extensions remain future work.

## 2. On the use of Fourier-sparse cosine potentials and the generality of the coupling-graph construction

We agree that the original experiments should do more to show what happens as the Hamiltonian becomes less structured. To address this point, we added a **structured multi-component stress test** that progressively densifies the coupling graph.

In the fixed-total-coupling setting ($\alpha_{\mathrm{tot}} = 0.5$, $K_0 = 5$, $t = 0.5$, $R_{\mathrm{hops}} = 1$), increasing the number of Fourier components from $J=1$ to $J=8$ enlarges the reference set from $|R|=3$ to $17$ and the reduced size from $243$ to $1377$, while the density error remains near $3 \times 10^{-2}$ and $\Delta Z_F$ stays in the $10^{-3}$ to $10^{-2}$ range.

Fourier components $J$|R size|Reduced size|Density error|$\Delta Z_F$|Leakage $\ell_{\mathrm{rms}}$
|---|---:|---:|---:|---:|---:|
1|3|243|0.029049|0.007733|0.242161
2|5|405|0.028939|0.024563|0.214447
4|9|729|0.030072|0.001924|0.137493
6|13|1053|0.030358|0.006105|0.116444
8|17|1377|0.029789|0.003565|0.101099

We also quantified the closure-depth tradeoff on the densest structured case ($J = 8$). Increasing the closure depth from $1$ to $3$ grows the reduced size from $1377$ to $9315$, while reducing $\Delta Z_F$ from $3.57 \times 10^{-3}$ to $4.78 \times 10^{-4}$ and lowering $\ell_{\mathrm{rms}}$ from $1.01 \times 10^{-1}$ to $7.77 \times 10^{-2}$.

Closure depth $R_{\mathrm{hops}}$|R size|Reduced size|Density error|$\Delta Z_F$|Leakage $\ell_{\mathrm{rms}}$
|---|---:|---:|---:|---:|---:|
1|17|1377|0.029789|0.003565|0.101099
2|55|4455|0.029789|0.002468|0.085052
3|115|9315|0.029789|0.000478|0.077749

These added results clarify the regime boundary: denser couplings do shrink the dimensionality advantage by enlarging $R$, but do not automatically destroy approximation quality in the tested regime. We will incorporate this analysis into the revision and state more clearly that the current construction is validated for structured Fourier-coupled regimes rather than claimed to cover fully broadband or non-periodic Hamiltonians.

## 3. On KDD/ML relevance and the relation to learning-based AI4Science pipelines

We agree that the current paper is not a learned-surrogate or data-mining paper. Our intended contribution is instead an **error-certified reduced simulation framework** for scientific modeling: ShadowFluid provides a task-driven low-dimensional operator representation, a reduced evolution rule, and a **state-independent worst-case certificate**.

In this sense, we view the paper as contributing to the broader **scientific modeling and reduced-simulation stack** that can complement downstream AI4Science pipelines, rather than itself being a trained AI model. We will clarify this positioning more carefully in the revision and state explicitly that any direct hybridization with neural surrogates or learned ROM baselines remains future work.

We thank the reviewer again for the constructive suggestions. We believe these clarifications and added stress tests improve the paper by sharpening its scope, making the Hamiltonian assumptions more explicit, and better locating the work relative to broader AI4Science and ML directions.

# Response to Reviewer swV6

We thank the reviewer for the careful and constructive feedback. We agree that the submission should have been clearer on scope, approximate closure, error decomposition, proof presentation, and several experimental details. We address these points below.

## 1. On fit to KDD/data science

We agree that the current paper is not a learned-model or data-mining paper in the narrow sense. Our intended contribution is an **error-certified reduced simulation framework** for scientific dynamical systems: ShadowFluid provides a task-driven reduced representation, a reduced evolution rule, and a **state-independent worst-case certificate**. In the revision, we will clarify that the paper contributes to the broader **scientific modeling and reduced-simulation stack**, and that any integration with neural surrogates or hybrid physics-AI pipelines is future work.

## 2. On the positioning of approximate closure

We agree that this point should have been positioned more carefully. Our setting is narrower than BBGKY-type, Gaussian, or kinetic moment closure. What we study is **operator-subspace closure induced by Hamiltonian commutators**: exact closure means the commutator stays in the dictionary span; approximate closure means projecting/truncating commutator dynamics onto a task-selected operator subspace and quantifying the residual leakage outside that subspace. We will revise the related-work and method sections to make this distinction explicit.

## 3. On separating low-pass truncation error from shadow-approximation error

We agree that this separation should have been shown more explicitly. Our interpretation compares, under the same cutoff $K_0$, (i) the full-state solution, (ii) the exact full low-pass solution, and (iii) the ShadowFluid approximation.

The existing $V=0$ sanity check already isolates these effects cleanly: ShadowFluid matches the full low-pass baseline up to machine precision, so the total density error is entirely explained by the spectral truncation induced by the cutoff.

To make the same interpretation explicit in the $V \neq 0$ regime, we added a direct decomposition experiment using the standard single-cosine benchmark. We report:
- $\varepsilon_{\mathrm{cutoff}} = \mathrm{norm2}(\rho_{\mathrm{full,LP}}-\rho_{\mathrm{full}}) / \mathrm{norm2}(\rho_{\mathrm{full}})$
- $\varepsilon_{\mathrm{shadow}} = \mathrm{norm2}(\rho_{\mathrm{shadow}}-\rho_{\mathrm{full,LP}}) / \mathrm{norm2}(\rho_{\mathrm{full}})$
- $\varepsilon_{\mathrm{total}} = \mathrm{norm2}(\rho_{\mathrm{shadow}}-\rho_{\mathrm{full}}) / \mathrm{norm2}(\rho_{\mathrm{full}})$

$\alpha$|$K_0$|$\varepsilon_{\mathrm{cutoff}}$|$\varepsilon_{\mathrm{shadow}}$|$\varepsilon_{\mathrm{total}}$|shadow fraction
|---|---:|---:|---:|---:|---:|
0.2|8|0.006038|0.000079|0.006038|0.0131
0.5|6|0.020021|0.000886|0.020004|0.0443
1.0|4|0.055400|0.010831|0.054553|0.1985

Across all tested $V \neq 0$ single-cosine settings, the **dominant source of density error is the task cutoff $K_0$**, while the additional shadow approximation error remains secondary. Even in the strongest tested case $(\alpha=1, K_0=4)$, the shadow contribution stays below $20\%$ of the total density error; at more moderate cutoffs it drops to the single-digit-percent regime. We will incorporate this decomposition explicitly into the revision so that the role of the full low-pass baseline is directly shown rather than left implicit.

## 4. On the proof sketch and experimental reporting

We agree that the theoretical presentation was too compressed. The core argument is that the error dynamics can be written in variation-of-constants form, and the use of unitary conjugation preserves the Frobenius norm, which is why the resulting certificate grows linearly in time rather than exponentially. We agree that this logic should have been unpacked more explicitly in the submission. In particular, we will clarify more explicitly how the leakage term leads to the fully computable bound used in the experiments.

We also agree that several reporting details should be stated more explicitly. We will state the numerical precision used in the experiments (`complex128` for state evolution and `float64` for density-level quantities), make the benchmark potential more explicit (for the standard case, $V(x)=\alpha \cos(q\cdot x)$ with $q=(1,0)$), and present the role of the full low-pass baseline more directly.

We thank the reviewer again for the constructive feedback. We believe these clarifications will improve the paper by making the scope more precise, the notion of approximate closure better grounded, the error decomposition more explicit, and the presentation more transparent.

# Response to Reviewer gBE6

We thank the reviewer for the thoughtful feedback and for recognizing the principled error theory and task-driven dimension reduction perspective of ShadowFluid. We address the main concerns below.

## 1. On the narrow Hamiltonian class and regime boundary

We agree that the submitted version mainly studies Fourier-sparse cosine potentials, and that denser couplings can enlarge the reference set and reduce the dimensionality advantage. To make this regime boundary more explicit, we added a **structured multi-component stress test** that progressively densifies the coupling graph.

In the fixed-total-coupling setting ($\alpha_{\mathrm{tot}}=0.5$, $K_0=5$, $t=0.5$, $R_{\mathrm{hops}}=1$), increasing the number of Fourier components from $J=1$ to $J=8$ enlarges the reference set from $|R|=3$ to $17$ and the reduced size from $243$ to $1377$, while keeping the density error near $3\times10^{-2}$ and $\Delta Z_F$ in the $10^{-3}$ to $10^{-2}$ range. On the densest structured case ($J=8$), increasing the closure depth from $R_{\mathrm{hops}}=1$ to $3$ grows the reduced size from $1377$ to $9315$, while reducing $\Delta Z_F$ from $3.57\times10^{-3}$ to $4.78\times10^{-4}$ and lowering $\ell_{\mathrm{rms}}$ from $1.01\times10^{-1}$ to $7.77\times10^{-2}$.

These added results clarify the **regime boundary** of the current construction rather than claiming full generality over broadband or non-periodic Hamiltonians. In the revision, we will state this boundary more explicitly: the current coupling-graph construction remains effective in structured Fourier-coupled regimes, but its cost scales upward as the Hamiltonian becomes less sparse.

## 2. On the current scope: linear dynamics and controlled validation

We agree that the present paper is restricted to **linear Schrödinger-type dynamics** and does not address nonlinear PDEs such as Navier-Stokes. We will sharpen this boundary in the introduction, discussion, and limitations sections.

We also agree that the current paper does not experimentally demonstrate hardware-level quantum advantage. Our intended claim is narrower: the paper establishes an **error-certified reduced simulation framework** together with controlled validation of its approximation behavior. Beyond the classical matrix-form experiments in the manuscript, we also implemented the same reduced dynamics as a Qiskit-based quantum-circuit realization and used it as an implementation-level cross-check in a noiseless setting against the classical version. This does **not** constitute a hardware-level validation of quantum advantage; the current paper does not study device noise, real-hardware behavior, or end-to-end measurement savings on quantum hardware.

## 3. On robustness and future extensions

We agree that the submitted version relied too heavily on a single initial-condition family. To address this, we added two additional initial-condition families under the same standard single-cosine benchmark setting ($\alpha=0.5$, $q=(1,0)$, $t=0.5$, $R_{\mathrm{hops}}=1$): a deterministic multi-scale packet and a random band-limited state with fixed seed.

Across all tested initial-condition families, the same qualitative hierarchy is preserved: the **dominant source of density error is the task cutoff $K_0$**, while the additional shadow approximation error remains secondary. In particular, the ratio $\varepsilon_{\mathrm{shadow}}/\varepsilon_{\mathrm{total}}$ stays in the $2\%$-$5\%$ range across all six tested settings. This shows that the qualitative error hierarchy is not specific to the Gaussian-vortex initialization used in the original submission. Even for the random band-limited state, whose absolute density error is much larger because substantial spectral mass lies outside the chosen cutoff, the extra shadow contribution remains secondary relative to the cutoff-induced error.

We also agree that adaptive dictionary construction, tighter state-aware estimators, explicit physicality management of the reduced object, and broader comparisons to learned ROM baselines are important next-step extensions. In the revision, we will reflect these directions more explicitly in the limitations and future-work discussion, and clarify that the current leakage-based bound is a **state-independent worst-case certificate**, the present dictionary construction is structural rather than adaptive, and the reduced block is used as a task-level carrier rather than a standalone physical density operator.

We thank the reviewer again for the careful and constructive feedback. We believe these clarifications and additions will improve the paper by making the current regime boundary more explicit, sharpening the scope of the claims, and more clearly separating the demonstrated contributions of ShadowFluid from the broader set of valuable future extensions suggested by the reviewer.