# DevOps Lab: Troubleshooting Missing Environment Variables (Q13)

This practical lab maps out variable scope boundaries, build-time compilation flags, and runtime environment variable inspection techniques.

## 🚀 The Operational Metric Matrix: ARG vs ENV

| Parameter Type | Keyword Syntax | Active Boundary | Best Used For | Visible in Container Runtime? |
| :--- | :--- | :--- | :--- | :--- |
| **Build Argument** | `ARG` | Only during `docker build`. Completely drops away after compilation. | Pinned package versions, base tags, compilation flags. | ❌ No |
| **Environment Variable** | `ENV` | Baked into the image metadata. Active throughout the entire lifecycle. | DB connection endpoints, API keys, operational profiles. |  Yes |

---

## 💻 Forensic Verification Commands Reference

### 1. Extract Environment Variables From a Running Container
```bash
docker exec -it <container_name> printenv
```

### 2. Inspect Image Metadata Profiles
```bash
docker inspect <image_name_or_id> --format='{{json .Config.Env}}'
```

### 3. Complete Emergency Lab Clean Up
```bash
docker rm -f failing-env stable-env 2>/dev/null
rm -rf ~/docker-env-lab
```
