# Calyx Project Archive (THOR/Synapse/Calyx Era)

This document contains the historical session logs and reconstructed summaries from the development of Calyx.

---

### [2026-03-02] - Session 21: PC Master (The Launch Sequence)

### Status update
- **Node Identification:** Confirmed session running on **Desktop (PC_MASTER)**.
- **Organization Setup (DONE):** Created **Calyx-Mesh** GitHub Organization.
- **Repo Initialization (DONE):** Created and initialized `calyx-core`, `calyx-framework`, and `calyx-docs`.
- **Codebase Sanitization (DONE):** Removed personal logs and hardcoded local paths. Replaced GitHub URLs with placeholders.
- **Framework Extraction (DONE):** Moved meta-tools (`JOURNAL.md`, `MAIL_TO_...`, `mcp_setup_guide.md`) to `Calyx/Framework/`.
- **Roadmap Expanded (DONE):** Defined Phases 5-8 with 20 granular tasks per phase, hardware delegation, and DAP (Distributed Authority Protocol).
- **Public Sync (DONE):** Created and executed `sync_to_org.ps1`. Successfully pushed sanitized code to the public Org.
- **Content Strategy (DONE):** Created `Creative/Content_Creation/calyx_video_strategy.md` and `calyx_theories_and_usecases.md`.

### Technical Decisions
1. **DAP Implementation:** Formally established the **Distributed Authority Protocol** to manage multi-node file ownership and prevent merge conflicts in a decentralized environment.
2. **Sanitization Strategy:** Moved all sensitive settings to `.env`. Log files containing personal paths were purged.
3. **Framework Separation:** Packaged the "AI-Builder Framework" as a standalone toolkit for the community, while keeping the core neuromorphic logic in `calyx-core`.

---

### [2026-02-24 to 2026-02-27] - Sessions 10-17: The Bridge & Sharding Epoch (RECONSTRUCTED)
*Note: These entries were reconstructed after a synchronization gap. They represent the core technical milestones achieved during the transition from Synapse to Calyx.*

- **The Naming Pivot:** Officially transitioned from "Project THOR" to "Calyx."
- **Neural Sharding Alpha:** Developed the `shard_manager.py` to handle the splitting of `.pt` weight files across nodes.
- **The Mailroom Protocol:** Created the first version of the file-based "Postal Service" for nodes to communicate intent without a persistent socket connection.
- **DAP (Distributed Authority Protocol):** Established the first rules for machine-file ownership to prevent Git merge conflicts between the PC and Laptop.
- **Mesh Topology:** Implemented the `mesh_discovery/` directory system, allowing nodes to broadcast their existence via JSON heartbeats.
- **Inference Hardening:** Successfully executed a 3-hop "Virtual Spike" across PC, Laptop, and a mock Mobile node.

---

### [2026-02-23] - Session 9: Reward Validation & High-Frequency Benchmarking
- **Reward Integrity Secured:** Implemented `reward_validator.py`. The node can now verify Ed25519 signatures on every spike.
- **Stress-Test Ready:** Developed `high_throughput_probe.py` to fire 100+ signed spikes per second.

---

### [2026-02-23] - Session 8: Physical LAN Bridge Verified
- **Physical Handshake SUCCESS:** Verified end-to-end physical connection between Laptop and PC using Raw TCP.
- **Firewall Solved:** Explicitly opened ports 60001 and 60005 on the PC.

---

### [2026-02-23] - Session 7: Physical LAN Mesh Success
- **Physical LAN Mesh Verified:** Successfully linked PC and Laptop via LAN IP.
- **Hybrid Discovery Success:** Verified file-based fallback logic.

---

### [2026-02-22] - Session 1: The Virtual Pivot & Environment Setup
- **Environment Setup:** Installed `torch`, `snntorch`, `libp2p`, and `trio`.
- **Logic Pillars:** Created 20+ core modules including the LIF neuron model, MessagePack binary optimization, Ensemble Consensus, and Solarpunk mobile mockup.
- **Security:** Implemented Ed25519 node identities and zkML proof mocks.

---
*Archived for historical reference by The Architect.*
