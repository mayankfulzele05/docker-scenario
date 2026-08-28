# Local Lab: Resolving Database Data Loss via Named Volumes (Q68)

This training lab covers the critical architectural shift from transient, ephemeral container layouts to automated persistent storage configuration engineering.

## 🚀 Persistent Storage Architecture Rules

### 1. The Ephemeral Container Layer
By design, all files created or mutated inside an active container filesystem are locked directly into a lightweight, transient writable layer. If the container engine drops or undergoes redeployment upgrades, that layer is discarded entirely.

### 2. Managed Named Volumes
To run stateful engines (like PostgreSQL, MySQL, or Redis) safely, the storage target path must be decoupled from the container lifecycle using high-utility Named Volumes:
* **Syntax Model**: `-v volume_identifier:/target/container/storage/path`
* **Host Tracking Blueprint**: Docker shifts the active bitstream storage completely outside the container and isolates it inside `/var/lib/docker/volumes/` managed directly by the host operating system kernel layers.

---

## 💻 Forensic Commands Cheat-Sheet

### Audit Host Storage Matrix
```bash
docker volume ls
```

### Inspect Target Volume Metadata Location
```bash
docker volume inspect pg_prod_data
```
