# DevOps Lab: Troubleshooting Successful Builds that Crash on Run

This practical lab covers Docker build boundary layers, container exit code analysis, and executable path verification techniques.

## 🚀 The Build vs. Runtime Verification Boundary

A common pitfall for emerging engineers is assuming that a successful `docker build` execution guarantees a stable runtime lifecycle. 

* **The Build Phase**: The build engine validates Dockerfile instruction keywords and compiles files. It treats strings inside `CMD` or `ENTRYPOINT` parameters as unverified command arrays.
* **The Runtime Phase**: The moment `docker run` is called, the kernel boots `PID 1` inside the namespace. If the target binary file path is missing, misspelled, or lacks executable permissions, the primary process terminates instantly, collapsing the container state down to an exit code status.

---

## 💻 Forensic Debugging Checklist

### 1. Identify Container Lifecycle Exit Codes
```bash
docker ps -a --filter "name=brittle-app"
```

### 2. Harvest Crash Logs
```bash
docker logs brittle-app
```

### 3. Complete Infrastructure Clean Up
```bash
docker rm -f brittle-app stable-app 2>/dev/null
rm -rf ~/docker-crash-build
```

CMD ["node", "src/server.js"] just the line to this it all about wrong path 


