# Active Directory Configuration

## Overview

Deployed Active Directory Domain Services (AD DS) on Windows Server 2022 to simulate centralised identity and access management in a small business environment.

The domain controller, DC01, manages domain users, computers and departmental security groups within the `adlab.test` domain.

A Windows 10 Pro workstation, PC01, was joined to the domain to test authentication and access to domain resources.

## 1. Business Scenario and Objectives

The simulated organisation consists of three departments:

- Sales
- IT
- Staff

The business requires a centralised system for managing employee accounts and controlling access to departmental resources.

### Project Objectives

- Install and configure Active Directory Domain Services.
- Promote Windows Server 2022 to a domain controller.
- Create an Active Directory forest and domain.
- Organise employee accounts into departmental Organisational Units.
- Create and manage domain user accounts.
- Configure departmental security groups.
- Verify security group membership using PowerShell.
- Join a Windows 10 workstation to the domain.
- Test domain connectivity and authentication.

## 2. Domain Controller Deployment

### 2.1 AD DS Installation

Installed the Active Directory Domain Services server role through Windows Server Manager.

Following installation, promoted DC01 to a domain controller and created a new Active Directory forest.

### 2.2 Domain Configuration

| Setting | Configuration |
|---|---|
| Server hostname | DC01 |
| Operating system | Windows Server 2022 Standard Evaluation |
| Domain name | adlab.test |
| NetBIOS name | ADLAB |
| Forest type | New forest |
| Forest functional level | Windows Server 2016 |
| Domain functional level | Windows Server 2016 |
| DNS Server | Installed |
| Global Catalog | Enabled |
| Read-only Domain Controller | No |

After promotion, DC01 restarted and operated as the domain controller for `adlab.test`.

### 2.3 Technical Purpose

AD DS provides centralised directory services, including authentication and management of domain objects.

DNS supports domain operations by resolving domain names and helping clients locate domain controllers.

DC01 was assigned the static IPv4 address `192.168.50.10` to provide a consistent address for domain and DNS services.

### Evidence

[View Initial AD DS Server Configuration](Screenshot%202026-10-12%20223054.png)

This screenshot documents an earlier stage of the server configuration. The final network configuration is documented in [Network Services](../03-Network-Services/README.md).

## 3. Organisational Unit Structure

Created three departmental Organisational Units within the `adlab.test` domain.

### OU Structure

```text
adlab.test
|
+-- Sales
|
+-- IT
|
+-- Staff
```

Each OU provides a logical location for organising departmental directory objects.

### Design Decisions

Organisational Units were used to:

- Separate users by department.
- Simplify administrative management.
- Provide a structure for applying department-specific Group Policy settings.
- Support future expansion of the directory.

OUs organise directory objects and support administrative delegation and Group Policy targeting. They do not independently grant access to shared folders.

Departmental security groups were configured separately for access management.

## 4. Domain User Account Management

Created individual domain accounts for employees across the three departments.

### Example User Accounts

| Department | Example account |
|---|---|
| Sales | ASmith |
| Sales | APatel |
| Sales | JReynolds |
| Sales | OSmall |
| IT | MJones |
| Staff | DBrown |
| Staff | LWilliams |

These accounts represent employees in the simulated organisation.

Users were organised into their corresponding departmental OUs.

### Purpose

Individual domain accounts allow administrators to:

- Manage user identities centrally.
- Control account access and credentials.
- Apply domain security policies.
- Assign access through security group membership.
- Support authentication from domain-joined workstations.

### Verification

Confirmed user accounts appeared in Active Directory Users and Computers.

### Evidence

[View Domain User Account Creation](Confirming%20users%20has%20been%20added%20%283%29.png)

[View Domain User Verification](Confirmed%20User%20to%20be%20in%20Domain.png)

## 5. Security Group Configuration

Created departmental security groups to manage access to business resources.

| Security group | Department | Intended access |
|---|---|---|
| Sales-Team | Sales | Sales shared folder |
| IT-Team | IT | IT shared folder |
| Staff-Team | Staff | Staff shared folder |

Assigned domain users to their corresponding departmental groups.

### Why Security Groups?

Group-based access control simplifies permission administration.

Instead of assigning folder permissions individually to every user, permissions are granted to a security group.

When employees join or leave a department, administrators update group membership rather than reconfigure each shared resource.

### PowerShell Verification

Used PowerShell to inspect departmental security group membership.

```powershell
Get-ADGroupMember -Identity "Sales-Team"
Get-ADGroupMember -Identity "IT-Team"
Get-ADGroupMember -Identity "Staff-Team"
```

These commands retrieve the members of each security group from Active Directory.

Group membership was also managed using the `Add-ADGroupMember` cmdlet.

### Evidence

[View User Assignment to Security Groups](adding%20each%20users%20to%20security%20group.png)

[View Security Group Membership Verification](confirming%20users%20being%20added%20to%20SG%282%29.png)

Departmental folder permissions and access testing are documented in [File Server and Permissions](../04-File-Server-and-Permissions/README.md).

## 6. Windows 10 Domain Integration

### Objective

Join PC01, a Windows 10 Pro virtual machine, to the `adlab.test` domain.

This allows the workstation to authenticate domain users and access centrally managed resources.

### Configuration

PC01 was configured to communicate with DC01 through the VirtualBox Internal Network `ADLabNet`.

The client used DC01 (`192.168.50.10`) as its DNS server.

The workstation was then joined to the domain using Windows system settings.

### Verification

- PC01 successfully joined `adlab.test`.
- The workstation was recognised as a domain-connected computer.
- Domain user authentication was tested on PC01.

### Technical Importance

Successful domain joining requires the client to communicate with the domain controller and locate the appropriate Active Directory services.

Correct DNS configuration is essential because Windows domain clients rely on DNS to locate domain controllers.

### Evidence

[View PC01 Domain Join Configuration](Adding%20PC01%20to%20domain%28%20adlab.test%29.png)

[View Successful Domain Join](Joined%20PC01%20to%20adlab.test%20domain.png)

## 7. Verification Summary

The following checks were completed during Active Directory configuration.

| Test | Result |
|---|---|
| AD DS role installation | Successful |
| New Active Directory forest creation | Successful |
| Domain controller promotion | Successful |
| Departmental OU creation | Successful |
| Domain user account creation | Successful |
| Security group creation | Successful |
| Security group membership verification | Successful |
| PC01 domain join | Successful |
| Domain user authentication on PC01 | Successful |

These results establish the core identity and authentication components required for the simulated business environment.

## 8. Troubleshooting and Lessons Learned

### 8.1 Windows Server Installation Boot Loop

During the initial Windows Server installation, the virtual machine repeatedly returned to the installation screen.

**Cause:** The installation ISO remained attached and the VM boot configuration prioritised the optical drive.

**Resolution:** Removed the installation ISO and configured the virtual machine to boot from its virtual hard disk.

**Lesson learned:** Virtual machine boot order and attached installation media should be checked when an operating system repeatedly restarts its installer.

### 8.2 Domain Connectivity and DNS Configuration

During the lab setup, network adapter and IP addressing configurations required adjustment to establish communication between DC01 and PC01.

**Resolution:** Configured both virtual machines to use the same Internal Network and assigned the client the domain controller's DNS address.

**Lesson learned:** Domain joining depends on correct network connectivity and DNS configuration.

Additional network troubleshooting is documented in [Network Services](../03-Network-Services/README.md).

## 9. Outcome

Established an Active Directory domain using Windows Server 2022 and integrated a Windows 10 Pro client workstation.

The environment supports:

- Centralised domain authentication.
- Departmental user account organisation.
- Security group-based access management.
- Domain-joined workstation administration.
- Group Policy and departmental resource configuration.

This implementation provides the identity management foundation for the remaining lab components.

### Related Documentation

- [Lab Architecture](../01-Lab-Architecture/README.md)
- [Network Services](../03-Network-Services/README.md)
- [File Server and Permissions](../04-File-Server-and-Permissions/README.md)
- [Group Policy](../05-Group-Policy/README.md)
