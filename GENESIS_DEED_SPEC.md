# INGRVM: Genesis Deed Specification

*Proof of Participation for the Founding 100 Neurons.*

---

## 💎 What is a Genesis Deed?
A Genesis Deed is a unique cryptographic entry in the INGRVM ledger (`ledger.db`) granted to the first 100 unique nodes that successfully synchronize with the mesh and complete a validation task.

It is **not** an NFT or a speculative asset. It is a **Reputation Anchor**.

---

## 🛠️ Technical Implementation
- **Identification:** Tied to the `node_id` (the 10,000D HDC Fingerprint) of the device.
- **Verification:** The node must process at least **5,000 useful spikes** during the "Detection Event" to qualify.
- **On-Chain Record:** A permanent flag in the `accounts` table of the blockchain: `is_genesis_node = True`.

---

## 🎁 Utility & Rewards
1.  **Reputation Multiplier:** Genesis nodes receive a permanent **1.5x multiplier** on their reputation growth. This means they earn $DOPA slightly faster and have more "Gravity" in the DAO.
2.  **Visual Badge:** In the **INGRVM Mobile App**, genesis nodes display a unique "Founding Shard" visual pulse in their neural heatmap.
3.  **Governance Weight:** Genesis nodes act as the first "Layer 0" of the fractal governance system, helping to approve the first community-driven protocol changes.

---

## 📜 How to Claim
1.  Download the **INGRVM APK**.
2.  Join the mesh during the **Act I: Detection** event.
3.  Maintain 99% uptime for the first 48 hours.
4.  The `RewardEngine` will automatically mint the Deed to the first 100 qualified nodes.

**Summary:** The Genesis Deed rewards the pioneers who provided the compute power that allowed the signal to stabilize.
