# PowerShell Administration

## 1. Summary

Used Windows PowerShell and command-line utilities to configure, manage and verify services within the Active Directory home lab.

Performed administrative tasks involving user and security group management, NTFS permissions, SMB file sharing and network diagnostics.

These tasks demonstrate practical command-line administration and troubleshooting skills within a Windows Server environment.

## 2. Administrative Commands

The following commands were used throughout the project on DC01 (Windows Server 2022) and PC01 (Windows 10 Pro).

| Area | Commands | Purpose |
|---|---|---|
| Active Directory | `Add-ADGroupMember`, `Get-ADGroupMember` | Manage and verify departmental security group membership |
| NTFS Permissions | `icacls` | Configure, inspect and troubleshoot folder permissions |
| SMB File Sharing | `New-SmbShare`, `Get-SmbShare`, `Get-SmbShareAccess` | Create shares and verify network permissions |
| DNS | `Resolve-DnsName` | Verify domain name resolution |
| Network Diagnostics | `ipconfig /all` | Inspect IPv4, DNS and DHCP configuration |

PowerShell cmdlets were used alongside native Windows utilities such as `icacls` and `ipconfig`.

These commands supported manual administration and verification rather than automated scripting.

## 3. Supporting Evidence

Detailed configuration steps, command outputs and verification screenshots are documented in the relevant project sections.

| Area | Documentation |
|---|---|
| Active Directory Administration | [AD Configuration](../02-Active-Directory%20Configuration/README.md) |
| NTFS Permissions | [Permission Verification](../04-File-Server-and-Permissions/02-NTFS-Permissions.png) |
| SMB Share Administration | [SMB Permissions](../04-File-Server-and-Permissions/SMB-Share%20Permissions%20Verification.png) |
| DNS and Network Diagnostics | [Network Services](../03-Network-Services/README.md) |

## 4. Future Development

Develop PowerShell automation scripts for Active Directory user provisioning, departmental security group assignment and account verification.

This extension will build on the manual administrative tasks completed during the project.

---

[Return to Main Project README](../README.md)
