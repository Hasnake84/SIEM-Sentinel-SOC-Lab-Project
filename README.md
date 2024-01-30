# Building a SOC + Honeynet in Azure (Live Traffic)
<a href="https://imgur.com/KTYSwZJ"><img src="https://i.imgur.com//KTYSwZJ.png" title="source: imgur.com" /></a>

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Microsoft Defender For Cloud)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (NSG Inbound Malicious Flows allowed)

## Architecture Before Hardening / Security Controls
<a href="https://imgur.com/oURrFxS"><img src="https://i.imgur.com/oURrFxS.png" title="source: imgur.com" /></a><br>

## Architecture After Hardening / Security Controls
<a href="https://imgur.com/meQ3hpt"><img src="https://i.imgur.com/meQ3hpt.png" title="source: imgur.com" /></a><br>

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet (no use for Private Endpoints).

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

<h1>KQL(Kusto Query Languege)</h1>Used on Log Analytics Workbook to query log data from connected sources that are injested.
<a href="https://imgur.com/dniWa75"><img src="https://i.imgur.com/dniWa75.png" title="source: imgur.com" /></a><br>

## Attack Maps Before Hardening / Security Controls

<a href="https://imgur.com/kdOK7gI"><img src="https://i.imgur.com/kdOK7gI.png" title="source: imgur.com" /></a><br>
<a href="https://imgur.com/4LAbtdo"><img src="https://i.imgur.com/4LAbtdo.png" title="source: imgur.com" /></a><br>
<a href="https://imgur.com/nB6Qqih"><img src="https://i.imgur.com/nB6Qqih.png" title="source: imgur.com" /></a><br>
<a href="https://imgur.com/0rXRTXu"><img src="https://i.imgur.com/0rXRTXu.png" title="source: imgur.com" /></a><br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 1/12/2024, 11:02:29.977 PM		
Stop Time 1/13/2024, 11:02:29.977 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 66139
| Syslog                   | 5878
| SecurityAlert            | 21
| SecurityIncident         | 237
| AzureNetworkAnalytics_CL | 4755

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 1/14/2024, 11:00 PM
Stop Time	1/15/2024, 11:00 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9097
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.



