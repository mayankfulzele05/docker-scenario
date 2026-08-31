# Local Lab: Resolving Host Storage Exhaustion via Automated Log Rotation (Q76)

This lab trains engineers on infrastructure storage management, daemon-level event profiling, and automated log retention policy deployments.

## 🚀 Log Rotation Engineering Architecture

### 1. The Threat of Unbounded Log Streams
By default, the standard `json-file` logging driver captures all application runtime console logs without setting bounds. In high-traffic microservice environments, these text matrices eventually eat 100% of `/var/lib/docker`, freezing the host filesystem and blocking container scheduling mechanics.

### 2. System Daemon Configuration Hardening
Instead of relying on manual cron utilities or ad-hoc interventions to clean full partitions, we patch the global system configuration layer (`/etc/docker/daemon.json`):
* **max-size**: Caps the active filesystem log snapshot boundary line (e.g., `10m`).
* **max-file**: Instructs the execution engine to drop ancestral text chunks automatically when rolling over to new storage files, keeping the disk size predictable.

---

## 💻 Emergency Space Recovery Reference

### Profile Active Storage Footprint
```bash
docker system df -v
```

### Emergency Truncate Script (Zero Downtime)
```bash
sudo sh -c "truncate -s 0 /var/lib/docker/containers/*/*-json.log"
```

### Nuclear Cleanup Command
```bash
docker system prune -a --volumes --force
```
