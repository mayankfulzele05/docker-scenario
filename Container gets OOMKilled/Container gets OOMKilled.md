# Local Lab: Debugging OOM-Killed Container Restart Loops (Q63)

This lab trains cloud infrastructure engineers on cgroup memory resource budgeting, Linux kernel OOM Killer signaling structures, and container troubleshooting matrices.

## 🚀 The Forensic Science of Exit Code 137

### 1. Kernel Out-Of-Memory (OOM) Protection Mechanisms
When allocating resource constraints inside a deployment blueprint (such as `--memory` caps), Docker maps these rules straight to Linux **cgroups (control groups)**. If a container breaches its memory pool threshold, the host kernel aggressively executes a `SIGKILL` to protect the host machine's total stability.

### 2. Forensic Signal Matrix Decoding
When auditing an unstable container process, the engine's termination trace vectors yield instant structural answers:
* **Exit Code `1`**: Indicates an application logic exception, runtime crash, or missing dependency variable file.
* **Exit Code `137`**: Proof of a brutal forced stop (`128` base signal offset + standard signal `9` `SIGKILL`). When paired with `OOMKilled: true`, it establishes that memory allocation starvation collapsed the infrastructure unit.

---

## 💻 Emergency Forensics Cheat-Sheet

### Query Daemon Runtime Termination Metadata
```bash
docker inspect <container_name> --format='{{json .State}}'
```

### Clean Up Local Practice Footprint
```bash
docker rm -f stable-app
rm -rf ~/docker-oom-lab
```

just increase memory in this line 
 docker run -d \
  --name stable-app \
  --restart always \
  -m 1g \
  resource-test:leak
