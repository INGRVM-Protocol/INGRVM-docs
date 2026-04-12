# Synapse Creator Guide

Build skills, upload content, and earn $DOPA on the INGRVM mesh.

---

## Quick Start (5 Minutes)

### 1. Register a SKILL synapse

```python
import requests, time, json

HUB = "http://localhost:8000"

# Your synapse definition
skill = {
    "synapse_id": "my_sentiment_v1",
    "name": "My Sentiment Analyzer",
    "synapse_type": "skill",
    "description": "Analyzes text sentiment using custom rules",
    "input_schema": {
        "type": "object",
        "properties": {"text": {"type": "string"}},
        "required": ["text"]
    },
    "output_schema": {
        "type": "object",
        "properties": {
            "sentiment": {"type": "string"},
            "confidence": {"type": "number"}
        }
    },
    "author_id": "YOUR_NODE_ID",
    "version": "1.0.0",
    "tags": ["nlp", "sentiment"],
    "signature": "your_ed25519_signature",
    "timestamp": time.time()
}

resp = requests.post(f"{HUB}/api/skills/register", json=skill)
print(resp.json())
# {"status": "REGISTERED", "synapse_id": "my_sentiment_v1"}
```

### 2. See it in the marketplace

```
GET http://localhost:8000/api/skills/marketplace?q=sentiment
```

### 3. Invoke it

```python
resp = requests.post(f"{HUB}/api/skills/invoke", json={
    "synapse_id": "my_sentiment_v1",
    "input_data": {"text": "I love this product"},
    "peer_id": "CALLER_NODE_ID",
    "signature": "caller_signature",
    "timestamp": time.time()
})
print(resp.json())
# {"synapse_id": "my_sentiment_v1", "status": "success", "output": {...}}
```

Every time your skill is invoked, **you earn royalties** (default 10%, configurable up to 90%).

---

## The 6 Synapse Types

### SKILL — Callable Function
Execute a task against input. Most common type.

**Examples:** Sentiment analysis, translation, code generation, image classification

```python
{
    "synapse_type": "skill",
    "input_schema": {
        "properties": {"text": {"type": "string"}},
        "required": ["text"]
    },
    "output_schema": {
        "properties": {
            "result": {"type": "string"},
            "confidence": {"type": "number"}
        }
    }
}
```

**Handler pattern:** Register a Python function that takes `dict` input, returns `dict` output.

---

### KNOWLEDGE — Corpus Retrieval (RAG)
Answers questions from a bundled text corpus. No model needed — keyword matching.

**Examples:** Medical research KB, legal FAQ, product documentation, mesh status info

```python
{
    "synapse_type": "knowledge",
    "input_schema": {
        "properties": {
            "query": {"type": "string"},
            "top_k": {"type": "integer"}
        },
        "required": ["query"]
    },
    "output_schema": {
        "properties": {
            "passages": {"type": "array"},
            "num_results": {"type": "integer"}
        }
    }
}
```

**Corpus:** Register a list of text passages. The executor scores them by keyword overlap with the query.

---

### MEDIA — Content Hosting & Streaming
Host audio, video, images, or any binary content. The backbone for INGRVM Music/Video.

**Examples:** Music tracks, podcast episodes, video clips, art galleries

```python
{
    "synapse_type": "media",
    "content_type": "audio/mp3",
    "royalty_percent": 70.0,  # Creator gets 70% of play revenue
    "input_schema": {
        "properties": {"action": {"type": "string"}},
        "required": ["action"]
    }
}
```

**Upload flow:**
```bash
curl -X POST http://localhost:8000/api/media/upload \
  -F "file=@my_song.mp3" \
  -F "name=My Song" \
  -F "author_id=ARTIST_NODE" \
  -F "content_type=audio/mp3" \
  -F "royalty_percent=70" \
  -F "signature=..." \
  -F "timestamp=..."
```

**What happens:**
1. Hub splits your file into 256KB chunks
2. Each chunk gets a CID hash (content-addressed, tamper-proof)
3. Your song is registered as a MEDIA synapse
4. Anyone can stream it; you earn $DOPA per play
5. Fans can volunteer to host your chunks, earning $DOPA too

**Streaming:**
```
GET /api/media/{synapse_id}/manifest    → chunk map
GET /api/media/{synapse_id}/stream/0    → first chunk
GET /api/media/{synapse_id}/stream/1    → second chunk
...
```

---

### COMPUTE — Raw Task Execution
Run GPU/CPU-intensive jobs. The handler receives task parameters and returns results.

**Examples:** Image generation, video transcoding, data processing, ML inference

```python
{
    "synapse_type": "compute",
    "input_schema": {
        "properties": {
            "task": {"type": "string"},
            "params": {"type": "object"}
        },
        "required": ["task"]
    },
    "output_schema": {
        "properties": {
            "result": {"type": "object"},
            "compute_time_ms": {"type": "number"}
        }
    }
}
```

**Earnings:** Compute synapses naturally earn more spikes (longer latency = more reward). Nodes with NPU/GPU get multiplied earnings (1.5x for NPU, 1.0x for GPU, 0.5x for CPU-only).

---

### ORACLE — External Data Feed
Bridge external APIs into the mesh. Fetch real-time data on demand.

**Examples:** Weather data, crypto prices, sports scores, news headlines

```python
{
    "synapse_type": "oracle",
    "input_schema": {
        "properties": {
            "feed": {"type": "string"},
            "params": {"type": "object"}
        },
        "required": ["feed"]
    },
    "output_schema": {
        "properties": {
            "data": {"type": "object"},
            "source": {"type": "string"},
            "timestamp": {"type": "number"}
        }
    }
}
```

**Handler example:** Your handler calls an external API, formats the response, returns it.

---

### COMPOSITE — Skill Pipeline
Chain multiple skills in sequence. Output of skill A becomes input of skill B.

**Examples:** "Translate then summarize", "Extract keywords then find related articles"

```python
{
    "synapse_type": "composite",
    "pipeline": ["translate_v1", "summarize_v1"],
    "input_schema": {
        "properties": {"text": {"type": "string"}},
        "required": ["text"]
    }
}
```

**How it works:**
1. Input goes to `translate_v1` → output: `{"translated": "..."}`
2. Output merged with original input, fed to `summarize_v1`
3. Final output includes both steps + the last step's result

Each skill in the pipeline must already be registered and have a handler.

---

## Royalties — How You Get Paid

Every `SkillDefinition` has a `royalty_percent` field (default: 10%).

**When your skill is invoked:**
1. System calculates total spikes based on execution latency
2. `royalty_percent` of those spikes go to your `author_id`
3. The rest goes to the hosting node that ran it
4. Hardware multiplier applies to the host (NPU=1.5x, GPU=1.0x)
5. Spikes convert to $DOPA at end of epoch

**Example:** Your skill takes 150ms → 15 spikes total. At 70% royalty:
- You (creator): 10 spikes
- Host node: 5 spikes (multiplied by hardware tier)

**For MEDIA:** Set royalty to 70%+ since the content is your IP.
**For SKILL/COMPUTE:** 10-30% is typical since the host does the compute work.

---

## Marketplace

### Publishing
When you register a skill via `/api/skills/register`, it's automatically published to the marketplace.

### Discovery
```
GET /api/skills/marketplace?q=sentiment&type=skill&sort=popular
```

Sort options: `popular`, `newest`, `rating`, `earnings`

### Ratings
Other nodes can rate your skill 1-5 stars:
```
POST /api/skills/rate
{"synapse_id": "...", "node_id": "...", "rating": 5, "comment": "Great skill!"}
```

### Stats
```
GET /api/skills/{synapse_id}/stats
```
Returns: invocation count, avg latency, total $DOPA earned, avg rating.

### Creator Earnings
```
GET /api/skills/creator/{author_id}/earnings
```
Returns: total skills, total $DOPA earned, total invocations across all your skills.

---

## Full API Reference

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/api/skills/catalog` | GET | List all registered skills |
| `/api/skills/{id}/schema` | GET | Get input/output schema |
| `/api/skills/register` | POST | Register new skill |
| `/api/skills/invoke` | POST | Invoke a skill (earns $DOPA) |
| `/api/skills/match?q=` | GET | Find skills by query |
| `/api/orchestrate` | POST | Multi-skill orchestration |
| `/api/skills/marketplace` | GET | Browse marketplace |
| `/api/skills/rate` | POST | Rate a skill |
| `/api/skills/{id}/stats` | GET | Usage statistics |
| `/api/skills/{id}` | DELETE | Delete (creator only) |
| `/api/skills/creator/{id}/earnings` | GET | Creator earnings |
| `/api/media/upload` | POST | Upload media content |
| `/api/media/{id}/manifest` | GET | Chunk manifest |
| `/api/media/{id}/stream/{chunk}` | GET | Stream a chunk |
| `/api/media/{id}/host` | POST | Volunteer to host |
| `/api/media/catalog` | GET | List all media |
| `/api/proof/generate` | POST | Generate ZK proof |
| `/api/proof/verify` | POST | Verify ZK proof |
