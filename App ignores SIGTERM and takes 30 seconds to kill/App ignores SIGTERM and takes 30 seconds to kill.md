# Local Lab: Resolving Container Upgrades Hanging via Signal Forwarding (Q78)

This lab trains cloud engineers on POSIX system signals, container PID 1 execution mechanics, and microservice graceful termination architecture patterns.

## 🚀 Lifecycle Engineering Core Architecture

### 1. The Trap of Shell Form execution
When defining execution commands without JSON bracket configurations (e.g., `CMD python app.py`), the runtime engine wraps the string inside an explicit shell process tree (`/bin/sh -c`). The shell grabs position `PID 1` inside the namespace but fails to forward incoming orchestration signals like `SIGTERM` down to the application layer.

### 2. Exec Form Engine Mapping
Utilizing strict JSON array declarations (e.g., `CMD ["python", "app.py"]`) overrides intermediate shell wrappers. The application process acts as the core parent `PID 1` context block directly, facilitating zero-latency signal captures:
* **SIGTERM**: Instructs the code stack to trigger internal lifecycle cleanup events, drain network sockets, and dump storage logs before stopping.
* **SIGKILL**: The hard stop engine hammer triggered by the orchestration framework if a container refuses to yield after a specific timeout window.

---

## 💻 Validation Reference Checklist

### Audit Process IDs Inside an Active Container
```bash
docker run -d --name signal-audit signal-test:hanging
docker top signal-audit
docker rm -f signal-audit
```
