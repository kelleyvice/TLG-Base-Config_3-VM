﻿# TLG-Base-Config_3-VM

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F100-blank-template%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F100-blank-template%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/visualizebutton.png"/>
</a>

This template deploys the **TLG-Base-Config_3-VM** solution, a Test Lab Guide (TLG) solution used as a base build for a variety of test labs. 
The **TLG-Base-Config_3-VM** solution provisions a Windows Server 2012 R2 Active Directory domain controller using the specified domain name, 
an application server running Windows Server 2012 R2, and a client VM running Windows 10.

`Tags: TLG, Test Lab Guide, Base Configuration`

## Solution overview and deployed resources

The following resources are deployed as part of the solution:

+ **ADDC VM**: Windows Server 2012 R2 VM configured as a domain controller and DNS with static private IP address
+ **App Server VM**: Windows Server 2012 R2 VM joined to the domain
+ **Client VM**: Windows 10 client joined to the domain
+ **NSG**: Network security group configured to allow inbound RDP on 3389
+ **Virtual network**: Virtual network for internal traffic, configured with custom DNS pointing to the ADDC's private IP address
+ **Network interfaces**: 1 NIC per VM
+ **Public IP addresses**: 1 public IP per VM
+ **Storage accounts**: 2 storage accounts for VHDs and diagnostics respectively
+ **JoinDomain**: Each member VM uses the **JsonADDomainExtension** extension to join the domain.
+ **BGInfo**: The **BGInfo** extension is applied to all VMs.
+ **Antimalware**: The **iaaSAntimalware** extension is applied to all VMs.

## Solution notes

* The *App server* and *Client* VM resources depend on the **ADDC** resource deployment to ensure that the AD domain exists prior to execution of 
the JoinDomain extensions. The asymmetric VM deployment adds a few minutes to the overall deployment time.
* The private IP address of the **ADDC** VM is always *10.0.0.10*. This IP is set as the DNS IP for the virtual network and all member NICs.
* Deployments with vmSize set to *DS3* or smaller may appear to fail due to timeout of the VM agent on the Client VM, but deployment of all 
resources will succeed. This is a known issue with Windows 10 VM deployments as of the date of publication.
* Deployment outputs include public IP address and FQDN for each VM.