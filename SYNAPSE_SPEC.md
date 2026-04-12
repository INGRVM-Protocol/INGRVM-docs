# INGRVM Synapse Spec v1.0: Spiking Attention Protocol
**Objective:** Define the communication standard for native 1-bit neuromorphic LLMs operating across a decentralized mesh.

## 1. The Synapse Primitive
Unlike standard LLMs that pass 32-bit floating point activations, **INGRVM Synapse** passes **Binary Spikes (1-bit)** with a **Temporal Offset**.

### Packet Structure:
```json
{
  "source_node": "PEER_ID",
  "synapse_id": "SYN_01",
  "spikes": "011010...", 
  "timing_ref": 1710105600.420,
  "threshold": 0.85
}
```

## 2. Spiking Attention (Coincidence Detection)
Instead of calculating $Softmax(QK^T)$, INGRVM nodes perform **Coincidence Detection**:
- **Mechanism:** A neuron fires only if a sufficient number of spikes from different shards arrive within a 5ms window.
- **Efficiency:** This reduces attention complexity from $O(N^2)$ matrix math to $O(N)$ integer additions.

## 3. Asynchronous Backpressure
Since nodes may have different NPUs (Pixel 8 vs Desktop), the protocol uses **Dynamic Refractory Periods**:
- If a node is overloaded, it increases its `refractory_period` signal.
- The upstream node automatically buffers spikes or reroutes to a faster peer (Task #5 multi-hop).

## 4. Federated Learning (STDP)
Weight updates are handled locally via **Spike-Timing-Dependent Plasticity (STDP)**:
- **Causal:** If Pre-synaptic spike arrives *before* Post-synaptic fire -> Strengthen Connection (+DOPA).
- **Acausal:** If Pre-synaptic spike arrives *after* Post-synaptic fire -> Weaken Connection (-DOPA).

## 5. Security (ZK-Synapse)
Every Synapse packet must include a **Poseidon Commitment** of the local shard weights. 
- This prevents a malicious node from "poisoning" the LLM with fraudulent spikes.
- Commitments are audited by the Hub's `CircuitVerifier` (Phase 11 Task #2).
