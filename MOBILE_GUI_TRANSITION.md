# INGRVM: Mobile GUI Transition (Beyond the Terminal)

*Objective: To replace the complex TUI (Terminal User Interface) with a beautiful, high-trust mobile app (APK/iOS) for the Genesis Grafting event.*

---

## 🎨 1. The Design Philosophy: "Biophilic Cyberpunk"
To avoid overwhelming users, the app must look like a **Biological Dashboard**, not a hacking tool.
- **Visuals:** Dark mode, pulsating soft-green neural waves, and clean typography.
- **Key Metric:** Total time from "Open App" to "Join Mesh" must be < 10 seconds.

---

## 🏗️ 2. Technical Roadmap (The GUI Bridge)

### Phase A: The INGRVM-UI Bridge
We will leverage the existing **`INGRVM/Mobile/ingrvm-ui`** (React/Vite) project.
- **Current State:** A web-based UI that can be served locally.
- **Immediate Task:** Wrap the `ingrvm-ui` in a **Capacitor** or **WebView** container to generate an `.apk` (Android) and `.ipa` (iOS) package.

### Phase B: The One-Click Bootstrap
The app will automate all the terminal commands we've been using:
1.  **Wallet Creation:** Automatically generates a $DOPA address on first boot.
2.  **DHT Discovery:** Finds the Hub without manual IP entries.
3.  **Metabolic Setup:** A single slider for energy consumption.

---

## 🕸️ 3. The 'Grafting' UI
Instead of typing `python ingrvm_cli.py graft --target [ID]`, the user will:
1.  **Open the Camera:** Built directly into the app.
2.  **Scan the QR:** Scans your "Anamorphic Hoodie" or a physical sticker.
3.  **Confirm Graft:** A simple "Touch to Sync" button that establishes the Mycelium trust link instantly.

---

## 📱 4. Distribution Strategy (Bypassing App Stores)
Since Big Tech might try to ban the "Antidote":
- **Direct APK:** We host the Android `.apk` directly on the **Signal Terminal** (IPFS).
- **TestFlight/Sideload:** For iOS, we use private TestFlight links or instructions for AltStore.
- **Trust Factor:** Because the code is open-source, we provide a **Checksum** so advanced users can verify the app hasn't been tampered with.

---

## ⚖️ Summary: Lowering the Barrier
By the time we hit the streets of Austin, the terminal will be for the "Founding Shards" (Developers) only. The general swarm will use the **INGRVM Mobile App**, making the complex math of neuromorphic spikes as easy to use as a text message.

**Next Action:** Begin wrapping the React `ingrvm-ui` into a mobile-friendly container and implementing the "Scan-to-Graft" logic.
