# INGRVM: Neuromorphic Testing Plan

## 🧪 Objectives
To verify the correctness of the new neuromorphic improvements (Sparsity, Lava Backend, Reservoir) across the decentralized mesh.

---

## 🛠️ Test Case 1: Structural Sparsity (The Pruning Test)
- **Goal:** Ensure the `STDPTrainer` correctly zeroes out weak weights.
- **Action:** 
    1. Initialize a set of weights.
    2. Set some weights to very small values (e.g., 0.005).
    3. Run the `update_weights` function.
- **Expected:** Any weight absolute value < 0.01 must be exactly 0.0 in the output.

## 🛠️ Test Case 2: Hardware-Aware Backend (Lava Logic)
- **Goal:** Verify that the `LavaLIFBackend` falls back correctly if Intel Lava is missing.
- **Action:** 
    1. Initialize `LavaLIFBackend` on a standard system.
    2. Run a forward pass with dummy spikes.
- **Expected:** The system should print "⚠️ STANDARD HARDWARE" and proceed with the PyTorch/Mock simulation without crashing.

## 🛠️ Test Case 3: Liquid State Stability (Reservoir Test)
- **Goal:** Verify that the reservoir creates unique internal states for different temporal inputs.
- **Action:** 
    1. Input Sequence A (1, 0, 1) and Sequence B (0, 1, 0).
    2. Compare the resulting internal states.
- **Expected:** Internal states must be numerically different and stable (not exploding to infinity).

---

## 🚀 How to Run (Validation Script)
We have provided a "Logic-Only" test script that runs even if high-end ML libraries are missing (using mocks).

**Command:**
`python INGRVM/Core/tests/neuromorphic_audit.py`
