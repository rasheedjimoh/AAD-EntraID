# Integrating EntraID with On-Prem Active Directory and Cloud Services

## Introduction
In this lab, we will be focusing on the seamless integration of Azure Active Directory(AAD) / EntraID which will be used to turn our environment with on-prem Active Directory into a hybrid environment by utilizing CloudSync.

 ### Let's break it down:

let's simplify it:

**EntraID/Azure Active Directory(AAD/Azure AD)**: EntraID is a system that helps manage user access to various company resources like files, applications and databases. It's like a digital keycard system for your company's digital assets.

**On-prem AD (Active Directory)**: Think of Active Directory as a central database that stores information about all the users, computers and other resources in your company's network. It's like a master directory that keeps track of who's who and what they have access to within your organization.

**Cloud Sync Agent**: This is a piece of software that helps bridge the gap between your on-premises (local) Active Directory and cloud-based services. It's like a messenger that securely transfers information between your company's internal systems and the external cloud services you use.

Now, let's connect them:

EntraID needs to know who has access to what resources within your company. It gets this information from your on-prem Active Directory, which contains all the user accounts and permissions. However, if your company also uses cloud-based services (like Google Workspace or Microsoft 365), you need a way to keep EntraID updated with changes made in your on-prem Active Directory. That's where the Cloud Sync Agent comes in. It acts as a bridge, making sure that any changes made in your on-prem Active Directory are synchronized with EntraID, ensuring that everyone has the right access permissions no matter where they're accessing company resources from—whether it's on-site or in the cloud.

---------------

![aadsync7](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/0ad01670-1d08-497e-9eac-9e0db2040d34)


--------

![aadsync6](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/ea5106cc-71d1-40de-837f-959f3ef52c79)



  
## Technologies/Stacks
- VirtualBox as Hypervisor Type 2
- Azure Active Directory / EntraID
- Windows Server 2022 (DC): Mydomain's Domain Controller
- Cloud Sync Agent
## Process/Procedure Overview

## Pre-Installation Checks
**1. Check if the IP address is assigned by our domain controller and in the DHCP scope we created in Active Directory Lab.**
   ![IPconfig](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/20e383f3-b9ce-441e-b8a0-005dda6706ce)
**2. Verify SSH is working form DC(domain controller)**
   ![SSH-from-DC](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/12f58d22-1335-453f-888f-cd56b86d9aef)
**3. Verify the date, time and timezone.**
   ![date, time and timezone](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/ff7e2263-17a1-4bcd-a117-efa2008ebfed)


## Problem: Inability to join DC because of reverse DNS
**Try joining the domain after everything is in place but unable to so we troubleshot and find the problem.**
![rdns-problem](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/17f88038-a0f8-4086-9b3a-1a4fe7d1392a)

## Resolution
**Reverse DNS(rdns) set to false: To be able to successfully join the domain, You have to edit the /etc/krb5.conf and set rdns to false like this then restart**
![rdns-set-false](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/d1d0f01f-0749-4225-9785-cb7accc3446b)

## Configuration file modifications
Entries added to the krb5.conf file:
udp_preference_limit = 0: This entry sets the preference for using TCP (Transmission Control Protocol) over UDP (User Datagram Protocol) for communication. A value of 0 means that TCP is preferred.
rdns = False: This entry disables reverse DNS (Domain Name System) lookup. It prevents the system from trying to find the hostname associated with an IP address.
dns_lookup_kdc = True: This entry enables DNS (Domain Name System) lookup for Key Distribution Center (KDC) servers. KDC is a component of Kerberos responsible for authentication.
dns_lookup_realms = True: This entry enables DNS lookup for Kerberos realms. It allows the system to use DNS to locate Kerberos realms.
![krb5.conf](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/f2321536-ac1d-4db4-9256-c286bfca97fd)

Entries added to the sssd.conf file:
krb5_keytab = /etc/krb5.keytab: This entry specifies the location of the Kerberos keytab file (krb5.keytab). The keytab file contains keys for service principals and is used for authentication.
ldap_keytab_init_creds = True: This entry, when set to True, instructs SSSD to use the credentials stored in the keytab file (krb5.keytab) for initiating LDAP (Lightweight Directory Access Protocol) operations.
![sssd.conf](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/728cc170-8f7d-4a25-90de-24b42ac1fd50)

**Making domain member sudoer on Ubuntu**
Became a sudoer.
![sudoers](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/c87cdcf9-6663-4225-b3ab-c566a897873b)


## Successfully Signed on
**We successfully joined the domain(mydomain.com) now.**
![Success](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/fcc0fa82-3cec-4a8d-a2da-5e78bf16e004)


## Verifications
**Verified we can join the domain and become a Kerberos member. "klist" command checks for Kerberos members.**
![klist](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/1db9b40a-b3f9-41c9-a194-e8492aba3585)

**Restart SSSD and check SSD status then enable**
![restart and status](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/50badcc9-7527-45c6-8cac-c13e840593b3)

**We enabled SSSD to start automatically each time the system start up**

![Screenshot 2024-02-06 010502](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/73d034a9-5975-49e8-8762-17c0999d87a3)

**Checking Keberos list to see our ticket**
![klist](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/dbc6debe-f17d-4026-b8d7-ad2de5e50ac5)

## ADUC
**We can now see our computer has joined our domain computer for management.**
![DC-computer](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/b30df6ca-fb29-4d12-a184-0cf1485b2919)


## Automate user home folder creation on the first logon
**The home folder will be created for each user on the first sign-on.**
![home folder creation](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/62686382-4813-4a7e-a5a2-ec4e3019052a)

**First sign on to the domain using the domain account, The User's folder was created as expected.**
![domain-first sign on](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/ee260651-59d7-48f4-a1ec-3a7c1bfdc5f7)

**Proof domain user became a sudoer**
![sudoer](https://github.com/rasheedjimoh/UbuntuAD/assets/157264080/9abeb090-5cad-4970-86c4-1118fb1e5f0d)


## Conclusion
The successful integration of Ubuntu Server into the Active Directory domain showcases our adeptness in harmonizing diverse systems. With flawless authentication and heightened security through Kerberos and LDAP, we stride forward, well-prepared for intricate IT landscapes. This achievement marks a milestone in crafting resilient and interconnected digital environments.
