# Architecture — RBAC Auto-Removal (Tag-Gated)

This playbook implements a **human-approved remediation pipeline** for RBAC privilege escalation using Microsoft Sentinel + Logic Apps + Azure Activity logs.

---

## Components

### 1) Microsoft Sentinel (Workspace: `law-central-security`)
- Runs the analytic rule **RBAC Privilege Escalation – Subscription Role Assignment**
- Creates an incident that aggregates the RBAC write event(s)
- Hosts an **Automation Rule** that triggers the playbook **only after human approval via tag**

### 2) Sentinel Automation Rule (Incident Updated)
- Trigger: **When incident is updated**
- Condition: Tag contains `RBAC-AutoRemove`
- Action: Run playbook `PB-RBAC-Auto-Remove`

### 3) Logic App Playbook (`PB-RBAC-Auto-Remove`)
- Trigger: Microsoft Sentinel incident (updated)
- Reads incident metadata + labels (tags)
- Queries Log Analytics (AzureActivity) to find the **role assignment write** event
- Extracts the **RoleAssignment GUID** from the AzureActivity record
- Calls **ARM** to delete the role assignment
- Writes audit comments back to the Sentinel incident

### 4) Log Analytics (AzureActivity table)
- Authoritative source of the RoleAssignment ID because Sentinel incident payload does **not** reliably include it.

### 5) Azure Resource Manager (RBAC Control Plane)
- Deletes the role assignment via:
  `Microsoft.Authorization/roleAssignments/{roleAssignmentGuid}`

---

## End-to-End Flow (Accurate)

```text
[User / Attacker]
   |
   |  (1) Create high-privilege RBAC assignment
   |      - Owner / Contributor / User Access Admin
   v
[Azure Subscription Control Plane]
   |
   |  (2) Emits control-plane logs
   v
[AzureActivity Logs  →  Log Analytics Workspace (law-central-security)]
   |
   |  (3) Scheduled analytic rule queries AzureActivity
   v
[Microsoft Sentinel Analytic Rule]
   |
   |  (4) Creates Sentinel incident
   v
[Sentinel Incident]
   |
   |  (5) Human approval step:
   |      Analyst adds tag "RBAC-AutoRemove"
   v
[Sentinel Automation Rule: When incident is updated]
   |
   |  (6) If tag present → run playbook
   v
[Logic App Playbook: PB-RBAC-Auto-Remove (System Assigned MI)]
   |
   |  (7) Get Incident (V3) → confirm tag
   |  (8) Run Log Analytics query:
   |      AzureActivity where OperationNameValue has roleAssignments/write
   |  (9) Extract roleAssignment GUID from ResourceId / Properties.entity
   v
[ARM REST API Call (DELETE roleAssignments/{GUID})]
   |
   |  (10) Role assignment removed
   v
[Audit]
   |
   |  (11) Add comment to Sentinel incident + optional processed tag
   v
[Sentinel Incident Timeline shows: tag → playbook → delete action]
