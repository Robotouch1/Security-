# Architecture — Key Vault RBAC Containment

## Components

- **Azure Key Vault**
- **Azure Diagnostics**
- **Log Analytics Workspace**
- **Microsoft Sentinel**
- **Sentinel Automation Rule**
- **Logic App (Stateful)**
- **Azure Resource Manager (RBAC APIs)**

---

## Architecture Flow

```text
Key Vault
  │
  ├─ Secret Access (Audit Event)
  │
Azure Diagnostics
  │
  ├─ Logs sent to Log Analytics
  │
Microsoft Sentinel
  │
  ├─ Analytic Rule → Incident
  │
  ├─ Automation Rule (Tag + Status)
  │
Logic App (Managed Identity)
  │
  ├─ Query Logs (Who + Which Vault)
  ├─ ARM API: Get Role Assignments
  ├─ ARM API: Delete Role Assignment(s)
  └─ Comment on Incident
