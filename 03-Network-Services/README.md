 Network Services Configuration

 Project Overview

Configured network services for a simulated small business environment using Windows Server 2022 and Windows 10 Pro in Oracle VirtualBox.

The environment consists of a domain controller (DC01) and a Windows 10 client (PC01), connected through an isolated virtual network.

The project focuses on three areas:

- IPv4 addressing and network connectivity
- DNS configuration and domain name resolution
- DHCP installation, scope management and automatic IP allocation

 1. Network Architecture

| Component | Configuration |
|---|---|
| Virtualisation | Oracle VirtualBox |
| Network mode | Internal Network |
| Network name | ADLabNet |
| Network address | 192.168.50.0/24 |
| Domain | adlab.test |
| Domain controller | DC01, Windows Server 2022 |
| Client | PC01, Windows 10 Pro |

Both virtual machines use the same internal network, allowing communication without connecting the lab directly to the physical home network.

No router or default gateway was configured.

## 2. IP Addressing

### Domain Controller: DC01

Configured DC01 with a static IPv4 address.

| Setting | Value |
|---|---|
| IPv4 address | 192.168.50.10 |
| Subnet mask | 255.255.255.0 |
| Default gateway | None |
| Preferred DNS | 192.168.50.10 |

The domain controller requires a consistent IP address because clients depend on its DNS and Active Directory services.

The static address also falls outside the DHCP allocation range, reducing the risk of address conflicts.

### Client: PC01

PC01 initially used the manually configured address `192.168.50.20`.

After installing DHCP, the client's Ethernet adapter was changed to obtain IPv4 and DNS settings automatically.

The DHCP lease details require final verification.

## 3. DNS Configuration

### Implementation

Installed the DNS Server role alongside Active Directory Domain Services on DC01.

Configured the Active Directory DNS zone:

`adlab.test`

DC01 uses `192.168.50.10` as its DNS server address.

DNS supports domain operations by resolving hostnames and allowing clients to locate Active Directory services.

### Testing

Executed the following command in PowerShell:

```powershell
Resolve-DnsName -Name adlab.test -Server 192.168.50.10 -DnsOnly
```

**Result:** The DNS server successfully returned records for `adlab.test`.

This confirmed basic domain DNS resolution.

The test did not independently verify every DNS record or Active Directory service.

## 4. DHCP Server Configuration

### Installation

The DHCP management console was initially unavailable in Server Manager.

Inspection of the installed server roles confirmed that DHCP Server had not been installed.

Installed the DHCP Server role on DC01 and completed the post-installation configuration.

The wizard confirmed:

- DHCP security groups created successfully.
- DHCP server authorised successfully in Active Directory.

### DHCP Scope

Created and activated the following IPv4 scope:

| Setting | Value |
|---|---|
| Scope name | ADLab-Client-Scope |
| Network | 192.168.50.0/24 |
| Start address | 192.168.50.100 |
| End address | 192.168.50.200 |
| Subnet mask | 255.255.255.0 |
| Lease duration | 8 days |
| Default gateway | None |
| DNS server entered | 192.168.50.10 |
| Parent DNS domain entered | adlab.test |
| WINS | Not configured |
| Scope status | Active |

The allocation pool contains 101 IPv4 addresses.

The range starts at `.100`, leaving lower addresses available for infrastructure requiring static configuration.

No exclusions were added because DC01's static address, `192.168.50.10`, is outside the DHCP pool.

### Server-Side Verification

Opened DHCP Manager and inspected the configured scope.

Confirmed:

- The scope was active.
- The address pool started at `192.168.50.100`.
- The address pool ended at `192.168.50.200`.

These checks established that the DHCP scope was configured and enabled.

## 5. Client Configuration and Verification

### Initial Configuration

PC01 was originally configured with a static IPv4 address:

`192.168.50.20`

The `ipconfig /all` output showed:

`DHCP Enabled: No`

This indicated that PC01 was not requesting its IP address through DHCP.

### Configuration Change

Opened the Windows 10 Ethernet adapter's IPv4 properties and selected:

- Obtain an IP address automatically.
- Obtain DNS server address automatically.

### Verification Command

```powershell
ipconfig /all
```

The following values must be confirmed from the updated output:

| Field | Expected result |
|---|---|
| DHCP Enabled | Yes |
| IPv4 address | 192.168.50.100–200 |
| DHCP server | 192.168.50.10 |
| DNS server | 192.168.50.10 |

**Status:** Client adapter settings changed. Successful DHCP lease assignment has not yet been independently confirmed in the documented results.

## 6. Troubleshooting

### Issue 1: Incorrect VirtualBox Network Mode

**Symptom:** PC01 initially used a NAT network address (`10.0.2.15`) instead of an address on the intended lab subnet.

**Investigation:** Inspected the VirtualBox network adapter settings and Windows IP configuration.

**Resolution:** Changed the network attachment to Internal Network (`ADLabNet`) and configured the client to use the lab network and DC01's DNS service.

**Verification:** PC01 subsequently joined `adlab.test` successfully.

### Issue 2: DHCP Console Missing

**Symptom:** DHCP was not available under Server Manager → Tools.

**Investigation:** Checked the installed Windows Server roles and found DHCP Server was not installed.

**Resolution:** Installed DHCP Server, completed Active Directory authorisation and created an IPv4 scope.

**Verification:** DHCP Manager became available, and the new scope displayed an Active status.

### Issue 3: PC01 Not Using DHCP

**Symptom:** `ipconfig /all` displayed `DHCP Enabled: No`, with the static address `192.168.50.20`.

**Cause:** The client's Ethernet adapter was configured for manual IPv4 addressing.

**Resolution:** Changed IPv4 and DNS configuration to automatic assignment.

**Verification:** Pending confirmation of the DHCP lease details.

## 7. Configuration Evidence

## 7. Configuration Evidence

| Screenshot | Description |
|---|---|
| [DC01 Static IP](DC01%20static%20IP%20.png) | Domain controller static IPv4 configuration |
| [DC01 Network Settings](DCO1%20Net%20settings%20.png) | Virtual network configuration |
| [DHCP Scope](DHCP%20Scope%20.png) | DHCP address pool and allocation range |
| [DNS Resolution Test](DNS%20Resolution%20Test.png) | Successful domain DNS lookup |
| [DNS Zone](DNS%20Zone%20.png) | Active Directory DNS zone |
| [PC01 DHCP Configuration](PC01%20DHCP%20configuration.png) | Client DHCP settings |
| [PC01 Network Configuration](PC01%20net%20config.png) | Windows 10 client network configuration |

Screenshots are stored alongside this README and should be referenced using their actual filenames.

## 8. Results and Next Steps

### Completed

- Established an isolated VirtualBox network.
- Configured DC01 with a static IPv4 address.
- Installed and configured DNS for the Active Directory domain.
- Successfully tested basic DNS resolution.
- Installed and authorised DHCP Server.
- Created and activated an IPv4 DHCP scope.
- Verified the DHCP address pool.
- Changed PC01 to automatic IPv4 and DNS configuration.
- Successfully joined PC01 to the Active Directory domain.

### Outstanding Verification

Confirm that PC01 receives an IPv4 lease from `192.168.50.10`, with the expected DNS settings.

Once verified, record the assigned address and lease information, and update the DHCP testing result.

## Key Learning

This project reinforced the relationship between IP addressing, DNS, DHCP and Active Directory.

The troubleshooting process demonstrated the importance of checking both server-side configuration and client-side network settings.

Configuring a service is only one part of administration. Testing its behaviour from a client provides stronger evidence that the service operates as intended.
