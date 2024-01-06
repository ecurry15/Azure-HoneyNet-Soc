# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/098ed371-c9b0-4f30-a101-40cdfe8f103c)



## Introduction

Hello everyone! In this project, I built a mini honeynet in Azure and ingested logs from various resources into a Log Analytics workspace. I then used the logs and Microsoft Sentinel to build attack maps(based on imported GeoIp data), trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 12 hours, applied some security controls to harden the environment, measured metrics for another 12 hours, and then showed the results below. This was a very interesting lab that helped further my skills in Azure, Cloud Security, Incident Response, and NIST SP 800-53.

## Lab Metrics:
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Honeynet Architecture:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (1 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

## Architecture Before Hardening / Security Controls
<b> For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups configured to <ins>Allow All INBOUND Traffic</ins>, and the storage and key vault had no private endpoint protection, leaving them completely vulnerable! </b>
<b> </b>

![image](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/1f0357e8-4889-45a7-9645-943f8bd40d18)
![AllowAnyRule](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/92a35efd-03fa-4d92-ade6-e7ff2608a93e)



## Architecture After Hardening / Security Controls
<b>For the "AFTER" metrics, Network Security Groups were hardened by deleting the previous inbound rule, and by creating a new rule to block ALL traffic except for my IP address. I also created private endpoints on all other resources and created a Network Security Group for the network's subnet. </b>
<b> </b>
### Hardened Network Topology Below: 
![image](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/5ceafa04-f5a7-4632-be8f-3bc912241abb)
### Comparison between running <ins>Nslookup</ins> within the network vs outside the network after creating private endpoints. ```Notice the Ip address```:
![image](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/9c6bef86-bde5-48a4-8198-5048c08ddb30)





## Attack Maps Before Hardening / Security Controls
![nsg-malicious-allowed-in](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/a7c68013-9b57-4020-bc21-c4e7ddc3809a)
<br>
![linux-ssh-fail](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/c1903176-1630-44d8-b19b-f0aeec199c02)
<br>
![windows-rdp-auth-failMap](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/041f8285-080b-433e-a0a3-f960ffac31e4)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in my insecure environment for 12 hours:

![image](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/22971252-39d3-4c45-bfda-7dfc26954ee4)



## Attack Maps Before Hardening / Security Controls

```All map queries returned no results due to no instances of malicious activity for the 12 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics I measured in my environment for another 12 hours, but after I applied security controls:

![image](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/81b89858-bf29-4337-b808-8a321232d211)




## Results After Hardening

![image](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/f6052da3-3478-4220-b725-8ed2cc434370)





## Further Hardening Steps: 

- I enabled <ins>Microsoft Defender for Cloud</ins> and followed a few of the configuration recommendations like : 
  - Enabling multi-factor Authentication
  - Configuring Machines to automatically check for updates
- I enabled <ins>NIST SP 800-53</ins> and fulfilled the standards associated with <ins>AC-5 Separation of Duties</ins>

<b> </b>

#### After following configuration recommendations : 
![image](https://github.com/ecurry15/Azure-HoneyNet-Soc/assets/87204188/ae0ac78c-b3b5-4b40-8b59-4d2c31fe6590)


## Conclusion

In this project, I constructed a honeynet using Microsoft Azure and collected logs in a Log Analytics workspace. Microsoft Sentinel was used to create attack maps and trigger alerts. Additionally, metrics were measured in the insecure environment before security controls were applied and then measured again after I hardened the network. Security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.
<b> </b>

Thanks for viewing!
