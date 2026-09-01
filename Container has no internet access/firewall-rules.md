# DevOps Lab: Diagnosing Host Firewall (iptables) Container Blockades

This practical training lab covers advanced Linux kernel container networking, software-defined subnets, and host-level packet filtering engineering. It simulates a scenario where containers communicate flawlessly within their local environment but are hard-blocked from accessing the internet by a misconfigured or overly restrictive host firewall rule.

---

## 🚀 Scenario Architecture: The Netfilter Guard

When you create a custom bridge network, Docker automatically provisions a virtual switch inside host memory. 
* **Internal Routing**: Packets traveling between containers on the same switch are routed locally and never try to leave the host.
* **External Routing**: To connect to the internet, packets must pass through the Linux kernel's **`FORWARD` chain** via Network Address Translation (NAT).

If an IT or security administrator injects a restrictive firewall rule directly into the host's `iptables` kernel architecture that drops forwarding frames from the container subnet range, outbound internet traffic is instantly cut off—while local container-to-container synchronization remains completely untouched.

---

## 💻 Step-by-Step Lab Execution

### 1. Provision the Target Subnet Matrix
Create an explicit software-defined bridge network switch environment to host your microservices:
```bash
docker network create firewall-playground-net
```

### 2. Launch the Multi-Container Infrastructure
Deploy an internal database service node and an application worker service node onto your new network:
```bash
# Container 1: The Backend Database
docker run -d --name backend-db --network firewall-playground-net alpine sleep 3600

# Container 2: The Application Worker
docker run -d --name app-worker --network firewall-playground-net alpine sleep 3600
```

### 3. Extract the Virtual Network Subnet Range
Query the metadata metrics of your new custom network to locate the exact IP pool layout assigned by the engine:
```bash
docker network inspect firewall-playground-net --format='{{range .IPAM.Config}}{{.Subnet}}{{end}}'
```
*Take note of the output string (it will typically look like **`172.x.0.0/16`**, e.g., `172.19.0.0/16`).*

### 4. Inject the Aggressive Firewall Blockade
Simulate a severe security misconfiguration on the host. Use the insert flag (`-I`) to place a rule at the absolute top of the host kernel's packet forwarding chain, forcing it to drop all outbound packets exiting your container subnet pool:
```bash
# REPLACE 172.19.0.0/16 with the exact subnet string you extracted in the step above
sudo iptables -I FORWARD -s 172.19.0.0/16 ! -d 172.19.0.0/16 -j DROP
```

---

## 🔍 The Systematic Forensic Troubleshooting Process

With the blockade active, execute your DevOps diagnostic troubleshooting routine from the host command line:

* **Step 1: Check Cross-Service Local Delivery**
  Test if the app worker can still ping the database over the virtual switch:
  ```bash
  docker exec -it app-worker ping -c 2 backend-db
  ```
  *Observation:* **Success.** The packets travel locally. The firewall's `FORWARD` block is ignored because the frames never attempt to leave the local bridge interface.

* **Step 2: Test Outbound Internet Access**
  Attempt to pass a direct network query out to a public internet address:
  ```bash
  docker exec -it app-worker ping -c 2 8.8.8.8
  ```
  *Observation:* **Total Failure.** You will experience 100% packet loss. The security rule at the host kernel layer drops your outbound frames immediately.

* **Step 3: Profile the Host Kernel Firewall Rules**
  Log onto the host machine and audit the active kernel packet tracking counters to locate the exact line dropping traffic:
  ```bash
  sudo iptables -L FORWARD -n -v
  ```
  *Forensic Trace:* You will observe packet dropping counters ticking up rapidly next to your custom `DROP` rule string, proving the firewall is the core bottleneck.

---

## 🛠️ The Production Fix: Purging the Blockade Rule

To restore internet connectivity to your container architecture, execute a target delete flag (`-D`) command against the precise rule blocking your network adapter layers:

```bash
# Delete the blocking forwarding rule using your exact network subnet string
sudo iptables -D FORWARD -s 172.19.0.0/16 ! -d 172.19.0.0/16 -j DROP
```

### Verify System Boundary Restoration
Re-test the internet connection check from inside your app worker container:
```bash
docker exec -it app-worker ping -c 2 8.8.8.8
```
*Success Verification Matrix:* Outbound packets immediately stream out to the public internet gateway flawlessly!

---

## 🧼 Infrastructure Clean Up
Always purge your temporary practice layers once operational milestones are completed:
```bash
docker rm -f app-worker backend-db 2>/dev/null
docker network rm firewall-playground-net 2>/dev/null
```


<img width="1365" height="422" alt="image" src="https://github.com/user-attachments/assets/45f485a0-b471-4961-a714-1c3e2c13cfc0" />

