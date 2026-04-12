# INGRVM Session Prep — Remaining Work (as of 2026-03-25)

> Cross-referenced from: ROADMAP.md, PHASE_STATUS.md, PLAN_TO_LIVE.md, SECRETS_AUDIT.md, test suite audit

---

## WHAT'S VERIFIED & DONE

| Area | Status | Tests |
|------|--------|-------|
| Horizons 0-2 | Gate PASSED | 323+ |
| Spike Protocol | Verified | ✓ |
| $DOPA Economy | Verified | ✓ |
| Ed25519 Identity | Verified | ✓ |
| DAO Governance | Verified | 10 |
| Distributed Inference (GPT-2 124M) | Verified | ✓ |
| ZK Proofs (Groth16) | Verified | 15 |
| Hub Discovery (UDP + bootstrap) | Verified | 12 |
| Paid Inference Loop | Verified | 13 |
| Synapse Skill Protocol (Track C) | Verified | 49 |
| Phase 7.2 Security Hardening | Verified | 16 |
| Physical 2-Node Mesh | Verified | 4 |
| Staking Flow | Verified | 8 |
| Media Streaming (chunked) | Verified | 16 |
| Hub Server API Tests | **NEW (this session)** | 51 |
| Secrets Remediation (PSK, tokens) | **NEW (this session)** | — |
| Mobile UI (6 views + PWA) | **NEW (this session)** | — |
| Mesh Health Monitor | **NEW (this session)** | — |
| Solarpunk Exchange (data models + API) | **NEW (this session)** | — |

**Total tests across suite: ~378**

---

## REMAINING WORK — ORDERED BY PRIORITY

### TIER 0: SECURITY (Do First in Next Session)

| # | Task | Detail | Est. |
|---|------|--------|------|
| 0.1 | **Regenerate mesh PSK** | Run `python -c "import secrets; print(secrets.token_hex(32))"`, update INGRVM/.env on all nodes | 5 min |
| 0.2 | **Rotate Google API key** | `AIzaSyCnMK7KP-_uT_jVrUxDJ-S07F24ibNq0ko` duplicated in 4 .env files (JARVIS/Sentinel, DnD Gate_Breakers, Isekai, narrator_tool) — rotate in Google Cloud Console, use project-specific keys | 15 min |
| 0.3 | **Rotate Supabase credentials** | `DnD_Campaign/narrator_tool/.env` — rotate JWT anon key in Supabase dashboard, verify RLS policies | 10 min |
| 0.4 | **Verify .gitignore coverage** | Ensure ALL .env files are gitignored; scan for any committed secrets | 5 min |

---

### TIER 1: CRITICAL TESTING GAPS (1-2 Sessions)

| # | Task | Detail | Est. |
|---|------|--------|------|
| 1.1 | **WebSocket connection tests** | Test /ws endpoint, connection lifecycle, broadcast, disconnect handling | 1hr |
| 1.2 | **Mesh state sync tests** | Test topology updates, TTL pruning, peer registration lifecycle | 1hr |
| 1.3 | **Solarpunk exchange tests** | Test skill profiles, service requests, trust scores, barter matching from mesh/solarpunk.py | 1hr |
| 1.4 | **Reconnection monitor tests** | Test mesh/reconnection.py — node degradation, backoff, recovery | 1hr |
| 1.5 | **Fill 6 empty test files** | test_dpos_auction.py, test_inter_mesh_stress.py, test_lava.py, test_p2p_node.py, test_port.py, test_visualizer_density.py — write or delete | 2hr |
| 1.6 | **Config management tests** | Test config.py, INGRVMConfig loading/validation | 30min |
| 1.7 | **Install PyTorch in CI** | Would unblock 30-50% of currently-skipped tests. Add to GitHub Actions workflow | 30min |

---

### TIER 2: MOBILE DEPLOYMENT PIPELINE (1-2 Sessions)

| # | Task | Detail | Est. |
|---|------|--------|------|
| 2.1 | **Vercel/Netlify deployment** | Deploy the PWA. Configure env vars (VITE_HUB_URL). Set up CI build on push | 30min |
| 2.2 | **Onboarding flow (no CLI)** | In-app "Join Mesh" wizard: enter hub URL → generate Ed25519 identity → register node → show QR code. This is the CRITICAL UX gap | 2hr |
| 2.3 | **Capacitor project setup** | `npx cap init`, add Android platform, configure for WiFi Direct plugins | 1hr |
| 2.4 | **APK build + signing** | Build debug APK, test side-load on Pixel 8 (or emulator) | 1hr |
| 2.5 | **Mobile UI polish** | Loading states, error boundaries, pull-to-refresh, toast notifications, responsive edge cases | 2hr |

---

### TIER 3: MESH ROBUSTNESS (2-3 Sessions)

| # | Task | Detail | Est. |
|---|------|--------|------|
| 3.1 | **Shard rebalancing** | When nodes join/leave, dynamically redistribute model layers. Currently static assignment only | 3hr |
| 3.2 | **KV cache sync endpoints** | Cross-device KV cache is tested (7/7) but endpoints not on hub. Wire `/api/cache/sync` and `/api/cache/prefetch` | 2hr |
| 3.3 | **Error handling for network failures** | Graceful degradation when hub unreachable, partial mesh operation, queue-and-retry for failed requests | 2hr |
| 3.4 | **Pipeline parallelism** | Token N on node A while token N-1 on node B. Would cut latency significantly for autoregressive generation | 4hr |
| 3.5 | **WAN NAT traversal** | `hole_puncher.py` exists but untested. Needs live testing, STUN/TURN fallback | 3hr |

---

### TIER 4: MODEL MATURITY (2 Sessions)

| # | Task | Detail | Est. |
|---|------|--------|------|
| 4.1 | **Fine-tuned model beyond GPT-2** | TinyLlama 1.1B is scaffolded. Implement sharding config, test cross-device | 3hr |
| 4.2 | **Model download + auto-shard on join** | New nodes should auto-download shards from hub on join. Hub endpoints exist (`/api/marketplace/{id}/download/{file}`), client-side flow missing | 2hr |
| 4.3 | **Inference quality validation** | Benchmark: coherence scoring, perplexity measurement, output comparison vs single-device | 2hr |
| 4.4 | **7B+ model support** | Requires multi-device memory pooling (significant engineering). Defer to Horizon 4 unless prioritized | 8hr+ |

---

### TIER 5: WiFi DIRECT & BLUETOOTH (2-3 Sessions)

| # | Task | Detail | Est. |
|---|------|--------|------|
| 5.1 | **Android Nearby Connections API** | Capacitor plugin for WiFi Direct discovery and group formation | 4hr |
| 5.2 | **WiFi Direct ↔ shard manager integration** | Route layer activations over WiFi Direct when LAN unavailable | 2hr |
| 5.3 | **Fallback chain** | WiFi Direct → LAN → WAN relay. Automatic fallback selection | 2hr |
| 5.4 | **Bluetooth mesh (BLE GATT)** | Low-bandwidth fallback for metadata sync, not model inference | 4hr |

---

### TIER 6: MARKETPLACE UX (2-3 Sessions)

| # | Task | Detail | Est. |
|---|------|--------|------|
| 6.1 | **Skill marketplace UI** | Discovery, search, install, ratings — build React components for `/api/skills/marketplace` | 3hr |
| 6.2 | **Model marketplace UI** | Browse catalog, download, version management — connected to `/api/marketplace/*` | 3hr |
| 6.3 | **DOPA payment flow in UI** | Visual payment confirmation, transaction history, staking controls | 2hr |
| 6.4 | **Skill rating/review UI** | Rate 1-5 stars, leave comments. Already wired on backend (`/api/skills/rate`) | 1hr |

---

### TIER 7: CONTENT PLATFORM — Horizon 3 Track B (3-4 Sessions)

| # | Task | Detail | Est. |
|---|------|--------|------|
| 7.1 | **Abstract transport layer** | Extend spike protocol for general payloads (files, media, arbitrary data) | 4hr |
| 7.2 | **File distribution** | BitTorrent-style chunking + swarming across mesh nodes | 4hr |
| 7.3 | **User-defined recommendation algorithms** | Pluggable content discovery algorithms (anti-algorithmic-manipulation by design) | 4hr |
| 7.4 | **Creator economy** | $DOPA gating for premium content, royalty splits, creator dashboards | 3hr |

---

### TIER 8: SOLARPUNK EXCHANGE (3-5 Sessions)

Data models and API endpoints were built this session. Remaining:

| # | Task | Detail | Est. |
|---|------|--------|------|
| 8.1 | **Exchange UI pages** | Skill profiles, service requests, barter matching — React components | 3hr |
| 8.2 | **Trust dashboard** | Visual trust score breakdown, exchange history, time credits | 2hr |
| 8.3 | **Tool library** | Community tool sharing with check-in/check-out, deposit staking | 3hr |
| 8.4 | **Dispute resolution** | DAO-voted dispute arbitration flow | 2hr |
| 8.5 | **Time banking** | 1 hour = 1 credit system, cross-category exchange | 2hr |

**Extended 10-part Solarpunk integrations (each 2-4hr):**
- Community Energy Grid
- Seed & Plant Exchange
- Repair Cafe Network
- Mutual Aid Circles
- Commons Mapping
- Carbon Offset Ledger
- Local Food Networks
- Knowledge Commons
- Cooperative Governance
- Resilience Score

---

### TIER 9: v1.0 / APP STORE (Future)

| # | Task | Detail |
|---|------|--------|
| 9.1 | Google Play submission | Signing, store listing, screenshots |
| 9.2 | Apple App Store | Requires iOS Capacitor + Mac build |
| 9.3 | FHE for private inference | Fully homomorphic encryption (research) |
| 9.4 | WebRTC browser P2P | Browser-native mesh without app install |
| 9.5 | CRDT offline-first state sync | Conflict-free replicated data types |
| 9.6 | Bittensor subnet registration | Requires TAO tokens, Test TAO untested |

---

## UNTESTED CRITICAL MODULES (83 modules without tests)

**High-priority modules needing tests:**
- `hub_server.py` (integration level)
- `lib_node.py` (core neural node logic)
- `reward_engine.py` (has economy tests but not engine-specific)
- `spike_protocol.py` (has lifecycle tests but protocol-specific gaps)
- `config.py` (INGRVMConfig)
- `hole_puncher.py` (NAT traversal)
- `identity_manager.py` (Ed25519 — tested implicitly, no dedicated file)

**6 empty test files to either fill or delete:**
- test_dpos_auction.py
- test_inter_mesh_stress.py
- test_lava.py
- test_p2p_node.py
- test_port.py
- test_visualizer_density.py

---

## 2 TODOs IN CODEBASE

1. `p2p_debug.py:122` — `# TODO: Implement /api/mesh/address to get real multiaddr from PC`
2. `tools/debug/p2p_debug.py:243` — same TODO (duplicate file)

---

## RECOMMENDED SESSION PLAN

### Session N+1 (Next): Security + Tests + Deployment
1. Regenerate PSK, rotate API keys (Tier 0)
2. Write WebSocket + mesh state sync tests (Tier 1.1-1.2)
3. Write Solarpunk exchange tests (Tier 1.3)
4. Deploy PWA to Vercel (Tier 2.1)

### Session N+2: Onboarding + Capacitor
1. Build onboarding wizard (Tier 2.2) — MOST IMPORTANT UX work
2. Capacitor project setup + debug APK (Tier 2.3-2.4)
3. Mobile UI polish (Tier 2.5)

### Session N+3: Mesh Robustness
1. Shard rebalancing (Tier 3.1)
2. KV cache sync endpoints (Tier 3.2)
3. Network error handling (Tier 3.3)

### Session N+4: Model Maturity
1. TinyLlama 1.1B sharding (Tier 4.1)
2. Auto-shard on join (Tier 4.2)
3. Inference quality benchmarks (Tier 4.3)

### Session N+5-6: WiFi Direct
1. Nearby Connections API (Tier 5.1)
2. Shard routing integration (Tier 5.2-5.3)

### Session N+7-8: Marketplace UX
1. Skill marketplace UI (Tier 6.1)
2. Model marketplace UI (Tier 6.2)
3. DOPA payment + rating UI (Tier 6.3-6.4)

### Session N+9+: Content Platform + Solarpunk
- Track B content platform (Tier 7)
- Solarpunk exchange UI (Tier 8)
- Extended integrations as time allows

---

## METRICS

- **Total remaining items:** 94+
- **Estimated MVP completion:** ~10-12 focused sessions
- **Current horizon:** 3 (Platform Layer) in progress
- **Tests passing:** 378
- **Test coverage:** 3.5% of root modules have dedicated test files (83/86 untested)
- **Mobile UI:** Built (6 views), needs polish and onboarding
- **PWA:** Configured, needs deployment
- **Deployment:** Not yet deployed anywhere
