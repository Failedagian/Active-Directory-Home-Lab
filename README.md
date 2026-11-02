 Active Directory Small Business IT Lab

 1. Project Overview

I designed and built a virtual IT environment to simulate how a small business manages its users, computers, network resources and security.

The lab uses Windows Server 2022 as a domain controller and a separate Windows 10 Pro virtual machine as a domain-joined client workstation.

The objective was to develop practical experience in Windows Server administration, centralised identity management, network services, access control and Group Policy.

I approached the project from an IT support perspective, focusing on common administrative tasks such as managing user accounts, troubleshooting connectivity, controlling access to shared resources and configuring client workstations.

 2. Business Scenario

A small business has employees across three departments: Sales, IT and Staff.

Each department requires access to its own shared files. The IT environment needs centralised user management, controlled resource access, consistent security policies and reliable network services.

This lab simulates how an IT administrator would configure and manage these requirements using Microsoft Windows Server technologies.

 3. Lab Architecture

| Component | Configuration |
|---|---|
| Virtualisation | Oracle VirtualBox |
| Server OS | Windows Server 2022 Standard Evaluation |
| Client OS | Windows 10 Pro |
| Domain Controller | DC01 |
| Client Workstation | PC01 |
| Domain Name | adlab.test |
| NetBIOS Name | ADLAB |
| Network Type | VirtualBox Internal Network |
| Network Name | ADLabNet |
| Network Services | DNS and DHCP |

 4. Network Configuration

| Setting | DC01 |
|---|---|
| IPv4 Address | 192.168.50.10 |
| Subnet Mask | 255.255.255.0 |
| Preferred DNS | 192.168.50.10 |
| Default Gateway | Not configured |

The lab uses the `192.168.50.0/24` private network.

DC01 provides DNS and DHCP services to the lab environment. PC01 uses the lab network to communicate with the domain controller.

*The final DHCP-assigned address for PC01 will be recorded here after verification.*

 5. Technologies and Tools

- Windows Server 2022
- Windows 10 Pro
- Oracle VirtualBox
- Active Directory Domain Services (AD DS)
- Domain Name System (DNS)
- Dynamic Host Configuration Protocol (DHCP)
- Group Policy Management
- PowerShell
- NTFS permissions
- Server Message Block (SMB)

 6. Implementation

 6.1 Active Directory and Domain Configuration

- Installed the Active Directory Domain Services role.
- Promoted DC01 to the first domain controller in a new forest.
- Created the `adlab.test` domain.
- Configured DNS during domain controller promotion.
- Joined PC01 to the Active Directory domain.

Purpose: Centralised authentication and administration of domain-connected users and computers.

 6.2 User and Group Management

- Created departmental organisational units for Sales, IT and Staff.
- Created domain user accounts.
- Created departmental security groups.
- Assigned users to their respective groups.

Purpose: Organised directory objects and managed access through group membership.

 6.3 Network Services

- Configured a static IPv4 address for DC01.
- Configured DNS for domain name resolution.
- Configured DHCP for automatic client IP addressing.
- Connected PC01 to the internal lab network.
- Verified DNS resolution and domain connectivity.

Purpose: Provided the network services required for domain-connected devices to communicate and locate domain resources.

 6.4 File Server and Access Control

- Created departmental folders for Sales, IT and Staff.
- Configured NTFS permissions using departmental security groups.
- Created SMB network shares.
- Configured share-level permissions.
- Tested access from the Windows 10 client.
- Tested authorised and unauthorised departmental access.

Purpose: Restricted access to company resources according to departmental responsibilities.

6.5 Group Policy Management

- Configured domain password policies through the Default Domain Policy.
- Created departmental Group Policy Objects.
- Configured network drive mapping through Group Policy Preferences.
- Assigned drive letters to departmental shared folders.

Purpose: Applied centralised security settings and automated access to departmental network resources.

 6.6 PowerShell Administration

Used PowerShell to perform administrative and verification tasks, including:

- Managing Active Directory group membership.
- Configuring NTFS permissions with `icacls`.
- Creating SMB shares.
- Inspecting share permissions.
- Verifying group membership and network configuration.

Purpose: Developed familiarity with command-line administration and repeatable administrative tasks.

## 7. Testing and Verification

Testing was performed from the Windows 10 domain-joined client.

| Test | Expected Result |

| Domain join | PC01 successfully joins adlab.test |
| DNS resolution | Domain name resolves through DC01 |
| Domain authentication | Domain user accounts authenticate |
| Departmental file access | Users access authorised shared folders |
| Restricted access | Users are denied access to unauthorised departmental resources |
| Network drive mapping | Departmental drives appear on the client |
| SMB connectivity | Shared folders are accessible over the network |

Screenshots and command outputs will provide supporting evidence for each test.

## 8. Troubleshooting and Problem Solving

During implementation, I encountered configuration issues involving:

- VirtualBox installation and boot configuration.
- Virtual network adapter settings.
- DNS configuration and domain connectivity.
- Domain controller promotion warnings.
- SMB share creation and account permission errors.
- File and folder access permissions.
- Group Policy configuration and client-side verification.

I documented the symptoms, investigation steps, solutions and verification results in the separate troubleshooting log.

See: `07-Troubleshooting/troubleshooting-log.md`

## 9. Skills Developed

- Windows Server administration.
- Active Directory and identity management.
- DNS and DHCP configuration.
- IPv4 addressing and subnetting.
- Domain joining and authentication.
- User and security group administration.
- NTFS and SMB access control.
- Group Policy configuration.
- PowerShell administration.
- Network and access troubleshooting.
- Technical documentation and testing.

 10. Project Evidence

Supporting screenshots, configuration records and test results are organised into the repository folders.

The evidence includes:

- Virtual machine configuration.
- Domain controller and Active Directory setup.
- Departmental users, groups and organisational units.
- Network configuration.
- File sharing and permissions.
- Group Policy settings.
- Client-side testing.
- Troubleshooting records.

 11. Key Learning Outcomes

This project gave me practical experience building and administering a Windows domain environment.

I developed a better understanding of how identity management, network services, file permissions and Group Policy work together in a business IT environment.

I also gained experience investigating configuration problems, testing solutions and documenting technical work.
