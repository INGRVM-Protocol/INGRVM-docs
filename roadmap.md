# INGRVM Roadmap

> **Single source of truth for project planning.**
> Updated: 2026-03-27 | Maintained by: phase_gate.py automated checks + manual audit

---

## Current State (Verified)

### Gate Evidence

| Horizon | Status | Gate Hash | Report |
|---------|--------|-----------|--------|
| Horizon 0 (Prove the Thesis) | **VERIFIED** | `2d9d311cba50` | `phase_horizon_0_20260317_205139.json` |
| Horizon 1 (Working Distributed AI) | **VERIFIED** | `ca168fdd0e91` | `phase_horizon_1_20260318_001136.json` |
| Horizon 2 (Useful Product) | **VERIFIED** | `2485b6f5` | `phase_horizon_2_20260320_194041.json` |
| Horizon 3 (Platform Layer) | **IN PROGRESS** | `2d90656f` (infra foundations 7/7) | `phase_horizon_3_20260320_211809.json` |

### What's Actually Working (~378 tests)

| Component | Status | Tests | Evidence |
|-----------|--------|-------|----------|
| Spike Protocol | VERIFIED | smoke tests | creation, serialization, encrypt/decrypt, sign |
| $DOPA Economy | VERIFIED | economy + staking | genesis, mint, transfer, halving, staking flow |
| Ed25519 Identity | VERIFIED | security tests | keygen, sign, verify |
| DAO Governance | VERIFIED | 10 | proposal, quadratic voting, tally, auto-execute |
| Distributed Inference (GPT-2 124M) | VERIFIED | orchestrator tests | 8-bit shards, coherent English, ~158ms/token local |
| ZK Proofs (Groth16) | VERIFIED | 15 | circom + snarkjs, tamper rejection |
| Hub Discovery | VERIFIED | 12 | UDP broadcast + bootstrap fallback |
| Paid Inference | VERIFIED | 13 | payment → execution → reward loop |
| Synapse Skill Protocol | VERIFIED | 49 | 5 seed skills, router, orchestrator, 7 API endpoints |
| Phase 7.2 Security | VERIFIED | 16/16 | all POST endpoints verified, weights_only on torch.load |
| Physical 2-Node Mesh | VERIFIED | 4 | PC_MASTER + LAPTOP_RELAY on LAN |
| Staking Flow | VERIFIED | 8 | mint → stake → access rights → inference |
| Media Streaming | VERIFIED | 16 | chunked upload/stream |
| Hub Server API | VERIFIED | 51 | endpoint coverage |
| Slashing Protocol | VERIFIED | consensus tests | multi-node slashing |
| DPoS Auction | VERIFIED | auction tests | validator election, bids |
| Model Catalog API | VERIFIED | 21 | `/api/models`, config, shard endpoints |
| Health Check | VERIFIED | 13 | `/api/health` with active features, deps, node status |
| End-to-End Integration | VERIFIED | 8 | ping, registration, identity, ledger, marketplace, DAO, vitals |

### What's Coded but Unverified

- P2P Gossip (libp2p falls back to MOCK mode)
- Thalamus router (hard-requires torch)
- IPFS storage (mock CID, requires IPFS daemon)
- KV cache sync (4 tests, requires torch)
- Solana integration (SPL token program for $DOPA on-chain — needs wallet + deployment)
- Hole puncher / NAT traversal (exists, untested)
- Mobile UI (6 views built, needs onboarding and polish)
- One-command join (`python -m ingrvm join` with Ed25519 — needs live testing)

### What's Not Started

- WiFi Direct / Bluetooth mesh
- Content platform (Horizon 3 Track B)
- 7B+ model support
- Consumer apps (mobile/desktop/web beyond current React UI)
- Public deployment (no Vercel/VPS/HTTPS yet)

---

## Honest Constraints

- **Latency:** Each network hop adds 2-5ms (LAN) or 50-200ms (internet). Autoregressive generation is serial. For a 12-layer model split across 3 devices, expect 3x inter-device overhead per token.
- **Quantization:** 1-bit post-hoc quantization destroys model quality. Practical starting point is 4-bit (GPTQ) or 8-bit. 1-bit requires training from scratch (BitNet-style).
- **Single-device comparison:** For models under 7B, a single machine with 4-bit quantization (llama.cpp) beats a 3-device mesh. The mesh wins only for models too large for any single node's memory.
- **Content bandwidth:** Video streaming needs 5-50 Mbps. WiFi handles it. Bluetooth (3 Mbps max) and LoRA (kilobits/sec) do not.
- **Energy:** Distributing inference across consumer devices does not save total energy vs data center GPUs. The value is cost distribution and decentralized ownership.
- **SNN on CPUs:** SNN energy savings only materialize on neuromorphic hardware (Intel Loihi, etc.). On regular CPUs, SNN simulation is less efficient than transformers.

---

## Vision: Horizons 0-5

### Horizon 0: Prove the Thesis — VERIFIED

**Goal:** Real model generating coherent text across 2+ processes.

- GPT-2 (124M) downloaded, quantized to 8-bit, split into 2 shards
- Each shard runs in separate process, activations passed via socket
- Coherent English output verified, latency measured: 42ms/token fp32 vs 158ms distributed local vs 420ms LAN
- **Gate: PASS 5/5** | `2d9d311cba50`

### Horizon 1: Working Distributed AI — VERIFIED

**Goal:** Model converter CLI, real multi-device pipeline, work verification.

- `model_converter.py` CLI — download, quantize, shard, package
- Cross-device spike forwarding via `distributed_orchestrator.py`
- Work verification v1: spot-check consensus (honest hash match + cheater detection)
- Latency measurement harness integrated into `phase_gate.py`
- Phase 7.2 security: `verify_node_request` on all endpoints, `weights_only=True` on all `torch.load`
- **Gate: PASS 5/5** | `ca168fdd0e91`

**Remaining:** KV cache synchronization for autoregressive generation

### Horizon 2: Useful Product — VERIFIED

**Goal:** Something a second person can install and use.

- Multiple model support (user picks from marketplace)
- One-command node setup: `python -m ingrvm join` (real Ed25519)
- Cold start: seed nodes with pre-sharded popular models
- $DOPA economy with verified work (paid inference + staking)
- Health check: `/api/health` with active features, missing deps, node status
- Hub discovery without hardcoded IPs (`mesh/hub_discovery.py`)
- Paid inference API: `POST /api/inference` + `GET /api/inference/{id}`
- DAO governance: proposal, quadratic voting, tally, auto-execute
- Staking flow: mint → stake → access rights → inference
- **Gate: PASS 7/7** | `2485b6f5`

**Remaining:** Documentation for non-architect users

### Horizon 3: Platform Layer — IN PROGRESS

**Goal:** Beyond AI — content hosting, synapse skills, marketplace.

**Track A: AI Mesh Maturity**
- [ ] Larger model support (7B+ with enough nodes)
- [ ] Model marketplace (upload, discover, install INGRVMs)
- [x] DAO governance for mesh decisions — `test_governance_dao.py` verified
- [x] Real Proof-of-Inference (Groth16 ZK proofs via circom + snarkjs)
- [ ] Speculative decoding: LAPTOP_RELAY runs draft model, PC_MASTER verifies in batch (2.6-2.9x throughput, see SLED/DSD papers)
- [ ] Dynamic shard scheduling: replace static assignment with Parallax-style dynamic programming for heterogeneous devices (github.com/GradientHQ/parallax)
- [ ] Upgrade libp2p from MOCK mode to real libp2p with WebTransport (QUIC-based, enables browser-tab nodes via PWA)
- [ ] Split learning privacy: add dimensionality-reducing cut layer between nodes to prevent input reconstruction from activations

**Track C: Synapse Skill Protocol (MCP-like Callable Skills)** — VERIFIED
- [x] Synapse Skill Schema: `SkillDefinition` with input/output JSON schemas, types (SKILL, KNOWLEDGE, MEDIA, COMPUTE, ORACLE, COMPOSITE)
- [x] Skill Executor, Skill Router, Skill Invocation API (7 endpoints)
- [x] LLM Orchestrator: query → skill discovery → invocation → synthesis
- [x] 5 seed skills auto-registered on hub boot
- [x] Creator royalties (configurable %, default 10%)
- [x] Reward wiring: skill invocations trigger $DOPA minting for hosting nodes

**Track B: Content Platform** — NOT STARTED (see `PLATFORM_VISION.md` for full design)
- [ ] Abstract transport layer: spike protocol carries general payloads
- [ ] File/content distribution (BitTorrent-style chunking)
- [ ] User-defined recommendation algorithms (algorithm synapses — see `PLATFORM_VISION.md` §3)
- [ ] Creator economy: content hosts earn $DOPA
- [ ] Knowledge synapse infrastructure: embedding-based retrieval, multi-host distribution, version management (see `PLATFORM_VISION.md` §1)
- [ ] GraphSAGE distributed knowledge maps: each node builds local embeddings, shares aggregated embeddings via gossip — mesh collectively builds semantic search without centralized index
- [ ] Synapse creation pipeline: no-code creation via INGRVM AI, templates for power users, SDK for devs (see `PLATFORM_VISION.md` §6)
- [ ] Bluesky Ozone-style labeling: moderation labels as synapse type, independent labeling services, composable moderation (github.com/bluesky-social/ozone)

**Track F: Development Governance** — NOT STARTED (see `PLATFORM_VISION.md` §11)
- [ ] INGRVP proposal standard: draft → review → DAO vote → implement → deploy
- [ ] Fast-track process for critical security fixes (48-hour DAO vote)
- [ ] Node version negotiation: backward-compatible wire protocol, graceful degradation
- [ ] Upgrade signaling: nodes vote on protocol changes via adoption threshold
- [ ] MeritRank reputation: decay-based Sybil resistance alongside staking (transitivity, connectivity, epoch decay — solves cold-start when $DOPA has no market value)
- [ ] Soulbound reputation tokens: non-transferable tokens representing verified compute history (prevents reputation buying, separate from transferable $DOPA)
- [ ] Vote-escrowed governance: lock $DOPA for 1-4 years → boosted rewards (up to 2.5x) + voting power (Curve model for long-term alignment)
- [ ] x402 payment integration: HTTP-native micropayments (Linux Foundation, Coinbase, Google, Visa) as external payment rail — $DOPA stays internal accounting

**Track G: Public Presence & Growth** — NOT STARTED
- [ ] **Solana domains:** Register `ingrvm.sol` + `ghostinternet.sol` (+ `dopa.sol`, `meshbrain.sol` if available) via [sns.id](https://www.sns.id/). Requires Phantom wallet + ~$20 SOL/USDC per domain. Lifetime ownership, no renewals. Domains are NFTs — tradeable on Solana marketplaces.
- [ ] **INGRVM website (Vercel):** Next.js + Tailwind, mobile-first. Pages: landing/hero with mesh visualization, "How It Works" interactive simulation, waitlist (email + optional Phantom wallet connect for early $DOPA), FAQ/chatbot (INGRVM AI on docs), visual roadmap, links page. Deploy to Vercel free tier. Domain: `ingrvm.sol` resolves via SNS, plus traditional domain as fallback.
- [ ] **Interactive mesh simulation on website:** Use Gemini's 3D model generation or Three.js to build animated mesh visualization — nodes light up, spikes travel between devices, $DOPA accumulates in real-time. Embed on landing page as the "wow factor." Eventually this same page becomes the browser neuron (WebTransport + libp2p.js).
- [ ] **Waitlist system:** Email capture + optional Phantom wallet connect. Early signups get priority mesh access + genesis $DOPA allocation when network launches. Database: Supabase (free tier) or simple JSON on Vercel KV. Display signup count publicly ("423 neurons waiting").
- [ ] **AI chatbot on website:** Lightweight INGRVM knowledge bot trained on all INGRVM docs (PLATFORM_VISION, ROADMAP, Deep Dive, etc.). Answers "What is INGRVM?", "How does $DOPA work?", "How do I join?" etc. Implementation: embed a small LLM with RAG over docs, or use Claude API with INGRVM docs as context.
- [ ] **Linktree / bio links page:** Centralized link hub for all platforms. Links: GitHub repo, Instagram, TikTok, YouTube, website, waitlist, Discord (when ready). Use Linktree, Bento, or bio.link. Set up immediately so videos can reference "link in bio" with actual destinations.
- [ ] **Video b-roll asset pipeline:** Streamline graphics workflow — HTML b-roll assets should be accessible on phone without laptop transfer. Options: host on GitHub Pages (open URL on phone, screenshot), or generate as PNG during build. Current assets are mobile-responsive (viewport meta + clamp() fonts).

**Track E: Trust & Quality Layer** — NOT STARTED (see `PLATFORM_VISION.md` §10)
- [ ] Peer review synapses: reviews attach to target synapse, visible alongside query results
- [ ] Credential verification (optional): ORCID, professional license, institutional attestation, web-of-trust
- [ ] Community fact-check synapses: claim-by-claim analysis, fact-checkers earn $DOPA for reviews consumed
- [ ] Transparent trust signals: review count, creator track record, citation count, stake backing, fact-check results
- [ ] User-configurable trust weighting: integrate into algorithm synapses (e.g., "only show health synapses with verified reviews")
- [ ] Stake-backed claims: creators stake $DOPA behind high-stakes synapses, DAO dispute resolution for slashing
- [ ] Dispute flow: flag → respond → DAO vote → label (never remove) + slash/reward

**Infra foundations gate: PASS 7/7** | `2d90656f`

### Horizon 4: Research (Years 1-3)

- [ ] SpikeLLM: LLM architecture natively designed for SNN
- [ ] 1-bit training from scratch (BitNet-style)
- [x] Real ZK proofs (Groth16 via circom — verified)
- [ ] Neuromorphic hardware support (Intel Loihi)
- [ ] Media streaming optimization
- [ ] WAN mesh (beyond LAN)
- [ ] INGRVM AI agent: local LLM with tool-use layer for neuron management (see `PLATFORM_VISION.md` §5)
- [ ] App framework: COMPOSITE synapse renderer + Synapse SDK (Python/JS/WASM) (see `PLATFORM_VISION.md` §6)
- [ ] Mainstream clones v1: Neural Streams (audio), SpikeNet (social), Mesh Messaging (see `PLATFORM_VISION.md` §4)
- [ ] Rust core modules: ZK proofs, gossip protocol, spike encoding via PyO3 bindings (see `PLATFORM_VISION.md` §12)
- [ ] Mobile neuron core: Rust background service for Android/iOS (see `PLATFORM_VISION.md` §12)
- [ ] Offline-first sync protocol: deferred $DOPA settlement, message queueing, store-and-forward (see `PLATFORM_VISION.md` §8, §13)
- [ ] ExecuTorch / Google LiteRT-LM: evaluate as mobile inference runtime (50KB footprint, Qualcomm QNN delegate = 20x CPU speedup on mobile NPU)
- [ ] A2A protocol bridge: connect Synapse Skill Protocol to Google's Agent-to-Agent standard so external AI agents discover and use INGRVM synapses
- [ ] TEE-based inference privacy: Intel SGX/TDX or ARM TrustZone for sensitive queries without FHE performance overhead
- [ ] Persistent agent memory: INGRVM AI agent retains strategy, past interactions, and reputation context across restarts (ref: 0G Aristotle model)
- [ ] QuantSpec self-speculative decoding: draft model shares architecture with target via 4-bit quantized KV cache — up to 2.5x speedup, no separate draft model needed

### Horizon 5: Vision (Years 3-10)

Long-term aspirations, not engineering deliverables:
- Full YouTube/Instagram replacement at scale (see `PLATFORM_VISION.md` §4 clone map)
- BCI brain-link interfaces
- ASIC chip design for INGRVM-native hardware
- Sovereign web hosting
- Formal verification of mesh protocols
- Mesh-wide knowledge graph: cross-synapse citation linking, semantic discovery, collective intelligence layer (GraphSAGE distributed embeddings at scale)
- FHE-encrypted governance: private DAO voting using fully homomorphic encryption (Zama fhEVM, now on Ethereum mainnet)
- Browser-extension nodes: lightweight mesh participation via WebTransport without app install (ref: OptimAI 1M+ nodes model)
- Wi-Fi HaLow (802.11ah) for long-range outdoor mesh: sub-1GHz, 1km+ range, low power (gossip/coordination layer, not inference data)
- AI-assisted synapse creation at scale: "build me an app" → AI scaffolds composite synapse pipelines (see `PLATFORM_VISION.md` §6)
- Encrypted queries: homomorphic or ZK-based query privacy so hosts can't see what you're asking
- Decentralized academic peer review: replace journal paywalls with mesh-native open review + reputation

### Track B: Content Platform — NOT STARTED (Extended, see `PLATFORM_VISION.md`)

- [ ] Abstract transport layer: spike protocol carries general payloads, not just neural activations
- [ ] **Mesh messaging ("Mesh'aging"):** Direct peer-to-peer encrypted messaging using spike protocol. Messages are signed spikes routed through the mesh — no central server, no company reading them. Leverages existing Ed25519 identity system for end-to-end encryption. Lowest-effort high-impact feature since the transport layer already exists.
- [ ] File/content distribution: nodes host chunks of content (like BitTorrent)
- [ ] User-defined recommendation algorithms: algorithm synapses — user picks their ranking logic (see `PLATFORM_VISION.md` §3)
- [ ] Creator economy: content hosts earn $DOPA when their content is accessed
- [ ] Knowledge synapse v2: embedding-based retrieval, gossip distribution, multi-host serving, incremental updates (see `PLATFORM_VISION.md` §1)
- [ ] No-code synapse creation: INGRVM AI processes uploads into synapses (see `PLATFORM_VISION.md` §6)
- [ ] Synapse templates: knowledge base, music channel, blog, storefront, data feed, custom skill (see `PLATFORM_VISION.md` §6)
- [ ] Peer review layer: review synapses attach to targets, visible alongside results (see `PLATFORM_VISION.md` §10)
- [ ] Fact-check synapses: claim-by-claim analysis as a synapse type, fact-checkers earn $DOPA (see `PLATFORM_VISION.md` §10)
- [ ] Trust signals: transparent, decomposed — review count, credentials, track record, citations, stake (see `PLATFORM_VISION.md` §10)
- [ ] Stake-backed claims: creators put $DOPA behind high-stakes synapses, DAO slashing for disputes (see `PLATFORM_VISION.md` §10)

### Track D: Rights & Identity

- [ ] Mesh DNS: `mesh://` name resolution (DHT-backed, no central registrar)
- [ ] Mesh Handle verification: link `@artist` handles to verified external IDs (ISRC/EIDR/ISBN)
- [ ] Social key recovery: Guardian-based N-of-M account recovery for lost node keys
- [ ] Sybil resistance: Stake-weighted identity uniqueness scoring; penalize clusters of >N nodes per stake entity

### Honest Constraints

- Video streaming requires WiFi/internet bandwidth. Bluetooth (3 Mbps) handles small data only.
- Content platform is a different product from AI mesh — shared infrastructure, separate UI and UX.

---

## Remaining Work (Priority Order)

### Phase 1: Make It Run — DONE
- [x] Merge conflicts resolved
- [x] Security vulnerabilities patched (secrets scrubbed, PSK rotated)
- [x] Deployment configs fixed (Docker, systemd point to V3 hub)

### Phase 2: Test Infrastructure — IN PROGRESS
- [x] Hub server API tests (51 tests)
- [x] Create `conftest.py` (centralize path setup, remove sys.path hacks from 8 test files)
- [x] Create `pytest.ini` (standardize test discovery)
- [ ] Update CI workflow (add `trio` dep, set robust PYTHONPATH, remove hardcoded ignores)
- [x] Move 4 demo files to `demos/`, clean non-test scripts from `tests/`
- [ ] Install PyTorch in CI to unblock skipped tests

### Future Horizon 4 Items
- [ ] SpikeLLM: LLM architecture natively designed for SNN (coincidence detection replacing softmax attention, STDP learning)
- [ ] 1-bit training from scratch (BitNet-style, not post-hoc quantization)
- [x] Real ZK proofs for work verification (Circom circuits compiled, Groth16 proofs generated + verified via snarkjs)
- [ ] Neuromorphic hardware support (Intel Loihi, if available)
- [ ] Media streaming optimization for content platform
- [ ] WAN mesh (beyond LAN — real internet-scale distribution)
- [ ] Cross-chain bridge: DOPA ↔ ETH atomic swap (IBC-style relay node) — SOL is native chain
- [ ] Training data royalties: Attribute model training contributions; AI Collaboration Credits per token influenced
- [ ] Model poisoning detection: Cryptographic audit trail for weight integrity across shards
- [ ] Node ID pseudonymity: Onion-routed node identities for privacy-sensitive compute contributions

### Phase 2.5: Code Optimization — NOT STARTED
> Codebase audit (2026-04-08) found 55,683 LOC across 328 files. These fixes improve performance
> and maintainability without changing functionality. See `PLATFORM_VISION.md` §14 for precautions.

**Quick wins (< 1 hour each):**
- [ ] Add SQLite index on `accounts.node_id` in reward_engine.py (5-10x faster lookups)
- [ ] Pre-compute skill searchable text at registration in skill_router.py (10x faster at 100+ skills)
- [ ] Delete duplicate spike_protocol.py fallback class (-170 lines, single bug-fix location)
- [ ] Consolidate duplicate API endpoints in hub_api.py (`/api/skills/*` and `/api/synapse/skills/*` overlap)
- [ ] Cache nvidia-smi output for 30s in hub_server.py (66% less subprocess overhead)
- [ ] Move lazy imports to module level in hub_api.py (ModelRegistry imported 3 times inside functions)
- [ ] Purge mock scaffolding once real components verified (zk_proof_mock.py, MockBrain, spike_protocol fallbacks) — reduces cognitive load for new contributors

**Medium effort:**
- [ ] Split hub_api.py (2,158 lines, 91 routes) into focused routers: mesh, skills, rewards, marketplace, media
- [ ] Add SQLite connection pooling in reward_engine.py (18 separate `sqlite3.connect()` calls → pool)
- [ ] Replace JSON serialization with msgpack in p2p_gossip.py (35-50% message size reduction)
- [ ] Replace log polling (0.5s sleep loop) with file watcher in hub_server.py (50-70% idle CPU reduction)
- [ ] Single-pass payout calculation in reward_engine.py (currently iterates all nodes twice)
- [ ] Batch config saves in config.py (currently writes to disk on every `set()` call)

### Phase 3: Make It Deployable
- [ ] Deploy hub to VPS (Docker on Hetzner/Vultr)
- [ ] HTTPS via reverse proxy or Cloudflare Tunnel
- [ ] Monitoring + alerting beyond `/api/health`
- [ ] Auto-restart via systemd
- [ ] Periodic DB backup script
- [ ] Deploy PWA to Vercel/Netlify

### Phase 3.5: Pre-Launch Safety (see `PLATFORM_VISION.md` §14)
> These MUST be done before any public user touches the mesh.

- [ ] **Key backup during onboarding**: Force seed phrase export or encrypted cloud backup before first $DOPA earned. Lost key = lost everything.
- [ ] **Battery/storage controls**: User-configurable thresholds — inference only while charging, stop hosting below 30% battery, max synapse storage budget
- [ ] **Synapse registration stake**: Small $DOPA deposit to publish a synapse (returned on delete). Anti-spam without gatekeeping.
- [ ] **Transparent economics**: Dashboard shows exactly what you earn, why, and what it's worth. No hidden mechanics.
- [ ] **Legal disclaimer**: Node operators understand they may host third-party content. Know-your-jurisdiction warning.
- [ ] **Version negotiation protocol**: Backward-compatible wire format. Old nodes talk to new nodes gracefully. No forced upgrades.
- [ ] **Social key recovery**: Guardian-based N-of-M account recovery (Track D item, but critical for launch safety)

### Phase 4: Make It Usable
- [ ] Onboarding flow: join mesh from UI without CLI (critical UX gap)
- [ ] Mobile UI polish: loading states, error boundaries, responsive
- [ ] Capacitor APK: side-loadable Android app
- [ ] Documentation for non-architect users
- [ ] Web dashboard served by hub's FastAPI

### Phase 5: Platform Features
- [ ] Skill marketplace UI (discovery, search, install, ratings)
- [ ] Model marketplace UI (browse, download, version management)
- [ ] DOPA payment flow in UI (confirmation, history, staking controls)
- [ ] Solana devnet deployment ($DOPA as SPL token, test wallet flows)
- [ ] Solana mainnet launch ($DOPA token, liquidity pool, Phantom wallet integration)
- [ ] Shard rebalancing (dynamic redistribution on node join/leave)
- [ ] KV cache sync endpoints on hub
- [ ] Pipeline parallelism for autoregressive generation
- [ ] WiFi Direct mesh (Android Nearby Connections API)
- [ ] Transport abstraction layer (WiFi Direct → LAN → WAN fallback)

### Track E: Mobile/Edge Inference — Hardware Acceleration
Goal: Every device contributes real compute to the mesh using its best available hardware (TPU, GPU, NPU, CPU). PyTorch is the dev/conversion tool; inference nodes run lightweight runtimes.

- [ ] **ONNX Runtime inference backend**: Convert GPT-2 shards to ONNX format. Replace `torch.load()` + manual tensor ops in `GPT2ShardWorker` with `onnxruntime.InferenceSession`. ONNX Runtime is ~50MB (vs PyTorch 2GB), has official ARM64/Android wheels, and supports NNAPI execution provider for automatic TPU/GPU/DSP routing on Android. Socket protocol and gossip layer unchanged.
- [ ] **TFLite delegation for Pixel TPU**: For the Android app, use TensorFlow Lite with Tensor G3 TPU delegate. Maximum hardware utilization on Pixel devices. Shard worker becomes a native Android service that the gossip layer talks to via localhost API.
- [ ] **Abstract inference backend**: `GPT2ShardWorker` becomes a thin interface. Backends: `TorchBackend` (dev/GPU), `ONNXBackend` (cross-platform), `TFLiteBackend` (Android TPU). Nodes advertise their hardware capabilities via gossip so the orchestrator can optimize shard assignment (e.g., give attention-heavy layers to TPU nodes, embedding layers to CPU nodes).
- [ ] **NPU energy optimization**: Route inference to NPU via ONNX NNAPI / ExecuTorch QNN delegate — 10-20x less power than CPU. Energy budget system: user sets max battery drain/hour, core throttles when approaching limit, full utilization when charging. (see `PLATFORM_VISION.md` §8b)
- [ ] **Browser-based neurons (WebTransport)**: libp2p.js + WebTransport + WASM neuron core. Browser tab becomes a mesh node — no install, no Python. Hosts small knowledge synapses, participates in gossip, earns $DOPA. Onboarding funnel to native app. (see `PLATFORM_VISION.md` §8c)
- [ ] **WebNN inference**: Chrome 127+ WebNN API routes ML ops to device GPU/NPU from browser. Light inference tasks (sentiment, classification, embeddings) from a browser tab with native hardware acceleration.
- [ ] **Ad-hoc mesh / Ghost Internet**: WiFi Direct + WiFi Aware (NAN) for direct device-to-device mesh without any router or ISP. Multi-hop routing — nodes relay packets for each other. INGRVM becomes the network layer itself. Termux can already create WiFi hotspots as a near-term bridge.
- [ ] **Cross-network (WAN) mesh**: Cloudflare Tunnel / Tailscale for near-term. DHT-based peer discovery (Kademlia) for fully decentralized WAN without relay servers. Bootstrap relay VPS as optional fallback. Existing `hole_puncher.py` and `circuit_relay.py` as starting points.

### Phase 6: Scale & Research
- [ ] 7B+ model support (multi-device memory pooling)
- [ ] WAN NAT traversal (hole_puncher hardening, STUN/TURN)
- [ ] TinyLlama 1.1B sharding + cross-device test
- [ ] Content platform (Horizon 3 Track B)
- [ ] Solarpunk Exchange UI + extended integrations
- [ ] Consumer apps: React Native mobile, Tauri desktop
- [ ] SpikeLLM research
- [ ] Neuromorphic hardware integration

---

## Strategic Pillars

### Consumer Apps
Nobody will use a CLI. Priority: mobile PWA → Capacitor APK → desktop (Tauri) → web dashboard. All connect to hub API at `hub_url:8000/api/*`.

### Solana Integration
$DOPA launches as an SPL token on Solana. Path: devnet deployment → wallet integration (Phantom) → mainnet token launch → liquidity pool (Raydium/Orca) → `ingrvm.sol` domain via SNS. Solana chosen for speed (<400ms finality), low fees (<$0.01), and ecosystem alignment with consumer crypto (Phantom wallet, Jupiter aggregator, SNS domains).

### Solarpunk Vision
Data models and API endpoints built. Skills profiles, service requests, trust scores, barter matching. Extended integrations: community energy grid, seed exchange, repair cafe, mutual aid, commons mapping, carbon offset ledger, local food networks, knowledge commons, cooperative governance, resilience scoring.

### Offline Mesh
WiFi Direct (250 Mbps, 200m range) as primary offline transport. Bluetooth LE for discovery + small messages only. LoRa for heartbeat/signaling in rural areas. Fallback chain: WiFi Direct → LAN → WAN relay.

### Horizon 5 Vision Items
- [ ] Live streaming: Neural Stream real-time protocol (< 500ms latency, mesh-native HLS replacement)
- [ ] Content decay economics: Storage fees scale with demand decay; zero-fee for high-traffic content, auto-purge for abandoned content
- [ ] Offline mesh operation: Delay-tolerant networking (DTN) for air-gapped / rural meshes with eventual consistency
- [ ] Off-mesh fiat rails: DOPA ↔ fiat on/off ramp via custodial bridge node (KYC-gated, sovereign-optional)

---

## Anti-Hallucination Rules

1. No horizon can be claimed complete without a `phase_gate.py` PASS
2. Higher horizons cannot start until lower horizon risk checkpoints pass
3. Agents must include gate report hash when claiming completion in mail
4. This ROADMAP is the only planning authority — journal entries and mail are historical claims
5. "CODED" is not "DONE" — code without gate evidence is unverified

**Gate command:** `cd INGRVM/Core && python -m tools.phase_gate <horizon>`

---

## Tier Mapping

| Horizon | Phase Tiers | Status |
|---------|-------------|--------|
| Prerequisites | Tier A (Phases 1-4) | VERIFIED |
| Horizon 0 | Tier B work (Phase 7+) | VERIFIED |
| Horizon 1 | Tier B completion + new | VERIFIED |
| Horizon 2 | Tier C (Phases 8-9) | VERIFIED |
| Horizon 3 | Tier D (Phases 10-13) | IN PROGRESS |
| Horizon 4-5 | Tier E (Research) | ASPIRATIONAL |

---

## Archived Docs

The following files were merged into this document and moved to `99_LEGACY/`:
- `PHASE_STATUS.md` — phase tracker (gate evidence embedded in Current State)
- `MASTER_ROADMAP.md` — strategic pillars (merged into Strategic Pillars)
- `PLAN_TO_LIVE.md` — MVP checklist (merged into Remaining Work)
- `SESSION_PREP.md` — execution plan (merged into Remaining Work)
- `POST_RELEASE_ROADMAP.md` — aspirational post-MVP (referenced in Horizons 4-5)
- `ROADMAP_LAPTOP.md` — device-specific (incorporated into mesh-wide planning)
- `ROADMAP_DEPENDENCIES.md` — dependency mapping (incorporated into horizon task lists)
- `LIVE_MESH_VALIDATION_ROADMAP.md` — validation steps (incorporated into Horizons 0-1)
