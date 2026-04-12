# INGRVM Synapse Creation Guide
*The Technical Blueprint for Building the Autonomous Mind*

---

## 1. What is a Synapse?
In traditional AI, a "model" is a monolithic black box. In INGRVM, the network runs a massive, distributed base model (like `SpikeLLM_Master_V1.pt`). A **Synapse** is a small, specialized adapter (a LoRA, a continuous learning vector index, or a deterministic script) that plugs into the base model to filter, refine, or direct its output based on your specific needs.

## 2. Types of Synapses

### A. The "Vibe" Filter (Semantic Router Synapse)
*Best for: Content curation, ideological alignment, personalized feeds.*
These do not require model training. They use a localized Vector Database (`ChromaDB` or `FAISS`) to filter the output of the base model before it reaches you.
- **How to Build:** 
  1. Create a `sys_prompt.txt` defining your persona (e.g., "Solarpunk Engineer").
  2. Embed 10-50 examples of "good" outputs into a `.json` or `.db` file using `sentence-transformers`.
  3. Upload the resulting file via the Hub Marketplace.

### B. The "Action" Script (Deterministic Synapse)
*Best for: Smart home control, financial trading, triggering local APIs.*
These are standard Python scripts wrapped in our `pydantic` interface. The base model decides *when* to fire the synapse, but the synapse itself executes hardcoded logic.
- **How to Build:**
  1. Write a Python script with a single entry point: `def trigger_synapse(context: dict) -> dict:`
  2. Define the input parameters (e.g., `target_temp: int`).
  3. The Hub will execute this script when the LLM outputs a specific JSON command.

### C. The Neuromorphic LoRA (Weight-based Synapse)
*Best for: Specialized domain knowledge (Medical, Legal, Deep Coding).*
This is a literal fine-tuning of the SNN weights. 
- **How to Build:**
  1. Train a standard PyTorch LoRA on your specialized dataset.
  2. Run our converter: `python INGRVM/Core/tools/convert_lora_to_snn.py --input my_lora.pt --output my_synapse.pt`.
  3. Upload the `my_synapse.pt` file to the Hub.

## 3. Creating a New Kind of Synapse
If you want to create a completely new interaction paradigm (e.g., a "Hardware Synapse" that pulses an external LED array via Arduino):
1. Navigate to `INGRVM/Core/hub_api.py`.
2. Add a new `category` to the `UploadFile` endpoint parser.
3. Write a plugin script in `INGRVM/Core/plugins/` that listens to the `cortex_bus.py` TCP socket for your specific category tag.
4. When the base model fires a spike with that tag, your plugin intercepts it and controls the hardware.