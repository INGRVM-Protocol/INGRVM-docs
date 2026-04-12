# INGRVM Hub API Guide

**Complete API reference for the INGRVM mesh hub endpoints.**

---

## Overview

The INGRVM Hub API is a FastAPI-based REST API that manages mesh coordination, inference requests, governance, and economy. All endpoints are served from the hub node, which acts as the central coordinator for the decentralized mesh.

**Base URL:** `http://<hub-ip>:8000`
**API Version:** `3.0.0` (as of INGRVM v0.2.0-horizon2)

---

## Authentication

### Node Identity Verification

All POST endpoints require cryptographic signatures to verify the sender's identity. The signature is an Ed25519 signature over a payload containing the request data and timestamp.

**Signature payload format:** `<field1><field2>...<timestamp>.encode()`

**Required headers:**
```json
{
  "Content-Type": "application/json",
  "X-Node-ID": "<node_id>",
  "X-Signature": "<signature>",
  "X-Timestamp": "<unix_timestamp>"
}
```

**Example signature creation (Python):**
```python
from identity.identity_manager import NodeIdentity

identity = NodeIdentity.load_or_create("node_id")
payload = f"node_idfield1field2{timestamp}".encode()
signature = identity.sign_data(payload)
```

---

## API Reference

### Health & Status

#### `GET /api/health`

Returns hub health information, connected nodes, and feature availability.

**Response:**
```json
{
  "status": "healthy",
  "uptime_seconds": 123.45,
  "active_nodes": 3,
  "active_node_ids": ["nodeid1", "nodeid2", "nodeid3"],
  "features": {
    "governance": true,
    "marketplace": true,
    "rewards": true
  },
  "missing_deps": [],
  "version": "0.2.0-horizon2",
  "node_id": "HUB_NODE_ID"
}
```

**Status values:**
- `healthy` - Hub is operational with at least one connected node
- `degraded` - Hub is running but has no connected nodes

---

#### `GET /api/mesh/ping`

Simple health check endpoint.

**Response:**
```json
{
  "status": "PONG",
  "timestamp": 1672531200.0
}
```

---

### Node Management

#### `GET /api/mesh/nodes`

Returns the current dynamic mesh topology including all connected nodes.

**Response:**
```json
{
  "HUB_NODE_ID": {
    "label": "PC_MASTER_HUB",
    "type": "HUB",
    "online": true,
    "last_seen": 1672531200.0,
    "staked_balance": 100.0,
    "reputation": 1.5
  },
  "NODE_ID_1": {
    "label": "LAPTOP_RELAY",
    "type": "DESKTOP",
    "online": true,
    "last_seen": 1672531195.0,
    "staked_balance": 50.0,
    "reputation": 0.8
  }
}
```

---

#### `POST /api/mesh/register`

Registers a node in the dynamic mesh registry.

**Request Body:**
```json
{
  "node_id": "YOUR_NODE_ID",
  "role": "COMPUTE",
  "shard_range": "11-20",
  "label": "LAPTOP_RELAY",
  "signature": "base64_ed25519_signature",
  "timestamp": 1672531200.0
}
```

**Signature payload:** `<node_id><role><timestamp>.encode()`

**Response:**
```json
{
  "status": "REGISTERED",
  "node": "YOUR_NODE_ID"
}
```

**Error responses:**
- `403 Forbidden` - Invalid signature or timestamp
- `400 Bad Request` - Missing required fields

---

#### `POST /api/vitals`

Submits hardware vitals and refreshes the node's TTL in the registry.

**Request Body:**
```json
{
  "temp": 65.5,
  "vram_used": 4.2,
  "vram_total": 8.0,
  "load": 45,
  "psk": "INGRVM_SECURE_2026",
  "node_id": "YOUR_NODE_ID",
  "signature": "base64_ed25519_signature",
  "timestamp": 1672531200.0
}
```

**Signature payload:** `<node_id><temp><timestamp>.encode()`

**Response:**
```json
{
  "status": "OK"
}
```

**Error responses:**
- `403 Forbidden` - Invalid PSK, signature, or timestamp

---

#### `GET /api/mesh/qr-data`

Returns unique Hub identity payload for QR code-based node registration.

**Response:**
```json
{
  "peer_id": "HUB_PEER_ID",
  "label": "PC_MASTER_HUB",
  "hub_url": "http://192.168.1.100:8000",
  "shard_range": "0-10",
  "role": "HUB"
}
```

---

### Identity & Wallet

#### `GET /api/identity/self`

Returns cryptographic passport info for a node. If no `peer_id` is provided, returns the Hub's own identity.

**Query Parameters:**
- `peer_id` (optional) - The node ID to query. Defaults to calling node.

**Response:**
```json
{
  "peer_id": "HUB_NODE_ID",
  "node_id": "PC_MASTER_HUB",
  "balance": 42.5,
  "staked_balance": 100.0,
  "reputation": 1.5,
  "trust_score": 0.75,
  "role": "HUB",
  "installed_synapses": ["gpt2", "sentiment_alpha"]
}
```

**Field descriptions:**
- `balance` - Total $DOPA balance
- `staked_balance` - $DOPA currently staked for governance
- `reputation` - Accumulated reputation score from verified work
- `trust_score` - Derived trust score (capped at 1.0)
- `installed_synapses` - List of installed model shards

---

### Inference API

#### `POST /api/command`

Queues an inference request for the Neural Worker (legacy command interface).

**Request Body:**
```json
{
  "text": "The quick brown fox",
  "peer_id": "YOUR_NODE_ID",
  "signature": "base64_ed25519_signature",
  "timestamp": 1672531200.0
}
```

**Signature payload:** `<peer_id><text><timestamp>.encode()`

**Response:**
```json
{
  "status": "QUEUED"
}
```

---

#### `POST /api/inference`

Submit a paid inference request. Client must have sufficient stake and balance.

**Request Body:**
```json
{
  "client_id": "YOUR_CLIENT_ID",
  "model_name": "gpt2",
  "input_text": "The quick brown fox",
  "max_tokens": 50,
  "temperature": 0.8,
  "signature": "base64_ed25519_signature",
  "timestamp": 1672531200.0
}
```

**Signature payload:** `<client_id><input_text><timestamp>.encode()`

**Response:**
```json
{
  "status": "submitted",
  "request_id": "inf_1672531200_abc123"
}
```

**Error responses:**
- `402 Payment Required` - Insufficient balance or stake
- `403 Forbidden` - Invalid signature or timestamp

**Status values:**
- `submitted` - Request accepted and queued
- `payment_rejected` - Payment verification failed

---

#### `GET /api/inference/{request_id}`

Check the status of a paid inference request.

**URL Parameters:**
- `request_id` - The ID returned from `/api/inference`

**Response (queuing):**
```json
{
  "request_id": "inf_1672531200_abc123",
  "status": "queued",
  "position": 0,
  "estimated_time_ms": 500
}
```

**Response (processing):**
```json
{
  "request_id": "inf_1672531200_abc123",
  "status": "processing",
  "progress": 0.45,
  "current_shard": "shard_001"
}
```

**Response (completed):**
```json
{
  "request_id": "inf_1672531200_abc123",
  "status": "complete",
  "output_text": "jumps over the lazy dog.",
  "tokens_generated": 9,
  "latency_ms": 420,
  "cost_dopa": 0.05
}
```

**Status values:**
- `queued` - Waiting in queue
- `processing` - Currently being computed
- `complete` - Finished successfully
- `failed` - Error occurred during processing

---

### Model Management

#### `GET /api/models`

List all available models via ModelRegistry (for cold start discovery).

**Response:**
```json
{
  "models": [
    {
      "name": "gpt2",
      "shards": ["shard_001.pt", "shard_002.pt"],
      "param_count": 124000000,
      "quantization": "8-bit",
      "category": "Foundation Model"
    },
    {
      "name": "sentiment_alpha",
      "shards": ["sentiment_alpha.pt"],
      "param_count": 100000,
      "quantization": "32-bit",
      "category": "Classification"
    }
  ]
}
```

---

#### `GET /api/models/{name}/config`

Return `shard_config.json` for a specific model.

**URL Parameters:**
- `name` - Model name (e.g., `gpt2`)

**Response:**
```json
{
  "model_name": "gpt2",
  "param_count": 124000000,
  "quantization": "8-bit",
  "shards": [
    {
      "id": "shard_001",
      "layers": [0, 1, 2, 3, 4, 5],
      "file": "shard_001.pt"
    },
    {
      "id": "shard_002",
      "layers": [6, 7, 8, 9, 10, 11],
      "file": "shard_002.pt"
    }
  ],
  "num_shards": 2
}
```

**Error responses:**
- `404 Not Found` - Model not found
- `500 Internal Server Error` - Registry unavailable

---

#### `GET /api/models/{name}/shard/{filename}`

Stream a shard `.pt` file for download (cold start).

**URL Parameters:**
- `name` - Model name (e.g., `gpt2`)
- `filename` - Shard filename (e.g., `shard_001.pt`)

**Response:**
- Binary file (`application/octet-stream`)

**Error responses:**
- `404 Not Found` - Shard not found
- `500 Internal Server Error` - Registry unavailable

---

### Marketplace API

#### `GET /api/marketplace/catalog`

List all available INGRVMs in the marketplace.

**Response:**
```json
{
  "ingrvms": [
    {
      "ingrvm_id": "gpt2_1672531200",
      "name": "GPT-2 Small",
      "author_id": "AUTHOR_1",
      "version": "1.0.0",
      "category": "Foundation Model",
      "description": "124M parameter GPT-2 model",
      "cid": "abc123def456",
      "architecture": "SNN-LIF",
      "downloads": 42
    }
  ]
}
```

---

#### `POST /api/marketplace/upload`

Upload a new INGRVM to the marketplace.

**Request:** `multipart/form-data`

**Form fields:**
- `name` - INGRVM name
- `author_id` - Publisher's node ID
- `version` - Version string
- `category` - Category (e.g., "Foundation Model")
- `description` - Human-readable description
- `signature` - Ed25519 signature
- `timestamp` - Unix timestamp
- `file` - `.pt` model file

**Signature payload:** `<author_id><name><timestamp>.encode()`

**Response:**
```json
{
  "status": "UPLOADED",
  "ingrvm_id": "gpt2_1672531200"
}
```

**Error responses:**
- `403 Forbidden` - Invalid signature or timestamp

---

#### `POST /api/marketplace/install`

Install a synapse (model shard) from the marketplace.

**Request Body:**
```json
{
  "peer_id": "YOUR_NODE_ID",
  "synapse_name": "gpt2",
  "category": "Foundation Model",
  "signature": "base64_ed25519_signature",
  "timestamp": 1672531200.0
}
```

**Signature payload:** `<peer_id><synapse_name><timestamp>.encode()`

**Response:**
```json
{
  "status": "INSTALLED",
  "assigned_layers": "11-20",
  "ingrvm_id": "gpt2_1672531200"
}
```

**Layer assignment logic:**
- HUB nodes: Layers 0-10 (initial layers)
- DESKTOP nodes: Layers 11-20 (middle layers)
- MOBILE nodes: Layers 21-31 (final layers)

**Error responses:**
- `403 Forbidden` - Invalid signature or timestamp
- `404 Not Found` - Synapse not found in marketplace

---

### Governance API

#### `POST /api/mesh/governance/create_proposal`

Create a new governance proposal for mesh-wide voting.

**Request Body:**
```json
{
  "proposer_id": "YOUR_NODE_ID",
  "description": " proposal description...",
  "target_ingrvm": "gpt2_1672531200",
  "weights_hash": "abc123def456",
  "options": ["YES", "NO"],
  "signature": "base64_ed25519_signature",
  "timestamp": 1672531200.0
}
```

**Signature payload:** `<proposer_id><description>...<timestamp>.encode()`

**Response:**
```json
{
  "status": "PROPOSAL_CREATED",
  "proposal_id": "prop_1672531200",
  "expires_at": 1672622400.0
}
```

---

#### `POST /api/mesh/governance/vote`

Cast a vote on an existing proposal.

**Request Body:**
```json
{
  "peer_id": "YOUR_NODE_ID",
  "proposal_id": "prop_1672531200",
  "vote": "YES",
  "signature": "base64_ed25519_signature",
  "timestamp": 1672531200.0
}
```

**Signature payload:** `<peer_id><proposal_id><vote><timestamp>.encode()`

**Response:**
```json
{
  "status": "VOTE_RECORDED",
  "proposal_id": "prop_1672531200",
  "your_vote": "YES"
}
```

---

#### `GET /api/mesh/governance/get_proposal/{proposal_id}`

Get proposal details and current vote tallies.

**URL Parameters:**
- `proposal_id` - Proposal ID

**Response:**
```json
{
  "proposal_id": "prop_1672531200",
  "proposer_id": "AUTHOR_1",
  "description": "Proposal description...",
  "votes": {
    "YES": { "count": 8, "stake_weight": 120.5 },
    "NO": { "count": 2, "stake_weight": 30.0 }
  },
  "total_votes": 10,
  "status": "OPEN",
  "created_at": 1672531200.0,
  "expires_at": 1672622400.0
}
```

**Status values:**
- `OPEN` - Voting is still active
- `PASSED` - Proposal passed, will execute soon
- `FAILED` - Proposal failed
- `EXECUTED` - Proposal has been executed

---

#### `POST /api/mesh/governance/tally_votes`

Tally votes and determine the outcome of a proposal (can also trigger automatically).

**Request Body:**
```json
{
  "peer_id": "YOUR_NODE_ID",
  "proposal_id": "prop_1672531200",
  "signature": "base64_ed25519_signature",
  "timestamp": 1672531200.0
}
```

**Signature payload:** `<peer_id><proposal_id><timestamp>.encode()`

**Response:**
```json
{
  "status": "TALLY_COMPLETE",
  "proposal_id": "prop_1672531200",
  "winner": "YES",
  "total_votes": 10,
  "execution_scheduled": true
}
```

---

### Staking API

#### `POST /api/mesh/stake`

Stake $DOPA tokens to become an active validator with governance rights.

**Request Body:**
```json
{
  "peer_id": "YOUR_NODE_ID",
  "amount": 10.0,
  "signature": "base64_ed25519_signature",
  "timestamp": 1672531200.0
}
```

**Signature payload:** `<peer_id><amount><timestamp>.encode()`

**Response:**
```json
{
  "status": "STAKED",
  "peer_id": "YOUR_NODE_ID",
  "amount_staked": 10.0,
  "total_staked": 10.0,
  "governance_power": 10.0
}
```

---

#### `POST /api/mesh/unstake`

Unstake $DOPA tokens (with unbonding period).

**Request Body:**
```json
{
  "peer_id": "YOUR_NODE_ID",
  "amount": 5.0,
  "signature": "base64_ed25519_signature",
  "timestamp": 1672531200.0
}
```

**Signature payload:** `<peer_id><amount><timestamp>.encode()`

**Response:**
```json
{
  "status": "UNSTAKING",
  "peer_id": "YOUR_NODE_ID",
  "amount_unstaking": 5.0,
  "remaining_staked": 5.0,
  "unbonding_complete": 1673619200.0
}
```

**Unbonding period:** 7 days (504,000 seconds)

---

### Mobile Node API

#### `POST /api/mobile/vitals`

Submit mobile node vitals with PSK authentication.

**Request Body:**
```json
{
  "temp": 45.0,
  "battery": 75,
  "network_type": "WIFI",
  "cpu_load": 30,
  "psk": "INGRVM_SECURE_2026",
  "node_id": "MOBILE_NODE_ID",
  "signature": "base64_ed25519_signature",
  "timestamp": 1672531200.0
}
```

**Signature payload:** `<node_id><temp><timestamp>.encode()`

**Response:**
```json
{
  "status": "OK"
}
```

---

### Utilities

#### `GET /api/version`

Get the hub API version information.

**Response:**
```json
{
  "version": "0.2.0-horizon2",
  "api_version": "3.0.0",
  "commit": "abc123def456"
}
```

---

## WebSocket API

The hub provides a WebSocket endpoint for real-time updates on mesh topology, node vitals, and log streaming.

**Endpoint:** `ws://<hub-ip>:8000/ws`

**Connection request:**
```javascript
const ws = new WebSocket('ws://192.168.1.100:8000/ws');
```

**Message types:**

### 1. MESH_UPDATE
Broadcast when a node joins, leaves, or updates its status.

```json
{
  "type": "MESH_UPDATE",
  "payload": {
    "HUB_NODE_ID": {
      "label": "PC_MASTER_HUB",
      "type": "HUB",
      "online": true,
      "last_seen": 1672531200.0
    }
  }
}
```

### 2. VITALS
Broadcast when a node submits new vitals.

```json
{
  "type": "VITALS",
  "payload": {
    "temp": 65.5,
    "vram_used": 4.2,
    "vram_total": 8.0,
    "load": 45,
    "node_id": "NODE_ID"
  }
}
```

### 3. LOG
Broadcast when new log entries are generated.

```json
{
  "type": "LOG",
  "payload": {
    "timestamp": 1672531200.0,
    "level": "INFO",
    "message": "Node registered: LAPTOP_RELAY"
  }
}
```

### 4. INFERENCE_UPDATE
Broadcast during inference progress updates.

```json
{
  "type": "INFERENCE_UPDATE",
  "payload": {
    "request_id": "inf_abc123",
    "status": "processing",
    "progress": 0.5
  }
}
```

---

## Error Handling

All endpoints may return standard HTTP error responses:

### 400 Bad Request
**Cause:** Missing or invalid request parameters

**Response:**
```json
{
  "detail": "Invalid request: missing required field 'node_id'"
}
```

### 401 Unauthorized
**Cause:** Missing or invalid authentication credentials

**Response:**
```json
{
  "detail": "Authentication required"
}
```

### 403 Forbidden
**Cause:** Invalid signature, stale timestamp, or insufficient permissions

**Response:**
```json
{
  "detail": "Invalid node signature or timestamp"
}
```

### 404 Not Found
**Cause:** Requested resource does not exist

**Response:**
```json
{
  "detail": "Model 'gpt3' not found"
}
```

### 500 Internal Server Error
**Cause:** Server-side error

**Response:**
```json
{
  "detail": "Registry unavailable"
}
```

---

## Rate Limiting

Currently no rate limiting is enforced. Clients are expected to:

1. Respect the mesh's capacity
2. Implement client-side rate limiting
3. Use WebSocket subscriptions for real-time updates instead of polling

---

## Best Practices

1. **Always sign requests** - Never send unauthenticated requests except to `GET /api/health` and `GET /api/mesh/ping`

2. **Include accurate timestamps** - Use current Unix timestamp (±60 seconds tolerance)

3. **Check health before operations** - Call `/api/health` to verify hub status

4. **Use WebSocket for real-time updates** - More efficient than polling

5. **Handle errors gracefully** - Implement retry logic with exponential backoff

6. **Verify signatures server-side** - The hub does this, but double-check if building custom clients

7. **Secure your node identity file** - Keep `neuromorphic_env/identity.key` private

---

## Example Clients

### Python Client

```python
import requests
import time
from identity.identity_manager import NodeIdentity

class INGRVMClient:
    def __init__(self, hub_url: str, node_id: str):
        self.hub_url = hub_url
        self.node_id = node_id
        self.identity = NodeIdentity.load_or_create(node_id)

    def sign_payload(self, payload: str) -> str:
        timestamp = time.time()
        signature = self.identity.sign_data(
            f"{payload}{timestamp}".encode()
        )
        return {
            signature": signature,
            "timestamp": timestamp
        }

    def submit_inference(self, text: str) -> str:
        payload = f"{self.node_id}{text}"
        auth = self.sign_payload(payload)

        response = requests.post(
            f"{self.hub_url}/api/inference",
            json={
                "client_id": self.node_id,
                "model_name": "gpt2",
                "input_text": text,
                "max_tokens": 50,
                "temperature": 0.8,
                **auth
            }
        )
        return response.json()["request_id"]

    def get_inference_status(self, request_id: str):
        response = requests.get(
            f"{self.hub_url}/api/inference/{request_id}"
        )
        return response.json()

# Usage
client = INGRVMClient("http://192.168.1.100:8000", "MY_NODE_ID")
req_id = client.submit_inference("The future is")
result = client.get_inference_status(req_id)
print(result)
```

### WebSocket Client (Python)

```python
import asyncio
import websockets
import json

async def listen_to_mesh(hub_url: str):
    ws_uri = hub_url.replace("http://", "ws://") + "/ws"

    async with websockets.connect(ws_uri) as websocket:
        print(f"Connected to {ws_uri}")

        while True:
            message = await websocket.recv()
            data = json.loads(message)

            if data["type"] == "MESH_UPDATE":
                print(f"Mesh updated: {data['payload']}")
            elif data["type"] == "LOG":
                print(f"Log: {data['payload']['message']}")
            elif data["type"] == "VITALS":
                print(f"Vitals: {data['payload']}")

asyncio.run(listen_to_mesh("http://192.168.1.100:8000"))
```

---

## Changelog

### v3.0.0 (Current)
- Added paid inference API
- Added model registry and cold start distribution
- Enhanced governance with proposal voting
- Added staking/unstaking endpoints
- Integrated mobile vitals with PSK authentication
- Improved error handling and logging

### v2.0.0
- WebSocket real-time updates
- Node registration with signature verification
- Basic inference queue management

### v1.0.0
- Initial API implementation
- Health checks and mesh topology
- Basic node management

---

**For questions, issues, or contributions, see the GitHub repository or join the community chat.**

**Built with FastAPI, powered by the INGRVM mesh. 🌿**