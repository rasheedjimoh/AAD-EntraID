# Integrating AzureAD / EntraID with On-Prem Active Directory and Cloud Services

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

### Installation Process:

**Download**: The first step is to download the Cloud Sync Agent software from the provider's website or designated portal. This is usually a straightforward process, similar to downloading any other software.

**Installation**: Once the software is downloaded, it needs to be installed on your company's local servers or computers. This typically involves running an installer program and following on-screen prompts to complete the installation.
![entraid-confirm](https://github.com/rasheedjimoh/AAD-EntraID/assets/157264080/f2deab35-4a9f-4e84-a3ef-da9242b2ef66)


**Configuration**: After installation, the Cloud Sync Agent needs to be configured to establish connections with both your on-premises systems and the cloud services you're using. This involves providing necessary credentials, such as account information and authentication tokens, to ensure secure communication. 

**Scoping filter**: Next, you'll need to define coping filters that specify which data from your on-premises systems should be synchronized with which data in the cloud. This ensures that the right information is transferred to the appropriate locations.
![aadsync4](https://github.com/rasheedjimoh/AAD-EntraID/assets/157264080/de68c142-027c-4922-acaa-a0b2a1cb695c)

**Attribute mapping**: connecting the dots between different systems in your company. It ensures that information matches across all systems, in our case we changed the language to "US English" then we set it to always so it's always enforced.

![aadsync5](https://github.com/rasheedjimoh/AAD-EntraID/assets/157264080/50072c55-4819-4bc3-8486-7b214a80be5c)


**Testing**: Once configured, it's important to test the Cloud Sync Agent to ensure that data is being synchronized correctly and securely. This may involve running test syncs and verifying that data integrity is maintained throughout the process. We will use **Provision on demand** for this purpose.

![aadsync8](https://github.com/rasheedjimoh/AAD-EntraID/assets/157264080/3b18cc0e-7204-443d-9ea6-7d4ceda6c067)


**Deployment**: Finally, once testing is successful, the Cloud Sync Agent can be deployed across your organization's infrastructure as needed. This may involve installing the agent on additional servers or computers to ensure comprehensive data synchronization across your network. We click **Review and enable** to proceed.
![aadsync3](https://github.com/rasheedjimoh/AAD-EntraID/assets/157264080/db612daf-612b-4904-ad0c-d658022c9076)



## Conclusion
In conclusion, we highlighted the critical role of seamless integration between Azure AD/EntraID, Active Directory, and CloudSync in optimizing access management and security within our organization. By bridging the gap between on-premises and cloud-based systems, we ensure efficient user access control while maintaining data integrity and security across all platforms. This integration not only streamlines access management processes but also enhances collaboration and productivity in an increasingly digital environment.

![image](https://github.com/rasheedjimoh/AAD-EntraID/assets/157264080/ae5f0537-1ae2-4293-9751-4b75d12f7baf)
