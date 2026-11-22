# BB84 with partial intercept-resend
A Python and Qiskit simulation of the **BB84 Quantum Key Distribution protocol** evaluating security under partial and full intercept-resend eavesdropping attacks.

## Setup
    !pip install -U qiskit qiskit-aer qiskit-algorithms matplotlib pylatexenc
Tested with Python 3.12, qiskit 2.6.2, qiskit-aer 0.18.4.

## Overview
This project models quantum state preparation, basis measurement, sifting, and Quantum Bit Error Rate (QBER) estimation using Qiskit Aer. By splitting each transmission into two sequential quantum circuits (Alice to Eve and Eve to Bob) the simulation captures quantum state collapse and re-preparation across varying interception probabilities f in [0, 1].

### Key Theoretical Results Verified
* **Linear Disturbance Scaling:** When Eve intercepts a fraction f of transmitted qubits and measures in a random basis (Z or X), the expected error rate in the sifted key follows:
  `QBER = f/4`
* **Security Threshold Detection:** Using the standard BB84 security threshold of  0.11 (11%), the critical interception fraction where eavesdropping begins to trigger key aborts on average is:
  `f = 4 * 0.11 = 0.44`

---

## Simulation Architecture
1. **State Preparation (`prepare_state`):** Encodes classical bits (`0`, `1`) into rectilinear (Z: |0⟩, |1⟩) or diagonal (X: |+⟩, |-⟩) basis states using X and H gates.
2. **Circuit 1 (`qc1` - Alice to Eve):** Alice prepares n qubits; Eve intercepts each qubit independently with probability f and measures in a randomly selected basis.
3. **Circuit 2 (`qc2` - Eve to Bob):** Eve re-prepares intercepted qubits according to her measurement outcomes while unattacked qubits pass undisturbed; Bob measures all n qubits in his random bases.
4. **Sifting & Parameter Estimation:** Alice and Bob retain positions with matching bases, sample k bits to compute QBER, and **accept** (QBER < 0.11) or **abort** (QBER > 0.11) the key.

---


