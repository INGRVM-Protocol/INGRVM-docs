# Calyx $SYN Grant Program
*Phase 8: The Global Neural Fabric Expansion*

## **Overview**
The Calyx Mesh is built by many brains. To accelerate the growth of our decentralized intelligence, we are launching the **$SYN Grant Program**. This program rewards developers who create, optimize, and share high-efficiency neuromorphic "synapses" (skills) on the Calyx Marketplace.

---

## **Grant Tiers**

### **Tier 1: Neural Seeds (500 - 2,000 $SYN)**
For developers who upload their first functional neuromorphic skill.
- **Criteria:** Skill must pass the `skill_validator.py` security audit and have documented architecture.
- **Examples:** Niche sentiment models, basic image classifiers (MNIST-scale), or logic gates.

### **Tier 2: Efficiency Hardening (2,000 - 10,000 $SYN)**
For developers who optimize existing open-source models into 1-bit BitNet or SNN formats.
- **Criteria:** Must show a >50% reduction in VRAM usage compared to the FP16 baseline while maintaining >90% accuracy.
- **Examples:** Llama-3-8B 1-bit quantization, specialized BERT sharding.

### **Tier 3: Core Infrastructure (10,000+ $SYN)**
For developers building fundamental mesh utilities.
- **Criteria:** Tools that improve routing, security (zkML), or multi-node synchronization.
- **Examples:** New P2P relay protocols, advanced dashboard visualizations, or cross-platform Termux bridges.

---

## **Submission Process**

1. **Build & Package:** Use `synapse_packager.py` to bundle your model and metadata.
2. **Upload:** Use the `sdk.py` or Marketplace UI to push your skill to the Hub.
3. **Verify:** Ensure your skill passes the `skill_validator.py` peer-review check.
4. **Propose:** Create a DAO proposal via `governance_dao.py` with the memo "GRANT_REQUEST: [SynapseID]".
5. **Vote:** If the mesh community (weighted by reputation) accepts the proposal, the $SYN is automatically minted to your node balance.

---

## **The Rule of Sovereignty**
All granted skills must be open-source and follow the **Distributed Authority Protocol (DAP)**. Once a grant is accepted, the skill becomes a public asset of the Calyx neural fabric.

*Build the brain. Earn the future.*
