# Calyx Phase Roadmap (DAP-Enabled)
*Version: 2.0.0 (Launch Sequence)*

## Authority Matrix
| Node | Role | File Authority (Core) |
| :--- | :--- | :--- |
| **PC_MASTER** | Hub & Orchestrator | `hub_server.py`, `governance_dao.py`, `reward_engine.py`, `dashboard.html` |
| **LAPTOP_RELAY** | Network & Discovery | `p2p_debug.py`, `lan_discovery.py`, `circuit_relay.py`, `bootstrap_beacon.py` |
| **MOBILE_EDGE** | Shard Runner | `termux_boot.sh`, `pytorch_mobile_bridge.py`, `vitals_reporter.js` |
| **SHARED** | Logic DNA | `config.py`, `spike_protocol.py`, `shard_manager.py`, `neural_node.py` |

---

## Phase 5: The Autonomous Sovereign & Mobile Jump
1.  **[MOBILE]** **File: `termux_bootstrap.sh`** - Install Termux-ready Pytorch binaries (ARM64).
2.  **[MOBILE]** **File: `vitals_reporter.js`** - Implement battery/thermal monitoring API for Pixel 8.
3.  **[PC]** **File: `hub_server.py`** - Create WebSocket endpoint for real-time mesh visualization.
4.  **[LAPTOP]** **File: `lan_discovery.py`** - Implement Zeroconf/mDNS to auto-find PC_MASTER IP.
5.  **[SHARED]** **File: `config.py`** - Add `CALYX_NODE_ROLE` to differentiate behavior on boot.
6.  **[MOBILE]** **File: `neural_node.py` (Mobile Hook)** - Port spike receiver to run within Termux.
7.  **[PC]** **File: `hub_orchestrator.py`** - Build the "Rule Zero" validator to force cross-node sync.
8.  **[LAPTOP]** **File: `p2p_debug.py`** - Verify 192.168.x.x communication reliability between nodes.
9.  **[PC]** **File: `shard_manager.py`** - Define the "Layer Shard Schema" for Llama 3 8B (BitNet).
10. **[PC]** **File: `hub_server.py`** - Implement "Shard Distribution API" to push layers to peers.
11. **[MOBILE]** **File: `neural_node.py`** - Load first 5 layers of the model on Pixel 8.
12. **[LAPTOP]** **File: `neural_node.py`** - Load intermediate layers 6-25 on Laptop.
13. **[PC]** **File: `neural_node.py`** - Load final layers 26-32 on PC 1080 Ti.
14. **[SHARED]** **File: `spike_protocol.py`** - Add `hop_signature` to prevent replay attacks during sharding.
15. **[PC]** **File: `hub_server.py`** - Implement "Self-Healing" (if Mobile drops, PC re-claims layers).
16. **[LAPTOP]** **File: `mailroom.py`** - Automate task handoff: Laptop requests Shard 2 from PC.
17. **[PC]** **File: `dashboard.html`** - Add "Neural Flow" animation to show spikes moving across hardware.
18. **[MOBILE]** **File: `synapse_installer.sh`** - Create a single command to "Join Mesh" from any Android device.
19. **[ALL]** **STRESS TEST** - Run 500 concurrent inference requests across the 3-node shard.
20. **[ALL]** **Phase 5 Review** - Finalize `JOURNAL.md` and verify zero merge conflicts via DAP.

## Phase 6: The Marketplace & Incentives
1.  **[PC]** **File: `synapse_registry.py`** - Design SQL schema for skill metadata and CIDs.
2.  **[LAPTOP]** **File: `synapse_packager.py`** - Create tool to bundle `.pt` models into `.synapse` files.
3.  **[PC]** **File: `hub_server.py`** - Build IPFS-compatible storage endpoint for Synapse files.
4.  **[MOBILE]** **File: `marketplace_cli.py`** - Implement "Browse & Download" command for mobile nodes.
5.  **[PC]** **File: `reward_engine.py`** - Define the virtual $SYN RewardToken contract logic.
6.  **[SHARED]** **File: `spike_protocol.py`** - Implement `Proof-of-Inference` (PoI) hash validation.
7.  **[PC]** **File: `hub_server.py`** - Integrate PoI validation into the reward distribution loop.
8.  **[PC]** **File: `dashboard.html`** - Build "Creator Dashboard" to track $SYN earnings per node.
9.  **[LAPTOP]** **File: `synapse_packager.py`** - Implement semantic versioning (v1.0.1) for skills.
10. **[MOBILE]** **File: `neural_node.py`** - Add "Auto-Update" flag to refresh Synapses from Hub.
11. **[PC]** **File: `synapse_registry.py`** - Implement full-text search for the Synapse registry.
12. **[SHARED]** **File: `governance_dao.py`** - Define "Network Parameter" proposals (e.g. adjust Reward weights).
13. **[LAPTOP]** **File: `governance_dao.py`** - Implement node-side "Vote Casting" CLI.
14. **[PC]** **File: `governance_dao.py`** - Build the "Tally Service" to finalize DAO votes.
15. **[PC]** **File: `config.py`** - Automate parameter updates based on successful DAO votes.
16. **[PC]** **File: `slashing_protocol.py`** - Implement "Reputation Burn" for malicious/offline behavior.
17. **[LAPTOP]** **File: `skill_validator.py`** - Create a peer-review tool for community skills.
18. **[PC]** **File: `hub_server.py`** - Deploy "Marketplace Alpha" to the private mesh.
19. **[ALL]** **SYSTEM TEST** - Execute a "Paid" inference loop (User spends $SYN, Nodes earn $SYN).
20. **[ALL]** **Phase 6 Documentation** - Update `Docs/SDK_BLUEPRINT.md`.

## Phase 7: The Consumer Layer & Hardening
1.  **[PC]** **File: `Dockerfile`** - Containerize the entire Hub environment for easy cloud deploy.
2.  **[LAPTOP]** **File: `setup.msi`** - Create a Windows installer for the Mesh Node service.
3.  **[MOBILE]** **File: `CalyxApp`** - Start "Compose Multiplatform" wrapper for a native Android UI.
4.  **[LAPTOP]** **File: `circuit_relay.py`** - Implement libp2p Relay v2 for cross-internet mesh.
5.  **[SHARED]** **File: `hole_puncher.py`** - Implement UDP hole-punching for NAT traversal.
6.  **[PC]** **File: `onboarding_wizard.html`** - Create a "Step-by-Step" UI for first-time node setup.
7.  **[SHARED]** **File: `security_gateway.py`** - Implement Noise-protocol (TLS) for all LAN/WAN traffic.
8.  **[LAPTOP]** **File: `efficiency_monitor.py`** - Add "Resource Quotas" (Limit CPU/RAM usage) to node.
9.  **[MOBILE]** **File: `foreground_service.kt`** - Prevent Android OS from killing the node in the background.
10. **[PC]** **File: `mesh_health_map.js`** - Build a global "Mesh Status" map (using sanitized regions).
11. **[LAPTOP]** **File: `bootstrap_beacon.py`** - Implement "Auto-Seed" to find public mesh entry points.
12. **[ALL]** **SCALE TEST** - Join 10 nodes across different physical locations/ISPs.
13. **[LAPTOP]** **File: `quantization.py`** - Optimize 1-bit kernels for low-end CPUs (older laptops).
14. **[MOBILE]** **File: `metabolism.py`** - Optimize battery drain for "Idle Mesh Listening" mode.
15. **[SHARED]** **File: `synapse_registry.py`** - Add support for Vision (Image) and Audio synapses.
16. **[PC]** **File: `calyx_sdk_py`** - Package the Python SDK for external devs to build on Calyx.
17. **[ALL]** **SECURITY AUDIT** - Conduct penetration testing on the P2P message layer.
18. **[MOBILE]** **File: `remote_control.js`** - Allow Mobile app to "Control" the PC Hub remotely.
19. **[ALL]** **RECOVERY TEST** - Verify system survives 50% node loss without crashing the mesh.
20. **[ALL]** **Phase 7 Final Stress** - Hardening for public release.

## Phase 8: MVP Launch & Public Beta
1.  **[PC]** **File: `cloud_bootstrap.py`** - Deploy 5 high-uptime "Seed Nodes" to DigitalOcean.
2.  **[SHARED]** **File: `scrub_manifest.json`** - Final automated check for secrets/PII in commit history.
3.  **[ALL]** **File: `MANIFESTO.md`** - Finalize the "AI-Built" story for the public.
4.  **[PC]** **File: `calyx.io`** - Build and deploy the official landing page and technical docs.
5.  **[PC]** **File: `genesis.json`** - Create the Genesis block for the Reward Ledger.
6.  **[PC]** **File: `how_to_videos`** - Record tutorials for Node setup and Skill creation.
7.  **[PC]** **ORCHESTRATION** - Create the GitHub Organization and migrate `Calyx` repo.
8.  **[ALL]** **BETA INVITE** - Open the mesh to the first 50 external "Curious Builders".
9.  **[PC]** **File: `mesh_monitor_v2.html`** - Real-time dashboard for public beta tracking.
10. **[ALL]** **BUG SQUASH** - Resolve "Day 0" beta feedback via AI-first rapid dev cycles.
11. **[PC]** **File: `grant_program.md`** - Launch the $SYN grant program for new skill developers.
12. **[SHARED]** **File: `FRAMEWORK.md`** - Release the "AI-Builder Framework" to the public.
13. **[PC]** **File: `ROADMAP_V2.md`** - Publish the Community-driven roadmap for the next 12 months.
14. **[ALL]** **UPDATE v1.0.0** - Push the first "Stable" version to all mesh nodes.
15. **[PC]** **CONTENT** - Official Launch Announcement (X, LinkedIn, Solarpunk forums).
16. **[PC]** **COMMUNITY** - Launch the Discord/Matrix "Neural Nexus" for mesh providers.
17. **[ALL]** **LAUNCH EVENT** - Execute a mesh-wide "Inference Celebration" query.
18. **[PC]** **SCALING** - Monitor and scale the seed cluster as mesh hits 100+ nodes.
19. **[ALL]** **POST-MORTEM** - Document lessons learned from the "AI-First" building process.
20. **[ALL]** **Phase 9 Kickoff** - "The Global Neural Fabric" begins.

## Phase 9: The Sovereign Mainnet (Real Value & Absolute Privacy)
1.  **[SHARED]** **File: `zk_proof_generator.py`** - Upgrade Proof-of-Inference (PoI) to use **zk-SNARKs** (Zero-Knowledge Proofs). This allows nodes to prove they did the work without revealing their identity, providing absolute anonymity in the financial layer.
2.  **[PC]** **DEX Liquidity** - Prepare the $SYN token architecture for listing on Decentralized Exchanges (Uniswap/Jupiter) to allow permissionless swapping for real-world value without centralized KYC.
3.  **[ALL]** **Mainnet Migration** - Transition from the local SQLite virtual ledger to a sovereign, globally distributed ledger.
