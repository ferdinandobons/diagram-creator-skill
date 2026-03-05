# Example: Kubernetes Pod Networking

**Input:** "Visualize Kubernetes pod networking architecture"

**Topology:** nested

**Layers:**
- Level 1: Internet / External Traffic — red (solid)
- Level 2: Kubernetes Cluster — orange (solid)
- Level 3: Worker Node — blue (solid)
- Level 4: Pod Network (CNI - Calico) — cyan (dashed)

**Nodes (inside Level 4):**
- nginx-ingress | IP: 10.244.0.5 | Port: 80:30080 | Ingress controller
- api-server | IP: 10.244.0.12 | Port: 8080 | Backend API
- postgres | IP: 10.244.0.20 | Port: 5432 | StatefulSet + PVC
- redis-cache | IP: 10.244.0.25 | internal only | Cache
- prometheus | IP: 10.244.0.30 | Port: 9090:30090 | Monitoring

**Connections:**
- Internet → Cluster: HTTPS on port 443
- Cluster → Node: kube-proxy iptables/IPVS rules
- Node → Pod Network: CNI assigns Pod IPs from CIDR 10.244.0.0/16
- Within Pod Network: direct communication via virtual switch

**Legend:** red=External, orange=Cluster boundary, blue=Worker node, cyan=Pod network, green=Exposed port
