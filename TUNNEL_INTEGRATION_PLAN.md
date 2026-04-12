# INGRVM: Cloudflare Tunnel Orchestrator
**Objective:** Pierce the firewall securely and provide a permanent WAN endpoint for the Hub.

## 1. Why Cloudflare Tunnels?
- **Privacy:** Your home IP address is never revealed to the public internet.
- **Security:** No open ports on your router. Only Cloudflare's edge can talk to your PC.
- **Persistence:** Even if your home IP changes, the URL (e.g. `hub.ingrvm.io`) stays the same.

## 2. Integration across INGRVM
I have updated the core logic to support a **`WAN_ALIAS`** in the configuration. 

### How it works:
1.  **Hub Boot:** The Hub starts locally on Port 8000.
2.  **Tunnel:** The `cloudflared` daemon creates a secure bridge to Cloudflare.
3.  **DHT Announcement:** Instead of announcing `192.168.x.x`, the Hub announces its Cloudflare alias to the Global DHT.
4.  **Mobile Find:** When your phone finds the Hub on the DHT, it sees the alias and routes its spikes through the secure tunnel.

## 3. Setup Script (Powershell)
Save this as `INGRVM/Infrastructure/setup_tunnel.ps1`:

```powershell
# 1. Download Cloudflared
Invoke-WebRequest -Uri "https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-windows-amd64.msi" -OutFile "cloudflared.msi"
Start-Process msiexec.exe -ArgumentList "/i cloudflared.msi /quiet" -Wait

# 2. Login (This will open a browser)
cloudflared tunnel login

# 3. Create Tunnel
cloudflared tunnel create ingrvm-hub

# 4. Route to local Hub
# Replace <TUNNEL_ID> with the ID from step 3
# cloudflared tunnel route dns ingrvm-hub hub.yourdomain.com
```

## 4. INGRVM Configuration Update
I will now update `INGRVM/Core/ingrvm_config.json` to prioritize the Tunnel Alias for WAN coordination.
