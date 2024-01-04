# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I built a mini honeynet in Azure and ingested log sources from various resources into a Log Analytics workspace, which was then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 12 hours, applied some security controls to harden the environment, measured metrics for another 12 hours, and then showed the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![image](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/87699bc4-25e8-421d-8864-a9b187db51b4)



## Architecture After Hardening / Security Controls
![image](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/5ceafa04-f5a7-4632-be8f-3bc912241abb)
![Nslookup](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/48d91258-7a50-4884-a6ad-14bea4b434b7)




The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (1 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups configured with a firewall rule to Allow All traffic, leaving them completely vulnerable.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![nsg-malicious-allowed-in](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/a7c68013-9b57-4020-bc21-c4e7ddc3809a)
<br>
![linux-ssh-fail](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/c1903176-1630-44d8-b19b-f0aeec199c02)
<br>
![windows-rdp-auth-failMap](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/041f8285-080b-433e-a0a3-f960ffac31e4)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in my insecure environment for 12 hours:
Start Time 2023-12-31 01:54:40
Stop Time 2024-01-01 01:54:40

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 12543
| Syslog                   | 836
| SecurityAlert            | 3
| SecurityIncident         | 24
| AzureNetworkAnalytics_CL | 1032

## Attack Maps Before Hardening / Security Controls

```All map queries returned no results due to no instances of malicious activity for the 12 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-01-02 23:00:12
Stop Time	2024-01-03 23:00:12

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 4746
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
