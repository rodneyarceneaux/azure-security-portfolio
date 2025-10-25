# 🛡️ Azure Defender + Microsoft Sentinel Integration

This project extends the **Azure Secure Landing Zone** by integrating **Microsoft Defender for Cloud (XDR)** and **Microsoft Sentinel (SIEM)**.  
It establishes a unified detection, investigation, and response environment across your Azure resources — enabling visibility into security alerts, network flows, and policy compliance.

---

## 🎯 Objective

To design and implement a **detection and response layer** for the Secure Landing Zone using Azure-native tools.

| Capability | Tool | Description |
|-------------|------|-------------|
| Threat Detection | Microsoft Defender for Cloud | Identifies vulnerabilities, misconfigurations, and attacks across workloads |
| Security Information and Event Management (SIEM) | Microsoft Sentinel | Centralized log ingestion, analytics, and incident management |
| Network Visibility | NSG Flow Logs + Activity Logs | Enables traffic flow and administrative operation monitoring |

---

## 🧩 Architecture Overview

![Sentinel Integration](./docs/sentinel-integration-diagram.png)

> 💬 **Commentary:**  
> Defender for Cloud provides XDR telemetry from Azure workloads, while Sentinel ingests these events into a centralized Log Analytics workspace.  
> This integration enables automated detection and alert correlation across your **hub-and-spoke landing zone**.

---

## 🧱 Implementation Steps

### Step 1️⃣ — Create or Verify Log Analytics Workspace
```bash
az monitor log-analytics workspace create \
  --resource-group rg-logs \
  --workspace-name law-landingzone \
  --location eastus

**### Step 2️⃣ — Enable Microsoft Sentinel**

Enable Sentinel on your workspace:

az sentinel workspace enable \
  --resource-group rg-logs \
  --workspace-name law-landingzone


Alternatively, in the Azure Portal:

Search Microsoft Sentinel → + Add

Select your existing Log Analytics Workspace

Click Add Microsoft Sentinel
Step 3️⃣ — Connect Azure Activity Logs

Connect subscription-level activity logs to Sentinel for audit and compliance monitoring:

az monitor diagnostic-settings create \
  --name "activitylog-to-law" \
  --resource "/subscriptions/<your-subscription-id>" \
  --workspace "/subscriptions/<your-subscription-id>/resourceGroups/rg-logs/providers/Microsoft.OperationalInsights/workspaces/law-landingzone" \
  --logs '[{"category": "Administrative", "enabled": true}]'


This ensures all management actions (create/delete/modify resources) are tracked.

Step 4️⃣ — Enable NSG Flow Logs (Network Watcher)

In the Azure Portal, search Network Watcher

Navigate to NSG Flow Logs

Select your hub-vnet NSG

Enable Flow Logs

Set destination as your Log Analytics Workspace (law-landingzone)

This provides visibility into allowed/denied traffic flows across your virtual networks.

Step 5️⃣ — Enable Microsoft Defender for Cloud Plans

Enable enhanced protection tiers for key services:

az security pricing create --name VirtualMachines --tier 'Standard'
az security pricing create --name AppServices --tier 'Standard'
az security pricing create --name SqlServers --tier 'Standard'


Defender for Cloud “Standard” tier activates vulnerability scanning, adaptive application controls, file integrity monitoring, and security recommendations.

Step 6️⃣ — Connect Microsoft Defender to Sentinel

Go to Microsoft Sentinel → Data connectors

Locate Microsoft Defender for Cloud

Click Open connector page → Connect

Now, Defender alerts will automatically be sent to Sentinel as incidents.

Step 7️⃣ — Create a Custom Analytics Rule

In Sentinel → Analytics → Create → Scheduled query rule

Use the following KQL query to detect failed sign-ins:

SigninLogs
| where ResultType != 0
| project TimeGenerated, UserPrincipalName, IPAddress, Location


Set trigger:

Condition: count > 5

Period: 5 minutes

Severity: Medium

This helps detect potential brute-force login attempts.

Step 8️⃣ — Create a Workbook Dashboard

Go to Sentinel → Workbooks → + Create new

Add visualizations for:

Defender alerts

Failed sign-ins

NSG flow logs

Save it as Landing Zone Security Overview

This dashboard acts as your visual SOC console.

Step 9️⃣ — (Optional) Deploy a Logic App for Alert Automation

You can automate alert responses (e.g., send email/Teams notifications):

Create a Logic App

Trigger: When a Sentinel incident is created

Action: Send an email or Teams message with incident details

This demonstrates automation (SOAR) integration with Sentinel.
