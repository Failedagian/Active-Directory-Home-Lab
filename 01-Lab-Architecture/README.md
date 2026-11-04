 Lab Architecture

 1. Overview

The lab was built using Oracle VirtualBox to simulate a small business IT environment.

The environment consists of two virtual machines running on an isolated internal network. Windows Server 2022 acts as the domain controller, while Windows 10 Pro acts as a client workstation connected to the domain.

The purpose of this architecture was to create a controlled environment to practise Windows Server administration, domain management, network configuration and client support.

 2. Virtual Machine Configuration

| Component | DC01 | PC01 |
|---|---|---|
| Operating System | Windows Server 2022 Standard Evaluation | Windows 10 Pro |
| Hostname | DC01 | PC01 |
| Role | Domain Controller | Client Workstation |
| Network Adapter | Internal Network | Internal Network |
| Network | ADLabNet | ADLabNet |

 3. Network Architecture

Both virtual machines use a VirtualBox Internal Network named `ADLabNet`.

This allows the server and client to communicate within an isolated virtual network, separate from the host's physical network.

The lab uses the private IPv4 network `192.168.50.0/24`.

DC01 provides centralised domain services, including Active Directory, DNS and DHCP. PC01 connects to the domain and accesses resources provided by the server.

 4. Domain Information

| Setting | Value |
|---|---|
| Domain | adlab.test |
| NetBIOS Name | ADLAB |
| Domain Controller | DC01 |
| Client Workstation | PC01 |

 5. Network Addressing

| Device | IPv4 Address | Configuration |
|---|---|---|
| DC01 | 192.168.50.10 | Static |
| PC01 | DHCP assigned | Automatic |

Subnet mask: `255.255.255.0`

DNS server: `192.168.50.10`

 6. Architecture Design Decisions

- VirtualBox provided a platform for building and managing separate operating system environments.
- A dedicated server and client allowed domain administration and client-side testing.
- An isolated internal network allowed both machines to communicate without exposing the lab directly to the physical network.
- A dedicated domain controller provided centralised authentication and management.

 7. Screenshots and Evidence

The following screenshots demonstrate the lab architecture:

- VirtualBox overview showing DC01 and PC01.
- DC01 virtual machine configuration.
- PC01 virtual machine configuration.
- Internal Network adapter configuration for both machines.
- Running server and client virtual machines.
