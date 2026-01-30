# Permissions Model

## Logic App Managed Identity Roles

### Log Analytics Workspace
- Microsoft Sentinel Responder
- Log Analytics Reader

### Subscription / Key Vault Scope
- User Access Administrator

---

## Why These Are Required

| Capability | Permission |
|----------|------------|
| Read incidents | Sentinel Responder |
| Query logs | Log Analytics Reader |
| List RBAC | ARM Read |
| Delete RBAC | User Access Administrator |

Without **both data-plane and control-plane permissions**, containment fails.
