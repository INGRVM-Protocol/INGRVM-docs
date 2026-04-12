# INGRVM Relayer Security Blueprint

## 1. Governance-Controlled Bridging
To prevent single-point-of-failure (SPoF) attacks, the $DOPA -> Subtensor bridge will not be controlled by a single private key.

### **Initial Implementation: Multi-Sig (Wait-for-Finality)**
- **Rule:** A bridge exit transaction is only valid if it has been recorded in the `INGRVMLedger` and has reached **absolute finality** (verified in a block with a confirmed Merkle root).
- **Mechanism:** The Relayer Service monitors the `transactions` table but waits for the `block_id` to be finalized by the PC Master Hub.

### **Phase 9.5 Goal: Threshold Signatures (TSS)**
- **Architecture:** Move from a single relayer to a **Federated Bridge**.
- **Signing:** Use an off-chain Multi-Party Computation (MPC) ceremony. 3-of-5 nodes (PC, Laptop, and verified 3rd-party community nodes) must sign the public Subtensor burn/mint transaction.
- **Benefit:** No single node (not even the PC Master) can unilaterally mint assets on the public Bittensor network.

---

## 2. Economic Security (Slashing Consensus)

### **The Stake Requirement**
Any node wishing to act as a **Bridge Validator** must lock a minimum of **500 $DOPA** using the `staking_cli.py`.

### **Slashing Conditions**
The `Slashing Consensus` logic implemented in `governance_dao.py` allows the mesh to vote on burning a validator's stake if:
1.  **Fraudulent Minting:** Attempting to sign a public Bittensor transaction without a corresponding `SLASH` event in the INGRVM Ledger.
2.  **Liveness Failure:** If the bridge validator is unresponsive for > 48 hours, a portion of their stake is burned to compensate for network delay.

---

## 3. Implementation Roadmap
1.  [x] **Done:** Created `staking_cli.py` to allow validators to lock collateral.
2.  [x] **Done:** Integrated `burn_stake` and `slash` DAO actions into the core.
3.  [ ] **Next:** Port the `bridge_relayer_poc.py` to require a `finalized` status from the Ledger V2.
4.  [ ] **Next:** Implement a simple 2-of-2 multisig script for the initial PC-Laptop bridge handshake.
