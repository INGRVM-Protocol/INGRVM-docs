# Doc 7: INGRVM Austin Genesis (Hardware & Strategy)

**Date:** February 26, 2026
**Project:** INGRVM (INGRVM Placeholder)
**Status:** Architecture Phase / Tier II Prototype

## 🛡️ The Hardware Hierarchy
We categorize nodes by Neural Density, Power Efficiency, and Uptime Reliability.

### Tier I: The Global Core (The Thalamus)
*   **Hardware:** SpiNNaker2 (SpiNNcloud Systems).
*   **Role:** Heavy-lifting training and cross-mesh orchestration.
*   **2026 Context:** Full production systems are now live at UT San Antonio and Sandia Labs.
*   **Access Strategy:** We utilize the SpiNNcloud FPGA Cloud for remote evaluation of "ingrvms" before sharding them to the local Austin mesh.

### Tier II: The Permanent Backbone (The "Home Neuron")
This is where the revolution starts. To make the network resilient, we need a ladder of affordable hardware options.

| Hardware Option | Cost (Feb 2026) | Role | Why This? |
| :--- | :--- | :--- | :--- |
| **BrainChip Akida™ 2.0 (M.2)** | $499 | Pro Node | Supports On-Chip Learning. Your node can "learn" and adapt to your local environment without cloud pings. |
| **Raspberry Pi AI HAT+ 2 (Hailo-10H)** | $130 | Hybrid Node | 40 TOPS (INT4) + 8GB dedicated RAM. The best entry-level board for hosting LLM shards and Generative AI. |
| **SynSense Speck™ Demo Kit** | $199 | Vision Specialist | Ultra-low-power vision SoC. Processes only change in pixels. Ideal for security/monitoring ingrvms. |
| **Akida™ Pico (uW Core)** | < $50 (Est.) | Always-On Filter | Less than 1mW power profile. Ideal for "Wake-up" ingrvms (listening for specific keywords or anomalies). |

## ⚙️ The OS & Cognitive Architecture

### 1. The Austin "Genesis" Mesh Strategy
In Austin, we aren't building a "global" network yet; we are building a High-Density Localized Lobe.
*   **The Tier II "Home Neuron" Stack:** 
    *   **Host:** Raspberry Pi 5 ($125 for 16GB / $77 for 4GB).
    *   **Accelerator:** Hailo-10H AI HAT+ 2 ($130) or Akida 2.0.
    *   **Connectivity:** Wi-Fi 6 + Bluetooth 5.3 (for p2p gossip when ISPs fail).

### 2. The INGRVM-Core (Rust)
This manages the "ingrvm Shards" and handles the zkML proofs.

### 3. The Cognitive OS: "ingrvm Sharding" & "Spikes"
**A. Event-Driven Communication**
Traditional AI uses massive bandwidth. Your mesh uses Temporal Spike Encoding.
*   **The Spike:** Data is sent only when a "neuron" reaches its firing threshold.
*   **Bandwidth Efficiency:** This reduces the data footprint of an AI query by 90%, allowing the mesh to operate over low-speed mesh radios or even Bluetooth during emergencies.

**B. Neural Sharding (Mixture of Experts)**
A complex ingrvm (e.g., Austin Medical Assistant) is broken into 1,000 shards. 
*   **Parallel Inference:** When a query hits the mesh, the nearest 10 Home Neurons (the "Ensemble") process the shards simultaneously and reach a consensus on the answer.

## 🚀 Reality Check (The Architect's Path)
To keep this real and grounded, let's ignore the "Global Brain" hype for a moment and focus on the **Physics of the Spike**:

*   **Phase 0 (Virtual):** Use the BrainChip Akida FPGA Cloud (which became available this month) to test your SNN models remotely. This costs almost nothing compared to buying the boards and proves the code works on real neuromorphic IP.
*   **Phase 1 (The Hardware Seed):** Acquire a Raspberry Pi 5 and a Hailo-10H HAT. This is the most cost-effective way to get 40 TOPS of power on your desk in Austin today.
*   **Phase 2 (The Handshake):** Once you have the Pi, we'll write the script to send a "Spike" from your phone to the Pi via libp2p over a local connection.

## 🏙️ Austin Marketing Strategy
Your marketing should be about **Physical Presence**.
*   **Live Stream:** "Building a Brain in Austin: Day 1 of the Neuromorphic Mesh."
*   **The Hook:** "The big AI companies want your data in their cloud. I want your AI on your desk."

---
*Generated for The Architect from Doc 7*

