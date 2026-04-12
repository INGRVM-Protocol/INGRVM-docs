# INGRVM: 3-Device Live Mesh Test Plan

*Objective: Run a sharded SpikeLLM inference across PC, Laptop, and Mobile in the real world.*

---

## 🏗️ The Setup
1.  **Device A (PC - The Master Hub):** 
    -   **Role:** Validator & Orchestrator. 
    -   **Task:** Run `hub_server.py`. This node will manage the $DOPA ledger and assign shards to the other devices.
2.  **Device B (Laptop - The Compute Relay):** 
    -   **Role:** Layer 1-16 Shard. 
    -   **Task:** Run `ingrvm_cli.py launch`. This node will listen for spikes from the Hub, process the first half of the AI model, and pass it on.
3.  **Device C (Mobile - The Edge Node):**
    -   **Role:** Inference Requester & Layer 17-32 Shard.
    -   **Task:** Use the Mobile UI or CLI to send a "Message Request." It will process its own local layers and send the final "Spike Result" back to the Hub.

---

## 🚀 Execution Steps

### Step 1: Connectivity (The Tunnel)
-   Ensure the **Cloudflare Tunnel** is active on the PC.
-   The Laptop and Mobile must be able to "Ping" the PC Hub via its `.ingrvm.online` address.

### Step 2: The "Hello World" Message
-   We will use the **Spike Protocol** to send a raw text message encoded as spikes from the Mobile to the Laptop.
-   **Test Command:** `python INGRVM/Core/ingrvm_cli.py send --target LAPTOP_NODE --msg "Hello Mesh"`

### Step 3: The Sharded Inference
-   Mobile node requests a "Sentiment Analysis" of a sentence.
-   Hub breaks the request into two shards.
-   Laptop processes Shard 1 -> Sends spikes to Mobile.
-   Mobile processes Shard 2 -> Result displayed on phone.

---

## 💎 Bittensor Testnet Week
-   We will register our **Subnet** on the Bittensor testnet.
-   For 7 days, your nodes will participate in the global "Finney" testnet environment.
-   **Goal:** Maintain a >0.9 Reputation Score for the entire week to prove mesh stability.
