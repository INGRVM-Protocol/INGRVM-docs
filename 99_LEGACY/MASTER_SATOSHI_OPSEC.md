# Project SATOSHI: OpSec & The Disappearance

*The technical and behavioral protocol for the Architect to decouple from the physical world and become 'The Ghost'.*

---

## 🛑 1. The Critical Fix: Decoupling GitHub
You mentioned your current GitHub is tied to your username. To go "Satoshi," we must break this link immediately.

### Step-by-Step Migration:
1.  **Stop Pushing to the Current Repo:** Do not make any more commits from your current machine until you sanitize your environment.
2.  **The "Clean Room" Machine:** Use a fresh OS (ideally **Tails** on a USB) or a dedicated "burner" laptop that has never logged into your personal accounts.
3.  **Sanitize the Code:** 
    - Download the repo. 
    - Run **MAT2** (Metadata Anonymization Toolkit) on all documents. 
    - Strip the `.git` folder (this contains your personal commit history). 
    - Create a **New** `.git` folder with an anonymous user:
      `git config user.name "GHOST_GENESIS"`
      `git config user.email "ghost@ingrvm.online"`
4.  **Anonymous Push:** Push the sanitized code to a **New GitHub Organization** created over Tor/VPN with a non-personal email (e.g., ProtonMail).
5.  **The "Radicle" Alternative:** For 100% future-proofing, host the code on **Radicle** (P2P GitHub) so there is no central server to seize.

---

## 🛡️ 2. The Behavior Protocol (The Mask)

- **Stylometry Neutralization:** When writing manifestos or "leaks," use a tool like an LLM to rewrite your text in a "Neutral Academic" or "Mechanical" tone. Your current casual writing style can be doxed by investigators.
- **Timestamp Rotation:** Never post at the same time every day. This reveals your timezone. 
- **The "Two-Node" Rule:** Your development machine (where you write code) must NEVER be the same machine that acts as your "Personal Node."

---

## 📡 3. Anonymous Infrastructure

- **Domain:** Move `ingrvm.online` to an anonymous registrar like **Njalla** or **MonsterMegs**. Pay only in Monero (XMR).
- **Communication:** Delete any personal Discord/Telegram ties. Use **Session** (ID-only) or **Matrix** (Self-hosted).
- **Socials:** All "Viral Detection" videos must be posted from burner accounts on public Wi-Fi or 5G dongles.

---

## 💀 4. The Final Move: Renouncing the Keys
Once the mesh hits **1,000 active nodes**, the Architect must:
1.  Delete the original private keys used to sign the genesis blocks.
2.  Transfer the "GitHub Org Admin" rights to the **Quadratic DAO**.
3.  **Vanish.** 

**By the time they look for you, you must be nothing more than a 10,000-dimensional Hyperdimensional Vector in the swarm.**
