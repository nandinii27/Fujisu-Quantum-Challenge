# Fujitsu Quantum Challenge 2026 — Team 42 Submission

Distributed quantum algorithms for quantum chemistry, built for the Fujitsu Quantum
Challenge 2026.

## What this is

Modeling the electronic structure of metalloprotein active sites is a hard quantum
chemistry problem: transition-metal centers like iron-sulfur clusters have near-degenerate
d-orbitals, strong electron correlation, and open-shell character that break standard
mean-field methods (DFT, RHF) and require multi-reference treatment (CASSCF, FCI) to get
right. That multi-reference treatment is exactly where classical computation runs out of
room as the active space grows and exactly where quantum phase estimation (QPE) is
expected to win eventually, since it lets you reach FCI-level accuracy without the
exponential wall. The catch is near-term quantum hardware doesn't have enough qubits for
chemically interesting active spaces on its own, so the question we're exploring is whether
splitting a system across multiple QPU nodes and simulating a small distributed quantum
network rather than one monolithic device can get you there sooner.

## What we did

- Built a full QM/MM + PCM embedded model of a [2Fe-2S] ferredoxin active site: protein
  backbone point charges for the ionic environment, implicit solvent for the protein
  interior, ROHF reference, and a CASSCF(6e,6o) active space around the Fe 3d manifold.
- Mapped the active space to a 12-qubit Hamiltonian via Jordan-Wigner, then partitioned it
  across two simulated QPU nodes (one per Fe center) using Foster-Boys orbital localization,
  with Fisher-information-guided ancilla allocation to combine each node's phase estimate.
- Implemented Kitaev iterative QPE and ran it both single-node and distributed, benchmarking
  energy accuracy against classical CASSCF/FCI, Trotter error convergence, and resource
  scaling toward larger systems (P450, FeMo-cofactor).
-  Ran a parallel VQE study on hydrogen lattices
  to compare centralized vs. distributed variational approaches.
-  Compared to QPE for Hydrogen running on the Fujitsu Quantum Simulator as provided by Qulacs (https://dojo.qulacs.org/en/latest/notebooks/7.1_quantum_phase_estimation_detailed.html)


## Status

A few bugs in the main notebook are still being worked through around the final benchmarking setup. 

## Repository structure

| File | Contents |
|---|---|
| `distributed_qpe_for_metalloproteins.ipynb` | Main submission notebook. |
| `distributed_vqe_hydrogen_lattice.ipynb` | VQE on Fermi-Hubbard hydrogen lattices, centralized vs. distributed. |
| `dqsweeptests/dqpe.py` | SquidASM/NetQASM distributed QFT validation test. |
| `Submission_report` | Written submission report as submitted to the challenge. |

