# Local Lab: Diagnosing "Running but Unreachable" Container Network Faults

This training lab covers container network namespace isolation mechanisms, interface socket binding configurations (`0.0.0.0` vs `127.0.0.1`), and host-to-container port mapping alignment.

## 🚀 The Two Primary Root Causes of Unreachable Containers

### 1. The Localhost (127.0.0.1) Loopback Trap
When an application inside a container binds to `127.0.0.1`, it binds exclusively to the container's private loopback interface. Because the container is isolated inside its own network namespace, it remains completely deaf to traffic arriving from the host via the Docker virtual network bridge (`eth0`). 
* **The Fix**: Applications inside containers must always bind to **`0.0.0.0`**, instructing the kernel to listen across all available network interfaces.

### 2. Port Realignment Errors
Docker allows you to map any host port to any container port using `-p <Host_Port>:<Container_Port>`. If the container port parameter does not match the exact port number your application code is actively listening on, incoming traffic hits a dead end.

---

## 💻 Forensic Diagnostic Checklist

### 1. Inspect Application Listening Points
```bash
docker logs stable-app
```

### 2. Clean Up Local Training Resources
```bash
docker rm -f stable-app
rm -rf ~/docker-unreachable-lab
```

command to change the 127.0.0.0 to 0.0.0.0
sed -i "s/const HOST = '127.0.0.1';/const HOST = '0.0.0.0';/" app.js

Correct container port
Correct host port
Firewall/security group if running on EC2.