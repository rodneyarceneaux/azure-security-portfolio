# 🧱 Azure Secure Landing Zone

This project builds a secure Azure foundation using a hub-and-spoke model with governance, RBAC, and Policy-as-Code enforcement.

## 🗺️ Architecture Diagram
![Architecture](./docs/architecture-diagram.png)

## ⚙️ Key Components
| Component | Description |
|------------|--------------|
| Management Group | Governance root for security policies |
| Azure Policy | Enforces tagging, region restrictions, and encryption |
| VNets (Hub/Spoke) | Isolated workloads with secure routing |
| Log Analytics | Centralized monitoring and auditing |

## 🛡️ CMMC Compliance Mapping
| Control | Azure Feature | Description |
|----------|----------------|--------------|
| AC-2 | RBAC | Role-based access control for least privilege |
| SC-13 | Azure Storage Encryption | Enforces encryption at rest |
| AU-2 | Diagnostic Logs | Captures admin and user activities |

## 🚀 Deployment Steps
1. Create Management Group and Resource Groups  
2. Deploy VNets and peer connections  
3. Assign Azure Policies  
4. Configure Log Analytics and diagnostic settings  

## 📸 Screenshots
## 🗺️ Architecture Diagram
![Azure Secure Landing Zone](./docs/architecture-diagram.png)

> 💬 **Commentary:**  
> - **Management Group**: Root governance layer that enforces RBAC, Azure Policies, and compliance controls across all subscriptions.  
> - **Hub VNet**: Centralized network containing shared services such as Bastion and Azure Firewall.  
> - **Spoke VNets**: Isolated application networks peered to the hub for secure communication without direct internet exposure.  
> - **Azure Bastion**: Provides secure RDP/SSH access to VMs via the Azure Portal without exposing public IPs.  
> - **Log Analytics Workspace**: Aggregates diagnostic and security logs from all VNets for unified monitoring.

