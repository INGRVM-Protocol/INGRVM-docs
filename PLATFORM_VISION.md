# INGRVM Platform Vision: The Living Mesh

**Date:** 2026-04-08
**Status:** Strategic ideation — not a spec, not a roadmap. A north star.

---

## The Core Reframe

The current Synapse system treats synapses as **tasks** — you invoke them, get a result, done. The new vision treats synapses as **living knowledge** — neurons host them, other neurons query them, and the mesh becomes a distributed brain where every node knows something useful.

The difference:
- **Old model:** "Call this function, get a result" (RPC)
- **New model:** "This neuron knows about X. Ask it." (Knowledge mesh)

---

## 1. Synapses as Knowledge, Not Just Skills

### What Changes

A synapse stops being just a callable function. It becomes a **downloadable knowledge pack** that a neuron hosts and serves.

**Examples of knowledge synapses:**
- A neuron hosts the latest cancer research papers. Other neurons query it. The host earns $DOPA for every query served.
- A neuron hosts a curated dataset of housing prices in Austin. Real estate apps on the mesh query it.
- A neuron hosts INGRVM's own documentation. New users ask it how the mesh works.
- A neuron hosts a local community's event calendar. The neighborhood's neurons subscribe.
- A breakthrough in quantum computing gets packaged as a synapse. Neurons that install it can answer questions about it. The researcher who published it earns royalties every time it's queried.

### How It Works

```
KNOWLEDGE SYNAPSE LIFECYCLE:

1. CREATION
   - Creator packages knowledge (text corpus, structured data, embeddings)
   - Registers synapse with metadata, license, royalty %
   - Signs with Ed25519 identity

2. DISTRIBUTION
   - Synapse announced via gossip protocol
   - Other neurons browse marketplace, discover it
   - Neurons choose to download and host it
   - Multiple hosts = redundancy + faster serving

3. QUERYING
   - Any neuron can query any hosted synapse
   - Query routed to nearest/fastest host
   - Host's INGRVM AI processes query against knowledge
   - Result returned, host earns $DOPA, creator earns royalty

4. UPDATES
   - Creator publishes new version (semver)
   - Neurons that host it get notified via gossip
   - Auto-update or manual update (user choice)
   - Old versions still serveable (version pinning)
```

### What's Missing in Current Implementation

The KNOWLEDGE synapse type exists in the schema but it's barely functional:
- Uses keyword matching only (no semantic search, no embeddings)
- No distribution model (synapse lives on one node only)
- No update/versioning mechanism
- No "install this synapse on my neuron" flow

**To build:**
- Embedding-based retrieval (lightweight — MiniLM or similar, runs on phone)
- Synapse distribution via gossip (like model shards, but for knowledge)
- Version management with diff-based updates (don't re-download 500MB when 1KB changed)
- Host discovery (which neurons near me have this synapse?)

---

## 2. The INGRVM AI — Your Neuron's Voice

### The Concept

Every neuron gets a local AI assistant that understands the mesh. Not a cloud chatbot — a **local agent** that can:

- **Answer questions about INGRVM** — "How does staking work?" "What's a synapse?" (trained on INGRVM docs)
- **Query other neurons** — "Find me the latest research on neuromorphic computing" → routes to a neuron hosting that knowledge → returns answer → that neuron gets paid
- **Manage your node** — "Install the top 5 most popular synapses" → browses marketplace → downloads → installs
- **Configure your algorithm** — "Show me content sorted by newest, not popular" → swaps your feed algorithm synapse
- **Monitor your earnings** — "How much $DOPA did I earn today?" → reads local wallet state
- **Interact with the mesh** — "Who's online right now?" → queries mesh status

### How It Works

```
USER: "What's the latest on CRISPR gene editing?"

INGRVM AI:
1. Checks local synapses — do I have any KNOWLEDGE synapses about CRISPR? No.
2. Queries mesh — which neurons host CRISPR-related synapses?
3. Finds 3 neurons: one with a 2026 research corpus, one with a biotech news feed, one with a medical encyclopedia
4. Routes query to highest-rated one
5. Returns answer to user
6. Host neuron earns $DOPA, creator earns royalty
7. AI suggests: "Want me to install this synapse locally so you get faster answers next time?"

USER: "Yeah, install it."

INGRVM AI:
1. Downloads synapse from host
2. Registers locally
3. Now YOUR neuron can answer CRISPR questions too
4. Other neurons that query you for CRISPR info → you earn $DOPA
```

### Is This Dystopian?

No. Here's why:

- **You control the AI.** It runs locally on your device. It can't phone home.
- **You choose what to install.** The AI suggests, you decide.
- **You own your data.** Your queries don't get logged by a corporation.
- **The AI can't override you.** It's an assistant, not an authority.
- **Transparency is built in.** Every query, every earning, every synapse installation is logged locally.

The dystopian version is what we already have: an algorithm you can't see, controlled by a company you can't influence, optimized for engagement you didn't consent to. INGRVM's version puts the controls in your hands and the AI helps you use them.

**The one rule:** The AI never makes economic decisions without confirmation. "Want me to install this?" not "I installed this for you." "This synapse costs 0.5 $DOPA per query" not silent spending.

---

## 3. Content Sharing & Consumption

### The Feed

Every user has a **feed** — a stream of content from synapses they follow or that the mesh recommends. But unlike Instagram or TikTok:

- **You pick the algorithm.** Your feed is sorted by a SKILL synapse of your choosing.
- **Algorithms are synapses too.** Anyone can publish a ranking algorithm. You install the one you like.
- **No engagement optimization.** Unless you install an algorithm that does that. Your choice.

### Algorithm Examples

| Algorithm Synapse | What It Does |
|---|---|
| `chronological_v1` | Show everything in time order. No ranking. |
| `local_first_v1` | Prioritize content from neurons geographically close to you. |
| `expertise_weighted_v1` | Rank by creator's reputation in the topic area. |
| `contrarian_v1` | Surface content that disagrees with what you usually consume. |
| `serendipity_v1` | Random sampling weighted toward low-view-count content. |
| `friends_only_v1` | Only show content from neurons you've explicitly followed. |

Users can switch algorithms instantly. The AI can recommend algorithms based on your usage patterns — "You seem to engage most with local content. Want to try `local_first_v1`?"

### Content Types on the Mesh

| Type | How It's Shared | How It's Consumed | Who Earns |
|---|---|---|---|
| **Knowledge** | KNOWLEDGE synapse downloaded to neurons | Queried via AI or search | Host + creator |
| **Media** | MEDIA synapse, chunked BitTorrent-style across hosts | Streamed to client, chunks served by nearest host | Host + creator |
| **Posts/Updates** | Lightweight KNOWLEDGE synapse (text + optional media CID) | Feed aggregation via algorithm synapse | Host + creator |
| **Apps/Tools** | COMPOSITE synapse (pipeline of skills) | Rendered by INGRVM client UI | Creator + all skill authors in pipeline |
| **Data Feeds** | ORACLE synapse (real-time external data) | Subscribed to via gossip | Oracle operator |
| **Storefronts** | COMPOSITE synapse with payment escrow | Browsed via marketplace UI | Seller |

---

## 4. App Clones — Mainstream Replacements

Every mainstream app becomes a **COMPOSITE synapse** — a pipeline of skills that the INGRVM client renders.

### How Apps Are Built

```
APP = COMPOSITE SYNAPSE = PIPELINE OF SKILLS

Example: "MeshTunes" (Spotify replacement)
├── synapse:meshtunes_catalog     (KNOWLEDGE — music library metadata)
├── synapse:meshtunes_player      (MEDIA — chunked audio streaming)
├── synapse:meshtunes_recommend   (SKILL — recommendation algorithm)
├── synapse:meshtunes_playlist    (KNOWLEDGE — user playlists, stored locally)
└── synapse:meshtunes_royalty     (SKILL — payment splitter for artists)
```

### How Anyone Can Build Apps

1. **No SDK needed.** Register synapses via the API. Chain them into a COMPOSITE.
2. **No app store approval.** Sign it, publish it, it's live.
3. **No platform fee.** Creator sets their own royalty. Hosting nodes earn their share.
4. **The INGRVM AI can help build it.** "Build me an app that lets people share recipes and earn $DOPA when someone cooks theirs." → AI scaffolds the synapse pipeline.

### The Clone Map

| Mainstream App | INGRVM Clone | Key Synapses |
|---|---|---|
| **Spotify/Apple Music** | Neural Streams | MEDIA (audio chunks) + SKILL (recommender) + KNOWLEDGE (catalog) |
| **YouTube** | Neural Streams (video) | MEDIA (video chunks) + SKILL (recommender) + ORACLE (live stats) |
| **Twitter/X** | SpikeNet | KNOWLEDGE (posts) + SKILL (feed algorithm) + KNOWLEDGE (profiles) |
| **Instagram** | SpikeNet (visual) | MEDIA (images) + SKILL (feed algorithm) + KNOWLEDGE (profiles) |
| **TikTok** | SpikeNet (short video) | MEDIA (short video) + SKILL (discovery algorithm) |
| **Shopify/Etsy** | Synapse Markets | COMPOSITE (storefront) + SKILL (escrow) + MEDIA (product images) |
| **Wikipedia** | Open Knowledge | KNOWLEDGE (encyclopedia corpus) + SKILL (search) |
| **Reddit** | Mesh Forums | KNOWLEDGE (threads) + SKILL (ranking algorithm) + SKILL (moderation) |
| **Discord/Slack** | Mesh Messaging | Spike protocol (encrypted P2P messages) + KNOWLEDGE (channel history) |
| **Google Search** | Mesh Search | SKILL (crawl + rank across all KNOWLEDGE synapses on mesh) |
| **ChatGPT** | INGRVM AI | Local LLM + KNOWLEDGE synapses + SKILL routing |

### The Key Insight

You don't build "an app." You build **synapses** and **compose them.** The INGRVM client renders any COMPOSITE synapse as an app experience. This means:

- One client, infinite apps
- Apps are interoperable (a music synapse from one app can be used in another)
- No walled gardens
- Creators get paid at the synapse level, not the app level

---

## 5. The INGRVM AI as the Universal Interface

### Why This Changes Everything

Currently, users interact with the mesh via:
- TUI (terminal)
- Dashboard (web/mobile)
- API (developers)

The INGRVM AI becomes the **fourth and primary interface.** Like Claude Code is to a codebase, the INGRVM AI is to your neuron.

```
"Show me what's trending on the mesh"
→ AI queries SpikeNet feed synapse with your algorithm preference

"Play something like what I listened to yesterday"
→ AI queries Neural Streams with your listening history

"I want to share this photo with my neighborhood"
→ AI publishes MEDIA synapse, announces via gossip to local neurons

"How much did I earn this week?"
→ AI reads local wallet state, summarizes

"Install the CRISPR research synapse everyone's talking about"
→ AI finds it in marketplace, shows you the details, you confirm, it installs

"Switch my feed to chronological"
→ AI swaps your algorithm synapse

"Build me a synapse that answers questions about Austin restaurants"
→ AI scaffolds a KNOWLEDGE synapse from data you provide, registers it
```

### Architecture

```
┌─────────────────────────────────┐
│         INGRVM AI Agent         │
│  (local LLM + tool use layer)   │
├─────────────────────────────────┤
│  Tools:                          │
│  - query_synapse(id, query)      │  ← Ask any synapse anything
│  - install_synapse(id)           │  ← Download knowledge to your node
│  - browse_marketplace(query)     │  ← Discover synapses
│  - manage_wallet()               │  ← Check balance, stake, send
│  - set_algorithm(synapse_id)     │  ← Change your feed ranking
│  - publish_synapse(definition)   │  ← Create and share knowledge
│  - mesh_status()                 │  ← Who's online, what's available
│  - configure_node(settings)      │  ← Change node behavior
│  - query_remote(neuron_id, q)    │  ← Ask a specific neuron directly
└─────────────────────────────────┘
         ↕ runs on your device
┌─────────────────────────────────┐
│        Your INGRVM Neuron       │
│  (local synapses + mesh conn)    │
└─────────────────────────────────┘
         ↕ mesh protocol
┌─────────────────────────────────┐
│        Other Neurons            │
│  (their synapses, their data)    │
└─────────────────────────────────┘
```

The AI is not the mesh. The AI is **your lens into the mesh.** It runs locally, it's trained on INGRVM docs, and it has tools to interact with your neuron and the broader mesh.

---

## 6. How Synapses Are Made

### The Problem

The current synapse creation flow requires a developer: write a Python handler, define JSON schemas, call the registration API. That locks out 99% of people. If synapses are the fundamental unit of the mesh, everyone needs to be able to make them.

### Three Creation Paths

#### Path A: Regular Person (No Code)

You have knowledge, media, or a collection of something useful. The INGRVM AI does the rest.

**Knowledge synapse:**
```
USER: "I want to share everything I know about beekeeping."

INGRVM AI: "I can help with that. You can:
  1. Upload files (PDFs, notes, docs)
  2. Paste text directly
  3. Link to sources I can read
  4. Just talk — I'll record and structure what you say"

USER: uploads 3 PDFs and a folder of notes

INGRVM AI: "I processed 47 pages. I found info on:
  - Hive setup and maintenance
  - Seasonal management
  - Disease identification
  - Honey harvesting
  Here's a preview of how I'd answer a question:
  Q: 'When should I check for mites?'
  A: 'Check varroa mite levels in late summer...'
  
  Want to set a name, royalty, and publish?"

USER: "Call it 'Backyard Beekeeping Guide'. 10% royalty. Mesh open license."

INGRVM AI: "Published. Your synapse is live. When someone asks 
  about beekeeping and gets routed to your neuron, you earn $DOPA."
```

**What happens under the hood:**
1. AI chunks the uploaded content into passages
2. Generates embeddings for semantic retrieval (MiniLM or similar — runs locally)
3. Creates the SkillDefinition with input/output schemas
4. Registers via the API
5. Announces via gossip protocol

The user never sees a JSON schema, a Python function, or an API call.

**Media synapse:**
```
USER: "I recorded an album. I want to share it on the mesh."

INGRVM AI: "Drop the tracks here. I'll handle the rest."

USER: uploads 12 MP3 files

INGRVM AI: "12 tracks processed. I auto-tagged them:
  - Artist: [your mesh handle]
  - Genre: detected as indie folk
  - Total size: 89MB (chunked for streaming)
  
  How do you want to price it?
  - Free (mesh_open — you earn hosting rewards when others stream)
  - Per-stream (set a $DOPA price per play)
  - Tip-only (free, but listeners can tip)"

USER: "Free with tips."

INGRVM AI: "Published as 'Porch Sessions'. 
  Other neurons can now stream it. You earn from hosting rewards + tips."
```

**Composite synapse (app):**
```
USER: "I want to make a recipe sharing app for my neighborhood."

INGRVM AI: "I can build that from existing synapses. Here's what I'd use:
  - A KNOWLEDGE synapse for the recipe database (you'll add recipes)
  - A MEDIA synapse for food photos
  - A SKILL synapse for search/filtering
  - A feed synapse so neighbors see new recipes
  
  Want me to set it up?"

USER: "Yeah."

INGRVM AI: "Done. 'Mesh Kitchen' is live. 
  Share the link with your neighbors: mesh://@youhandle/meshkitchen
  You can add recipes anytime — just tell me."
```

#### Path B: Creator / Power User (Low Code)

For people who want more control but aren't developers. A visual builder or template system.

**Templates:**
```
SYNAPSE TEMPLATES:

📚 Knowledge Base    — Upload docs, auto-chunked, semantic search
🎵 Music/Audio       — Upload tracks, auto-chunked for streaming
📹 Video Channel     — Upload videos, chunked + streaming
📝 Blog/Newsletter   — Write posts, auto-published to followers
🛒 Storefront        — List products, escrow payments
📊 Data Feed         — Connect an API, serve real-time data
🤖 Custom Skill      — Write a handler function (Python/JS/WASM)
🔗 App (Composite)   — Chain existing synapses into a pipeline
```

Each template pre-fills the schema, sets sensible defaults, and walks the user through the rest.

#### Path C: Developer (Full Control)

The current API path, plus a future SDK:

```python
from ingrvm import Synapse, Knowledge

@Synapse(
    name="austin_food_guide",
    type=Knowledge,
    royalty=15,
    license="mesh_open"
)
def austin_food(query: str) -> str:
    # Custom retrieval logic, embeddings, RAG pipeline, whatever
    return answer
```

Or for a WASM-based synapse (runs sandboxed on any device):

```rust
// Compiled to WASM, hot-reloadable on the mesh
#[synapse(name = "image_classifier", type = "compute", royalty = 20)]
fn classify(image_bytes: &[u8]) -> Classification {
    // Your model inference here
}
```

### The Creation Pipeline (All Paths)

Regardless of which path, every synapse goes through the same pipeline:

```
1. CONTENT PROCESSING
   └─ Text → chunk + embed
   └─ Media → chunk + hash + metadata
   └─ Code → validate + sandbox test

2. SCHEMA GENERATION
   └─ Auto-generate input/output JSON schemas from content type
   └─ Creator can override if they want

3. IDENTITY & LICENSING
   └─ Sign with Ed25519 key
   └─ Choose license (proprietary, mesh_open, CC_BY, etc.)
   └─ Set royalty % (0-90%)
   └─ Set co-authors and splits (if applicable)

4. LOCAL TESTING
   └─ AI runs a few test queries against the synapse
   └─ Creator reviews the results
   └─ "Does this answer make sense?" → iterate or publish

5. PUBLICATION
   └─ Register in local marketplace
   └─ Announce via gossip to mesh
   └─ Content fingerprint recorded in copyright registry
   └─ Live and earning within seconds

6. ONGOING
   └─ Creator can push updates (versioned, incremental)
   └─ Other neurons can host copies (earn hosting rewards)
   └─ Creator sees stats: queries served, $DOPA earned, ratings
```

---

## 7. What Needs to Be Built (Priority Order)

### Phase 1: Knowledge Synapse Infrastructure
- [ ] Embedding-based retrieval for KNOWLEDGE synapses (MiniLM or similar, runs on phone)
- [ ] Synapse distribution via gossip (download synapses from other neurons)
- [ ] Multi-host serving (multiple neurons serve same synapse, load balanced)
- [ ] Version management with incremental updates

### Phase 2: INGRVM AI Agent
- [ ] Local LLM integration (small model, runs on device)
- [ ] Tool-use layer (query synapse, install, marketplace, wallet, config)
- [ ] INGRVM documentation synapse (trained on all INGRVM docs, auto-installed)
- [ ] Confirmation UX for economic actions (never silent spending)

### Phase 3: Feed & Algorithm System
- [ ] Algorithm synapse standard (input: content list, output: ranked list)
- [ ] Default algorithm synapses (chronological, local-first, serendipity)
- [ ] Feed aggregation from followed neurons
- [ ] Algorithm swap via AI or settings

### Phase 4: App Framework
- [ ] COMPOSITE synapse renderer in client UI
- [ ] Synapse SDK (Python first, then JS)
- [ ] App templates (social feed, storefront, media player, knowledge base)
- [ ] AI-assisted app scaffolding

### Phase 5: Mainstream Clones
- [ ] Neural Streams v1 (music/audio streaming)
- [ ] SpikeNet v1 (social posts + feed)
- [ ] Mesh Messaging v1 (encrypted P2P chat)
- [ ] Mesh Search v1 (cross-synapse knowledge search)

---

## 8. Mobile Architecture: Design for the Phone First

### The Principle

The phone is the primary neuron. Not the afterthought. Most people will never run a PC hub — their first and only interaction with INGRVM is a mobile app. Every architectural decision should assume the phone is the device.

### The Long-Term Mobile Stack

```
┌──────────────────────────────────────────┐
│              INGRVM Mobile App            │
│  (React Native or Kotlin Multiplatform)   │
├──────────────────────────────────────────┤
│                                          │
│  ┌─────────────┐  ┌──────────────────┐   │
│  │ INGRVM AI   │  │  Synapse Viewer  │   │
│  │ (local LLM) │  │  (feed, chat,    │   │
│  │             │  │   marketplace,   │   │
│  │             │  │   wallet, apps)  │   │
│  └──────┬──────┘  └───────┬──────────┘   │
│         │                 │              │
│  ┌──────┴─────────────────┴──────────┐   │
│  │         Neuron Core Service        │   │
│  │  (background service, always on)   │   │
│  ├────────────────────────────────────┤   │
│  │  Inference Engine (ONNX/TFLite)   │   │
│  │  Synapse Host (knowledge + media) │   │
│  │  Gossip Protocol (peer discovery) │   │
│  │  Wallet ($DOPA, staking, payments)│   │
│  │  Identity (Ed25519 keychain)      │   │
│  └──────┬─────────────────────────────┘   │
│         │                                │
│  ┌──────┴─────────────────────────────┐   │
│  │        Transport Layer             │   │
│  │  WiFi Direct ↔ LAN ↔ WAN ↔ Relay │   │
│  │  (auto-fallback, always seeking   │   │
│  │   the most direct connection)     │   │
│  └────────────────────────────────────┘   │
└──────────────────────────────────────────┘
```

### Key Design Decisions

#### 1. Background Neuron Service

The app has two parts: the UI (what you see) and the Neuron Core (what runs in the background). The core:
- Stays alive even when the app is closed
- Hosts your synapses, serves queries, earns $DOPA while you sleep
- Handles gossip, peer discovery, inference requests
- Manages the transport layer (auto-switches between WiFi Direct / LAN / WAN)

On Android: a foreground service with a persistent notification ("Your neuron is active. Earned 2.4 $DOPA today.") On iOS: background processing with limitations (Apple restricts always-on services — use background fetch + push notifications as triggers).

#### 2. Inference Engine: ONNX First, Not PyTorch

PyTorch is 2GB. Phones can't run it. The long-term path:
- **ONNX Runtime** (~50MB) as the universal inference backend. Runs on every device. Supports NNAPI for automatic hardware routing (TPU/GPU/DSP on Android).
- **TFLite** as a secondary backend for Pixel TPU optimization.
- **PyTorch stays on PC/laptop only** — it's the development/conversion tool, not the runtime.
- Models are converted to ONNX once and distributed as ONNX shards. Every device speaks the same format.

#### 3. Transport Layer: Always Seeking the Best Path

The transport layer isn't one protocol — it's a stack that auto-negotiates:

```
PRIORITY ORDER (most direct → most relayed):

1. WiFi Direct     — device to device, no router, 250 Mbps, 200m
                      Best for: local mesh, no internet needed
                      
2. WiFi LAN        — devices on same router, near-instant
                      Best for: home/office mesh
                      
3. WAN (Tailscale) — encrypted tunnel over internet, near-term bridge
                      Best for: remote devices, cross-city mesh
                      
4. WAN (Kademlia)  — DHT peer discovery, fully decentralized
                      Best for: long-term, no relay dependency
                      
5. Relay VPS       — bootstrap fallback when nothing else works
                      Best for: NAT-busting, first connection
                      
6. Delay-Tolerant  — store and forward when no path exists now
                      Best for: offline, rural, disaster scenarios
```

The neuron tries 1 first, falls back down the list automatically. The user never thinks about it — it just works.

#### 4. Offline-First Design

Everything works offline first, syncs when connected:
- **Synapse queries** work against locally installed knowledge (no network needed)
- **$DOPA earning** is tracked locally, settled when reconnected (deferred settlement with cryptographic proof of work done offline)
- **Messages** queue locally, deliver when a path exists
- **Content** cached aggressively — once you've streamed a song, it's local
- **Feed** shows locally cached content first, backfills when online

This isn't a "graceful degradation" — it's the primary design. Network is a bonus, not a requirement.

#### 5. The App Screens (Long-Term)

```
INGRVM MOBILE APP:

🏠 Home           — Your neuron status, earnings today, synapse health
💬 Chat           — INGRVM AI conversation (queries mesh, manages neuron)
📡 Feed           — Content from followed neurons + algorithm synapse
🧠 Synapses       — Your installed knowledge, media, skills (manage, create)
🏪 Marketplace    — Discover and install synapses (search, browse, ratings)
💰 Wallet         — $DOPA balance, staking, transaction history, send/receive
🌐 Mesh           — Live mesh status, connected neurons, topology map
⚙️ Settings       — Algorithm choice, trust preferences, transport config, identity
```

The INGRVM AI is accessible from every screen — pull up from bottom, voice, or notification tap. It's the primary way most users interact.

#### 6. App Store Distribution

**Android:** Native app (Kotlin) or React Native. Distribute via:
- Google Play Store (mainstream reach)
- F-Droid (open source community)
- Direct APK sideload (sovereign option — no store gatekeeper)

**iOS:** React Native or KMP. Apple requires App Store only (no sideloading until EU DMA fully enforced). Design the app to comply with Apple guidelines while keeping sovereignty features intact. The background neuron service is limited by iOS — use background fetch and push notifications to approximate always-on behavior.

**Desktop:** Tauri (Rust-based, lightweight) wrapping the same web UI. Secondary priority — phones first.

#### 7. What to Build Now vs Later

**Now (design decisions that affect everything):**
- API contract between UI and Neuron Core (REST + WebSocket — already exists in hub_server.py)
- ONNX model format as the standard (convert once, run everywhere)
- Transport abstraction interface (pluggable backends behind one API)
- Offline-first data layer (local SQLite + sync protocol)

**Soon (Horizon 3):**
- Capacitor APK (wrap current React UI as native Android app — quick bridge)
- Background service proof of concept (foreground service earning $DOPA)
- WiFi Direct prototype (Android Nearby Connections API)

**Later (Horizon 4+):**
- Full native app (Kotlin or React Native rewrite)
- TFLite Pixel TPU optimization
- iOS app
- Multi-hop mesh routing (WiFi Direct relay chains)
- Delay-tolerant networking

---

## 8b. NPU Energy Optimization

### Why This Matters

A phone running inference on CPU drains battery in hours. The same phone running on its dedicated NPU uses 10-20x less power. This is the difference between "INGRVM kills your battery" and "INGRVM runs in the background and you don't notice."

### Current NPU Landscape (2026)

| Chip | Device | NPU Capability | Runtime |
|---|---|---|---|
| Google Tensor G3/G4 | Pixel 8/9 | Edge TPU, 10 TOPS | TFLite, LiteRT-LM |
| Qualcomm Snapdragon 8 Gen 3+ | Most Android flagships | Hexagon NPU, 45+ TOPS | ExecuTorch QNN, ONNX NNAPI |
| Apple A17/M4 | iPhone 15+, iPad, Mac | Neural Engine, 35 TOPS | Core ML |
| MediaTek Dimensity 9300 | Mid-range Android | APU, 46 TOPS | ONNX NNAPI |

### How INGRVM Uses NPUs

```
CURRENT (all CPU):
  Model shard → PyTorch → CPU → slow, hot, battery drain

NEAR-TERM (ONNX + NNAPI):
  Model shard → ONNX Runtime → NNAPI → auto-routes to NPU/GPU/DSP
  Result: 5-10x faster, 10x less power, phone stays cool

LONG-TERM (ExecuTorch + QNN / LiteRT-LM):
  Model shard → ExecuTorch → Qualcomm QNN delegate → NPU directly
  Result: 20x CPU speedup, ~1W power draw, runs all day as background service
```

The node advertises its hardware capabilities via gossip. The orchestrator knows "this Pixel has a Tensor G3 TPU — give it the attention layers" and "this old phone only has CPU — give it the embedding lookup." Shard assignment becomes hardware-aware, not random.

### Energy Budget System

The neuron core enforces an energy budget:
- User sets max battery drain per hour (e.g., "2% per hour")
- Core monitors actual draw via Android BatteryManager API
- If approaching budget, throttle inference (accept fewer jobs, sleep between requests)
- If charging, remove all limits (full NPU utilization)
- Dashboard shows: "Your neuron used 8% battery today and earned 1.2 $DOPA"

### Impact on the Mesh

If every phone node runs on NPU instead of CPU:
- 10x more inference capacity per device
- 10x longer participation per charge cycle
- Higher $DOPA earnings per watt
- More people willing to keep the background service running
- The mesh becomes genuinely useful on mobile, not just a demo

---

## 8c. Browser-Based Neurons (WebTransport)

### The Concept

You open a website. Your browser tab becomes a mesh node. No install. No Python. No app store. Just a URL.

### How It Works

```
BROWSER NEURON ARCHITECTURE:

┌─────────────────────────────────────────┐
│            Browser Tab                   │
├─────────────────────────────────────────┤
│                                         │
│  ┌──────────────┐  ┌────────────────┐   │
│  │  Web UI       │  │  Service Worker │  │
│  │  (React/Svelte│  │  (background,   │  │
│  │   dashboard)  │  │   persists when │  │
│  │              │  │   tab is open)  │  │
│  └──────┬───────┘  └───────┬────────┘   │
│         │                  │            │
│  ┌──────┴──────────────────┴─────────┐  │
│  │       WebTransport Connection      │  │
│  │  (QUIC-based, full-duplex,        │  │
│  │   no WebSocket limitations)       │  │
│  ├────────────────────────────────────┤  │
│  │  libp2p.js (peer discovery,       │  │
│  │   gossip, DHT participation)      │  │
│  └──────┬─────────────────────────────┘  │
│         │                               │
│  ┌──────┴─────────────────────────────┐  │
│  │    WASM Neuron Core (lightweight)  │  │
│  │  - Spike protocol encode/decode   │  │
│  │  - Ed25519 identity (IndexedDB)   │  │
│  │  - Synapse hosting (small KBs)    │  │
│  │  - WebNN inference (if available) │  │
│  │  - $DOPA wallet (local state)     │  │
│  └────────────────────────────────────┘  │
└─────────────────────────────────────────┘
         ↕ WebTransport (QUIC)
┌─────────────────────────────────────────┐
│        Rest of the Mesh                  │
│  (native nodes, phone nodes, other      │
│   browser nodes — all speak libp2p)     │
└─────────────────────────────────────────┘
```

### What a Browser Neuron Can Do

| Capability | Can It? | How |
|---|---|---|
| **Host small knowledge synapses** | Yes | IndexedDB stores text corpus, serves queries |
| **Participate in gossip** | Yes | libp2p.js + WebTransport, full peer discovery |
| **Earn $DOPA** | Yes | Serve synapse queries, relay messages, host content chunks |
| **Run inference** | Limited | WebNN API (Chrome 127+) routes to GPU/NPU if available; otherwise CPU-only WASM, slow |
| **Host media content** | Small files | IndexedDB has storage limits (~1-10GB depending on browser) |
| **Persist when tab is closed** | No | Service worker keeps connection briefly, but browser nodes are inherently ephemeral |
| **WiFi Direct mesh** | No | Browsers can't access WiFi Direct APIs |

### What Browser Neurons Can't Do

- **Heavy inference** — browsers don't have direct NPU access (WebNN is early, limited)
- **Always-on hosting** — when you close the tab, the node goes offline
- **WiFi Direct** — browser sandbox prevents direct device-to-device networking
- **Large storage** — IndexedDB caps vary by browser (typically 1-10GB)

### Why Browser Neurons Still Matter

The browser node isn't trying to replace native nodes. It's the **onboarding funnel:**

```
JOURNEY:

1. User clicks a link → browser tab opens → instant mesh participation
2. Browser neuron hosts a few small knowledge synapses, earns first $DOPA
3. User sees: "You earned 0.3 $DOPA in 20 minutes. Install the app to earn 10x more."
4. User installs native app → becomes a full neuron
5. Browser tab was the gateway, not the destination
```

This is how OptimAI hit 1M+ nodes — browser extensions and in-browser participation with zero install friction.

### The WebNN Opportunity

Chrome 127+ supports the WebNN API, which routes ML operations to the device's GPU or NPU from the browser. It's early, but if it matures:
- Browser neurons could run small model inference (sentiment, classification, embedding generation)
- No WASM performance penalty — native hardware acceleration from a browser tab
- This turns every Chrome user into a potential mesh node

### Technical Path

1. **Now:** Build PWA that connects to hub API (already have React UI)
2. **Soon:** Add libp2p.js for direct peer connections via WebTransport
3. **Later:** WASM-compiled neuron core for spike protocol + wallet
4. **Eventually:** WebNN inference for light compute tasks

---

## 9. The Competitive Moat

What makes INGRVM different from every other decentralized project:

1. **The AI is local.** Not a cloud API. Not a chatbot. A local agent that controls your neuron.
2. **Synapses are knowledge, not just compute.** You don't just run tasks — you host knowledge and earn for sharing it.
3. **Algorithms are user-chosen.** No corporation decides what you see.
4. **Apps are composable.** Build once, remix forever. No walled gardens.
5. **The mesh is the platform.** No servers, no CDN, no infra costs. The users ARE the infrastructure.
6. **Neuromorphic architecture.** Spiking neural networks aren't just a metaphor — they're the actual transport layer. This matters for energy efficiency on edge devices as neuromorphic hardware matures.

### vs. The Sovereign Network

TSN is building federated learning infrastructure (train models across devices). INGRVM is building a **knowledge economy** (share knowledge, earn rewards, consume content, run apps). TSN is plumbing. INGRVM is the house.

---

## 9. Open Questions

- **Synapse size limits?** A 50GB knowledge corpus is different from a 50KB text file. Need tiered hosting incentives.
- **Privacy?** When you query another neuron, they see your query. Need encrypted queries (homomorphic or ZK-based).
- **Offline knowledge?** If you install a synapse locally, you can answer queries offline. But how do you earn $DOPA without network connectivity? Deferred settlement?
- **AI model size?** The INGRVM AI needs to be small enough for phones but smart enough to be useful. Phi-3 Mini? TinyLlama? Distilled GPT-2?
- **Synapse discovery beyond marketplace?** Word of mouth via mesh messaging? "This neuron has great cooking knowledge" recommendations?

---

## 10. Trust Without Censorship: Peer Review & Fact-Checking

### The Problem

Someone publishes a KNOWLEDGE synapse claiming a miracle cancer cure. It's wrong. It's dangerous. But you also can't have a central authority deciding what's true — that's exactly the system INGRVM exists to replace. Free speech matters. So does not killing people with bad medical advice.

### The Principle

**Don't remove content. Add context.**

Anyone can publish anything. But the mesh gives every user the tools to evaluate what they're reading. The answer isn't censorship — it's transparency, peer review, and reputation that users can see and choose to trust.

### How It Works

#### 1. Peer Review Synapses

Reviews are themselves synapses — they attach to the synapse they're reviewing. Anyone can publish a review. Reviews are visible to anyone querying the original synapse.

```
KNOWLEDGE SYNAPSE: "Miracle Cancer Cure Protocol"
  └── REVIEW SYNAPSE: by @dr_sarah (oncologist, verified credentials)
      "This synapse contains unverified claims. The study cited 
       (Smith 2024) was retracted. No clinical trials support these 
       protocols. See [actual research synapse] for evidence-based info."
  └── REVIEW SYNAPSE: by @mesh_factcheck_collective
      "3 of 7 claims verified false. 2 unverifiable. 2 partially true.
       Detailed breakdown attached."
  └── REVIEW SYNAPSE: by @john_random
      "This cured my aunt!"
```

When a user queries the original synapse, the INGRVM AI can surface reviews alongside the answer:

```
USER: "What does the cancer cure synapse say about treatment X?"

INGRVM AI: "The synapse says [answer]. Note: this synapse has 3 peer 
  reviews. One from a verified oncologist flags the cited study as 
  retracted. A fact-check collective rated 3 of 7 claims false. 
  Want me to show the reviews or find alternative synapses on this topic?"
```

The user decides what to trust. The mesh just makes sure they have the information.

#### 2. Credential Verification (Optional, Never Required)

Creators can optionally verify credentials against external systems. This is never mandatory — anonymous synapses are always allowed. But verified credentials show up as trust signals.

```
VERIFICATION TYPES:
- Academic: Link to ORCID, university email, published papers
- Professional: Link to professional license (medical, legal, engineering)
- Institutional: Organization verifies member via signed attestation
- Community: N neurons vouch for expertise (web-of-trust)
- Track record: History of accurate, well-reviewed synapses on this topic
```

**Important:** Verification is a signal, not a gate. An unverified synapse about beekeeping from someone with 20 years of experience and great reviews is worth more than a verified PhD who's never kept a bee. The mesh shows both signals — the user decides.

#### 3. Community Fact-Check Synapses

Fact-checking is a synapse type. Fact-checkers earn $DOPA when their reviews are consumed. This creates an economic incentive to do the work.

```
FACT-CHECK SYNAPSE:
- Attaches to one or more target synapses
- Claim-by-claim analysis (verified / false / unverifiable / partially true)
- Sources cited with links
- Methodology disclosed
- Creator earns $DOPA when users read their fact-checks
```

Multiple fact-checkers can review the same synapse. They might disagree. That's fine — users see all perspectives.

#### 4. Trust Scoring (Transparent, User-Configurable)

Every synapse accumulates trust signals. These are never hidden and always decomposed — no black-box "trust score."

```
TRUST SIGNALS FOR A SYNAPSE:
- Peer review count and sentiment
- Creator credential status (verified / unverified / anonymous)
- Creator track record (accuracy of previous synapses in this topic)
- Citation count (how many other synapses reference this one)
- Age + update frequency (actively maintained vs abandoned)
- Stake backing (creator staked $DOPA behind this synapse — skin in the game)
- Fact-check results (if any)
- Community flags (if any, with reasons visible)
```

Users can configure how much weight they give each signal:
```
USER: "I only want to see health synapses with at least one 
  verified medical professional review."

USER: "Show me everything. I'll judge for myself."

USER: "Prioritize synapses with high citation counts."
```

The algorithm synapse handles this — your feed ranking factors in trust signals however you configure it.

#### 5. Stake-Backed Claims

For high-stakes topics (health, finance, legal), creators can optionally **stake $DOPA behind their synapse.** If a DAO dispute finds the content materially false and harmful, the stake gets slashed and distributed to fact-checkers who flagged it.

This is never mandatory. But staked synapses signal: "I'm putting money where my mouth is."

```
DISPUTE FLOW:
1. Fact-checker flags synapse with evidence
2. Creator can respond with counter-evidence
3. If unresolved, escalates to DAO vote
4. DAO reviews evidence from both sides
5. If content found materially false AND harmful:
   - Synapse gets a permanent "disputed" label (NOT removed)
   - Creator's stake gets slashed
   - Fact-checker gets rewarded from slashed stake
6. If dispute rejected:
   - Flag stays visible but marked "disputed — rejected by DAO"
   - Fact-checker's reputation takes a small hit
```

**Content is never removed.** Even a slashed synapse stays on the mesh with a "disputed" label. Users can still read it. They just have full context.

#### 6. The Hard Line: What About Genuinely Dangerous Content?

The content moderation system (already built) blocks upload of:
- Known illegal material (hash-matched via blocklist)
- MIME-type violations (executables disguised as media)

Beyond that? The mesh doesn't decide what's true. It provides tools for the community to annotate, review, fact-check, and flag. The user decides what they trust.

**The philosophy:** Centralized platforms censor because they're liable. INGRVM isn't a platform — it's infrastructure. Neurons are sovereign. The mesh gives you context. You make the call.

This is uncomfortable. It means wrong things will exist on the mesh. But it also means no corporation can decide what you're allowed to know. The trade-off is the point.

---

## 11. Development Governance: How Code Changes After Launch

### The Problem

You won't own INGRVM. That's the point. But you'll still want to push bug fixes, ship features, and improve the system. So will other developers. Without order, you get either chaos (anyone pushes anything) or centralization (one person controls the code). Both are bad.

### The Model: Protocol Governance

INGRVM development follows the same pattern as the mesh itself — decentralized, governed, transparent.

#### Feature Proposals (INGRVPs — INGRVM Proposals)

Inspired by BIPs (Bitcoin Improvement Proposals) and EIPs (Ethereum Improvement Proposals):

```
INGRVM PROPOSAL LIFECYCLE:

1. DRAFT
   - Anyone writes a proposal (problem, solution, technical spec)
   - Published as a KNOWLEDGE synapse on the mesh
   - Discussion happens via peer review synapses (same system as content reviews)

2. REVIEW
   - Core contributors and community review
   - Technical feasibility, security audit, UX implications
   - At least 2 independent reviews required for critical path changes

3. DAO VOTE
   - Proposal goes to governance DAO (already built)
   - Quadratic voting — prevents whale domination
   - Voting period: 7 days for features, 48 hours for critical bug fixes

4. IMPLEMENTATION
   - Approved proposals get implemented
   - Multiple developers can work on the same proposal
   - Code review by at least 1 other contributor before merge

5. DEPLOYMENT
   - Nodes choose when to update (sovereign — no forced updates)
   - Protocol changes require supermajority adoption to activate
   - Backward compatibility maintained for at least 1 version cycle
```

#### Bug Fixes vs Features

| Type | Process | Speed |
|---|---|---|
| **Critical security fix** | Fast-track: submit + 2 reviews + 48-hour DAO vote | Hours to days |
| **Bug fix** | Standard: submit + 1 review + merge | Days |
| **Minor feature** | Proposal + review + 7-day DAO vote | Weeks |
| **Protocol change** | Full proposal + security audit + 14-day vote + adoption threshold | Months |

Critical fixes can be fast-tracked because the DAO already has an expedited voting mechanism. The existing governance_dao.py supports this — just needs a `priority` field on proposals.

#### Your Role After Launch

You're not the owner. You're the **genesis contributor** — the person who wrote the first version. That gives you:
- Reputation and track record (trust signal in the governance system)
- No special privileges over the code (same proposal process as everyone else)
- The ability to keep building, improving, and shipping — with community oversight

This is how Linux, Bitcoin, and Ethereum work. The creator doesn't own the protocol. The protocol owns itself.

#### Preventing Forks

Hostile forks are always possible (it's open source). But the economic incentives discourage them:
- $DOPA has value on the main mesh. A fork starts from zero.
- Synapses, knowledge, and reputation don't transfer to forks.
- Network effects compound — the mesh with the most neurons wins.

---

## 12. Python to Rust: The Migration Question

### Current State

The core is Python (FastAPI). The only Rust mention in the codebase is a legacy note about porting `mini-snark` wrapper to Rust for faster mobile proofs.

### The Honest Answer: Don't Rewrite. Incrementally Replace.

A full Python-to-Rust rewrite would:
- Take 6-12 months minimum
- Reintroduce bugs in code that already works
- Block all feature development during the rewrite
- Probably fail (most rewrites do)

Instead, **incrementally replace hot paths** as they become bottlenecks:

```
WHAT STAYS PYTHON:
- Hub API (FastAPI is fast enough for HTTP/WebSocket)
- Synapse orchestration (business logic, not performance-critical)
- DAO governance (SQLite queries, voting math)
- CLI and configuration
- Development/testing tools

WHAT GETS PORTED TO RUST (when it matters):
- ZK proof generation/verification (crypto is CPU-bound — Rust = 10-50x faster)
- Gossip protocol core (network hot path, millions of messages)
- Transport layer (WiFi Direct, NAT traversal — needs system-level access)
- Spike encoding/decoding (binary protocol parsing)
- Mobile neuron core (the background service — Rust compiles to native Android/iOS)

HOW:
- Python calls Rust via PyO3 bindings (same process, no IPC overhead)
- Or Rust runs as a sidecar service (separate process, communicates via socket)
- Or Rust compiles to WASM for cross-platform sandboxed execution
```

The pattern: **Python stays as the glue and business logic. Rust replaces the engine where performance matters.** You never do a full rewrite. You swap parts under the hood while the car is running.

### The Mobile Angle

For the mobile neuron core service (the background process that earns $DOPA), Rust makes the most sense:
- Compiles to native ARM (Android + iOS)
- Low memory footprint (matters on phones)
- No Python runtime needed on the phone
- Can embed ONNX Runtime directly

The mobile app would be: **Kotlin/React Native UI → Rust neuron core → ONNX inference engine.** Python never runs on the phone.

---

## 13. PC & Laptop Offline Operation

### Yes, They Run Offline Too

The offline-first design from §8 applies to every device, not just phones:

```
ALL DEVICES OFFLINE:

PC (Hub)
├── Hosts synapses locally — responds to local queries
├── Runs inference on local model shards
├── Queues outbound gossip messages for when network returns
├── Tracks $DOPA earnings locally (deferred settlement)
└── Can serve as WiFi Direct hub for local mesh

Laptop (Relay)
├── Same as PC but also acts as relay between phone and PC
├── Hosts synapse copies for redundancy
├── Can create WiFi hotspot as mesh bridge
└── Aggregates and packages data for efficient sync when online

Phone (Edge)
├── Lightest footprint — hosts fewer synapses
├── Runs smaller model shards (ONNX/TFLite)
├── WiFi Direct connection to nearby neurons
├── Full INGRVM AI works against local knowledge
└── Stores queries for remote synapses until connected
```

### What's Different About PC/Laptop Offline vs Phone

PCs and laptops have more storage and compute, so they can:
- Host more synapses (act as local knowledge hubs)
- Run larger model shards (7B+ with enough RAM)
- Serve as the mesh backbone when online (higher bandwidth, more connections)
- Act as WiFi Direct hubs that phones connect to

The transport fallback chain is the same on every device. The difference is capacity, not architecture.

---

## 14. Precautions, Limitations & Things That Could Go Wrong

### Technical Limitations

**Latency is real.** Distributed inference adds network hops. For a 12-layer model split across 3 devices on LAN, expect 3x overhead per token. On WAN, worse. This is a physics constraint, not a software bug. The mesh wins on cost and privacy, not speed. Be honest about this in every user-facing context.

**Small models only (for now).** GPT-2 124M works across 3 devices. 7B requires more nodes and more RAM. The mesh can't run GPT-4-class models until there are enough high-end neurons. Don't promise what the hardware can't deliver.

**WiFi Direct range.** 200 meters per hop. Fine for a building, a campus, a neighborhood. Not a city. Multi-hop routing extends range but adds latency. True citywide mesh needs WAN or dedicated relay infrastructure.

**Battery drain.** A background service doing inference drains battery. Need aggressive power management: only process requests when charging or above 50%, sleep mode on battery saver, user-configurable thresholds.

**Storage pressure.** Hosting synapses takes disk space. A phone with 64GB can't host hundreds of media synapses. Need storage budgets, auto-eviction of least-used synapses, and clear user communication about what's using their space.

### Security Precautions

**Model poisoning.** If a malicious node distributes corrupted model shards, inference outputs garbage or worse. Need cryptographic weight verification (hash chains on shards) and community flagging. Currently have ZK proofs for work verification — extend to weight integrity.

**Synapse poisoning.** A KNOWLEDGE synapse could contain deliberately misleading info packaged to look authoritative. The trust/review system (§10) mitigates this but doesn't eliminate it. Users need to understand that "on the mesh" doesn't mean "verified true."

**Eclipse attacks.** A group of malicious neurons could surround a target neuron and feed it only manipulated information. Sybil resistance (stake-weighted identity) and diverse peer selection help, but this is an ongoing research problem for all P2P systems.

**Key management.** If a user loses their Ed25519 key, they lose their identity, their $DOPA, and their reputation. Social key recovery (N-of-M guardians) is in Track D but not built yet. Until then, lost key = lost everything. Make this VERY clear during onboarding.

**Legal gray area.** Running a node that hosts other people's content could create liability in some jurisdictions (DMCA, CSAM laws). The content moderation system blocks known-bad hashes, but mesh operators should understand the legal landscape. This is the same challenge IPFS, BitTorrent, and Tor face.

### Economic Precautions

**$DOPA value bootstrapping.** A token with no external value is just a point system. Until there's a fiat on-ramp or a Bittensor bridge, $DOPA is only worth what the community agrees it's worth. Don't overpromise on economic returns.

**Halving cliff.** The reward halving mechanism means early neurons earn more. But if halving is too aggressive, late joiners have no incentive. The "always a reason to join" design needs to be backed by math — model the economics under different growth scenarios.

**Marketplace spam.** If publishing synapses is free, the marketplace gets flooded with low-quality content. Need a small $DOPA registration fee (stake, not payment — returned if synapse is deleted) to create friction against spam without gatekeeping.

**Whale governance.** Quadratic voting helps but doesn't fully prevent wealthy nodes from dominating DAO decisions. Need ongoing governance research as the mesh scales.

### Things to Get Right Before Launch

1. **Onboarding flow.** If joining the mesh requires a CLI, nobody joins. The first experience must be: download app → create identity → start earning. Under 60 seconds.
2. **Key backup.** Before a user earns their first $DOPA, they must understand key recovery. Force a backup step during onboarding (seed phrase, guardian setup, or cloud backup with encryption).
3. **Transparent economics.** Show users exactly what they're earning, why, and what it's worth. No hidden mechanics. No "trust us, tokens will be valuable someday."
4. **Battery and storage controls.** Users must be able to set limits: "Only use 2GB for synapses," "Only run inference while charging," "Stop hosting if battery below 30%."
5. **Legal disclaimer.** Not legal advice, but users should understand: you're running a node, you may host content you didn't create, know your jurisdiction's rules.
6. **Versioning protocol.** Before multiple developers are pushing changes, the upgrade path must be clear. Node version negotiation, backward-compatible wire protocol, graceful degradation for old nodes talking to new ones.
