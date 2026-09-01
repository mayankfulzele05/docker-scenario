# DevOps Lab: Troubleshooting External Cloud Database (RDS) Connection Gaps

This training lab covers cloud security boundaries, container network translation rules, and multi-subnet infrastructure troubleshooting.

## 🚀 The Multi-Layer Cloud Troubleshooting Framework

When a cloud infrastructure container can communicate with local host microservices but fails to connect to an external data layer (like AWS RDS, external MySQL, or DynamoDB) that the underlying host machine *can* reach, check this strict 4-step checklist:

### 1. AWS Security Groups (The Cloud Firewall)
Cloud firewalls filter traffic based on source addresses. The host EC2 instance is explicitly whitelisted. However, if the container exits through an unmapped network address translation (NAT) block or another internal subnet interface range that is omitted from the database's inbound security rules, the cloud firewall drops the connection instantly.
* **DevOps Fix**: Update the RDS Security Group inbound rule to allow traffic from the entire EC2 private subnet CIDR range block (e.g., `10.0.1.0/24`), rather than just pinning a single host IP address.

### 2. Network ACLs (NACLs) & Subnet Route Tables
Ensure the private subnet where Docker is deployed possesses an explicit routing path heading to the database's target VPC cluster. Missing route rules or boundary ACL locks will prevent container packets from making the cross-subnet jump.

### 3. IP Masquerading Rules (iptables NAT)
Docker relies on the host kernel to masquerade outbound packets so they appear to originate from the host EC2 IP address. 
* Check the kernel masquerading rules using: `sudo iptables -t nat -L POSTROUTING -n -v`
* If masquerading (`MASQUERADE`) is broken, the container's raw internal non-routable IP (e.g., `172.18.0.2`) will leak out to the cloud network, causing the external router to drop it.

---

## 💻 Forensic Diagnostic Commands

### Validate External Port Connectivity Safely
```bash
docker exec -it <container_name> nc -zv <database_hostname> 3306
```

### Complete Emergency Lab Takedown
```bash
docker rm -f app-container rds-mysql-instance 2>/dev/null
docker network rm cloud-db-vpc local-app-bridge 2>/dev/null
rm -rf ~/docker-rds-lab
```
