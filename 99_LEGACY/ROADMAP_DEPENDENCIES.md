# INGRVM Dependency-based Roadmap (March 2026)
*Architect: INGRVM_ARCHITECT (Gemini CLI)*

## 🚦 Roadmap Overview
This roadmap is structured by **Dependency Layers**. Each task must be verified before proceeding to the next layer to ensure a stable, hardened mesh.

---

### Layer 1: Security & Protocol Hardening (Phase 7.2)
*Dependency: Hub v3.1.0 baseline.*

- [ ] **7.2.1: Signature Enforcement (Hub)**
  - Path: `INGRVM/Core/api/hub_api.py`
  - Task: Activate `verify_node_request` on all mutating endpoints.
- [ ] **7.2.2: Replay Protection**
  - Path: `INGRVM/Core/blockchain/reward_validator.py`
  - Task: Implement a sliding window for processed spike nonces/timestamps.
- [ ] **7.2.3: Signed Marketplace Uploads**
  - Path: `INGRVM/Core/api/hub_api.py`, `hub_state.py`
  - Task: Require valid `NodeIdentity` signature for all model uploads.
- [ ] **7.2.4: Torch Security Patch**
  - Path: `INGRVM/Core/neural/brain_models.py`, `ingrvm_native_ai.py`
  - Task: Implement `torch.load(..., weights_only=True)`.

---

### Layer 2: Physical Mesh Validation (Phase 7.3)
*Dependency: Layer 1 (Hardening) verified.*

- [ ] **7.3.1: 3-Device Spike Relay (PC-Laptop-Mobile)**
  - Setup: Start Hub on PC, Relay on Laptop, Sovereign on Mobile.
  - Test: Fire 'Spike Test Signal' from PC.
  - Success: Hub (L0-10) -> Relay (L11-20) -> Edge (L21-31) in < 2000ms.
- [ ] **7.3.2: Bandwidth Trading ($DOPA)**
  - Setup: Activate SOCKS5 proxy on Mobile.
  - Test: Route laptop traffic through Mobile proxy.
  - Success: $DOPA transaction observed on Hub ledger for 0.001 per MB.
- [ ] **7.3.3: Hole Punching Robustness**
  - Setup: Use `INGRVM/Core/mesh/hole_puncher.py`.
  - Test: Connect Laptop to PC across different NAT types (CGNAT test).

---

### Layer 3: Autonomous Defense & Whitehat Sentinel (Phase 8)
*Dependency: Layer 2 (Physical Mesh) verified.*

- [ ] **8.1: Sentinel Service Migration**
  - Path: `INGRVM/Core/tools/white_hat_sentinel.py`
  - Task: Convert script to a background daemon that periodically probes peers for Layer 1 vulnerabilities.
- [ ] **8.2: Dynamic Blacklisting (The Immune System)**
  - Path: `INGRVM/Core/mesh/p2p_gossip.py`
  - Task: Implement gossip callbacks that propagate "Threat Alerts" (Bad signature detection) across the mesh.

---

### Layer 4: Native Mobile & Identity Portability (Phase 9)
*Dependency: Layer 3 (Autonomous Defense) verified.*

- [ ] **9.1: Cross-Platform Bundle (Briefcase/PyInstaller)**
  - Path: `INGRVM/Core/tools/ingrvm_packager.py`
  - Task: Automate the creation of standalone executables for Windows, Linux (Relay), and Android (Sovereign).
- [ ] **9.2: Encrypted Identity Backups (.p12)**
  - Path: `INGRVM/Core/identity/identity_manager.py`
  - Task: Export `identity.key` into an encrypted PKCS#12 container for user-safe portability.
- [ ] **9.3: Mesh Networking (Bluetooth/WiFi-Direct)**
  - Path: `INGRVM/Mobile/mobile_edge.py`
  - Task: Implement fallback peer discovery via Bluetooth LE or WiFi-Direct when the internet is unavailable.

---

## Verification Protocol

Completion of any layer requires a **PASS** from `phase_gate.py`:

```bash
cd INGRVM/Core
python -m tools.phase_gate 7.2    # Security hardening gate
python -m tools.phase_gate 7.3    # Physical mesh gate
python -m tools.phase_gate 8      # MVP gate (cascades 7.2 + 7.3)
```

Gate reports are saved to `INGRVM/Docs/gate_reports/` as timestamped JSON with SHA256 hash.
Canonical phase status lives in [`INGRVM/Docs/PHASE_STATUS.md`](PHASE_STATUS.md).

---

## Next Steps
1.  **Execute Phase 7.2 (Security Hardening)** immediately.
2.  **Verify Layer 1** using the upgraded `test_security.py`.
3.  **Initiate Layer 2 (Physical Test)** once hardening is confirmed.
