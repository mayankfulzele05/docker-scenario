# Local Lab: Troubleshooting a Docker Container Restart Loop (Q63)

This hands-on lab exercises real-world system engineering forensics on a Linux machine by simulating a microservice container trapped in an infinite boot loop due to a missing deployment context profile.

---

## 🛠️ Step 1: Spin Up the Broken Loop Container

Run this single command on your local Linux terminal. This utilizes a base lightweight container and wraps it in a shell conditional check that enforces key registration checks.

```bash
docker run -d \
  --name auth-api \
  --restart always \
  -p 8080:8080 \
  node:20-slim sh -c 'echo "Starting up..."; sleep 1; if [ -z "$SECRET_KEY_BASE" ]; then echo "[FATAL ERROR]: Critical environment context variable SECRET_KEY_BASE is undefined."; exit 1; fi'
```

---

## 🔍 Step 2: Diagnostic Walkthrough

### 1. Track Container Process Lifecycles
Query the host machine network namespace process mapping table:
```bash
docker ps
```
* **Observation Framework**: Inspect the `STATUS` column. The time metric will continually flap or alternate rapidly between short bursts like `Up 1 second` and `Restarting (1) X seconds ago`.

### 2. Harvest Writable Layer Console Buffers
Dump standard error outputs out of the decaying application layer memory buffer to locate fatal trace paths:
```bash
docker logs auth-api --tail 10
```
* **Footprint Recovers**:
  ```text
  Starting up...
  [FATAL ERROR]: Critical environment context variable SECRET_KEY_BASE is undefined.
  ```

### 3. Query the Engine Exit Signals
Extract specific state machine metadata vectors from the container daemon engine to isolate infrastructure faults from code failures:
```bash
docker inspect auth-api --format='{{json .State.ExitCode}}'
```
* **Forensic Metrics Analysis**:
  * **Code `1`**: Explicit runtime or configuration framework crash (This lab instance).
  * **Code `137`**: Process killed abruptly by the Linux host kernel OOM (Out-Of-Memory) manager.

---

## 🛠️ Step 3: Apply the Production Hotfix

Because the code logic dictates an image cannot function without its target parameter string, wipe out the unstable deployment slice and rebuild a version including the parameter flag inject block.

### 1. Purge the Flapping Loop Container
```bash
docker rm -f auth-api
```

### 2. Relaunch with the Required Environment Key
Re-instantiate the container with its execution payload securely linked to an inline context variable flag:
```bash
docker run -d \
  --name auth-api \
  --restart always \
  -p 8080:8080 \
  -e SECRET_KEY_BASE="StagingVaultKey2026" \
  node:20-slim sh -c 'echo "Starting up..."; sleep 1; if [ -z "\$SECRET_KEY_BASE" ]; then echo "[FATAL ERROR]: Critical environment context variable SECRET_KEY_BASE is undefined."; exit 1; else echo "Initialization complete. App listening securely on port 8080..."; tail -f /dev/null; fi'
```

### 3. Certify System Boundary Health
```bash
docker ps
```
* **Resolution Proof**: The target indicator matrix under `STATUS` should now display a durable, stable running window tracking cleanly past `Up 30 seconds`, `Up 5 minutes`, etc.

---

## 🧼 Step 4: Infrastructure Clean Up
Remove the temporary training container and clean up local network bindings once practice operations are concluded:
```bash
docker rm -f auth-api
```
