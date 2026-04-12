# INGRVM Mesh Internet — Design Specification

**Status:** Horizon 3 Design Doc
**Last Updated:** 2026-03-29

---

## The Ghost Internet

INGRVM is not the world wide web. It is an **overlay sovereign mesh** that runs
on top of existing infrastructure but is owned and operated entirely by its nodes.
No domain registrars. No hosting companies. No CDN bills. No DMCA takedowns to
a central authority. Content lives in the mesh because nodes *choose* to host it
and earn $DOPA for doing so.

This document defines the naming, addressing, and application primitives of the
mesh internet — the ghost layer that runs beneath the visible web.

---

## Addressing: The `mesh://` Protocol

### Why not URLs?

Traditional URLs (`https://example.com/path`) bind content to a server location.
If the server goes down, the content disappears. The mesh binds content to its
**identity** — the content fingerprint (SHA-256 CID). Location is discovered,
not hardcoded.

### Mesh Address Format

```
mesh://<handle>/<resource>[@version]
```

| Part        | Description                                      | Example                        |
|-------------|--------------------------------------------------|-------------------------------|
| `handle`    | Author's mesh identity (human-readable alias)   | `@architect`, `@coldplay`     |
| `resource`  | Synapse name or content slug                    | `dark-matter-ep`, `portfolio` |
| `@version`  | Optional pinned version                         | `@1.2.0`                      |

**Examples:**
```
mesh://@architect/dashboard
mesh://@coldplay/dark-matter-ep@2.0.0
mesh://@veganshop/checkout-portal
mesh://@gnews/feed
```

### Mesh Handles

A **Mesh Handle** is a human-readable alias for a node's Ed25519 public key.
Like a username, but cryptographically bound to an identity.

- Format: `@username` (alphanumeric, hyphens, underscores)
- Registered in the mesh identity registry (`synapse_registry.py`)
- Resolves to: `node_public_key` → list of `synapse_ids` authored by that node
- Cannot be squatted: handle registration requires a signed proof from the node key

### Content IDs (CIDs)

Every piece of content also has a **direct CID address**:

```
mesh://cid/<sha256_fingerprint>
```

CID addresses are permanent. `mesh://` handle addresses are convenient aliases.
Both route to the same content via the gossip discovery layer.

---

## Synapse Types as Internet Primitives

The existing SynapseType enum maps directly to familiar internet concepts:

| Web Concept       | INGRVM Equivalent   | SynapseType    | Description                              |
|-------------------|---------------------|----------------|------------------------------------------|
| Website / App     | **Synapse Portal**  | `COMPOSITE`    | Bundle of synapses forming an experience |
| Web Page          | **Synapse Frame**   | `KNOWLEDGE`    | Static or semi-static rendered content   |
| Streaming service | **Neural Stream**   | `MEDIA`        | Music, video, podcast hosted on mesh     |
| API endpoint      | **Callable Synapse**| `SKILL`        | Invokable function with typed I/O        |
| Data feed / RSS   | **Oracle Feed**     | `ORACLE`       | Live external or mesh-internal data      |
| Cloud function    | **Compute Node**    | `COMPUTE`      | GPU/CPU tasks farmed to mesh nodes       |

---

## Synapse Portals — Mesh-Native "Websites"

A **Synapse Portal** is a `COMPOSITE` synapse that bundles multiple synapses into
a coherent interactive experience. Think of it as a website, but:

- **Stateless**: No server to crash. State lives in the mesh ledger or user's node.
- **Distributed**: Each component synapse can be hosted by different nodes.
- **Paid**: Every interaction can carry a micro-payment, or be free (creator's choice).
- **Owned**: Registered in CopyrightRegistry. Creator earns on every visit.

### Portal Schema (extends SkillDefinition)

```json
{
  "synapse_id": "portal:architect:main",
  "synapse_type": "composite",
  "name": "The Architect's Hub",
  "pipeline": [
    "synapse:architect:nav",
    "synapse:architect:feed",
    "synapse:architect:contact"
  ],
  "license_type": "mesh-open",
  "author_id": "architect_node_pubkey",
  "royalty_percent": 0.0
}
```

### Rendering

Portals are rendered by the **Mobile UI** (`INGRVM/Mobile/ingrvm-ui`) or any
mesh-compatible client. The client:
1. Resolves `mesh://@architect/hub` → `synapse_id` via gossip registry
2. Fetches `COMPOSITE` pipeline definition
3. Invokes each component synapse in order, streaming results
4. Renders the assembled output (JSON → UI components)

---

## The Three Mesh Platforms

### 1. Neural Streams — Music & Video

**What it is:** Spotify + YouTube replacement, native to the mesh.

**How it works:**
- Artists register a `MEDIA` synapse per track/video
- Fingerprint is the content hash — prevents duplicate uploads
- File is chunked and distributed to willing host nodes
- Every stream triggers micro-royalty via RoyaltySplitter
- Co-authors (producers, featured artists) get automatic splits

**Mesh address:** `mesh://@coldplay/yellow`

**Revenue model:**
- Listener pays micro-DOPA per stream (set by artist, default 0.01 $DOPA)
- OR artist sets `license_type: "mesh-open"` → free streaming, artist earns from hosting rewards
- Labels / managers can be co-authors with defined royalty shares

**Anti-piracy:**
- CopyrightRegistry blocks duplicate fingerprint registration
- Content hash is the CID — you can't re-upload the same file under a new name
- Derived works (remixes) must declare `derived_from` and upstream creator gets 10% cut

**File types supported:**
- Audio: `audio/mp3`, `audio/flac`, `audio/wav`, `audio/ogg`
- Video: `video/mp4`, `video/webm`
- Art: `image/png`, `image/jpeg`, `image/webp`

---

### 2. Synapse Markets — Mesh Ecommerce

**What it is:** Shopify / Etsy replacement, but the store IS a synapse.

**How it works:**
- Sellers publish a `COMPOSITE` synapse as their storefront
- Each product is a `SKILL` synapse (`synapse_type: skill`, `price_dopa > 0`)
- Checkout = skill invocation → escrow → delivery proof → settlement
- Physical goods: delivery confirmation oracle closes escrow
- Digital goods: content delivered via `MEDIA` synapse on payment

**Mesh address:** `mesh://@veganshop/store`

**Revenue model:**
- Seller sets `price_dopa` per product
- No platform fees — mesh routing nodes earn compute rewards
- Marketplace discovery is free (searching the skill marketplace)

**Trust layer:**
- Buyer stakes 5.0 $DOPA (existing requirement)
- Seller reputation tracked by mesh rating system (existing `skill_ratings`)
- Escrow held until delivery confirmed or dispute resolved

---

### 3. SpikeNet — Mesh Social Media

**What it is:** Twitter / Instagram replacement where your feed is a KNOWLEDGE synapse.

**How it works:**
- A "post" is a registered `KNOWLEDGE` synapse (small, free, author earns hosting rewards)
- A "profile" is a `COMPOSITE` synapse (portal) aggregating that creator's posts
- "Following" = subscribing to updates on a creator's handle via gossip feed
- "Feed" = LLM Orchestrator querying KNOWLEDGE synapses from followed handles
- "Shares/Reposts" = `derived_from` link — original creator earns upstream cut

**Mesh address:** `mesh://@architect/posts`

**Revenue model:**
- No ads. Ever. Revenue comes from:
  - Hosting rewards for nodes that serve popular content
  - Creator tip system (direct $DOPA transfer on content invoke)
  - Paid gated posts (`price_dopa > 0`)
- Viral content = more invocations = more $DOPA to creator AND hosts

**Censorship resistance:**
- No central authority can remove content — only the registrant can deactivate
- CopyrightRegistry dispute process handles IP violations (DAO-resolved)
- Content fingerprinting prevents spam re-uploads

---

## Naming Conventions Summary

| Concept          | Mesh Name           | Address Format                    |
|------------------|---------------------|-----------------------------------|
| Website          | Synapse Portal      | `mesh://@handle/portal-name`      |
| App              | Mesh App            | `mesh://@handle/app-name`         |
| Music track      | Neural Stream       | `mesh://@artist/track-name`       |
| Video            | Mesh Cast           | `mesh://@creator/video-name`      |
| Social profile   | Spike Profile       | `mesh://@username/`               |
| Social post      | Spike               | `mesh://@username/posts/slug`     |
| Online store     | Synapse Market      | `mesh://@seller/store`            |
| Product listing  | Market Synapse      | `mesh://@seller/products/name`    |
| API service      | Callable Synapse    | `mesh://@provider/service-name`   |
| News / Feed      | Oracle Feed         | `mesh://@source/feed`             |
| File / Content   | CID direct          | `mesh://cid/<sha256>`             |

---

## Protocol vs. HTTP Comparison

| HTTP Web                  | Mesh Internet                           |
|---------------------------|-----------------------------------------|
| DNS → IP → Server         | Handle → Node Key → CID → Gossip Nodes |
| `https://`                | `mesh://`                               |
| Domain registration       | Mesh Handle registration (free, signed) |
| Hosting bill              | Earn $DOPA for hosting                  |
| CDN                       | Mesh gossip replication                 |
| PayPal / Stripe           | $DOPA ledger (built-in)                 |
| DMCA takedown             | DAO dispute + CopyrightRegistry         |
| Ad revenue                | Invocation royalties + hosting rewards  |
| Account suspension        | Reputation slashing (non-destructive)   |
| App Store 30% cut         | 0% — royalty_percent set by creator     |

---

## Implementation Roadmap

| Feature                        | Horizon | Status      |
|-------------------------------|---------|-------------|
| SynapseType + MEDIA support    | H3      | CODED       |
| CopyrightRegistry              | H3      | **DONE**    |
| RoyaltySplitter                | H3      | **DONE**    |
| Mesh Handle registry           | H3      | STUB        |
| `mesh://` URL resolver         | H3      | NOT STARTED |
| Neural Streams player UI       | H4      | NOT STARTED |
| Synapse Portal renderer        | H4      | NOT STARTED |
| SpikeNet feed aggregator       | H4      | NOT STARTED |
| Synapse Market checkout flow   | H4      | NOT STARTED |
| Physical delivery oracle       | H5      | ASPIRATIONAL|
