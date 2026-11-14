# Active Directory Small Business IT Lab

 Project Summary

Designed, deployed and tested a virtual Windows domain environment to simulate IT administration for a small business.

**Technologies:** Windows Server 2022, Windows 10 Pro, Oracle VirtualBox, Active Directory, DNS, DHCP, Group Policy, PowerShell, NTFS and SMB.

**Key achievements:**
- Deployed an Active Directory domain and joined a Windows 10 workstation.
- Configured and verified DNS resolution and DHCP client allocation.
- Implemented departmental file sharing with group-based access restrictions.
- Configured password policies and verified departmental network drive mappings.
- Investigated technical issues and documented troubleshooting procedures.

The project demonstrates practical skills relevant to IT support, systems administration and infrastructure management.

---

## 1. Project Overview

Built a virtual small business IT environment using Oracle VirtualBox.

The environment consists of a Windows Server 2022 domain controller, DC01, and a Windows 10 Pro client workstation, PC01.

The project focuses on centralised identity management, network services, departmental file sharing, security policies and troubleshooting.

### Project Objectives

- Install and configure Windows Server 2022.
- Deploy Active Directory Domain Services (AD DS).
- Create a new Active Directory forest and domain.
- Manage domain users, Organisational Units (OUs) and security groups.
- Configure DNS and DHCP services.
- Join Windows 10 to the domain.
- Configure NTFS and SMB share permissions.
- Implement password policies and departmental network drive mappings.
- Use PowerShell for administrative and verification tasks.
- Test configurations and document troubleshooting results.

## 2. Business Scenario

The simulated organisation consists of three departments: Sales, IT and Staff.

Each department requires individual employee accounts and access to its own shared files.

The organisation needs:

- Centralised user authentication.
- Departmental account organisation.
- Group-based resource access.
- Consistent security policies.
- Automatic client IP address allocation.
- Reliable internal DNS resolution.
- Centralised workstation administration.

Windows Server provides the infrastructure required to support these business needs.

## 3. Lab Architecture

The lab consists of two virtual machines connected through a VirtualBox Internal Network named `ADLabNet`.

| Component | Configuration |
|---|---|
| Virtualisation Platform | Oracle VirtualBox |
| Server Operating System | Windows Server 2022 Standard Evaluation |
| Client Operating System | Windows 10 Pro |
| Domain Controller | DC01 |
| Client Workstation | PC01 |
| Active Directory Domain | adlab.test |
| NetBIOS Name | ADLAB |
| Network Type | VirtualBox Internal Network |
| Network Name | ADLabNet |
| Subnet | 192.168.50.0/24 |

### Network Topology

```text
                Oracle VirtualBox
                 Internal Network
                     ADLabNet
                 192.168.50.0/24
                         |
              +----------+----------+
              |                     |
         +---------+           +---------+
         |  DC01   |           |  PC01   |
         | Server  |           | Client  |
         +---------+           +---------+
              |                     |
        192.168.50.10          192.168.50.100
           Static                  DHCP
              |
       Active Directory
       DNS / DHCP
       SMB File Sharing
       Group Policy
```

DC01 provides centralised domain, authentication, DNS, DHCP and file-sharing services.

PC01 acts as a domain-joined workstation used for client-side testing.

The internal network isolates the virtual machines from the physical home network.

### Network Configuration

| Setting | Configuration |
|---|---|
| DC01 IPv4 Address | 192.168.50.10 |
| PC01 IPv4 Address | 192.168.50.100 |
| Subnet Mask | 255.255.255.0 |
| Preferred DNS Server | 192.168.50.10 |
| Default Gateway | Not configured |
| DHCP Scope | 192.168.50.100–192.168.50.200 |
| DHCP Lease Duration | 8 days |

PC01 successfully obtained its IPv4 configuration from the DHCP service running on DC01.

[View DHCP Client Verification](03-Network-Services/PC01%20DHCP%20configuration.png)

### Architecture Limitations

The environment uses a single domain controller and one client workstation.

The design does not provide domain controller redundancy or failover.

The lab uses evaluation software and an isolated network for educational purposes.

## 4. Project Documentation

Detailed implementation records and supporting evidence are organised into the following sections.

| Section | Description |
|---|---|
| [Lab Architecture](01-Lab-Architecture/README.md) | Virtual machines, network topology and design decisions |
| [Active Directory Configuration](02-Active-Directory%20Configuration/README.md) | Domain deployment, users, OUs and security groups |
| [Network Services](03-Network-Services/README.md) | IPv4 addressing, DNS, DHCP and client verification |
| [File Server and Permissions](04-File-Server-and-Permissions/README.md) | SMB shares, NTFS permissions and access testing |
| [Group Policy](05-Group-Policy/README.md) | Password policies, drive mapping and departmental verification |
| [PowerShell Administration](06-PowerShell/README.md) | Administrative commands and verification |
| [Troubleshooting](07-Troubleshooting/README.md) | Technical issues, investigations and resolutions |
| [Screenshots](08-Screenshots/README.md) | Supporting configuration and testing evidence |

## 5. Technologies and Tools

| Category | Technologies |
|---|---|
| Virtualisation | Oracle VirtualBox |
| Operating Systems | Windows Server 2022, Windows 10 Pro |
| Identity Management | Active Directory Domain Services |
| Networking | IPv4, DNS, DHCP |
| Access Control | Security Groups, NTFS, SMB |
| Policy Management | Group Policy Management |
| Administration | PowerShell, Command Prompt, icacls |
| Troubleshooting | ipconfig, nslookup, Resolve-DnsName, gpresult, net use |

## 6. Implementation

### 6.1 Active Directory Domain Services

Installed the Active Directory Domain Services role on DC01.

Promoted the server to a domain controller and created a new forest.

| Setting | Value |
|---|---|
| Domain Name | adlab.test |
| NetBIOS Name | ADLAB |
| Domain Controller | DC01 |
| Forest Functional Level | Windows Server 2016 |
| Domain Functional Level | Windows Server 2016 |
| DNS Server | Installed |
| Global Catalog | Enabled |

Joined PC01 to the domain and tested domain user authentication.

**Result: PASSED**

[View Active Directory Configuration](02-Active-Directory%20Configuration/README.md)

### 6.2 User and Security Group Management

Created three departmental Organisational Units:

- Sales
- IT
- Staff

Created individual domain user accounts and organised them into their respective departments.

Configured departmental security groups:

| Department | Security Group |
|---|---|
| Sales | Sales-Team |
| IT | IT-Team |
| Staff | Staff-Team |

Assigned users to the corresponding groups.

Verified group membership using Active Directory PowerShell commands.

**Result: PASSED**

### 6.3 DNS and DHCP Configuration

#### DNS

Configured DNS on DC01 to support Active Directory domain resolution.

Verified DNS name resolution using PowerShell.

```powershell
Resolve-DnsName -Name adlab.test -Server 192.168.50.10 -DnsOnly
```

**Result: PASSED**

#### DHCP

Installed and authorised the DHCP Server role.

Created and activated the `ADLab-Client-Scope` DHCP scope.

| Setting | Configuration |
|---|---|
| Scope Start | 192.168.50.100 |
| Scope End | 192.168.50.200 |
| Subnet Mask | 255.255.255.0 |
| DNS Server | 192.168.50.10 |
| DNS Domain | adlab.test |
| Lease Duration | 8 days |

Configured PC01 to obtain an IPv4 address and DNS settings automatically.

#### DHCP Client Verification

Executed `ipconfig /all` on PC01.

| Verification | Observed Result |
|---|---|
| DHCP Enabled | Yes |
| IPv4 Address | 192.168.50.100 |
| DHCP Server | 192.168.50.10 |
| DNS Server | 192.168.50.10 |
| DNS Suffix | adlab.test |
| Lease Obtained | 5 November 2026 |
| Lease Expires | 13 November 2026 |

The assigned address falls within the configured DHCP scope.

**Result: PASSED**

[View DHCP Verification Evidence](03-Network-Services/PC01%20DHCP%20configuration.png)

[View Network Services Documentation](03-Network-Services/README.md)

### 6.4 File Server and Access Control

Created departmental folders on DC01 and configured SMB network shares.

| Department | Network Share | Security Group |
|---|---|---|
| Sales | `\\DC01\Sales` | Sales-Team |
| IT | `\\DC01\IT` | IT-Team |
| Staff | `\\DC01\Staff` | Staff-Team |

Configured NTFS and SMB share permissions using departmental security groups.

Tested access from PC01 using domain accounts.

Verified successful access to authorised departmental resources and denial of unauthorised access.

For example, a Sales user accessed the Sales share and received an access-denied response when attempting to access the IT share.

**Result: PASSED**

[View File Server and Permissions](04-File-Server-and-Permissions/README.md)

### 6.5 Group Policy Management

#### Domain Password Policy

Configured password settings through the Default Domain Policy.

| Setting | Configuration |
|---|---|
| Minimum Password Length | 12 characters |
| Password Complexity | Enabled |
| Password History | 5 passwords |
| Maximum Password Age | 90 days |
| Minimum Password Age | 1 day |
| Reversible Encryption | Disabled |

#### Departmental Network Drive Mapping

Created three Group Policy Objects to configure departmental network drives.

| Department | GPO | Drive Letter | Network Path |
|---|---|---|---|
| Sales | Sales-Mapped-Drive | S: | `\\DC01\Sales` |
| IT | IT-Mapped-Drive | I: | `\\DC01\IT` |
| Staff | Staff-Mapped-Drive | T: | `\\DC01\Staff` |

#### Drive Mapping Verification

Tested the network drive mappings using separate domain accounts for Sales, IT and Staff on PC01.

Used the following commands to verify user identity, refresh Group Policy, inspect mapped drives and identify applied GPOs.

```cmd
whoami
gpupdate /force
net use
gpresult /r /scope user
```

### Verification Results

| Department | Expected Drive | Verified Drive | Status |
|---|---|---|---|
| Sales | S: | S: | Passed |
| IT | I: | I: | Passed |
| Staff | T: | T: | Passed |

Confirmed each departmental account received its intended network drive without mappings for other departments.

**Result: PASSED**

Supporting screenshots of user verification, mapped drives and Group Policy results are stored in the Group Policy documentation.

[View Group Policy Verification](05-Group-Policy/README.md)

### 6.6 PowerShell Administration

Used PowerShell and Windows command-line tools for configuration and verification.

#### Active Directory Group Membership

```powershell
Get-ADGroupMember -Identity "Sales-Team"
Get-ADGroupMember -Identity "IT-Team"
Get-ADGroupMember -Identity "Staff-Team"
```

Additional administrative tasks included:

- Managing group membership using `Add-ADGroupMember`.
- Creating SMB shares using `New-SmbShare`.
- Inspecting share permissions using `Get-SmbShareAccess`.
- Configuring NTFS permissions using `icacls`.
- Testing DNS resolution using `Resolve-DnsName`.
- Checking network settings using `ipconfig`.
- Inspecting mapped drives using `net use`.
- Verifying Group Policy application using `gpresult`.

These tasks provided practical experience in command-line administration and system verification.

[View PowerShell Administration](06-PowerShell/README.md)

## 7. Testing and Verification

Testing was performed using DC01 and the domain-joined PC01 workstation.

| Test | Observed Result | Status | Evidence |
|---|---|---|---|
| AD DS Deployment | DC01 promoted to domain controller | Passed | [AD Configuration](02-Active-Directory%20Configuration/README.md) |
| Domain Join | PC01 joined `adlab.test` | Passed | [Domain Join](02-Active-Directory%20Configuration/Joined%20PC01%20to%20adlab.test%20domain.png) |
| DNS Resolution | Domain name resolved through DC01 | Passed | [DNS Test](03-Network-Services/DNS%20Resolution%20Test.png) |
| Domain Authentication | Domain user signed in to PC01 | Passed | [AD Verification](02-Active-Directory%20Configuration/README.md) |
| User and Group Management | Accounts and group membership verified | Passed | [AD Configuration](02-Active-Directory%20Configuration/README.md) |
| DHCP Client Allocation | PC01 received `192.168.50.100` from DC01 | Passed | [DHCP Test](03-Network-Services/PC01%20DHCP%20configuration.png) |
| SMB Connectivity | Shared folders accessed from PC01 | Passed | [File Server](04-File-Server-and-Permissions/README.md) |
| Authorised File Access | Sales user accessed Sales share | Passed | [Permissions](04-File-Server-and-Permissions/README.md) |
| Restricted File Access | Sales user denied access to IT share | Passed | [Permissions](04-File-Server-and-Permissions/README.md) |
| Network Drive Mapping | Departmental drives mapped successfully | Passed | [GPO Evidence](05-Group-Policy/README.md) |
| Departmental GPO Targeting | Sales, IT and Staff received their respective drives | Passed | [GPO Verification](05-Group-Policy/README.md) |

All listed core implementation tests passed.

## 8. Troubleshooting and Problem Solving

Several configuration issues were encountered during deployment.

### 8.1 DNS Lookup and Reverse Resolution

During DNS troubleshooting, `nslookup` displayed `Server: UnKnown` and lookup timeout messages.

Investigated DNS configuration and created a reverse lookup zone and PTR record.

The PTR record supports reverse name resolution from the DNS server's IPv4 address to its hostname.

This exercise reinforced the distinction between forward DNS resolution, reverse DNS resolution and DNS communication failures.

### 8.2 Windows Installation and Edition Issues

Encountered Windows installation media and edition compatibility problems during virtual machine setup.

Windows 10 Home does not support joining a traditional Active Directory domain.

Used Windows 10 Pro for the domain-joined client workstation.

### 8.3 Windows Server Installation Boot Loop

The server repeatedly returned to the Windows installation screen.

**Resolution:** Removed the installation ISO and changed the virtual machine boot order to prioritise the virtual hard disk.

### 8.4 Virtual Network Connectivity

Initial network adapter settings prevented reliable communication between DC01 and PC01.

**Resolution:** Configured both virtual machines to use the `ADLabNet` Internal Network and corrected IP addressing and DNS settings.

### 8.5 SMB Share Permission Error

An SMB share configuration command returned Windows error 1332 because an account name could not be resolved.

**Resolution:** Corrected the administrative security principal used when assigning share permissions.

### 8.6 Departmental Drive Mapping

An initial Sales user test showed mapped drives for all three departments.

Investigated Group Policy targeting and performed further testing using Sales, IT and Staff domain accounts.

Final verification confirmed each department received its intended network drive.

**Result: RESOLVED**

[View Troubleshooting Documentation](07-Troubleshooting/README.md)

## 9. Skills Demonstrated

- Windows Server installation and administration.
- Active Directory Domain Services deployment.
- Domain user and security group management.
- Windows client domain integration.
- IPv4 addressing and subnet configuration.
- DNS and DHCP administration.
- NTFS and SMB access control.
- Group Policy configuration and verification.
- PowerShell and command-line administration.
- Troubleshooting Windows and networking issues.
- Technical documentation and evidence collection.

## 10. Key Learning Outcomes

This project provided practical experience deploying, configuring and testing a Windows domain environment.

I developed a stronger understanding of how Active Directory, DNS, DHCP, user authentication, security groups and file permissions work together.

The DHCP tests demonstrated successful client address allocation from a Windows Server DHCP service.

Departmental access testing demonstrated how NTFS and SMB permissions restrict access to shared resources.

Group Policy testing demonstrated how domain policies automate client configuration and how applied policies are verified using command-line tools.

Troubleshooting reinforced the importance of identifying symptoms, investigating configuration dependencies, applying corrective actions and retesting.

## 11. Future Improvements

Potential extensions to the environment include:

- Automating domain user provisioning with PowerShell.
- Adding additional Windows client workstations.
- Implementing and testing server backup and recovery.
- Expanding the environment with additional network segments.
- Exploring hybrid identity and Microsoft Azure integration.


## Project Status

**Core implementation and verification completed.**

The lab demonstrates a functioning Active Directory domain with verified authentication, DNS, DHCP, departmental file permissions and network drive mapping.

The repository contains supporting configuration records, screenshots and troubleshooting documentation.
