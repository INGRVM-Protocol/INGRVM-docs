# INGRVM: SpikeLLM Architecture (Native 1-bit SNN)
**Objective:** Design a Large Language Model architecture natively optimized for sharded, neuromorphic mesh execution.

## 1. The Core Philosophy
Traditional LLMs (Llama, GPT) are built for GPUs. They use dense floating-point math. **SpikeLLM** is built for the INGRVM Mesh. It uses **Temporal Sparsity**—information only exists when a spike occurs.

### Key Advantages:
- **Energy Efficiency:** Neurons only consume "Joules" (DOPA) when they fire.
- **Latency:** Binary spikes (1-bit) travel 32x faster across WAN than 32-bit floats.
- **Bio-Mimetic:** Mirrors the human brain's asynchronous processing.

## 2. Structural Components

### A. Temporal Spike Encoder
Converts text embeddings into a time-series of spikes. 
- High-value tokens generate spikes earlier in the time window.
- Lower-value/filler tokens generate spikes later or not at all.

### B. Spiking Attention (Coincidence Detection)
Instead of Softmax Attention ($QK^T$), SpikeLLM uses **Coincidence Detection**:
- Attention is high if spikes from two different streams arrive at a neuron within the same temporal window (e.g., <5ms).
- This replaces heavy matrix multiplication with simple integer increments and threshold checks.

### C. Sharded Recurrent Blocks
Each INGRVM node hosts a set of **LIF (Leaky Integrate-and-Fire)** blocks.
- Spikes enter the shard, build up potential, and once the `vth` (threshold) is reached, a new spike is emitted to the next node in the mesh.

## 3. Learning: Local STDP (Spike-Timing-Dependent Plasticity)
Phase 12 introduces **Local Fine-Tuning** without a central server.
- If a spike from Node A causes Node B to fire immediately, the connection weight is strengthened.
- If the spike arrives too late, the weight is weakened.
- This allows the model to "learn" the specific language patterns of the user locally on their Mobile Edge node.

## 4. Deployment Pipeline (Phase 12 Task #8)
1.  **Drafting:** (Current Task) Define the mathematical constraints.
2.  **Simulation:** Build a 3-layer SpikeLLM prototype in `snntorch`.
3.  **Sharding:** Use the `INGRVM-Adapter` to distribute the first 100M parameter SpikeLLM across the mesh.
