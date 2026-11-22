# Active Directory Small Business IT Lab

## 1. Project Summary

Designed, deployed and tested a virtual Windows domain environment simulating the IT infrastructure of a small business.

Using Windows Server 2022 and Windows 10 Pro, I implemented centralised identity management, DNS, DHCP, departmental file sharing, Group Policy and client-side authentication.

### Key Achievements

- Deployed the `adlab.test` Active Directory domain and joined a Windows 10 workstation.
- Created departmental Organisational Units (OUs), security groups and user accounts.
- Configured DNS and DHCP services, verifying domain resolution and client IP allocation.
- Implemented SMB network shares with departmental access permissions.
- Configured domain password policies and automated network drive mapping through Group Policy.
- Investigated six documented technical incidents, including incorrect GPO targeting, NTFS permission inheritance and DNS lookup issues.

**Technologies:** Windows Server 2022, Windows 10 Pro, Oracle VirtualBox, Active Directory, DNS, DHCP, Group Policy, PowerShell, NTFS and SMB.

## 2. Project Walkthrough and Network Architecture

The project simulates an organisation with three departments: Sales, IT and Staff.

DC01 provides centralised authentication, network services and shared resources, while PC01 serves as a domain-joined workstation for testing configurations and access permissions.

### Network Diagram

The diagram illustrates the virtual infrastructure, network configuration and relationship between the domain controller and client workstation.

![Active Directory Home Lab Network Diagram](https://github.com/user-attachments/assets/583f4d0b-b876-4301-8512-702820decc57)

### Infrastructure

| Component | Configuration |
|---|---|
| Virtualisation | Oracle VirtualBox |
| Domain Controller | DC01, Windows Server 2022 Standard Evaluation |
| Client Workstation | PC01, Windows 10 Pro |
| Active Directory Domain | adlab.test |
| NetBIOS Name | ADLAB |
| Network | ADLabNet, VirtualBox Internal Network |
| Subnet | 192.168.50.0/24 |
| DC01 IP Address | 192.168.50.10, Static |
| PC01 IP Address | DHCP |
| DHCP Scope | 192.168.50.100–192.168.50.200 |
| DNS Server | 192.168.50.10 |

The isolated VirtualBox network allows the environment to operate independently of the physical home network.

[View Detailed Lab Architecture](01-Lab-Architecture/README.md)

## 3. Implementation and Verification

Configured and tested the infrastructure using Windows administrative tools, PowerShell and domain accounts on PC01.

### Core Infrastructure Results

| Component | Implementation and Verified Result | Status | Evidence |
|---|---|---|---|
| Active Directory | DC01 deployed as domain controller for `adlab.test` | Passed | [AD Configuration](02-Active-Directory%20Configuration/README.md) |
| Domain Join | PC01 successfully joined the domain | Passed | [Domain Join](02-Active-Directory%20Configuration/README.md) |
| User Management | Departmental OUs, accounts and security group memberships configured | Passed | [AD Management](02-Active-Directory%20Configuration/README.md) |
| DNS | `adlab.test` resolved to `192.168.50.10` through DC01 | Passed, forward lookup | [DNS Verification](03-Network-Services/DNS%20Resolution%20Test.png) |
| DHCP | PC01 received `192.168.50.100` from DC01 | Passed | [DHCP Verification](03-Network-Services/PC01%20DHCP%20configuration.png) |
| SMB File Sharing | Sales, IT and Staff network shares created and permissions verified | Passed | [File Server](04-File-Server-and-Permissions/README.md) |
| Access Control | Authorised share access and cross-department access restrictions tested | Passed for documented tests | [Permissions](04-File-Server-and-Permissions/README.md) |
| Password Policy | Domain password settings configured and inspected in Group Policy Management | Configuration verified | [Group Policy](05-Group-Policy/README.md) |
| Drive Mapping | Departmental network drives mapped and verified on PC01 | Passed | [Drive Verification](05-Group-Policy/README.md) |

### Departmental Network Drive Mapping

Configured three Group Policy Objects (GPOs) linked to their respective departmental OUs.

| Department | Security Group | GPO | Verified Drive |
|---|---|---|---|
| Sales | Sales-Team | Sales-Mapped-Drive | S: → `\\DC01\Sales` |
| IT | IT-Team | IT-Mapped-Drive | I: → `\\DC01\IT` |
| Staff | Staff-Team | Staff-Mapped-Drive | T: → `\\DC01\Staff` |

Verified the mappings using `whoami`, `net use` and `gpresult /r /scope user`.

Final testing confirmed each departmental account received its intended GPO and network drive, without mappings for other departments.

[View Departmental Drive Mapping Evidence](05-Group-Policy/README.md)

## 4. Troubleshooting Highlights

Troubleshooting formed an important part of this project. Six technical incidents were documented across installation, networking, DNS, SMB permissions, NTFS inheritance and Group Policy.

### Significant Issues and Resolutions

| Issue | Identified Cause | Resolution |
|---|---|---|
| Incorrect GPO Targeting | Departmental GPOs were linked at the domain level, causing multiple drives to appear for individual users | Removed unnecessary domain-level links, confirmed departmental OU links and cleared persistent network mappings |
| NTFS Permission Inheritance | An inheritable IT-Team Modify entry on `C:\Company Shares` propagated to Sales and Staff | Removed the unintended parent permission and verified the resulting departmental ACLs |
| SMB Error 1332 | An administrative account reference could not be resolved to a security identifier | Corrected the security principal to `BUILTIN\Administrators` and verified the SMB share permissions |

### Group Policy: Before and After

**Before correction:** The Sales account received all three mapped drives, S:, I: and T:.

![Incorrect Network Drive Mapping](05-Group-Policy/Mapped-Drive-Network-Verification%282%29.png)

**After correction:** The Sales account received only its assigned S: drive.

[View Corrected Sales Drive](05-Group-Policy/Sales-Drive-Verification.png) | [View Applied GPO Results](05-Group-Policy/Sales-GPO-Results.png)

This investigation demonstrated the importance of checking GPO scope, distinguishing existing network connections from current policy application and verifying changes through client-side testing.

[View Full Troubleshooting Log](07-Troubleshooting/README.md)

## 5. Project Documentation

Detailed implementation procedures, configuration screenshots, verification results and troubleshooting records are organised by technical area.

| Section | Documentation |
|---|---|
| 01 | [Lab Architecture](01-Lab-Architecture/README.md) |
| 02 | [Active Directory Configuration](02-Active-Directory%20Configuration/README.md) |
| 03 | [Network Services: DNS and DHCP](03-Network-Services/README.md) |
| 04 | [File Server and Permissions](04-File-Server-and-Permissions/README.md) |
| 05 | [Group Policy Management](05-Group-Policy/README.md) |
| 06 | [PowerShell Administration](06-PowerShell/README.md) |
| 07 | [Troubleshooting Log](07-Troubleshooting/README.md) |

PowerShell and Windows command-line utilities were used for manual configuration, administration, diagnostics and verification. Automation scripting is planned as a separate future extension.

## 6.  Future Improvements


### Future Improvements

- Develop PowerShell automation for Active Directory user provisioning and group membership management.
- Extend NTFS permission hardening across all departmental folders.
- Implement and test server backup and recovery.
- Add further client workstations and network segments.
- Explore hybrid identity integration with Microsoft Azure.

## 7. Project Status

**Version 1.0: Core implementation and documented verification completed.**

The project demonstrates practical experience with Windows Server deployment, Active Directory administration, DNS and DHCP, group-based file permissions, Group Policy and technical troubleshooting.

Supporting evidence and detailed implementation records are available throughout the repository.
