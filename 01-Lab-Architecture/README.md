# Lab Architecture

## Overview

Designed and built a virtual small business IT environment using Oracle VirtualBox, Windows Server 2022 and Windows 10 Pro.

The lab consists of two virtual machines connected through an isolated internal network.

The purpose was to create a controlled environment for practising Windows Server administration, Active Directory, networking, user management and client support.

The architecture supports practical testing of domain authentication, departmental access permissions, Group Policy and network services.

## 1. Business Requirements

The simulated business requires an IT environment that supports:

- Centralised employee account management.
- Domain authentication for Windows workstations.
- Departmental access to shared files.
- Consistent security policies.
- Automatic client IP address allocation.
- Centralised DNS name resolution.

A server-client architecture was selected to reproduce common administrative tasks in a small Windows domain environment.

## 2. Virtual Machine Architecture

Two virtual machines were created in Oracle VirtualBox.

| Component | DC01 | PC01 |
|---|---|---|
| Operating system | Windows Server 2022 Standard Evaluation | Windows 10 Pro |
| Hostname | DC01 | PC01 |
| Primary role | Domain Controller | Domain-Joined Workstation |
| Network mode | Internal Network | Internal Network |
| Network name | ADLabNet | ADLabNet |
| Domain | adlab.test | adlab.test |

### DC01: Domain Controller

DC01 provides centralised services for the environment:

- Active Directory Domain Services (AD DS)
- Domain Name System (DNS)
- Dynamic Host Configuration Protocol (DHCP)
- Departmental SMB file shares
- Group Policy administration

### PC01: Client Workstation

PC01 represents a business workstation.

It is used to test:

- Domain joining and user authentication.
- Client network configuration.
- Access to departmental shared folders.
- Network drive mapping through Group Policy.
- Domain user permissions.

Separating the server and client into different virtual machines allows configuration changes to be tested from both administrative and end-user perspectives.

## 3. Network Topology

The virtual machines communicate through a VirtualBox Internal Network named `ADLabNet`.


                  Oracle VirtualBox
                 Internal Network
                     ADLabNet
                 192.168.50.0/24
                         |
             +-----------+-----------+
             |                       |
        +---------+             +---------+
        |  DC01   |             |  PC01   |
        | Server  |             | Client  |
        +---------+             +---------+
             |                       |
       192.168.50.10            DHCP Client
          Static
             |
       AD DS / DNS / DHCP
       SMB File Sharing
       Group Policy


Both virtual machines belong to the same IPv4 subnet.

The internal network allows communication between the VMs without directly connecting them to the physical home network.

No router or default gateway was configured for ADLabNet. Consequently, the internal network does not provide external routing or internet connectivity.

### Why Internal Network?

VirtualBox provides several network attachment modes.

| Network mode | Behaviour | Reason for selection |
|---|---|---|
| NAT | Provides outbound connectivity through VirtualBox NAT | Not selected for the final isolated lab network |
| Bridged Adapter | Connects the VM to the physical network | Not required and would expose the lab to the local network |
| Internal Network | Connects VMs sharing the same named internal network | Selected to isolate the server-client environment |

Internal Network was appropriate because the project focuses on local domain administration rather than internet connectivity.

Detailed network configuration is documented in [Network Services](../03-Network-Services/README.md).

## 4. Domain Architecture

Configured DC01 as the domain controller for a new Active Directory forest.

| Setting | Configuration |
|---|---|
| Domain name | adlab.test |
| NetBIOS name | ADLAB |
| Domain controller | DC01 |
| Client workstation | PC01 |
| DNS server | DC01 |
| Forest functional level | Windows Server 2016 |
| Domain functional level | Windows Server 2016 |

The domain provides a centralised identity management system for the simulated business.

Three departmental Organisational Units were created:

- Sales
- IT
- Staff

Departmental security groups support access management for shared resources.

The Active Directory implementation is documented in [Active Directory Configuration](../02-Active-Directory%20Configuration/README.md).

## 5. IP Addressing Design

The lab uses the private IPv4 network `192.168.50.0/24`.

| Device | IPv4 configuration | Address |
|---|---|---|
| DC01 | Static | 192.168.50.10 |
| PC01 | DHCP | Assigned from the configured DHCP scope |

### Network Settings

| Setting | Value |
|---|---|
| Network address | 192.168.50.0 |
| Prefix length | /24 |
| Subnet mask | 255.255.255.0 |
| DNS server | 192.168.50.10 |
| Default gateway | Not configured |
| DHCP allocation range | 192.168.50.100–192.168.50.200 |

### Addressing Decisions

DC01 uses a static IP address because domain clients require a consistent destination for DNS and directory services.

The DHCP allocation range begins at `192.168.50.100`, leaving lower addresses available for statically configured infrastructure.

PC01 was initially configured with a static address during domain setup. The client was later changed to automatic IPv4 and DNS configuration to test the DHCP service.

The DHCP scope was configured and activated on DC01. Final client lease verification is documented separately in the Network Services section.

## 6. Architecture Design Decisions

### Separate Server and Client

Using two virtual machines allows server configuration and client behaviour to be tested independently.

This supports troubleshooting scenarios such as domain authentication failures, DNS misconfiguration and permission errors.

### Centralised Identity Management

Active Directory was selected to manage user accounts, computers and security groups from a central location.

This reduces the need to maintain separate local accounts on individual workstations.

### Isolated Networking

The Internal Network configuration separates the lab from the physical home network.

This provides a controlled environment for testing network settings and administrative changes.

### Group-Based Resource Access

Departmental security groups were selected to simplify access management.

Permissions are assigned to groups rather than maintained separately for every employee account.

The implementation is documented in [File Server and Permissions](../04-File-Server-and-Permissions/README.md).

## 7. Screenshots and Evidence

The following screenshots document the virtual machines and network architecture.

| Evidence | Description |
|---|---|
| [VirtualBox Overview](VMBox%20Overview%20.png) | VirtualBox overview of the lab environment |
| [DC01 System Configuration](DC01%20Sys%20config.png) | Domain controller system configuration |
| [DC01 Additional Configuration](DC01%20Sys%20config%20%282%29.png) | Additional DC01 system configuration |
| [DC01 Network Settings](DCO1%20Net%20settings%20.png) | Server network adapter settings |
| [DC01 IP Configuration](Dc01%20Ip%20config.png) | Domain controller IPv4 configuration |
| [PC01 System Configuration](PC01%28Client%29%20Sys%20Config.png) | Windows 10 client system configuration |
| [PC01 Additional Configuration](PC01%20Sys%20config%282%29.png) | Additional PC01 system configuration |
| [PC01 Network Configuration](PC01%20net%20config.png) | Client network configuration |

Screenshots reflect different stages of the lab setup. Some early configurations were subsequently changed during troubleshooting.

## 8. Outcome

Established a virtual server-client architecture using Windows Server 2022 and Windows 10 Pro.

The environment provides the foundation for Active Directory administration, DNS and DHCP services, departmental file sharing, Group Policy and client-side testing.

The isolated network also supports controlled troubleshooting without affecting the physical home network.

This architecture serves as the foundation for the remaining technical sections of the project.
