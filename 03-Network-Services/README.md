 Network Services Configuration

 Overview

Configured network services for a simulated small business environment using Windows Server 2022 and Windows 10 Pro in Oracle VirtualBox.

The objective was to establish communication between a domain controller (DC01) and a domain-joined client (PC01), using centralised DNS and DHCP services.

 1. Network Topology

| Component | Configuration |
|---|---|
| Virtualisation platform | Oracle VirtualBox |
| Network type | Internal Network |
| Network name | ADLabNet |
| Network subnet | 192.168.50.0/24 |
| Domain controller | DC01 |
| Client computer | PC01 |
| Active Directory domain | adlab.test |

Both virtual machines were connected to the same isolated VirtualBox network.

This arrangement allowed the virtual machines to communicate without relying on the physical home network.

 2. Static IP Configuration

Configured DC01 with a static IPv4 address.

| Setting | Value |
|---|---|
| IPv4 address | 192.168.50.10 |
| Subnet mask | 255.255.255.0 |
| Default gateway | Not configured |
| Preferred DNS server | 192.168.50.10 |

A static address was necessary because DC01 provides DNS and other domain services. Client computers require a consistent address to locate these services.

No default gateway was configured because the lab operates on an isolated internal network without external routing.

 3. DNS Configuration

Installed the DNS Server role alongside Active Directory Domain Services.

DNS provides name resolution and helps domain clients locate Active Directory services.

Configuration:

- DNS server: DC01
- DNS server address: 192.168.50.10
- Domain: adlab.test

 Verification

Tested DNS resolution using PowerShell:

```powershell
Resolve-DnsName -Name adlab.test -Server 192.168.50.10 -DnsOnly
```

The command successfully returned DNS resolution results for the domain.

This confirmed the DNS server responded to the domain lookup.

 4. DHCP Configuration

Configured the DHCP Server role on DC01 to provide automatic IPv4 configuration for client devices.

DHCP reduces manual configuration and helps maintain consistent network settings.

 Configuration

- DHCP server: DC01
- Network: 192.168.50.0/24
- DHCP scope: [Add configured scope name]
- Address allocation range: [Add actual start and end addresses]
- DNS server option: [Verify configured value]
- DNS domain option: [Verify configured value]

PC01 was changed from manual IPv4 configuration to automatic address assignment.

 Verification

The client was configured to obtain its IP address through DHCP.

The following command provides the details needed to verify the lease:

```powershell
ipconfig /all
```

The output should identify the assigned IPv4 address, DHCP server, DNS server and DHCP-enabled status.

The exact lease details will be added after verification.

 5. Client Connectivity and Domain Integration

PC01 was configured as a Windows 10 Pro client on the ADLabNet internal network.

The client initially used a different network configuration, which required correction before domain integration.

After configuring the network and DNS settings, PC01 successfully joined the `adlab.test` domain.

 Verification performed

- Confirmed both virtual machines used the same VirtualBox internal network.
- Verified the domain DNS lookup.
- Successfully joined PC01 to the domain.
- Used domain credentials to sign in to PC01.

These tests provided evidence of basic client-to-domain connectivity and authentication.

 6. Troubleshooting: Incorrect Network Adapter Configuration

 Problem

PC01 initially used a NAT network configuration rather than the intended isolated internal network.

This prevented the client from using the intended lab network configuration.

 Investigation

Reviewed the VirtualBox network adapter settings and the client's IPv4 and DNS configuration.

The client initially had an address on the VirtualBox NAT network (`10.0.2.15`), rather than the intended `192.168.50.0/24` subnet.

 Resolution

Changed the VirtualBox network adapter attachment to Internal Network and selected `ADLabNet`.

Configured the client to use the domain controller's DNS service.

 Result

PC01 successfully joined `adlab.test` after the network configuration was corrected.

 7. Evidence

Screenshots included in this directory document:

- VirtualBox network adapter configuration
- DC01 static IPv4 settings
- DNS server configuration
- DHCP scope configuration
- PC01 network configuration
- DNS resolution test results

 8. Skills Demonstrated

- IPv4 addressing and subnet configuration
- Static and dynamic IP address assignment
- Windows Server DNS and DHCP administration
- VirtualBox internal network configuration
- Windows client network troubleshooting
- PowerShell network diagnostics
- Active Directory client connectivity

 Further Verification

The following details require confirmation before the network documentation is considered complete:

- DHCP scope name and allocation range
- DHCP options configured on DC01
- PC01's assigned DHCP lease
- Evidence of successful DHCP assignment from DC01
