# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

I setup a mini honeynet in Azure and collected logs from different sources into a Log Analytics workspace. Microsoft Sentinel used this data to create attack maps, send alerts, and generate incidents. I recorded security metrics in the unsecured environment for 24 hours, then applied security controls to make it more secure, and measured the metrics again for another 24 hours. The results are shown below and include the following metrics:

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

For the "BEFORE" metrics, all resources were set up and exposed to the internet. The Virtual Machines had no restrictions on their Network Security Groups or built-in firewalls, and all other resources were accessible through public endpoints without using Private Endpoints.

For the "AFTER" metrics, Network Security Groups were secured by blocking all traffic except from my admin workstation. Additionally, all other resources were protected by their built-in firewalls and Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-05-26 19:37:39
Stop Time 2024-05-27 19:37:39

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 315251
| Syslog                   | 3805
| SecurityAlert            | 11
| SecurityIncident         | 300
| AzureNetworkAnalytics_CL | 1696

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-05-31 03:25:02
Stop Time	2024-06-01 03:25:02

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 17606
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, I built a mini honeynet in Microsoft Azure and integrated log sources into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and create incidents based on these logs. Metrics were measured in the insecure environment before applying security controls, and again after implementing these measures. Notably, the number of security events and incidents significantly decreased after the security controls were applied, demonstrating their effectiveness.

It's important to note that if the resources in the network were heavily used by regular users, more security events and alerts might have been generated during the 24-hour period after the security controls were implemented.
