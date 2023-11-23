# Azure Honeynet: Simulating Real-World Cyber Attacks
![Cloud Honeynet / SOC](https://github.com/FUenal/Azure-SOC-Honey-Net/blob/main/assets/01.png)

## Introduction

I'm excited to introduce my most recent undertaking, centered around creating a honeynet within the Azure environment to replicate authentic cyber threats. This endeavor highlights my proficiency in Azure security, incident response, and fortifying digital environments.

## Objective
The primary goal of this project involved establishing intentionally vulnerable virtual machines within the Azure infrastructure, as detailed [here](https://github.com/FUenal/Azure-VM-Prep/blob/main/README.md). The aim was to attract and analyze cyber attacks, providing valuable insights into the tactics and techniques employed by attackers. Through this initiative, I demonstrated my capability to promptly and efficiently respond to identified issues.

## Technologies, Regulations, and Azure Components Employed:

- Azure Virtual Network (VNet)
- Azure Network Security Group (NSG)
- Virtual Machines (2x Windows, 1x Linux)
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Azure Key Vault for Secure Secrets Management
- Azure Storage Account for Data Storage
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Microsoft Defender for Cloud to Protect Cloud Resources
- Windows Remote Desktop for Remote Access
- Command Line Interface (CLI) for System Management
- PowerShell for Automation and Configuration Management
- [NIST SP 800-53 Revision 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) for Security Controls
- [NIST SP 800-61 Revision 2](https://www.nist.gov/privacy-framework/nist-sp-800-61) for Incident Handling Guidance

## Methodology

- <b>*Creating the honeynet*</b>: I started with [deploying multiple vulnerable virtual machines](https://github.com/FUenal/Azure-VM-Prep/blob/main/README.md) in Azure, simulating an insecure environment.

- <b>*Monitoring and analysis*</b>: Azure was set up to collect log data from diverse sources into a log analytics workspace. Subsequently, Microsoft Sentinel was employed to construct attack maps, initiate alerts, and generate incidents based on the gathered data.

- <b>*Security metrics measurement*</b>: I monitored the environment for a 24-hour period, documenting crucial security metrics during its vulnerable state. This established a baseline for comparison with metrics after implementing remedial measures.

- <b>*Incident response and remediation*</b>: Following the resolution of incidents and the identification of vulnerabilities, I initiated the fortification of the environment by implementing security best practices and adhering to Azure-specific recommendations.

- <b>*Post-remediation analysis*</b>: I conducted a second 24-hour observation of the environment to reassess security metrics, comparing the outcomes with the initial baseline.


## Architecture Prior to Implementing Hardening Measures and Security Controls
![Architecture Diagram](https://github.com/FUenal/Azure-SOC-Honey-Net/blob/main/assets/02.png)

<b>Before Hardening Measures and Security Controls:</b>

- During the project's "BEFORE" phase, all resources were initially launched with public accessibility on the internet. This deliberately insecure configuration aimed to draw potential cyber attackers and monitor their strategies. The Virtual Machines had open Network Security Groups (NSGs) and permissive built-in firewalls, enabling unrestricted access from any origin. Furthermore, other resources, including storage accounts and databases, were deployed with public endpoints exposed to the internet, without incorporating Private Endpoints for enhanced security.

## Architecture After Implementing Hardening Measures and Security Controls
![Architecture Diagram](https://github.com/FUenal/Azure-SOC-Honey-Net/blob/main/assets/03.png)
 <b>For the "AFTER" stage, I implemented a series of hardening measures and security controls to improve the environment's overall security posture. These improvements included:</b>

- <b>Network Security Groups (NSGs)</b>: I hardened the NSGs by blocking all inbound and outbound traffic, with the sole exception of my own public IP address. This ensured that only authorized traffic from a trusted source was allowed to access the virtual machines.

- <b>Built-in Firewalls</b>: I configured the built-in firewalls on the virtual machines to restrict access and protect the resources from unauthorized connections. This step involved fine-tuning the firewall rules based on the specific requirements of each VM, thereby minimizing the potential attack surface.

- <b>Private Endpoints</b>: To enhance the security of other Azure resources, I replaced the public endpoints with Private Endpoints. This ensured that access to sensitive resources, such as storage accounts and databases, was limited to the virtual network and not exposed to the public internet. As a result, these resources were protected from unauthorized access and potential attacks.

By contrasting the security metrics prior to and following the implementation of these fortification measures and security controls, I could illustrate how each step significantly enhanced the overall security stance of the Azure environment.

## Attack Maps Before Hardening / Security Controls
<br />


> <b>NOTE: The attack maps were created by extracting information from a workbook that utilized predefined [KQL .JSON](https://github.com/FUenal/Cloud-SOC-Project-Resources/blob/main/MS%20Sentinel%20Maps%20(JSON)/linux-ssh-auth-fail.json) map files. These files offered a organized portrayal of the attack patterns and their corresponding data, facilitating the development of visualizations that accurately depicted the cyber threats and their consequences on the system.</b>


 <br />
 <br />
 
- <b>This attack map demonstrates the consequences of leaving the Network Security Group (NSG) open, as it allowed for malicious traffic to flow unimpeded. This visualization underscores the importance of implementing proper security measures, such as restricting NSG rules, to prevent unauthorized access and minimize potential threats.</b>


![NSG Allowed Inbound Malicious Flows](https://github.com/FUenal/Azure-SOC-Honey-Net/blob/main/assets/(before)nsg-malicious-allowed-in.png)<br>

 <br />
 <br />
 
 - <b>This attack map highlights the numerous syslog authentication failures experienced by the Linux server I deployed, indicating that unauthorized access attempts were made from outisde. This serves as a reminder of the importance of securing Linux servers with strong authentication mechanisms and monitoring system logs for signs of intrusion attempts.</b>
 
![Linux Syslog Auth Failures](https://github.com/FUenal/Azure-SOC-Honey-Net/blob/main/assets/(before)linux-ssh-auth-fail.png)<br>

 <br />
 <br />
 
 - <b>This attack map showcases numerous RDP and SMB failures, illustrating the persistent attempts by potential attackers to exploit these protocols. The visualization emphasizes the need for securing remote access and file sharing services to protect against unauthorized access and potential cyber threats.</b>
 
![Windows RDP/SMB Auth Failures](https://github.com/FUenal/Azure-SOC-Honey-Net/blob/main/assets/(before)windows-rdp-auth-fail.png)<br>

 <br />
 <br />

## Attack Maps After Hardening / Security Controls

> All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.

 <br />
 <br />
 
## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-11-20 12:04:45 PM
Stop Time 2023-11-21 12:04:45 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent (Windows VM)            | 40705
| Syslog (Linux VM)                   | 3308
| SecurityAlert (Microsoft Defender for Cloud            | 1
| SecurityIncident (Sentinel Incidents)        | 175
| NSG Inbound Malicious Flows Allowed | 2212



## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-11-22 11:01:09 AM
Stop Time	2023-11-23 11:01:09 AM


| Metric                   | Count
| ------------------------ | -----
| SecurityEvent (Windows VM)            | 17022
| Syslog (Linux VM)                   | 2
| SecurityAlert (Microsoft Defender for Cloud            | 0
| SecurityIncident (Sentinel Incidents)        | 0
| NSG Inbound Malicious Flows Allowed | 0

## Conclusion

In summary, I established a concise yet robust honeynet using Microsoft Azure's powerful cloud infrastructure. Microsoft Sentinel was then employed to trigger alerts and generate incidents based on logs from implemented watch lists. Initial metrics were recorded in the unprotected environment before introducing any security controls. Subsequently, a variety of security measures were implemented to strengthen the network against potential threats. After the application of these controls, additional measurements were taken.

Comparing pre- and post-implementation metrics revealed a substantial decrease in security events and incidents, underscoring the effectiveness of the implemented security controls. It's noteworthy to acknowledge that if the network resources were actively used by regular users, a higher number of security events and alerts might have occurred within the 24-hour period following the implementation of security controls.
