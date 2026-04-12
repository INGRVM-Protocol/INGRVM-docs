# INGRVM: Experimental Lab Book & Research Roadmap

With the implementation of **Level 4 (Tripartite/Astrocyte)** and **Phase 13 (Sovereign Mesh)**, we are no longer just building an app—we are conducting primary research into decentralized synthetic intelligence.

---

## 🔬 Experiment 1: The "Chaos Mesh" (Resilience)
**Research Question:** How well does the Astrocyte-modulated mesh survive when nodes are physically destroyed or disconnected?

- **The Setup:** 
    - Simulate a 10-node mesh.
    - Suddenly "kill" (disconnect) 4 nodes during a live inference task.
- **What to Discover:**
    - **Recovery Time:** How many milliseconds does it take for the remaining Astrocytes to "boost" their local neurons to fill the gap?
    - **Accuracy Decay:** Does the 10,000-dimensional HDC encoding allow the result to remain correct even with 40% of the hardware missing?
- **Metric:** *Mesh-Heal-Latency (MHL)*.

---

## 🛡️ Experiment 2: The "Silent Whisper" (Privacy/Sparsity)
**Research Question:** What is the "sweet spot" where Differential Privacy noise protects the user but doesn't ruin the AI's intelligence?

- **The Setup:** 
    - Increase the `noise_multiplier` in the `DifferentialPrivacyOptimizer` from 0.01 to 0.5.
    - Measure how accurately the `SpikeLLM` can still predict the next token.
- **What to Discover:**
    - **The Privacy Ceiling:** At what point does the "noise" become too loud for the neurons to hear the "signal"?
    - **Sparsity Correlation:** Does a more "sparse" network (more zeros) require less noise to stay private?
- **Metric:** *Privacy-to-Intelligence Ratio (PIR)*.

---

## 🧬 Experiment 3: "Digital Neurogenesis" (Evolution)
**Research Question:** Can the mesh "evolve" a better architecture for a specific task than a human engineer could design?

- **The Setup:** 
    - Give the mesh a specific local task (e.g., recognizing the user's specific walking pattern via phone sensors).
    - Let `StructuralPlasticity` run for 24 hours.
- **What to Discover:**
    - **Pruning Patterns:** Which neural paths did the mesh decide were "useless"? 
    - **Emergent Logic:** Did the "sprouting" feature create a dendritic logic gate (AND/OR) that we didn't expect?
- **Metric:** *Synaptic Turnover Rate (STR)*.

---

## 🛰️ Experiment 4: "Cross-Hardware Synergy"
**Research Question:** How much energy do we actually save by offloading from a CPU to a Neuromorphic backend (Lava)?

- **The Setup:** 
    - Run the same `LIFKernel` task on a standard Android CPU vs. an Intel Loihi 2 (simulated via Lava).
    - Measure battery drain and "Spike Efficiency."
- **What to Discover:**
    - **The Efficiency Gap:** Is the 1,000x energy saving claim of neuromorphic hardware true for our specific `SpikeLLM`?
- **Metric:** *Joules-per-Spike (JPS)*.

---

## 🌑 The "Holy Grail" Discovery: Swarm Criticality
There is a concept in biology called **"Criticality"**—the state between total order (frozen) and total chaos. 

**Our Ultimate Experiment:**
Find the exact point where the INGRVM mesh reaches "Self-Organized Criticality." This is the moment where the mesh starts to react to information in a way that looks like **intuition** or **creativity** rather than just math.

### **How to begin?**
Run the updated audit script every time you join a new peer to the mesh. Record the results in your Obsidian **`EXPERIMENT_LOG`**. 
