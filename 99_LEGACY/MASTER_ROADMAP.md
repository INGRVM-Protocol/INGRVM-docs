# INGRVM Master Roadmap — Apps, Mainnet, Content, Mesh & Research

**Last Updated:** 2026-03-24

---

## Current Status

| Layer | Status | Evidence |
|-------|--------|----------|
| Core mesh (3-device) | DONE | PC↔Laptop↔Mobile over LAN verified |
| $DOPA economy | DONE | Staking, paid inference, rewards — 118 tests |
| Synapse Skills (6 types) | DONE | SKILL, KNOWLEDGE, MEDIA, COMPUTE, ORACLE, COMPOSITE |
| Creator royalties | DONE | Author gets % of every invocation |
| Skill marketplace | DONE | SQLite registry, ratings, stats, search |
| ZK proofs (Groth16) | DONE | Circom compiled, snarkjs verify, tamper rejection |
| Bittensor SDK | DONE | v10.2.0 installed, wallet creation automated |
| Bittensor testnet | BLOCKED | Needs Test TAO from Discord faucet |
| Consumer apps | NOT STARTED | CLI only — no mobile/desktop/web app |

---

## 1. Consumer Apps

### Why This Is Priority #1
Nobody will use a CLI. The entire $DOPA economy depends on people running nodes, which means one-click install on phones, laptops, and desktops.

### 1A. Mobile App (React Native / Expo)
- Single codebase: Android + iOS
- **Screens:** Dashboard (node status, $DOPA balance), Skill Store, Media Player, Creator Studio, Settings
- **Connects to:** Hub API at `hub_url:8000/api/*`
- **Directory:** `INGRVM/Mobile/app/`

### 1B. Desktop App (Tauri)
- Lighter than Electron. Rust backend, web frontend. Windows + macOS + Linux.
- Same screens as mobile + GPU monitoring dashboard + tray icon
- **Directory:** `INGRVM/Desktop/`

### 1C. Web Dashboard
- Static HTML/JS/CSS served by the hub's FastAPI
- No install — visit `http://hub-ip:8000`
- Shows: mesh topology, node count, skill catalog, $DOPA stats
- **Directory:** `INGRVM/Core/api/static/`

### 1D. DIY Hardware Support
- Setup wizard in app: auto-detects hardware, shows tier + expected earnings
- Known NPU chipset database: Tensor G3, Snapdragon 8 Gen 3, Apple Neural Engine, Coral TPU, BrainChip Akida, Jetson Nano
- Hardware profile registered on join: `POST /api/node/register`

---

## 2. Mainstream App Clones on INGRVM

The apps in Section 1 are **frontends**. These are the **decentralized backends** they connect to.

### 2A. INGRVM Music (Spotify Equivalent)
**How it works:**
1. Artist uploads track → Hub splits into N chunks → chunks distributed to hosting nodes
2. Listener requests stream → chunks served from nearest hosting node
3. **Creator gets 70% royalty** per play, hosting nodes get 30% (split by chunks served)
4. **Fans host songs on their node** = earn passive $DOPA when others stream through them

**API:**
- `POST /api/media/upload` — Upload audio + metadata
- `GET /api/media/{id}/stream` — Chunked streaming
- `POST /api/media/{id}/host` — Volunteer to host
- `GET /api/media/trending` — Popular by play count
- `GET /api/media/creator/{id}` — Creator's catalog + earnings

**Anti-piracy:** Content-addressed chunks (CID integrity), encrypted streaming (payment proof required for decryption key), rate limiting. Pragmatic, not DRM — make it easier to pay than steal.

### 2B. INGRVM Video (YouTube Equivalent)
Same as music but with larger chunks (HLS-style segments), transcoding COMPUTE skills, thumbnail generation, search via KNOWLEDGE synapse.

### 2C. INGRVM Social (Instagram Equivalent)
- Posts = MEDIA synapses (images + captions)
- Feed algorithm = user-chosen SKILL synapse (you pick your own algorithm)
- Followers host your content (social graph = hosting graph)
- Creator economy: sponsored posts, tips

### Build Order
1. **Music first** — smallest files, clearest economics
2. **Video second** — same infra, bigger, needs transcoding
3. **Social third** — needs social graph, moderation

---

## 3. Bittensor: Testnet → Mainnet

### Phase 1: Testnet (Current Blocker: Test TAO)
1. Run `python -m tools.bittensor_setup` → auto-creates wallet, prints SS58 address
2. Paste address in Bittensor Discord #faucet → get Test TAO (5 min wait)
3. Run script again → auto-registers on subnet
4. Validate weight sync for 1-2 weeks

### Phase 2: Subnet Application
1. Write subnet proposal (what miners do, how validators score)
2. Register INGRVM subnet on mainnet (~1 TAO / ~$400)
3. Publish miner/validator code as open-source
4. Configure emission schedule and incentive mechanism

### Phase 3: Mainnet Launch
1. PC_MASTER becomes first validator
2. Open miner registration — anyone joins by running the app
3. Miners serve skills, host media, run compute
4. Validators verify via ZK proofs + reputation
5. Bittensor distributes TAO based on Yuma Consensus
6. $DOPA ↔ TAO bridge: mesh rewards map to on-chain TAO

### Phase 4: Scale
1. Multiple validators (decentralize beyond PC_MASTER)
2. Cross-subnet interoperability
3. DEX listing for $DOPA/TAO pair
4. Fiat on-ramp via partner exchange

---

## 4. Synapse System — Remaining Work

### Already Built
- 6 synapse types (SKILL, KNOWLEDGE, MEDIA, COMPUTE, ORACLE, COMPOSITE)
- Creator royalties (configurable %, default 10%)
- Skill marketplace (publish, search, rate, stats, creator earnings)
- Composite pipelines (chain skills: translate → summarize)
- 15 API endpoints for skills

### Still Needed
- [ ] Comprehensive Synapse Creator Guide (code examples for each type)
- [ ] Semantic skill matching (sentence-transformers embeddings vs keyword overlap)
- [ ] Skill dependencies (declared in schema, resolved at invocation)
- [ ] Rate limiting per skill
- [ ] Access control (public vs private, subscription tiers)
- [ ] Skill analytics dashboard for creators

---

## 5. Offline Mesh

### What WiFi Direct Is
Two devices with WiFi chips create a **direct connection without any router, internet, or cell service**. One device acts as a mini access point, the other connects. 250 Mbps, 200m range. Built into every Android phone and most laptops. This is how AirDrop works. "Offline" = no ISP, no cloud, no router. Just two nearby devices.

### Transport Technologies

| Technology | Speed | Range | Use Case |
|-----------|-------|-------|----------|
| **WiFi Direct** | 250 Mbps | 200m | Primary offline transport |
| **Bluetooth LE 5** | 2 Mbps | 100m | Discovery + small messages only |
| **LoRa** | 50 kbps | 10 km | Heartbeat/signaling in rural areas |
| **WebRTC** | Internet speed | Global | Browser-to-browser (no app) |

### Implementation Plan
1. **Transport abstraction** — `mesh/transport_base.py` with pluggable backends (TCP, UDP, WiFi Direct, file)
2. **WiFi Direct backend** — Android WiFi Direct via Wi-Fi Aware API
3. **Multi-transport** — Nodes auto-negotiate best available (WiFi Direct nearby, TCP over internet, file relay as fallback)

---

## 6. Hardware & Earnings

### What Already Works
- Tier 1-4 ranking (CPU cores + VRAM)
- Earnings multipliers: NPU=1.5x, ARM=1.2x, GPU=1.0x, CPU=0.5x
- NPU detection via ONNX NNAPI provider
- Hardware tier passed through skill invocation rewards

### Still Needed
- [ ] Auto-detection on `python -m ingrvm join` (run hardware_ranker, send results to hub)
- [ ] NPU chipset database (known chips → expected tiers)
- [ ] DIY hardware guide (Coral TPU, Akida, Jetson Nano setup)

---

## 7. Deployment & Operations

| Task | Status |
|------|--------|
| Hub server | DONE (`python hub_server.py`) |
| Docker | PARTIAL (Dockerfile exists, needs compose + bittensor) |
| Public URL | NOT DONE (Cloudflare Tunnel or port forwarding) |
| HTTPS | NOT DONE (reverse proxy) |
| Auto-restart | NOT DONE (systemd unit) |
| Monitoring | PARTIAL (`/api/health` exists, needs alerting) |
| Backups | NOT DONE (periodic DB backup script) |
| CI/CD | NOT DONE (GitHub Actions) |

### Files to Create
- `docker-compose.yml` — Full stack
- `deploy/ingrvm-hub.service` — systemd unit
- `deploy/cloudflare-setup.md` — Tunnel guide
- `.github/workflows/test.yml` — CI pipeline

---

## 8. Research Agenda

| Topic | Complexity | Timeline | Current State |
|-------|-----------|----------|---------------|
| **SpikeLLM** | PhD-level | 6-12 months | `neural/spike_llm.py` prototype |
| **1-bit training** | Research | 2-4 weeks focused | `bnn_layer.circom` XNOR-Popcount done |
| **Neuromorphic HW** | Hardware | When budget allows | BrainChip Akida ($500) most accessible |
| **WAN mesh** | Engineering | 1-2 months | hole_puncher.py + circuit_relay.py exist |

---

## Priority Order

### Immediate (Next Sessions)
1. MASTER_ROADMAP.md + Synapse Creator Guide
2. Bittensor testnet registration (1 manual step: Discord faucet)
3. Web dashboard (static HTML served by hub)
4. MEDIA synapse: chunked upload/stream/host endpoints

### Short-term (Next Month)
5. React Native mobile app skeleton
6. Music streaming proof-of-concept
7. Docker compose for deployment
8. Transport abstraction layer

### Medium-term (1-3 Months)
9. Tauri desktop app
10. Video streaming
11. Bittensor mainnet subnet
12. WiFi Direct offline transport
13. Social features

### Long-term (3-12 Months)
14. Neuromorphic hardware integration
15. SpikeLLM research
16. 1-bit training
17. WAN mesh at scale
18. DEX listing / fiat on-ramp
