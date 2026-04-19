# Response to Reviewer gBE6

We thank the reviewer for the constructive feedback. Our claim is narrower than a general quantum-CFD solver: ShadowFluid is an **error-certified operator-first reduced simulation framework** for structured Schrödinger-type fluid benchmarks.

## 1. Hamiltonian boundary

We agree that the paper focuses on **structured Fourier-coupled Hamiltonians**. Broadband or rough/non-periodic potentials can enlarge the reference set and weaken the reduction advantage. To quantify this boundary, we added a denser structured-potential sweep under
$$
\alpha_{\mathrm{tot}}=0.5,\quad K_0=5,\quad t=0.5,\quad R_{\mathrm{hops}}=1.
$$

| Fourier components $J$ | $\lvert R\rvert$ | Reduced size | Density error | $\lVert \Delta Z\rVert_F$ | Leakage $\ell_{\mathrm{rms}}$ |
|---|---:|---:|---:|---:|---:|
| 1 | 3 | 243 | 0.029049 | 0.007733 | 0.242161 |
| 4 | 9 | 729 | 0.030072 | 0.001924 | 0.137493 |
| 8 | 17 | 1377 | 0.029789 | 0.003565 | 0.101099 |

For $(J=8)$, increasing closure depth from $R_{\mathrm{hops}}=1$ to $3$ enlarges $\lvert R\rvert$ from $17$ to $115$ and improves
$$
\lVert \Delta Z\rVert_F: 3.565\times10^{-3}\rightarrow 4.78\times10^{-4}.
$$
Thus, in the tested structured regime, the reduction advantage degrades gradually rather than collapsing as couplings densify.

## 2. Linear scope

We agree that the paper is restricted to **linear Schrödinger-type dynamics** and does not solve nonlinear PDEs such as Navier-Stokes. This is not a trivial next step but a recognized open difficulty. Meng et al. [6] already study vortex dynamics through a **linear** two-component Schrödinger formulation and note that more general body forces, especially Newtonian-fluid dissipation, lead back to nonlinear equations whose efficient quantum treatment remains open. Prior work on nonlinear differential equations achieves efficiency only under strong structural conditions [PNAS21Liu], while recent theory shows that quantum differential-equation solvers work best in regimes closest to genuine quantum dynamics [CMP25An]. Our claim is therefore narrower: ShadowFluid provides an error-certified reduced simulation framework in a controlled linear regime.

## 3. Classical validation and implementation benefit

We agree that the demonstrated benefit should be stated precisely. The present experiments validate the **error-control theory** in a classically simulable setting, rather than claiming hardware-level quantum advantage. However, we also implemented the reduced targets as quantum circuits and observed **compilation-level gains**:

| Case | Full target|Reduced target|Qubits |2Q gate count | 2Q depth |
|-|-|-|-:|-:|-:|
| State preparation | 128 amplitudes | 18 retained amplitudes packed into 32 | $7\rightarrow5$ | $268\rightarrow55$ | $260\rightarrow55$ |
| $V=0$ time evolution | $64\times64$ active-mode unitary | $9\times9$ reduced unitary padded to $16\times16$ | $6\rightarrow4$ | $174\rightarrow29$ | $164\rightarrow28$ |

Thus, while we do not claim a full hardware demonstration, the operator-first reduction already yields smaller compiled circuits.

## 4. Robustness across initial families

We added two initial-condition families under the same single-cosine benchmark $(\alpha=0.5, q=(1,0), t=0.5, R_{\mathrm{hops}}=1)$: a deterministic multi-scale packet and a random band-limited state. Across all tested families, the same decomposition remains stable: the dominant source of density error is still the task cutoff $K_0$, while the additional shadow approximation error remains secondary.

| Initial condition | $K_0$ | $\varepsilon_{\mathrm{cutoff}}$ | $\varepsilon_{\mathrm{shadow}}$ | $\varepsilon_{\mathrm{shadow}}/\varepsilon_{\mathrm{cutoff}}$ |
|---|---:|---:|---:|---:|
| Gaussian vortex | 6 | 0.020021 | 0.000886 | 0.0443 |
| Multi-scale packet | 6 | 0.026353 | 0.000832 | 0.0315 |
| Random band-limited state | 6 | 0.594620 | 0.020003 | 0.0337 |

The large cutoff error for the random band-limited state reflects poor low-pass approximability, not dominance of the shadow error. At $K_0=8$, the shadow fractions remain small: $0.0338$, $0.0202$, and $0.0493$.

## 5. Bound, physicality, and next steps

We agree that adaptive dictionary construction, tighter state-aware estimators, explicit physicality management, broader Hamiltonian stress tests, and ROM comparisons are important next steps. The current leakage bound is intentionally a **state-independent worst-case certificate**: a valid **pre-simulation quality indicator**, not a sharp predictor. Likewise, the reduced block $Z(t)$ is a **task-level carrier** for target observables, not a standalone autonomous physical density operator. Under this target, the main claim remains unchanged: ShadowFluid provides a principled, error-certified reduction whose usefulness can be assessed before simulation, and whose approximation error remains secondary to the dominant low-pass task truncation in the tested regime.

Thanks again for your constructive comments.