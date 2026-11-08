# Active Directory Configuration

## Overview

Deployed Active Directory Domain Services (AD DS) on Windows Server 2022 to simulate centralised identity and access management within a small business.

The environment includes three departments: Sales, IT and Staff.

The objective was to configure a Windows domain, organise departmental users, create security groups and join a Windows 10 Pro client to the domain.

This configuration provides the identity management foundation for the file permissions and Group Policy configurations documented in later sections.

## 1. Business Scenario

A small business requires a centralised system for managing employee accounts and controlling access to departmental resources.

Without centralised identity management, administrators would need to maintain local user accounts across individual computers.

Active Directory provides a central directory for managing users, computers, authentication and security groups.

The simulated business contains three departments:

- Sales: Employees responsible for sales operations.
- IT: Employees responsible for technical support and administration.
- Staff: General business employees.

Each department requires individual user accounts and group-based access to its resources.

## 2. Domain Controller Deployment

### Installation

Installed the Active Directory Domain Services role through Windows Server Manager.

Promoted DC01 to a domain controller by creating a new Active Directory forest.

### Domain Configuration

| Setting | Configuration |
|---|---|
| Server hostname | DC01 |
| Operating system | Windows Server 2022 |
| Active Directory domain | adlab.test |
| NetBIOS domain name | ADLAB |
| Forest functional level | Windows Server 2016 |
| Domain functional level | Windows Server 2016 |
| DNS Server | Installed |
| Global Catalog | Enabled |
| Read-only domain controller | No |

The forest and domain functional levels were configured as Windows Server 2016, the highest functional levels available in Windows Server 2022.

### Technical Purpose

DC01 provides centralised authentication and directory services.

DNS supports Active Directory by allowing clients to locate domain controllers and associated services.

The Global Catalog supports directory searches across the forest.

### Evidence

[View Initial AD DS Server Configuration](Screenshot%202026-10-12%20223054.png)

This screenshot documents an early stage of the server configuration. The displayed network address reflects the initial setup rather than the final static IP configuration.

## 3. Organisational Unit Structure

Created three departmental Organisational Units (OUs) using Active Directory Users and Computers.

```text
adlab.test
│
├── Sales
│
├── IT
│
└── Staff
```

### Purpose

Organisational Units provide a logical structure for organising Active Directory objects.

They also allow administrators to apply Group Policy settings to specific organisational areas.

For example, departmental drive-mapping policies can be linked to the relevant user OUs rather than applied indiscriminately across the domain.

An OU is an organisational container, not a security boundary or a replacement for a security group.

## 4. Domain User Account Management

Created individual user accounts and organised them within their corresponding departmental OUs.

Examples of configured accounts include:

| Department | User account |
|---|---|
| Sales | ASmith |
| Sales | APatel |
| Sales | JReynolds |
| IT | MJones |
| Staff | DBrown |
| Staff | LWilliams |

These are examples from the lab rather than an exhaustive account inventory.

### Purpose

Individual domain accounts provide employees with unique credentials for authentication.

Centralised management allows administrators to create, disable, reset and organise accounts through Active Directory.

### Verification

Opened Active Directory Users and Computers and confirmed that the created accounts appeared in their departmental OUs.

### Evidence

[View User Account Creation](Confirming%20users%20has%20been%20added%20(3).png)

[View Domain User Verification](Confirmed%20User%20to%20be%20in%20Domain.png)

## 5. Security Group Configuration

Created departmental security groups to support role-based access to shared resources.

| Security group | Department | Intended purpose |
|---|---|---|
| Sales-Team | Sales | Access to Sales resources |
| IT-Team | IT | Access to IT resources |
| Staff-Team | Staff | Access to Staff resources |

Assigned users to the security groups corresponding to their departments.

### Why Security Groups?

Assigning permissions directly to every employee becomes difficult to maintain as the number of users increases.

Group-based access management allows administrators to assign permissions to a security group and manage access through group membership.

For example, a new Sales employee can be added to Sales-Team rather than receiving individually configured permissions on each departmental resource.

Security group membership does not automatically grant access. Permissions must also be configured on the relevant resources.

### Verification Using PowerShell

Used the Active Directory PowerShell module to inspect departmental group membership.

```powershell
Get-ADGroupMember -Identity "Sales-Team"
Get-ADGroupMember -Identity "IT-Team"
Get-ADGroupMember -Identity "Staff-Team"
```

These commands return the directory objects belonging to each security group.

Reviewed the results to confirm that users had been assigned to their intended departmental groups.

### Evidence

[View User Assignment to Security Groups](adding%20each%20users%20to%20security%20group.png)

[View Security Group Membership Verification](confirming%20users%20being%20added%20to%20SG(2).png)

## 6. Windows 10 Domain Integration

### Objective

Join PC01 to the Active Directory domain so the client can authenticate domain accounts and access centrally managed resources.

### Configuration

Configured PC01 as a Windows 10 Pro client connected to the same VirtualBox internal network as DC01.

The client was configured to use the domain controller's DNS service.

Joined PC01 to:

`adlab.test`

Restarted the client to complete the domain join process.

### Verification

Confirmed the domain join and signed in using domain credentials.

Domain user sign-in format:

```text
ADLAB\username
```

This demonstrated that PC01 could authenticate a domain account through the Active Directory environment.

### Evidence

[View Domain Join Configuration](Adding%20PC01%20to%20domain(%20adlab.test).png)

[View Successful Domain Join](Joined%20PC01%20to%20adlab.test%20domain.png)

## 7. Verification Summary

| Test | Result |
|---|---|
| Active Directory domain creation | Successful |
| Domain controller promotion | Successful |
| Departmental OU creation | Successful |
| Domain user account creation | Successful |
| Departmental security group creation | Successful |
| Security group membership checks | Successful |
| PC01 domain join | Successful |
| Domain user authentication on PC01 | Successful |

These tests confirm the core identity management configuration.

Departmental file access and Group Policy behaviour are documented separately because they require additional permission and client-side testing.

## 8. Troubleshooting and Lessons Learned

### Domain Controller Installation

During the initial Windows Server installation, the VM repeatedly returned to the installation environment.

The VirtualBox boot configuration was reviewed, and the installation ISO was removed from the virtual optical drive after Windows Server installation.

The boot order was configured to prioritise the virtual hard disk.

This allowed Windows Server to boot normally.

### Domain Connectivity

The client initially used a network configuration that did not match the intended internal lab network.

The VirtualBox adapter settings and DNS configuration were reviewed and corrected before PC01 successfully joined the domain.

Detailed IP addressing and DNS troubleshooting are documented in [Network Services](../03-Network-Services/README.md).

## 9. Outcome

Successfully established an Active Directory domain using Windows Server 2022.

Created departmental OUs, individual domain user accounts and security groups to support centralised identity management.

Joined a Windows 10 Pro client to the domain and verified domain authentication.

The completed Active Directory configuration provides the foundation for:

- [Departmental File Sharing and Permissions](../04-File-Server-and-Permissions/README.md)
- [Group Policy Configuration](../05-Group-Policy/README.md)

These sections document how directory identities and security groups are used to manage access to business resources.
