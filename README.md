# DAY7-NetData
# Netdata Monitoring on EC2 — Step-by-Step What I Did

## Summary
I set up real-time monitoring for three AWS EC2 instances using Netdata. I ran a Netdata container, connected all nodes to Netdata Cloud, generated CPU load with `stress`, spun up an Nginx container, and verified that both VM-level and container-level metrics appeared (including CPU spikes) on the Netdata dashboard. I also reviewed Netdata logs on each node for validation.

---

## Step-by-Step

### 1) Created the infrastructure
- Launched **3 EC2 instances** (Ubuntu) to act as monitored nodes.

### 2) Deployed Netdata (container) for an immediate local dashboard
- On one instance, I **ran the Netdata Docker image** so I could quickly view a local dashboard on port **19999**.
- I **signed in with my GitHub account** on the Netdata interface to authenticate and tie things to my identity.

### 3) Generated CPU load to validate charts
- I used the `stress` tool to **simulate CPU load for ~60 seconds** (e.g., `stress --cpu <workers> --timeout 60s`).
- Result: The **CPU charts spiked** exactly during the stress window, confirming that Netdata was collecting and rendering real-time VM metrics.

### 4) Ran an Nginx container and observed container-level metrics
- I started an **Nginx Docker container** and verified that **container metrics** appeared in Netdata (along with VM metrics).
- In the Netdata dashboard, I checked both **Containers** and **Nodes/VM** views to compare utilization.

### 5) Connected all nodes to Netdata Cloud
- On each EC2 instance, I ran the **Netdata kickstart claim** command to register the agent to **Netdata Cloud** using a claim **token**, **room ID**, and **claim URL**.
  - _Note: The exact token/room values are sensitive and were used as provided. Avoid committing them to public repos._
- I then **enabled and started** the Netdata system service so the agent stays active across reboots.

### 6) Verified nodes in the Netdata Cloud dashboard
- I opened **Netdata Cloud** and confirmed that **all 3 nodes appeared** and were reporting metrics.
- I could now view a **centralized dashboard** for all nodes, drill into **per-node dashboards**, and switch to **containers view** to see the Nginx container metrics.

### 7) Re-validated with stress on the nodes
- I installed/used `stress` on the nodes and **re-ran CPU load** to verify the monitoring pipeline end-to-end.
- Result: CPU utilization **spiked in real time** on the Cloud dashboard for the corresponding nodes.

### 8) Reviewed Netdata logs on each node
- I looked at logs under **/var/log/netdata** (e.g., `error.log`, `access.log`) to verify the agent state and any warnings.
- This helped confirm that the service was running cleanly and the nodes were successfully claimed.

---

## What I Observed
- **CPU spikes** aligned with each stress test window on both **VM** and **container** charts.
- **Nginx container** metrics were visible alongside system metrics.
- All **three nodes** appeared in **Netdata Cloud** and kept reporting after enabling the service.
- **Logs** provided additional confirmation and were useful for quick troubleshooting.

---
