# INGRVM: Live Mesh Validation Roadmap

*Phase 13: Grounding the architecture through real-world multi-device testing.*

---

## 🧪 Step 1: The Neural Ping (Latency Test)
- **Goal:** Verify 1-bit spike delivery between devices without payload loss.
- **Devices:** Mobile (Sender) -> PC (Receiver).
- **Metric:** Round-trip latency over local Wi-Fi and 5G.
- **Success Criteria:** < 100ms latency for a burst of 1,000 spikes.

## 🧪 Step 2: The Sharded Pulse (Model Splitting)
- **Goal:** Execute a single SpikeLLM inference across two physical devices.
- **Setup:**
    - **Laptop:** Processes Layers 1-16 (The "Intuition" Shard).
    - **Mobile:** Processes Layers 17-32 (The "Analysis" Shard).
- **Success Criteria:** Mobile node outputs a coherent result based on spikes received from the laptop.

## 🧪 Step 3: Metabolic Survival Test
- **Goal:** Verify that **Metabolic Gating** actually saves battery.
- **Setup:** Run a continuous spike-stream on the phone until battery hits 20%.
- **Success Criteria:** The node automatically increases its firing threshold and reduces CPU usage by 50% when low power is detected.

## 🧪 Step 4: Bittensor Testnet Week (The Swarm)
- **Goal:** Register the mesh on the **Finney Testnet**.
- **Action:**
    - Connect the PC Hub to the subtensor.
    - Let the 3 devices earn testnet-DOPA for 7 continuous days.
- **Metric:** 100% uptime and >0.9 reputation score.
