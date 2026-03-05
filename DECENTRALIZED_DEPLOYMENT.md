# Calyx: Decentralized & Homelab Deployment Guide 🌿🏗️

To move beyond "Same Wi-Fi" and achieve global sovereignty without Big Tech (AWS/Google), we use **Lighthouses**. These are nodes with public IPs that bridge our firewalled devices (Phone/Laptop).

---

## 🏛️ Option 1: The Akash Network (Recommended)
*The Airbnb for Cloud Compute. Decentralized, open-source, and censorship-resistant.*

1. **Install Akash CLI:** (On your PC or Laptop)
2. **Create a Deployment (SDL):** Use the generated `Calyx/Core/lighthouse-compose.yml` but convert it to Stack Definition Language (SDL).
3. **Bid for Compute:** Providers will bid to host your node for a few cents a day in $AKT.
4. **Deploy:** Your Calyx Lighthouse is now live on a decentralized server.

**Why Akash?** It is physically impossible for a single entity to shut down your node.

---

## 🏠 Option 2: The Homelab (Personal Sovereignty)
*Use your own hardware at a different physical location.*

1. **Hardware:** Raspberry Pi 5 or an old PC.
2. **Static IP / Port Forwarding:**
   - On the router where the hardware is located, forward **Port 60000** (TCP) to the device.
3. **Installation:**
   ```bash
   git clone https://github.com/OhYesssItsTesss/The_Ecosystem.git
   cd The_Ecosystem/Calyx/Core
   python bootstrap_beacon.py
   ```
4. **Broadcast:** Share your `/ip4/<Your_Public_IP>/tcp/60000/p2p/<ID>` multiaddr with your other devices.

---

## 🌩️ Option 3: Independent VPS (Independent but Centralized)
*Bypassing "The Big Three" while keeping it simple.*

Use providers like **Hetzner**, **Vultr**, or **DigitalOcean**.
1. Create a basic Ubuntu VPS ($4-6/month).
2. Install Docker.
3. Run our pre-built stack:
   ```bash
   docker-compose -f Calyx/Core/lighthouse-compose.yml up -d
   ```

---

## 📱 What to do on your Phone (Pixel 8)
Once a Lighthouse is live (Akash or Homelab), your phone can find the PC even if you are on 5G/LTE:

1. **Update `bootstrap_list.json`:**
   Add the Lighthouse's multiaddr to your phone's config.
2. **Run Connectivity Test:**
   ```bash
   cd Calyx/Core
   python p2p_debug_v2.py client
   ```
   *If successful, your phone will say "ACK received from PC Master" via the Cloud Relay!*

---
*Drafted by The Architect | Powered by the Calyx Mesh*
