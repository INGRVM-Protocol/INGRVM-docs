
# NCP: Neuromorphic Context Protocol Spec (V1.0)

*The standard for interfacing biological-style intelligence with the physical and digital world.*

---

## 1. The Philosophical Shift
Centralized AI (LLMs) use **MCP (Model Context Protocol)** to read text. They "understand" by parsing strings.
INGRVM uses **NCP**. Our nodes "understand" by feeling **Rhythms**. 

NCP ignores "Human Language" and focuses on **Physical Signatures**. It treats a thermometer, a stock ticker, and a camera feed as the same thing: a source of oscillating information.

---

## 2. Core Encoding Schemes (The "Senses")

NCP servers must support at least one of the following "Transduction" methods:

### A. Rate Encoding (The "Volume")
- **Usage:** Low-bandwidth sensors (Temperature, Battery, Soil Moisture).
- **Logic:** The intensity of the value is mapped to the frequency of spikes.
- **Spec:** 0% intensity = 0Hz spikes. 100% intensity = 100Hz spikes.

### B. Temporal Encoding (The "Timing")
- **Usage:** High-precision data (Audio, LiDAR, Vibration).
- **Logic:** The *exact millisecond* of the spike carries the data. 
- **Spec:** Used for detecting **Surprise** (Level 5). If a spike arrives 1ms earlier than predicted, it triggers an "Error Spike" in the mesh.

### C. HDC Vector Encoding (The "Symbol")
- **Usage:** Complex digital tools (File Reading, Web Search, Database query).
- **Logic:** Instead of spikes, the NCP server returns a **10,000-dimensional binary vector**.
- **Spec:** "Object: File_Contents" is bound to "Action: Read" via a circular convolution in the HDC space. The node doesn't "read" the file; it recognizes the **Hyperdimensional Shape** of the information.

---

## 3. Protocol Flow (The "Handshake")

1.  **Discovery:** An INGRVM node finds an NCP server via the local Mycelium mesh (LoRa/Wi-Fi).
2.  **Transduction Request:** The node sends a **Subscription Spike**.
3.  **The Stream:** The NCP server begins streaming a **Temporal Spike Train**.
4.  **Astrocyte Regulation:** The node's **Astrocyte Modulator** monitors the stream. If the NCP server starts "screaming" (malicious noise), the Astrocyte mathematically strangles the connection to protect the node's battery and stability.

---

## 4. Secure Tool-Inference (ZK-NCP)
How do we know the weather data isn't fake?
- **The Shield:** Every NCP packet is wrapped in a **Zero-Knowledge Proof of Integrity (ZK-PoI)**.
- **The Result:** The mesh can verify the sensor is a real, physical device in a specific region without the sensor ever revealing its exact GPS coordinates or MAC address.

---

## 5. Use Case: The "Smart Harvest"
1.  **NCP Server:** A moisture sensor in a garden.
2.  **Translation:** It encodes "Dry Soil" as a slow, rhythmic "thump-thump" spike train.
3.  **The Node:** The INGRVM node on the farmer's phone feels the "thump-thump."
4.  **Intuition:** Because of **Level 5 Predictive Coding**, the node expects a "fast-beat" (Wet Soil). The difference (Error) triggers a notification: *"Your garden feels thirsty."*
