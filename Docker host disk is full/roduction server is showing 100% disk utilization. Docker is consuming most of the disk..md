# DevOps Lab: Automating Emergency Storage Reclaims on Full Docker Hosts

This practical training lab covers container storage layer analysis, garbage collection mechanics, and automated workspace pruning techniques.

## 🚀 The Three Pillars of Docker Disk Bloat

When a continuous integration (CI) runner node or cloud server triggers a 100% disk usage alert, the root cause typically maps to one of three storage sectors managed by the engine:

### 1. Dangling Images (`<none>:<none>`)
Every time an automated pipeline re-runs `docker build` using an identical tag (like `:latest`), the engine shifts the tag pointer to the new image layer block. The old image layers are detached, becoming "dangling" ghosts that sit on the disk consuming space indefinitely.

### 2. Orphaned Volume Blocks
When running `docker rm -f <container_name>`, the container container layer is dropped, but any attached volume configurations are preserved to safeguard data. If the workflow omits the explicit `-v` boundary flag during destruction, the volume is orphaned.

### 3. Build Kit Caching Matrices
Modern BuildKit compilation engines store aggressive structural layers locally to ensure rapid rebuild speeds. Over months of code iteration, this cache footprint expands until it completely starves the host operating system partition boundaries.

---

## 💻 Forensic Maintenance Reference Cheat-Sheet

### Profile Complete Disk Subsystem Distributions
```bash
docker system df -v
```

### Safe Operational Garbage Sweep (Run Weekly via Cron)
```bash
docker system prune -f
```




<img width="1829" height="1031" alt="image" src="https://github.com/user-attachments/assets/c36a4ae6-686c-435f-bdad-a0365be9f442" />


### Hard Nuclear Sweep (Reclaims Maximum Disk Space)
```bash
docker system prune -a --volumes --force
```
