 Active Directory Configuration

 1. Objective

The objective was to establish a centralised Windows domain environment to manage users, computers and departmental access.

Windows Server 2022 was configured as the domain controller for `adlab.test`.

 2. Active Directory Domain Services Installation

Installed the Active Directory Domain Services (AD DS) server role through Server Manager.

The server was then promoted to a domain controller by creating a new forest.

Configuration:
- Server hostname: DC01
- Domain name: adlab.test
- NetBIOS name: ADLAB
- Forest functional level: Windows Server 2016
- Domain functional level: Windows Server 2016
- DNS Server: Installed
- Global Catalog: Enabled

 Purpose

Active Directory Domain Services provides centralised authentication and management of domain users, computers and security groups.

DNS supports domain name resolution and helps domain-connected devices locate services such as domain controllers.

 3. Organisational Unit Structure

Created departmental Organisational Units (OUs) to organise users within the domain.

Departments:
- Sales
- IT
- Staff

 Purpose

OUs provide a logical structure for organising directory objects and applying Group Policy settings to specific groups of users or computers.

 4. User Account Management

Created individual domain user accounts for the three departments.

Users were placed within their corresponding departmental OUs.

Purpose

Individual accounts provide employees with their own domain credentials and allow administrators to manage access and account settings centrally.

 5. Security Group Management

Created departmental security groups:

- Sales-Team
- IT-Team
- Staff-Team

Assigned users to their corresponding departmental groups.

 Purpose

Security groups simplify permission management. Administrators can assign access to a group rather than configure permissions separately for every user.

 6. Verification

The following checks were performed:

- Confirmed the `adlab.test` domain was created.
- Confirmed departmental OUs and user accounts appeared in Active Directory Users and Computers.
- Checked departmental group membership using PowerShell.
- Confirmed PC01 successfully joined the domain.

7. Screenshots and Evidence

Evidence to include:

- AD DS role installation.
- Domain controller promotion configuration.
- Active Directory domain overview.
- Departmental OU structure.
- Departmental user accounts.
- Security groups and group membership.
- PC01 domain join confirmation.
