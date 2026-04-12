# INGRVM: Plan to Live

**Date:** 2026-03-24
**Status:** Draft — Awaiting Architect Approval
**Scope:** Everything between current state and public-testable MVP

---

## Current State Summary

### Verified & Working (323 tests, gate-passed)
- Spike Protocol (encryption, serialization)
- $DOPA Economy (minting, halving, reputation)
- DAO Governance (proposals, quadratic voting, auto-execution)
- Distributed Inference (GPT-2 cross-shard, coherent generation)
- ZK Proofs (Groth16 via circom + snarkjs)
- Hub Discovery (UDP broadcast + bootstrap fallback)
- Paid Inference (payment → execution → reward)
- Synapse Skill Protocol (5 seed skills, router, orchestrator)
- Staking Flow (mint → stake → access rights)
- Phase 7.2 Security (all POST endpoints verified, weights_only on all torch.load)
- Phase 7.3 Physical Mesh (2-node live mesh verified)

### Coded but Unverified
- P2P Gossip (libp2p fallback to MOCK mode)
- Media Store (chunked upload, no live streaming)
- Mobile UI (minimal boilerplate, ~2KB App.jsx)
- PyTorch/ONNX Mobile Bridges (fragile on Termux)
- Skill Marketplace (publish only, no discovery/install)
- Hole Puncher (exists, untested)

### Not Started
- WiFi Direct / Bluetooth Mesh
- Live Streaming Pipeline
- Content Platform (Horizon 3 Track B)
- 7B+ Model Support
- Cross-device KV Cache Sync
- Real Mobile UI Components
- User Onboarding Flow
- Solarpunk Exchange Layer

---

## Deployment Strategy

### Phase 1: PWA (Fastest path to testing)
- Add `manifest.json` with app icons, theme, display: standalone
- Add service worker for offline caching
- Install `vite-plugin-pwa`
- Deploy UI to Vercel/Netlify
- Result: Installable from browser on any device

### Phase 2: Capacitor APK (Native features)
- Wrap React app with Capacitor
- Enable WiFi Direct via Android Nearby Connections plugin
- Enable background sync, push notifications
- Side-load APK for testing
- Result: Real Android app without app store

### Phase 3: App Store (Public distribution)
- Google Play submission (APK signing, review process)
- Apple App Store (requires iOS Capacitor build + Mac for signing)
- Result: Public availability

### Backend Deployment
- Hub server: Docker on VPS (Hetzner/Vultr) or Akash Network
- Lighthouse node: `lighthouse-compose.yml` already ready
- CI/CD: GitHub Actions already running pytest

---

## MVP Checklist (Must-Have Before "Live")

### 1. Mobile UI Build-Out
- [ ] Wallet view (DOPA balance, transaction history, staking)
- [ ] Mesh status dashboard (connected nodes, shard map, health)
- [ ] Inference chat interface (send prompt, see distributed generation)
- [ ] Skill browser (browse/search synapse skills)
- [ ] Settings/profile (node config, identity, key management)
- [ ] Onboarding flow (join mesh without CLI)

### 2. PWA Infrastructure
- [ ] manifest.json with icons and theme
- [ ] Service worker for offline support
- [ ] vite-plugin-pwa integration
- [ ] Vercel/Netlify deployment config

### 3. Hub Server Hardening
- [ ] API endpoint tests (currently ZERO coverage)
- [ ] WebSocket connection tests
- [ ] Mesh state sync tests
- [ ] Error handling for network failures
- [ ] Reconnection logic for dropped nodes

### 4. Test Coverage Gaps
- [ ] Hub server API tests
- [ ] Governance DAO integration tests
- [ ] Mesh sync consensus tests
- [ ] Config management tests
- [ ] Install PyTorch in CI so skipped tests actually run

### 5. Stable 2-Node Mesh
- [ ] PC + Phone reliable connection
- [ ] Auto-reconnection on network drop
- [ ] Shard rebalancing when nodes join/leave
- [ ] Health monitoring with alerts

### 6. At Least 1 Practical Model
- [ ] Fine-tuned or curated model beyond base GPT-2
- [ ] Model download + auto-shard on join
- [ ] Inference quality validation

---

## Beta Checklist (Should-Have)

### 7. WiFi Direct Mesh
- [ ] Android Nearby Connections API integration
- [ ] WiFi Direct group formation
- [ ] Integrate with shard manager for layer routing
- [ ] Fallback chain: WiFi Direct → LAN → WAN

### 8. Streaming & Playback
- [ ] Pipeline parallelism (token N on node A while token N-1 on node B)
- [ ] KV cache prefetching across network
- [ ] Client-side output buffer for latency absorption
- [ ] Adaptive bitrate for media content

### 9. Skill Marketplace
- [ ] Persistent skill registry
- [ ] Discovery + search UI
- [ ] Install/update flow
- [ ] Rating + review system
- [ ] DOPA payment for premium skills

### 10. Model Marketplace
- [ ] Model catalog with metadata
- [ ] Download + auto-shard CLI
- [ ] Version management
- [ ] Community model submissions

### 11. Capacitor APK
- [ ] Capacitor project setup
- [ ] Native plugin integration (WiFi Direct, biometrics)
- [ ] APK build + signing pipeline
- [ ] Side-load testing on Pixel 8

---

## v1.0 Checklist (Nice-to-Have)

- [ ] App store submission (Google Play, Apple)
- [ ] 7B+ model support (requires multi-device memory pooling)
- [ ] WAN mesh with NAT traversal (hole puncher hardening)
- [ ] Bluetooth mesh fallback
- [ ] FHE for private inference
- [ ] WebRTC for browser-native P2P
- [ ] CRDT-based offline-first state sync

---

## Solarpunk Exchange Layer

### Core Features
- **Skill Profiles** — users list offerings (carpentry, tutoring, coding, gardening)
- **Service Requests** — post needs, matched by proximity + skill tags
- **DOPA Payment** — pay with tokens earned from mesh compute
- **Barter Matching** — equivalent-value service swaps
- **Time Banking** — 1 hour of any service = 1 hour credit

### Trust & Reliability Framework
- **Borrower/Lender Ratings** — timeliness, item condition, communication
- **Tool Library** — community tool sharing with check-in/check-out
- **Deposit Staking** — stake DOPA as collateral, returned on good behavior
- **Reputation Decay** — scores decay if inactive, prevents gaming
- **Dispute Resolution** — DAO-voted arbitration

### Extended Solarpunk Integrations
1. **Community Energy Grid** — track solar output, share surplus, DOPA for contributions. Renewable-powered nodes get bonus reputation.
2. **Seed & Plant Exchange** — seasonal swaps, grow logs, harvest sharing, local growing calendars.
3. **Repair Cafe Network** — map repair events, track items saved from landfill, skill-share for fixing things.
4. **Mutual Aid Circles** — neighborhood resource pooling (groceries, childcare, elder care). DOPA-backed commitments.
5. **Commons Mapping** — community maps of shared resources: water refills, free libraries, gardens, tool libraries, WiFi spots.
6. **Carbon Offset Ledger** — track personal carbon impact. Mesh compute < centralized cloud — quantify savings.
7. **Local Food Networks** — connect backyard growers with neighbors for surplus distribution.
8. **Knowledge Commons** — community-owned guides, tutorials, local history. Stored on the mesh — decentralized neighborhood wiki.
9. **Cooperative Governance** — extend DAO to real-world decisions: park schedules, garden plots, tool library hours.
10. **Resilience Score** — community-level metric: food, energy, tools, skills, connectivity self-sufficiency. Gamified progress.

---

## Modern Internet Features to Incorporate

| Technology | Use Case | Priority |
|-----------|----------|----------|
| **WebRTC** | Browser-native P2P, works through NATs | High |
| **ActivityPub / AT Protocol** | Federated social layer for skill sharing | Medium |
| **CRDTs** | Conflict-free offline-first mesh state | High |
| **WebTransport** | UDP-like unreliable streams for real-time inference | Medium |
| **Web Workers + WASM** | In-browser inference without backend | Medium |
| **Web Push API** | Notifications for inference completion, mesh events | High |
| **IndexedDB** | Local persistence for offline-first mobile | High |
| **Passkeys / WebAuthn** | Passwordless auth via device biometrics | Medium |

---

## Streaming Architecture (WiFi Direct)

### Bandwidth Reality
| Transport | Throughput | Latency | Use Case |
|-----------|-----------|---------|----------|
| WiFi Direct | 50-100 Mbps real | <5ms | Primary mesh transport |
| LAN WiFi | 50-300 Mbps | 2-5ms | Home network mesh |
| Bluetooth 5.0 | ~3 Mbps | 10-30ms | Fallback only |
| WAN (Internet) | Variable | 50-200ms | Remote nodes |

### Seamless Playback Architecture
1. **Pipeline Parallelism** — overlap computation across nodes
2. **KV Cache Prefetch** — send cache state ahead of activation
3. **Client Buffer** — absorb inter-hop latency (minimum 500ms buffer)
4. **Adaptive Quality** — degrade gracefully based on network health
5. **Shard-Aware Routing** — route through lowest-latency path

### Layer Handling Between Nodes
- ShardManager assigns layer ranges per node (e.g., layers 0-5 on phone, 6-11 on PC)
- Thalamus router forwards activations at layer boundaries
- Current limitation: synchronous hop-by-hop (no pipelining yet)
- Needed: async pipeline with prefetch buffers at each hop

---

## Test Suite Status

### What Tests Actually Do
- **Real functional tests** — not syntax checks. Examples:
  - Distributed inference spawns 2 processes, shards GPT-2, verifies coherent text over sockets
  - ZK proof lifecycle with Schnorr verification (20 tests)
  - Shard registration, discovery, routing, load balancing (19 tests)

### Why Tests Skip (~30-50% skip cold)
- No PyTorch → skips KV cache, distributed inference, PC core
- No py-ecc → skips Schnorr proof tests
- No hub running → skips live connectivity
- No thalamus → skips throughput, node lifecycle

### Critical Untested Components
| Component | Risk Level |
|-----------|-----------|
| Hub Server API endpoints | CRITICAL |
| Governance DAO integration | CRITICAL |
| Mesh sync/gossip consensus | CRITICAL |
| Actual model loading on hardware | HIGH |
| Mailroom protocol | MEDIUM |
| Config management | MEDIUM |

### Recommended Actions
1. Add hub server API endpoint tests
2. Add governance DAO integration tests
3. Install PyTorch in CI to unblock skipped tests
4. Convert simulation files (print-only) to real assertions
5. Add mesh sync consensus tests

---

## Priority Order (Recommended Execution Sequence)

1. **PWA setup** — manifest + service worker + Vercel deploy (1 session)
2. **Mobile UI components** — wallet, mesh status, chat, skills (2-3 sessions)
3. **Hub server tests** — fill the critical coverage gap (1 session)
4. **Onboarding flow** — join mesh from UI without CLI (1 session)
5. **Solarpunk data models** — skill profiles, trust scores, tool library schema (1 session)
6. **WiFi Direct integration** — Capacitor + Nearby Connections (2 sessions)
7. **Streaming pipeline** — pipeline parallelism + KV prefetch (2 sessions)
8. **Skill marketplace UI** — discovery, install, ratings (1-2 sessions)
9. **Capacitor APK build** — side-loadable Android app (1 session)
10. **Solarpunk exchange UI** — service posting, barter matching, reviews (2-3 sessions)
