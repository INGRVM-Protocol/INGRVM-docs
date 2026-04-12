# INGRVM: Bittensor Subnet & Bridge Specification
**Objective:** Transition the INGRVM virtual ledger ($DOPA) into a verified Bittensor Subnet.

## 1. Subnet Architecture
The INGRVM mesh operates as a **Bittensor Subnet**.
- **Validators (PC_MASTER / Tier 1):** Responsible for running "The Judge" and verifying ZK-proofs from the mesh.
- **Miners (MOBILE_EDGE / LAPTOP_RELAY):** Responsible for performing sharded inference and submitting `mini-snark` proofs.

## 2. Yuma Consensus Integration
Rewards are distributed via Yuma Consensus (YC).
- **Incentive:** Miners receive $TAO based on their "Proof of Intelligence" (inference accuracy and proof validity).
- **Dividends:** Validators receive $TAO for correctly auditing miners and maintaining mesh stability.

## 3. The $DOPA / $TAO Bridge
Since INGRVM uses a 1-bit high-frequency internal ledger, we use a **Burn-to-Mint** bridge for economic settlement.
- **Entry:** Users burn $TAO on the Subtensor chain to mint $DOPA on the INGRVM mesh (for low-latency compute credits).
- **Exit:** Nodes burn $DOPA on the mesh to claim $TAO rewards from the Subnet's incentive pool.

## 4. Security: Replay & Double-Spend Protection
- **Extrinsic Auditing:** The `SubtensorBurnRelayer` monitors the Bittensor chain for `BurnedRegistration` events.
- **Merkle Proofs:** Every bridge transaction must be included in an INGRVM block (Ledger v2) with a valid Merkle proof.

## 5. Technical Stack
- **Mesh Logic:** Python (libp2p, trio, torch).
- **Subnet Interface:** `bittensor` Python SDK.
- **ZK Prover:** Circom (PC) / Mini-snark (Mobile).
- **On-Chain:** Substrate (Bittensor blockchain).
