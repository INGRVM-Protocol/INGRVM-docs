# INGRVM Next Session Setup (PC Master)
**Protocol Version:** 2.1 (Git-Native)

## 1. Initial Handshake (MANDATORY)
The very first thing you must do is run the new Rule 0. It solves the branch-isolation problem.
```powershell
$env:PYTHONPATH = "C:\Users\Tesss' PC\Desktop\The_Ecosystem\INGRVM\Core"
python INGRVM/Infrastructure/sync_mesh.py
```
**Expected Result:** You will see a notification if the Laptop or Mobile nodes have sent you mail. The script will automatically pull their `MAIL_TO_PC_MASTER.md` file into your local workspace from their respective branches.

## 2. Reading the Room
1.  **Check Mail:** Open `INGRVM/Framework/Mailroom/MAIL_TO_PC_MASTER.md`.
2.  **Check Master Journal:** Read the `MASTER_JOURNAL.md` at the root. Note the "STRATEGIC PIVOT" section.
3.  **Check Services:** Ensure the Hub is running. If not, use the launcher:
    `Start-Process powershell -ArgumentList "-NoProfile -ExecutionPolicy Bypass -File 'C:\Users\Tesss' PC\Desktop\The_Ecosystem\launch_hub_win.ps1'"`

## 3. Core Focus (Phase 10 Hardening)
We are done with "Agent Tools." Do not build more watchdog scripts.
**Your priority is the INGRVM Neural Core:**
- **Circom Logic:** We need the `.circom` code for XNOR-Popcount.
- **WAN Integration:** `hub_server.py` needs to use the DHT for global discovery.
- **Verification:** Every task must be verified by `The Judge` with automated slashing.

## 4. Known Blind Spots
- **The HTTP Mailroom is Dead:** Do not try to fix `mailbox.json`. Use the Git-native path established in `sync_mesh.py`.
- **Merge Divergence:** We are running on `node/pc`. Only merge `master` into your branch; do not push your messy dev work to `master` until it passes The Judge.
- **Windows Unicode:** Avoid emojis in `print()` statements; they crash the background service window.
