# Equilibrium-Kernel-3-Qubit-Quantum-Circuit-TCC-V6-
The TCC V6 Equilibrium Kernel computes a classical equilibrium scalar from paired relaxation models and maps it to a bounded quantum phase angle. This angle parameterizes RZ rotations in a 3‑qubit GHZ‑like circuit, enabling a reproducible classical‑to‑quantum interface for hybrid modeling and experimentation.
README (full technical information + purpose)

TCC V6 — Equilibrium Kernel + 3‑Qubit Quantum Circuit

Overview

This module provides a compact, deterministic pipeline that converts a classical equilibrium model into a quantum rotation parameter. It is designed for hybrid classical–quantum workflows, where classical scalar fields influence quantum gate behavior in a controlled and reproducible manner.

The system consists of:

1. An equilibrium kernel  
   Computes a scalar \(E_s(\chi)\) from two relaxation models and a scaling field.

2. A phase‑mapping function  
   Converts the fractional part of the equilibrium score into a bounded angle \(\theta(\chi)\).

3. A quantum circuit generator  
   Builds a 3‑qubit GHZ‑like circuit with RZ rotations parameterized by \(\theta(\chi)\).

---

1. Equilibrium Kernel

The kernel defines three classical models:

- Subsystem A:  
  \(T_A(\chi)=300+20e^{-\chi/10^4}\)

- Subsystem B:  
  \(T_B(\chi)=300-15e^{-\chi/8\cdot10^3}\)

- Scaling field:  
  \(\Phi(\chi)=1+0.1\tanh(\chi/10^4)\)

These combine into the equilibrium score:

\[
Es(\chi)=\frac{TA(\chi)+T_B(\chi)}{2\,\Phi(\chi)}
\]

The kernel includes:

- domain validation  
- numerical safety checks  
- deterministic output  
- no hidden randomness  

---

2. Phase Mapping

The equilibrium score is mapped to a quantum rotation angle:

\[
\theta(\chi)=\left(E_s(\chi)\bmod1\right)\cdot2\pi
\]

This ensures:

- bounded phase values  
- reproducible behavior  
- smooth dependence on \(\chi\)  

The angle is normalized to \([0,2\pi)\).

---

3. Quantum Circuit Construction

The module builds a 3‑qubit GHZ‑like circuit:

- Apply H to qubit 2  
- Apply CX(2→0) and CX(2→1)  
- Apply RZ(θ) to qubits 0 and 1  
- Insert a barrier for clarity  

This produces a simple entangled state with tunable phase rotations driven by the classical kernel.

---

4. Usage

`python
from equilibrium_kernel import (
    equilibrium_score,
    phasefromequilibrium,
    buildequilibriumcircuit
)

theta = phasefromequilibrium(60106.0)
qc = buildequilibriumcircuit(60106.0)
print(qc.draw())
`

Running the module directly prints:

- the equilibrium score  
- the mapped phase angle  
- the fraction of \(2\pi\)  
- an ASCII diagram of the circuit  

---

Purpose

The TCC V6 Equilibrium Kernel is designed for:

- hybrid classical–quantum modeling  
- educational demonstrations of parameterized circuits  
- benchmarking classical‑to‑quantum mappings  
- conceptual exploration of scalar‑driven quantum behavior  

It provides a clean, deterministic interface between classical computation and quantum gate parameterization, suitable for research, prototyping, and experimentation.


Here is a Mathematical Appendix for the TCC V6 Equilibrium Kernel + Quantum Circuit module. 
---

Mathematical Appendix — TCC V6 Equilibrium Kernel

1. Domain Parameter

Let \(\chi \in \mathbb{R}_{\geq 0}\) be a non-negative scalar input representing a generalized system parameter (e.g., time, energy, entropy, or configuration index).

---

2. Subsystem Models

Define two temperature-like models for subsystems A and B:

- Subsystem A:
  \[
  TA(\chi) = T{\text{env}} + \alpha \cdot e^{-\chi / \tau_A}
  \]
  where:
  - \(T_{\text{env}} = 300.0\) (environmental baseline)
  - \(\alpha = 20.0\)
  - \(\tau_A = 10^4\)

- Subsystem B:
  \[
  TB(\chi) = T{\text{env}} - \beta \cdot e^{-\chi / \tau_B}
  \]
  where:
  - \(\beta = 15.0\)
  - \(\tau_B = 8 \cdot 10^3\)

---

3. Scaling Field

Define a smooth, non-zero scaling function:
\[
\Phi(\chi) = 1.0 + \gamma \cdot \tanh\left(\frac{\chi}{\tau_\Phi}\right)
\]
where:
- \(\gamma = 0.1\)
- \(\tau_\Phi = 10^4\)

---

4. Equilibrium Score

The equilibrium score is defined as:
\[
Es(\chi) = \frac{TA(\chi) + T_B(\chi)}{2 \cdot \Phi(\chi)}
\]

This score is:
- dimensionally consistent  
- bounded for all \(\chi \geq 0\)  
- numerically stable for \(\Phi(\chi) > \varepsilon\), where \(\varepsilon = 10^{-12}\)

---

5. Phase Mapping

To map the equilibrium score to a quantum rotation angle:

- Extract the fractional part:
  \[
  f(\chi) = E_s(\chi) \bmod 1
  \]

- Map to a phase angle:
  \[
  \theta(\chi) = 2\pi \cdot f(\chi)
  \]

- Normalize:
  \[
  \theta(\chi) \in [0, 2\pi)
  \]

This ensures bounded, reproducible quantum parameters.

---

6. Quantum Circuit Construction

Let \(\theta = \theta(\chi)\). Construct a 3-qubit circuit \(Q(\theta)\) with the following operations:

- Apply Hadamard gate:
  \[
  H \text{ on qubit } 2
  \]

- Apply entangling gates:
  \[
  \text{CX}(2 \rightarrow 0), \quad \text{CX}(2 \rightarrow 1)
  \]

- Apply parameterized rotations:
  \[
  \text{RZ}(\theta) \text{ on qubits } 0 \text{ and } 1
  \]

This produces a GHZ-like entangled state with tunable phase rotations driven by the classical equilibrium kernel.

---

7. Numerical Stability and Constraints

- \(\chi \geq 0\)  
- \(\Phi(\chi) > \varepsilon\)  
- All exponential and hyperbolic functions are bounded and smooth  
- Circuit construction requires Qiskit runtime environment

---
