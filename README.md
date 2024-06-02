# Azure-Honeynet
# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I created a mini honeynet in Azure and collected log data from various sources into a Log Analytics workspace. This workspace was then utilized by Microsoft Sentinel to develop attack maps, trigger alerts, and generate incidents. I measured specific security metrics in the unsecured environment for 24 hours, implemented security controls to enhance the environment's security, and then measured the metrics again for another 24 hours. The metrics I collected are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

Additionally, the SOC utilized the following tools, components and regulations:

- Microsoft Sentinel (SIEM)
- Microsoft Defender for Cloud (MDC)
- NIST SP 800-53 Revision 4
- PCI DSS 3.2.1
- Log Analytics Workspace
- Windows Event Viewer
- Kusto Query Language (KQL)
- PowerShell


For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-05-14 13:35:32
Stop Time 2024-05-15 13:35:32

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 53531
| Syslog                   | 367
| SecurityAlert            | 6
| SecurityIncident         | 245
| AzureNetworkAnalytics_CL | 2061

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Attack Maps After Hardening / Security Controls

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-05-30 18:25:02
Stop Time	2024-05-31 18:25:02

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Impact of Security Controls

| Metric                   | Change post-hardening 
| ------------------------ | -----
| SecurityEvent (Windows VMs) | 8778
| Syslog (Linux VMs) | 25
| SecurityAlert (Microsoft Defender for Cloud) | 0
| SecurityIncident (Sentinel Incidents) | 0
| AzureNetworkAnalytics_CL | 0

## NSG Allowed Malicious Inbound Flows

## Linux SSH Authentication Failures

## Windows RDP/SMB Authentication Failures

## MS SQL Server Authentication Failures


## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
