# Local Lab: Troubleshooting Multi-Container Internet Outbound Routing Gaps

This lab trains engineers on multi-container service communication, Linux kernel routing layers (`sysctl`), and troubleshooting software-defined infrastructure network blocks.

## 🚀 The Architecture of Packet Forwarding

### 1. Internal Bridge vs. External Gateway
When multiple containers are attached to a user-defined custom bridge network, they communicate directly via virtual switches inside the host memory space. This internal path remains open even if the host machine has zero internet connectivity.

### 2. Linux Kernel IP Forwarding
To allow packets to leave the container bridge network and access public internet routing blocks, the Linux host operating system must act as a router. If the kernel parameter `net.ipv4.ip_forward` is set to `0`, the host will drop all outbound container traffic immediately while maintaining internal inter-container communication channels.

---

## 💻 Forensic Commands Reference Cheat-Sheet

### Check Linux Host Routing State
```bash
sysctl net.ipv4.ip_forward
```

### Complete Lab Takedown and Reset
```bash
docker rm -f app-worker backend-db 2>/dev/null
docker network rm prod-isolated-net 2>/dev/null
rm -rf ~/docker-routing-lab
```




<img width="1933" height="1049" alt="Screenshot (95)" src="https://github.com/user-attachments/assets/0ccd945c-6fc9-4373-bffd-cda8ce4c2006" />
