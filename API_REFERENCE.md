# INGRVM API Reference

Generated automatically from the source code.

## `api_gateway.py`
### Class: `InferenceRequest`
No description.

### Class: `InferenceResponse`
No description.

### Function: `read_root()`
No description.

---

## `blockchain_epoch.py`
### Class: `SubtensorMock`
Simulates the Bittensor 'Subtensor' blockchain logic.
Handles Epoch settlement using a simplified Yuma Consensus model.

- **Method:** `__init__()`
- **Method:** `run_epoch()`
---

## `bootstrap_beacon.py`
### Function: `print_f()`
No description.

---

## `business_nexus.py`
### Class: `BusinessNexus`
The 'Action' layer of the INGRVM Mesh.
Triggers external n8n workflows when the neural node detects business value.

- **Method:** `__init__()`
- **Method:** `trigger_action()`
---

## `circuit_relay.py`
### Class: `LighthouseRelay`
Simulates a 'Lighthouse' Node with a Static Public IP.
Acts as a meet-up point for nodes behind firewalls (NAT).

- **Method:** `__init__()`
- **Method:** `register_node()`
- **Method:** `get_relay_path()`
### Class: `AutoNAT`
Logic for a node to detect if it is behind a restrictive firewall.

- **Method:** `check_reachability()`
---

## `config.py`
### Class: `INGRVMConfig`
Central Nervous System for Node Configuration.
Loads settings from 'ingrvm_config.json' or falls back to defaults.
Ensures all modules share the same DNA.

- **Method:** `__init__()`
- **Method:** `load()`
- **Method:** `save()`
- **Method:** `get()`
---

## `context_memory.py`
### Class: `ContextMemory`
Manages isolated neural states (short-term memory) for multiple mesh users.
Prevents 'Context Contamination' between different conversation threads.

- **Method:** `__init__()`
- **Method:** `get_state()`
- **Method:** `save_state()`
- **Method:** `clear_context()`
---

## `ingrvm_cli.py`
### Class: `INGRVMDashboard`
The Command Center for a INGRVM Node.
Orchestrates all modules into a single, beautiful Solarpunk interface.

- **Method:** `__init__()`
- **Method:** `generate_layout()`
- **Method:** `get_header()`
- **Method:** `get_vitality_panel()`
- **Method:** `get_stats_panel()`
- **Method:** `get_ingrvms_panel()`
- **Method:** `run()`
---

## `create_mock_data.py`
### Function: `generate_mock_dataset()`
Generates a synthetic sentiment dataset for SNN training.
Inputs: 3 normalized ASCII features.
Targets: 0 (Negative), 1 (Positive).

---

## `efficiency_monitor.py`
### Class: `EfficiencyMonitor`
Quantifies the 'Solarpunk Advantage' of Neuromorphic SNNs.
Converts Joules into tangible human metrics.

- **Method:** `__init__()`
- **Method:** `calculate_savings()`
---

## `encoder.py`
### Class: `TextSpikeEncoder`
Converts raw text into neuromorphic spike trains using ASCII normalization 
and Rate Coding.

- **Method:** `__init__()`
- **Method:** `encode()`
---

## `ensemble_manager.py`
### Class: `MiniBrain`
No description.

- **Method:** `__init__()`
- **Method:** `forward()`
### Class: `NeuralEnsemble`
Simulates a cluster of nodes processing the same task to reach consensus.

- **Method:** `__init__()`
- **Method:** `process_task()`
---

## `evolution_engine.py`
### Class: `EvolutionEngine`
Simulates the 'Breeding' of better AI ingrvms.
Takes a population of SNNs and evolves them based on Reputation.

- **Method:** `__init__()`
- **Method:** `breed()`
---

## `governance_dao.py`
### Class: `Proposal`
No description.

### Class: `INGRVMDAO`
Implements the 'Parliament' logic.
Nodes vote on network upgrades using stake-weighted consensus.

- **Method:** `__init__()`
- **Method:** `create_proposal()`
- **Method:** `cast_vote()`
- **Method:** `tally_votes()`
---

## `heartbeat.py`
### Class: `MeshHeartbeat`
Monitors the 'Vitals' of neighbor fireflies in the mesh.
Ensures the Trust Map is accurate by pruning stale peers.

- **Method:** `__init__()`
---

## `high_throughput_probe.py`
### Function: `print_f()`
No description.

---

## `hole_puncher.py`
### Class: `UDP_HolePuncher`
Simulates UDP Hole Punching for NAT Traversal.
Allows two firewalled nodes to talk directly by simultaneously 
sending 'probes' to each other.

- **Method:** `__init__()`
---

## `homeostasis.py`
### Class: `HomeostaticBrain`
A 3-layer Mini-Brain with Adaptive Thresholding (Homeostasis).
If it fires too much, it raises the threshold. If silent, it lowers it.

- **Method:** `__init__()`
- **Method:** `forward()`
---

## `identity_manager.py`
### Class: `NodeIdentity`
Manages the cryptographic identity of a INGRVM Node.
Uses Ed25519 for high-speed signing and verification.

- **Method:** `__init__()`
- **Method:** `load_or_create()`
- **Method:** `get_public_key_b64()`
- **Method:** `sign_data()`
- **Method:** `verify_signature()`
---

## `lan_relay.py`
### Function: `print_f()`
No description.

### Function: `run_server()`
Runs on PC: Receives raw binary spikes. 

### Function: `run_client()`
Runs on Laptop: Sends a raw binary spike. 

---

## `local_discovery.py`
### Class: `LocalDiscovery`
Simulates Zero-Config / mDNS style discovery.
Allows nodes on the same local network to find each other without 
a central bootstrap beacon.

- **Method:** `__init__()`
---

## `master_node.py`
### Function: `print_f()`
No description.

### Class: `INGRVMMasterNode`
The Completed INGRVM.
Wires all 17+ logical modules into a watertight neural pipeline.

- **Method:** `__init__()`
---

## `mercenary_log.py`
### Class: `MercenaryLogger`
Narrates node activity into the 'Cynical Engineer / Architect' persona.
Used for YouTube dev-log generation and content strategy.

- **Method:** `__init__()`
- **Method:** `log_event()`
---

## `metabolism.py`
### Class: `NodeMetabolism`
Solarpunk Concept: Nodes have a 'Virtual Energy Pool' (Joules).
Every spike consumed energy. Energy regenerates via 'Solar Harvest'.
Prevents mesh spamming and rewards efficiency.

- **Method:** `__init__()`
- **Method:** `_regenerate()`
- **Method:** `consume_spikes()`
- **Method:** `get_status()`
---

## `neural_node.py`
### Function: `print_f()`
No description.

### Class: `MiniBrain`
No description.

- **Method:** `__init__()`
- **Method:** `forward()`
---

## `p2p_debug.py`
### Function: `print_f()`
No description.

### Function: `make_insecure_host()`
Creates a libp2p host that ONLY uses insecure (plaintext) transport.
Uses 'sec_opt' as identified via function inspection.

---

## `pack_trained_ingrvm.py`
### Function: `main()`
No description.

---

## `peer_database.py`
### Class: `PeerRecord`
No description.

### Class: `PeerDatabase`
Persistent store for tracking network neighbors and their reliability.

- **Method:** `__init__()`
- **Method:** `load()`
- **Method:** `save()`
- **Method:** `update_peer()`
- **Method:** `get_peer()`
---

## `phoenix_supervisor.py`
### Class: `PhoenixSupervisor`
The Watchdog of the INGRVM Mesh.
Monitors the Neural Node process. If it crashes, it rises from the ashes.

- **Method:** `__init__()`
- **Method:** `rise()`
- **Method:** `handle_crash()`
---

## `pipeline_buffer.py`
### Class: `PipelineBuffer`
Queueing system for high-throughput sharding.
Collects spikes and processes them in batches to reduce P2P overhead.

- **Method:** `__init__()`
---

## `pipeline_router.py`
### Class: `PipelineRouter`
The Pipeline Router decides WHERE a spike goes next.
It links the SNN inference logic with the P2P networking.

- **Method:** `__init__()`
- **Method:** `route_spike()`
---

## `plasticity.py`
### Class: `STDPPlasticity`
Implements Spike-Timing-Dependent Plasticity (STDP).
Strengthens ingrvms if the pre-synaptic spike occurs just before 
the post-synaptic spike.

- **Method:** `__init__()`
- **Method:** `update_weights()`
---

## `playground.py`
### Function: `print_f()`
No description.

### Class: `INGRVMPlayground`
The ultimate end-to-end demo of the INGRVM 'Virtual Seed'.
Bridges human text -> spikes -> brain -> decision -> energy metrics.

- **Method:** `__init__()`
- **Method:** `process_input()`
### Function: `main()`
No description.

---

## `quantization.py`
### Class: `NeuromorphicQuantizer`
Solarpunk Optimization: Converts 32-bit weights into 1-bit 'Spiking' weights.
Reduces model size by 32x, allowing 80B models to fit on consumer hardware.

- **Method:** `quantize_1bit()`
- **Method:** `estimate_compression()`
---

## `rank_choice_voting.py`
### Class: `RankedChoiceConsensus`
Implements Instant-Runoff Voting (Ranked Choice).
Nodes submit a list of preferences. Logic eliminates bottom choices 
and redistributes until a winner emerges.

- **Method:** `get_winner()`
---

## `reward_engine.py`
### Class: `NodeStats`
No description.

### Class: `RewardEngine`
Implements the INGRVM Tokenomics from Document 2.
Formula: R_i = E * (w_i * q_i) / Sum(w_j * q_j)

- **Method:** `__init__()`
- **Method:** `register_work()`
- **Method:** `calculate_payouts()`
---

## `reward_validator.py`
### Class: `RewardValidator`
The 'Ticket Inspector' of the INGRVM Mesh.
Ensures every spike processed by the node is cryptographically signed
and attributed to a valid peer before it counts as 'Useful Work'.

- **Method:** `__init__()`
- **Method:** `verify_spike_integrity()`
---

## `run_virtual_mesh.py`
### Function: `print_f()`
No description.

---

## `security_gateway.py`
### Class: `SecurityGateway`
The Zero-Trust Firewall for your Brain.
Rejects any traffic that isn't signed by a verified node in the PeerDB.

- **Method:** `__init__()`
- **Method:** `verify_ingress()`
---

## `seed_generator.py`
### Class: `DigitalSeed`
Solarpunk Concept: Generates a unique, living visual representation 
of a node based on its ID and Reputation. (Emoji-free for audit stability)

- **Method:** `__init__()`
- **Method:** `generate_plant()`
---

## `shard_manager.py`
### Class: `ModelShard`
No description.

### Class: `ShardManager`
Handles Model Sharding across the mesh.
Supports both P2P (FloodSub) and File-based discovery for resilience.

- **Method:** `__init__()`
- **Method:** `register_shard()`
- **Method:** `_write_local_discovery_file()`
- **Method:** `send_file_spike()`
- **Method:** `find_next_hop()`
---

## `ingrvm_0_test.py`
---

## `ingrvm_packager.py`
### Class: `ingrvmPackage`
Bundles SNN weights, architecture metadata, and integrity hashes 
into a single '.ingrvm' binary file for mesh distribution.

- **Method:** `__init__()`
- **Method:** `create_package()`
- **Method:** `unpack_package()`
---

## `ingrvm_registry.py`
### Class: `ingrvmRegistry`
Manages the persistence of SNN 'ingrvms' (weights and metadata) on the local node.

- **Method:** `__init__()`
- **Method:** `save_ingrvm()`
- **Method:** `load_ingrvm()`
---

## `slashing_protocol.py`
### Class: `SlashingManager`
Implements the 'Slashing' security logic for the mesh.
Penalizes nodes for malicious or Byzantine behavior.

- **Method:** `__init__()`
- **Method:** `slash_node()`
---

## `speculative_spike.py`
### Class: `SpikeSpeculator`
Implements 'Speculative Spiking' to hide network latency.
The node predicts the next spike based on the recent firing pattern.

- **Method:** `__init__()`
- **Method:** `record_actual_spike()`
- **Method:** `predict_next_spike()`
- **Method:** `verify_prediction()`
- **Method:** `get_stats()`
---

## `spike_protocol.py`
### Class: `NeuralSpike`
The 'Blockchain-Ready' protocol for neuromorphic data transmission.
Optimized with Sparse-Vector Encoding for massive bandwidth reduction.

- **Method:** `set_spikes()`
- **Method:** `get_spikes()`
- **Method:** `to_bin()`
- **Method:** `from_bin()`
### Function: `generate_task_id()`
No description.

### Function: `hash_input()`
No description.

---

## `spike_queue.py`
### Class: `PrioritizedSpikeQueue`
Manages incoming neural traffic with reputation-based priority.
Prevents 'Request Floods' from overwhelming the Mini-Brain.

- **Method:** `__init__()`
- **Method:** `push()`
- **Method:** `pop()`
---

## `spike_sanitizer.py`
### Class: `SpikeSanitizer`
Security Middleware: Filters incoming 'Toxic Spikes' to prevent 
model crashes, overflows, or adversarial data poisoning.

- **Method:** `sanitize()`
---

## `spike_sender.py`
### Function: `print_f()`
No description.

---

## `spike_trace.py`
### Class: `SpikeTracer`
Developer Tool: Logs the exact timing of neural firing events.
Used to debug asynchronous spike trains and identify dead neurons.

- **Method:** `__init__()`
- **Method:** `record_spike()`
- **Method:** `save_to_disk()`
- **Method:** `visualize_ascii()`
---

## `test_lava.py`
---

## `test_p2p_node.py`
---

## `thalamus.py`
### Class: `ThalamusRouter`
Orchestrates multiple 'ingrvms' on a single node.
Routes incoming spikes to the correct local SNN brain.

- **Method:** `__init__()`
- **Method:** `register_ingrvm()`
- **Method:** `load_package()`
---

## `train_ingrvm_0.py`
### Class: `ingrvm0Net`
No description.

- **Method:** `__init__()`
- **Method:** `forward()`
### Function: `train_model()`
No description.

---

## `validator_gate.py`
### Class: `ingrvmNet`
No description.

- **Method:** `__init__()`
- **Method:** `forward()`
### Class: `ingrvmValidator`
Simulates a Tier II node's Pre-Frontal INGRVM audit logic.

- **Method:** `audit_ingrvm()`
---

## `vapi_bridge.py`
---

## `weighted_consensus.py`
### Class: `WeightedEnsemble`
Prevents Sybil Attacks by weighting decisions based on $DOPA stake and Reputation.
Integrates directly with the persistent PeerDatabase.

- **Method:** `__init__()`
- **Method:** `get_consensus()`
---

## `zk_proof_mock.py`
### Class: `ZKProofMock`
Simulates a Zero-Knowledge Proof of Inference commitment.
Proves the node ran the specific model weights on some input 
without revealing the input itself.

- **Method:** `generate_proof()`
- **Method:** `verify_proof()`
---



