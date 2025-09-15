Here’s a consolidated table combining **AKS features, add-ons, extensions, inbound/outbound relevance, and IPv6 support** based on verified Microsoft documentation:

---

###  **AKS Features, Add-ons, and IPv6 Support**

| Category | Feature / Add-on | Inbound | Outbound | IPv6 Support |
|----------|------------------|---------|----------|--------------|
| **Core Networking** | Dual-stack Networking (IPv4 + IPv6) | Yes | Yes | Yes |
|  | IPv6-only Cluster | N/A | N/A | **NO** |
|  | LoadBalancer Service (dual-stack) | Yes | N/A | Yes (v1.27+) |
|  | NAT Gateway | N/A | Yes | Yes |
|  | UDR (User Defined Routes) | N/A | Yes | Yes |
|  | Azure Firewall | N/A | Yes | Yes |
|  | Azure CNI Overlay - Linux​ | N/A | N/A | Yes |
|  | Azure CNI Overlay - Windows | N/A | N/A | Not Yet |
|  | Azure CNI Overlay - Cilium​ | N/A | N/A | Yes |
|  | Azure CNI – Pod Subnet | N/A | N/A | Preview |
| **Ingress / Egress** | Application Gateway Ingress Controller | Yes | N/A | Yes |
|  | NGINX Ingress Controller | Yes | N/A | Yes |
| **Add-ons** | KEDA (Event-driven Autoscaling) | N/A | N/A | Yes |
|  | Azure Monitor / Container Insights | N/A | N/A | Yes |
|  | Azure Policy | N/A | N/A | Yes |
|  | Azure Key Vault Secrets Provider | N/A | N/A | Yes |
|  | Virtual Nodes | N/A | N/A | Yes |
| **Cluster Extensions** | Dapr | N/A | N/A | Yes |
|  | Flux (GitOps) | N/A | N/A | Yes |
|  | Azure App Configuration | N/A | N/A | Yes |
|  | Azure Machine Learning | N/A | N/A | Yes |
|  | Azure Container Storage | N/A | N/A | Yes |
|  | Azure Backup for AKS | N/A | N/A | Yes |

---

###  **Key Notes**
- **IPv6-only clusters are not supported**.
- **Dual-stack requires Azure CNI Overlay and dual-stack VNet**.
- **LoadBalancer dual-stack support starts from AKS v1.27**.
- Most **add-ons and extensions are networking agnostic**, so they work with IPv6 as long as the cluster supports dual-stack.


<br/>

---

<br/>

Here’s a comprehensive breakdown of AKS features, add-ons, and networking options, focusing on IPv6 support. The details have been verified against official Microsoft documentation. 

### **AKS Core Networking Features**
https://learn.microsoft.com/en-us/azure/aks/configure-dual-stack 

| Feature | Description | IPv6 Support |
|---------|-------------|--------------|
| Dual-stack networking (IPv4 + IPv6) | Pods and nodes get both IPv4 and IPv6 addresses; services can be dual-stack or single-stack. <br/><br/> **Requires Azure CNI Overlay and dual-stack VNet**. | Yes (IPv4 + IPv6) |
| Single-Stack IPv6 | IPv6-only clusters. | No (Not supported) |
| LoadBalancer Services | Starting AKS v1.27, supports dual-stack LoadBalancer with both IPv4 and IPv6 public IPs. | Yes (v1.27+) |
| Outbound traffic | - Standard Load Balancer (default) <br/> - NAT Gateway (managed or user-assigned). NAT rules exist for both IPv4 and IPv6. <br/> - User-defined routing (UDR) for custom egress. Configure IPv6 default routes optionally. <br/> - Can also use Azure Firewall or network isolation for outbound control. | Yes <br/><br/> https://learn.microsoft.com/en-us/azure/aks/load-balancer-standard <br/><br/> https://learn.microsoft.com/en-us/azure/aks/egress-outboundtype <br/><br/> https://learn.microsoft.com/en-us/azure/aks/outbound-rules-control-egress  |
| Ingress Controllers | Can expose services over IPv4 and IPv6 using dual-stack services or separate services. | Yes (manual config for older versions) |
| Private Clusters & API Server VNet Integration | Works with dual-stack VNet. | Yes |

---
**Limitations**: 

* In Azure Linux node pools, service objects are only supported with _externalTrafficPolicy: Local_.
* AKS Automatic SKU does not support IPv6 yet. https://github.com/Azure/AKS/issues/4867 [3/26/2025]

---
### AKS Add-ons (Managed by AKS)

| Add-on | Purpose | IPv6 Support |
|--------|---------|--------------|
| Application Gateway Ingress Controller (ingress-appgw) | L7 ingress with Azure Application Gateway. | Yes (works with dual-stack LB) |
| KEDA | Event-driven autoscaling. | Yes (networking agnostic) |
| Monitoring (Container Insights, Managed Prometheus) | Observability and metrics. | Yes |
| Azure Policy | Governance and compliance. | Yes |
| Azure Key Vault Secrets Provider | Secure secret injection. | Yes |
| Virtual Nodes | Burst to Azure Container Instances. | Yes |
| Open Service Mesh | Retired. | N/A |


### AKS Cluster Extensions
| Extension | Purpose | IPv6 Support |
|-----------|---------|--------------|
| Dapr |  | Yes |
| Azure App Configuration | Centralized app settings. | Yes |
| Azure Machine Learning | ML model training/inference on AKS. | Yes |
| Flux (GitOps) | GitOps-based deployment. | Yes |
| Azure Container Storage | Persistent storage for AKS. | Yes |
| Azure Backup for AKS |  | Yes |


### Inbound & Outbound Traffic
* Inbound:
  * Dual-stack ___LoadBalancer___ services (IPv4 + IPv6) supported from AKS v1.27. 
  * For older versions, create two separate services (IPv4 and IPv6). 
* Outbound:
  * NAT rules for IPv6 supported in dual-stack clusters. 
  * Ensure IPv6 routes exist if using UDR for outbound traffic. 


### Summary of IPv6 Support
- Supported: Dual-stack clusters, LoadBalancer (v1.27+), most add-ons, cluster extensions.
- Not Supported: IPv6-only clusters, AKS Automatic SKU with IPv6.


