# DevOps Lab: Live Container Telemetry & Diagnostics Without SSH (Q15)

This training lab covers container namespace exploitation, secure execution process injection, and native infrastructure log stream harvesting techniques.

## 🚀 The Operational Forensics Pipeline

In cloud-native microservice topologies, embedding SSH servers (`sshd`) inside container images introduces massive configuration drift, bloats the security vulnerability surface area, and breaks architectural design patterns. DevOps engineers interact with live isolated filesystems using zero-downtime engine interfaces:

### 1. Telemetry Capture (`docker logs`)
Intercepts the primary runtime output buffers (`stdout`/`stderr`) directly out of the container engine runtime memory structures, eliminating the need to locate application log files manually on disk.

### 2. Namespace Process Injection (`docker exec`)
Instructs the parent Linux host kernel to spawn a brand new process thread (like a shell utility) inside the container's pre-existing cgroup and namespace boundaries, allowing real-time inspections without establishing network listening daemons.

---

## 💻 Forensic Telemetry Reference Cheat-Sheet

### Stream Live Container Logs in Real-Time
```bash
docker logs -f <container_name>
```

### Map Container Subsystem Performance Statistics Live
```bash
docker stats <container_name> --no-stream
```

### Complete Lab Takedown
```bash
docker rm -f live-prod-service
rm -rf ~/docker-debug-live
```
