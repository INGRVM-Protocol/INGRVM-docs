# INGRVM: ARG Technical Toolkit (The 'How-To')

*This is the operator's manual for executing Project Leviathan. It covers the software, hardware, and techniques needed to stay invisible and build the mystery.*

---

## 💻 1. The Stealth OS (Operational Security)
To operate as the lead architect, you must maintain a strict boundary between your personal and professional systems.
- **Software:** **Tails OS** (The Amnesic Incognito Live System).
- **How to use:** Download the Tails ISO, flash it to a USB drive (8GB+), and boot your computer from the USB. 
- **The Payoff:** Tails routes all internet traffic through **Tor** and wipes everything from RAM when you shut down. It ensures your core development environment remains decoupled from your daily hardware.

## 📁 2. File Sanitation (Metadata Scrubbing)
Every photo, video, or document has hidden "metadata" (GPS location, device name, time).
- **Software:** **MAT2** (Metadata Anonymization Toolkit).
- **How to use:** In Tails (or Linux), right-click any file and select "Remove Metadata." 
- **Critical:** Do this before uploading *any* file to GitHub or social media.

## 🔊 3. Audio Steganography (Hiding codes in Sound)
To hide QR codes or passwords inside audio static.
- **Software:** **Audacity** (for recording) + **Spek** (to view spectrograms).
- **How to use:** 
    1.  Create an image of a QR code.
    2.  Use a "Photo to Audio" converter (like **Coagula** or online spectrogram tools).
    3.  The output will sound like screeching static.
    4.  When users open this static in Spek or Audacity's "Spectrogram View," your image will appear in the frequencies.

## 🌐 4. Decentralized Hosting (The Trailhead)
Where to host the "Synaptic Gate" website without a credit card.
- **Service:** **Fleek** (IPFS hosting) or **Njalla** (Domain/Hosting).
- **The Payoff:** Host your site on IPFS (InterPlanetary File System). There is no server to "shut down." It is as decentralized as the mesh itself.

## 🕵️ 5. Global Ghost Team (Communication)
Building your secret team of 100 Shadow Validators.
- **Software:** **Session** (Messenger).
- **Why:** Requires NO phone number or email. It uses onion routing (like Tor) to hide your IP.
- **Protocol:** Generate your Session ID, share it only via the **Synaptic Gate**, and have your team members use pseudonyms based on their city (e.g., `GHOST_AUSTIN`, `GHOST_TOKYO`).

## 📽️ 6. Video Grouping (The Viral Hook)
How to make those 5 group videos feel "discovered."
- **App:** Any basic editor (CapCut/InShot).
- **Technique:** Record your phone screen running a terminal. Apply a "VHS" or "Glitch" filter. Keep it low-quality. High-quality looks like marketing; low-quality looks like a leak.
- **Strategy:** Post from 5 different accounts using a VPN. If one gets banned, the others survive.

**Summary:** Your mission is to maintain high OpSec. Use Tails for code, MAT2 for files, and Session for the team.
