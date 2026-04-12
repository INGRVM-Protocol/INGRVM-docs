# INGRVM Quickstart Guide

**Get your INGRVM node running in under 5 minutes.**

---

## What is INGRVM?

INGRVM is a **decentralized mesh network for AI inference**. Instead of relying on centralized cloud providers (OpenAI, Anthropic, etc.), INGRVM splits neural network models into shards and distributes them across consumer devices. Your device contributes compute power to the mesh and earns $DOPA tokens in return.

**Key Features:**
- ✅ Zero-trust model execution with cryptographic verification
- ✅ Earn $DOPA tokens for verified compute work
- ✅ Run local AI models on consumer hardware
- ✅ Privacy-first: your data never leaves the mesh encrypted
- ✅ Skill discovery and execution (Horizon 3)
- ✅ Solarpunk vision: decentralized, community-owned AI infrastructure

**Current Status:** Horizon 3 IN PROGRESS — Foundations for skill discovery and execution are being built. Horizons 0, 1, and 2 are fully verified.

---

## Prerequisites

### Minimum Requirements
- **CPU:** Any modern CPU (x86-64 or ARM64)
- **RAM:** 4GB minimum, 8GB+ recommended
- **Disk:** 5GB free space for models and state
- **Network:** Stable internet connection or local LAN
- **OS:** Linux, macOS, or Windows (with WSL or MinGW)

###Software Dependencies
- **Python:** 3.10 or 3.11
- **pip:** Python package manager
- **git:** Version control (for cloning the repository)

Optional (for faster inference):
- **torch:** PyTorch (recommended for model serving)
- **CUDA/Metal:** GPU acceleration (if you have a compatible GPU)

---

## Installation

### Step 1: Clone the Repository

```bash
git clone https://github.com/your-repo/ingrvm.git
cd ingrvm
```

```powershell
# Windows PowerShell
git clone https://github.com/your-repo/ingrvm.git
cd ingrvm
```

### Step 2: Install Python Dependencies

```bash
cd INGRVM/Core
pip install -r requirements.txt
```

**If torch installation fails** (common on some systems):

```bash
# CPU-only PyTorch (works everywhere)
pip install torch --index-url https://download.pytorch.org/whl/cpu
```

### Step 3: Configure Your Node

Copy the example environment file and fill in required values:

```bash
cd ../..
cp INGRVM/.env.example INGRVM/.env
```

**Edit `INGRVM/.env` with your values:**

```env
# Network Settings (adjust for your setup)
INGRVM_P2P_PORT=60001
INGRVM_DISCOVERY_PORT=60002
INGRVM_API_PORT=8000
INGRVM_NODE_IP=                  # Leave empty to auto-detect
INGRVM_NODE_ID=                   # Auto-generated on first run

# Hub Settings (use a known hub or bootstrap peer)
INGRVM_BOOTSTRAP_PEERS="/ip4/127.0.0.1/tcp/60001/p2p/12D3KooW_PC"
INGRVM_HUB_URL=http://127.0.0.1:8000
INGRVM_HUB_PORT=8000
INGRVM_HUB_HOST=0.0.0.0

# Paths (relative to project root works fine)
INGRVM_DOPAAPSES_DIR=neuromorphic_env/ingrvms
INGRVM_PACKAGES_DIR=neuromorphic_env/packages
INGRVM_IDENTITY_FILE=neuromorphic_env/identity.key
INGRVM_PEER_DB=neuromorphic_env/peer_db.json

# Economy & Rewards
INGRVM_SPIKE_COST=0.05
INGRVM_MAX_ENERGY=100.0

# Security - CRITICAL: Generate secure random values
ingrvm_secure_psk=REPLACE_WITH_STRONG_RANDOM_HEX_64_CHARS
ingrvm_mobile_psk=REPLACE_WITH_STRONG_RANDOM_HEX_64_CHARS
ingrvm_master_key=REPLACE_WITH_STRONG_RANDOM_HEX_64_CHARS
```

**Generate secure secrets:**

```bash
python -c "import secrets; print('INGRVM_SECURE_PSK=' + secrets.token_hex(32))"
python -c "import secrets; print('INGRVM_MOBILE_PSK=' + secrets.token_hex(32))"
python -c "import secrets; print('INGRVM_MASTER_KEY=' + secrets.token_hex(32))"
```

---

## Running Your Node

### Option 1: Using the Unified Binary (Recommended)

The `ingrvm.py` script provides a simple interface for all operations:

```bash
# From the project root
python ingrvm.py start      # Start as a neural node
python ingrvm.py hub        # Start as the hub (coordinator)
python ingrvm.py tray       # Start desktop status monitor
python ingrvm.py wallet     # Open the mesh wallet
python ingrvm.py bench      # Run hardware benchmark
python ingrvm.py sync       # Sync mesh state
```

### Option 2: Direct Python Execution

```bash
# Start the hub (run this on one device in your mesh)
cd INGRVM/Core
python api/hub_server.py

# In another terminal, start a neural node
cd INGRVM/Core
python mesh/node.py
```

### Option 3: Windows PowerShell

```powershell
# From project root
python ingrvm.py start
python ingrvm.py hub
```

---

## Joining an Existing Mesh

If you're joining an existing mesh (not creating your own):

1. **Get the Hub URL:** Ask the mesh operator for the hub URL (e.g., `http://192.168.1.100:8000`)

2. **Update your `.env`:**
   ```env
   INGRVM_HUB_URL=http://192.168.1.100:8000
   INGRVM_BOOTSTRAP_PEERS="/ip4/192.168.1.100/tcp/60001/p2p/REAL_PEER_ID"
   ```

3. **Start your node:**
   ```bash
   python ingrvm.py start
   ```

Your node will automatically:
- Generate its cryptographic identity (Ed25519 keys)
- Discover and register with the hub
- Begin receiving model shards
- Start earning $DOPA for verified compute work

---

## Running Your First Inference

Once your node is registered and connected:

### 1. Check Your Node Status

```bash
# Using Python
import requests
response = requests.get("http://localhost:8000/api/health")
print(response.json())
```

**Expected output:**
```json
{
  "status": "healthy",
  "uptime_seconds": 123.45,
  "active_nodes": 3,
  "node_id": "YOUR_NODE_ID",
  "features": {
    "governance": true,
    "marketplace": true,
    "rewards": true
  },
  "version": "0.2.0-horizon2"
}
```

### 2. List Available Models

```bash
import requests
response = requests.get("http://localhost:8000/api/mesh/models")
print(response.json())
```

### 3. Run Inference

```bash
import requests
import json

# Submit an inference request
payload = {
    "client_id": "YOUR_CLIENT_IP",
    "model_name": "gpt2",
    "input_text": "The quick brown fox",
    "max_tokens": 50,
    "temperature": 0.8
}

response = requests.post("http://localhost:8000/api/inference", json=payload)
result = response.json()
print(result)
```

**Expected output:**
```json
{
  "task_id": "abc123-def456-ghi789",
  "status": "queued",
  "position": 0
}
```

### 4. Check Your Inference Status

```bash
task_id = result["task_id"]
response = requests.get(f"http://localhost:8000/api/inference/{task_id}")
print(response.json())
```

When complete, you'll see:
```json
{
  "task_id": "abc123-def456-ghi789",
  "status": "complete",
  "output_text": "The quick brown fox jumps over the lazy dog.",
  "tokens_generated": 9,
  "latency_ms": 420
}
```

---

## Earning $DOPA Tokens

As your node processes inference requests, you'll automatically earn $DOPA tokens based on:

- **Verified compute work:** Each inference task generates rewards
- **Stake multiplier:** Nodes that stake more $DOPA earn greater rewards
- **Quality score:** Consensus verification determines trustworthiness

**Check your balance:**
```bash
python ingrvm.py wallet
```

**Under the hood,** rewards are calculated by the `RewardEngine` using the formula:

```
R_i = E × (w_i × q_i) / Σ(w_j × q_j)
```

Where:
- `E` = Total $DOPA emission for the epoch
- `w_i` = Your node's stake
- `q_i` = Your node's quality score
- `Σ(w_j × q_j)` = Sum of all nodes' stake × quality

---

## Troubleshooting

### "ModuleNotFoundError: No module named 'torch'"

**Solution:** Install PyTorch (CPU version works):
```bash
pip install torch --index-url https://download.pytorch.org/whl/cpu
```

### "Invalid PSK" error

**Solution:** Ensure `INGRVM_SECURE_PSK` in your `.env` is a strong random value and matches across all nodes in your mesh.

### "Could not connect to hub"

**Solution:** Check that:
1. The hub is running (`python ingrvm.py hub`)
2. `INGRVM_HUB_URL` in your `.env` is correct
3. Firewall allows connections on port 8000

### Node not appearing in mesh

**Solution:**
1. Check node logs for errors: `tail -f logs/node.log`
2. Verify node signature is valid (auto-generated on first run)
3. Check that bootstrap peer is reachable

### Port already in use

**Solution:**
- Change `INGRVM_P2P_PORT` in your `.env`
- Or stop the conflicting process
- On Linux/macOS: `lsof -i :8000` to find the process

---

## Next Steps

1. **Explore the API:** See `API_GUIDE.md` for full API documentation
2. **Join a public mesh:** Find active meshes in the community
3. **Contribute compute:** Leave your node running to earn $DOPA
4. **Develop applications:** Build apps on the INGRVM mesh
5. **Participate in governance:** Vote on mesh upgrades with your stake

---

## Resource Links

- **Full API Reference:** `API_GUIDE.md`
- **Architecture:** `ARCHITECTURE.md`
- **Roadmap:** `ROADMAP.md`
- **Phase Status:** `PHASE_STATUS.md`
- **GitHub Issues:** Report bugs and feature requests
- **Discord/Matrix:** Join the community chat

---

## Security Best Practices

1. **Never commit `.env` to version control** - it contains secrets
2. **Generate strong PSKs** - use the provided Python command
3. **Keep your node identity file safe** - `neuromorphic_env/identity.key`
4. **Run regular updates** - `git pull` to get security patches
5. **Monitor your logs** - watch for unauthorized access attempts

---

## Support

If you run into issues:

1. Check the **Troubleshooting** section above
2. Review the **logs** in `logs/` directory
3. Search existing **GitHub issues**
4. Ask in the **community chat**
5. Open a new issue with:
   - Your operating system
   - Python version: `python --version`
   - Error message or logs
   - Steps to reproduce

---

**Welcome to the mesh. 🌿 Let's build the future of decentralized AI together.**