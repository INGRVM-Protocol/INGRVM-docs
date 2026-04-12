# INGRVM: Post-Release Roadmap (V1.1.0 - V2.0.0)
**Theme:** The Ghost Internet - Stability, Scale, and Sovereignty.

## 🧪 Phase 14: The 7-Day Burn-In (Bittensor Testnet Stress)
**Goal:** 168 hours of continuous mesh uptime on Subnet 99. Zero manual interventions.

1.  **[Task #01]:** Set `INGRVM_LIVE=true` and execute the first real coldkey funding on Testnet.
2.  **[Task #02]:** Monitor `SubnetSettler` for 24 hours to ensure weight-sync never misses a window.
3.  **[Task #03]:** Implement "Graceful Reconnect" in `lib_node.py` for flaky cellular connections.
4.  **[Task #04]:** Verify the Hub's memory usage doesn't leak during 10,000+ spike processed.
5.  **[Task #05]:** Stress-test the Merkle-snapshot logic by rolling back weights 3 times in one day.
6.  **[Task #06]:** Test "Hub Collision": Ensure two Hubs don't claim the same node ID on the DHT.
7.  **[Task #07]:** Verify Gossip Slashing by intentionally sending one "Dirty Spike" from a test node.
8.  **[Task #08]:** Benchmark NPU thermal throttling over a 12-hour continuous inference run.
9.  **[Task #09]:** Verify Cloudflare Quick Tunnel auto-renews its ephemeral URL correctly.
10. **[Task #10]:** Implement "Headless Mode" for the Hub (No UI, just CLI/Logs).
11. **[Task #11]:** Finalize the `multisig_validator` logic for $TAO payout consensus.
12. **[Task #12]:** Audit DHT "Ticket" expiration (Ensure 24h tickets rotate correctly).
13. **[Task #13]:** Implement automatic log rotation to prevent SSD bloat.
14. **[Task #14]:** Verify the "Demand Heatmap" triggers a real shard download on a second laptop.
15. **[Task #15]:** Test Hub "Multi-Hop" by routing a spike through a chain of 3 Hubs.
16. **[Task #16]:** Monitor SQL Ledger for index fragmentation under heavy load.
17. **[Task #17]:** Perform a "Hard Reboot" stress: Kill all processes and run `bootstrap_ingrvm.ps1` to see if it recovers.
18. **[Task #18]:** Collect end-to-end latency stats for a 5G cellular route (Aim: <200ms).
19. **[Task #19]:** Verify "Stake-to-Validate" logic correctly bans nodes with <1.0 $DOPA.
20. **[Task #20]:** Graduation: Tag V1.1.0 - "Testnet Stable."

## 🚀 Phase 15: Public Beta & Community Growth
**Goal:** Onboard 100+ active nodes and open the Synapse Marketplace.

1.  **[Task #01]:** Launch the `ingrvm.io` landing page with the "Normie" installer.
2.  **[Task #02]:** Release the first "Lens": A community-curated 1-bit sentiment model.
3.  **[Task #03]:** Implement "Referral Dashboard" in the Sovereign UI.
4.  **[Task #04]:** Launch the Discord/Telegram "Mesh Pulse" bot for real-time mesh stats.
5.  **[Task #05]:** Port the `mini-snark` wrapper to pure Rust for 10x faster mobile proofs.
6.  **[Task #06]:** Implement "One-Click Update" in the `ingrvm.py` CLI.
7.  **[Task #07]:** Build the web-based Synapse Marketplace (Browse/Install models).
8.  **[Task #08]:** Integrate hardware-accelerated kernels for AMD/Intel NPUs.
9.  **[Task #09]:** Release the React Native Mobile App (V1.0 Alpha).
10. **[Task #10]:** Implement "Private Compute" mode (Local-only inference, no DHT).
11. **[Task #11]:** Build the INGRVM "Explorer" (Blockchain explorer for the mesh ledger).
12. **[Task #12]:** Launch the "Mycelium Graft" referral incentive program.
13. **[Task #13]:** Implement DAO voting directly in the System Tray.
14. **[Task #14]:** Verify 1000+ concurrent nodes on the DHT.
15. **[Task #15]:** Integrate external Oracle support (Weather/Price data) into SpikeLLM.
16. **[Task #16]:** Enable "Subnet Bridging": Route spikes to other Bittensor subnets.
17. **[Task #17]:** Build the INGRVM SDK for 3rd party developers.
18. **[Task #18]:** Perform external security audit of the Multi-sig bridge.
19. **[Task #19]:** Trigger the DAO Ownership Transfer (The "Exit to Community").
20. **[Task #20]:** V2.0.0 MAINNET LAUNCH.
