## Evidence Artifacts

| Control Objective | Evidence Artifact | What It Proves | Link |
|---|---|---|---|
| Platform / workload separation | Subscription boundary screenshot | Security controls are isolated from workloads | ./screenshots/01-subscription-boundary-proof.png |
| Centralized logging ownership | Log Analytics workspace overview | Logs are platform-owned and not workload-controlled | ./screenshots/02-central-law-ownership-proof.png |
| Control-plane telemetry ingestion | AzureActivity query results | Administrative actions are centrally observable | ./screenshots/03-azureactivity-telemetry-proof.png |
| Detection engineering in place | Sentinel analytics rules view | Custom, high-signal detections are deployed | ./screenshots/04-alert-rules.png |
| Network exposure drift detection | NSG risky inbound rule | Intentional exposure change occurred | ./screenshots/05-nsg-risky-rules.png |
| Incident generation & correlation | Sentinel incident details | Drift resulted in a security incident | ./screenshots/06-incident-events.png |
