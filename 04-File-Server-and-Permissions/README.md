# File Server and Permissions

## 1. Summary

Configured and tested departmental file sharing using Windows Server 2022, Active Directory security groups, NTFS permissions and SMB network shares.

Created shared folders for Sales, IT and Staff, assigned group-based permissions and tested network access from a domain-joined Windows 10 workstation.

Used PowerShell and `icacls` to configure permissions, investigate an SMB share creation error and correct unintended NTFS permission inheritance.

## 2. Overview

The simulated organisation required centralised file storage with controlled access to departmental resources.

The objective was to assign file permissions through Active Directory security groups rather than individual user accounts, simplifying access management.

| Component | Configuration |
|---|---|
| File Server | DC01 |
| Operating System | Windows Server 2022 |
| Client Workstation | PC01, Windows 10 Pro |
| Domain | adlab.test |
| File Sharing | SMB |
| Access Control | NTFS and Active Directory security groups |
| Administration | PowerShell and icacls |

### Departmental Folder Structure

Created three directories under `C:\Company Shares` on DC01.

```text
C:\Company Shares
├── Sales
├── IT
└── Staff
```

| Department | Security Group | Network Share |
|---|---|---|
| Sales | Sales-Team | `\\DC01\Sales` |
| IT | IT-Team | `\\DC01\IT` |
| Staff | Staff-Team | `\\DC01\Staff` |

[View Folder Structure](01-Departmental-Folders.png)

## 3. Configuration and Implementation

### NTFS Permissions

Assigned Modify permissions to the corresponding departmental security groups using `icacls`.

| Folder | Security Group | Permission |
|---|---|---|
| Sales | ADLAB\Sales-Team | Modify |
| IT | ADLAB\IT-Team | Modify |
| Staff | ADLAB\Staff-Team | Modify |

Example command:

```powershell
icacls "C:\Company Shares\Sales" /grant "ADLAB\Sales-Team:(OI)(CI)M"
```

The permission flags `(OI)(CI)M` specify inheritable Modify access for files and subfolders.

Verified departmental permissions using:

```powershell
icacls "C:\Company Shares\Sales"
icacls "C:\Company Shares\IT"
icacls "C:\Company Shares\Staff"
```

The initial permission review revealed unintended IT-Team access inherited from the parent directory. This was corrected, leaving each department with its own assigned Modify entry.

Further restricted the Sales folder by disabling inheritance and removing broad `BUILTIN\Users` permissions. The final Sales ACL retained:

- Sales-Team: Modify
- SYSTEM: Full Control
- Administrators: Full Control
- CREATOR OWNER: Inherit-only Full Control

The IT and Staff folders retained inherited Users permissions, with network access also governed by SMB share permissions.

**Evidence:**

- [Departmental NTFS Permissions](02-NTFS-Permissions.png)
- [Final Sales NTFS Permissions](03-Sales-NTFS-Final-Permissions.png)

### SMB Share Configuration

Created three SMB shares using PowerShell, assigning departmental groups Change access and Administrators Full Control.

Example configuration:

```powershell
New-SmbShare -Name "Sales" `
    -Path "C:\Company Shares\Sales" `
    -ChangeAccess "ADLAB\Sales-Team" `
    -FullAccess "BUILTIN\Administrators"
```

Verified the configured permissions using `Get-SmbShareAccess`.

| Share | Departmental Group | Share Permission |
|---|---|---|
| Sales | Sales-Team | Change |
| IT | IT-Team | Change |
| Staff | Staff-Team | Change |

`BUILTIN\Administrators` retained Full Control on all three shares.

NTFS permissions govern file-system access, while SMB permissions govern network share access. Both permission layers contribute to the effective access available through a shared folder.

[View SMB Share Permissions](SMB-Share%20Permissions%20Verification.png)

## 4. Testing and Verification

Performed network access tests from PC01 and inspected the permissions configured on DC01.

| Test | Observed Result | Status |
|---|---|---|
| Folder Structure | Sales, IT and Staff folders created | Passed |
| NTFS Permissions | Departmental Modify entries confirmed | Passed |
| SMB Permissions | Departmental Change entries confirmed | Passed |
| Sales Share | Share opened successfully | Passed |
| IT Share | Share opened successfully | Passed |
| Staff Share | Share opened successfully | Passed |
| Restricted Access | Access denied when attempting to open the Sales share during Staff testing | Passed |
| Sales ACL Correction | Unwanted Users entries removed | Passed |

### Client-Side Results

Screenshots demonstrate successful access to departmental shares and a restricted-access response.

- [Sales Share Access](07-Sales-Access-Granted.png)
- [IT Share Access](IT-Team%20acecss%20to%20r%2Cw%2Cm%20in%20IT%20folders%20only.png)
- [Staff Access and Sales Restriction](Staff%20dept%20acess%20%26%20restriction%20test.png)

The access screenshots show observed network behaviour but do not independently confirm every signed-in account or prove full Modify operations. The Sales access screenshot was captured before the final Sales ACL hardening.

## 5. Troubleshooting and Problem Solving

### SMB Share Creation Error 1332

**Issue:** SMB share creation failed with Windows error 1332, indicating a security principal could not be mapped to a security identifier.

**Resolution:** Investigated the account reference supplied to PowerShell and corrected the administrative principal to `BUILTIN\Administrators`.

**Verification:** Confirmed the resulting share permissions using `Get-SmbShareAccess`.

### Unintended NTFS Permission Inheritance

**Issue:** The IT-Team security group unexpectedly had inherited Modify permissions on the Sales and Staff folders.

**Investigation:** Inspected the parent directory using:

```powershell
icacls "C:\Company Shares"
```

Identified an inheritable IT-Team Modify entry on the parent directory.

**Resolution:** Removed the unwanted parent permission and verified the resulting departmental ACLs.

Further hardened the Sales folder by disabling inheritance and removing broad Users permissions.

**Verification:** The final Sales ACL retained Sales-Team Modify access, together with SYSTEM and Administrators Full Control.

This troubleshooting demonstrated the importance of checking inherited access control entries and validating permissions after configuration changes.

## 6. Supporting Evidence

| Screenshot | Description |
|---|---|
| [Departmental Folders](01-Departmental-Folders.png) | Sales, IT and Staff directory structure |
| [Departmental NTFS Permissions](02-NTFS-Permissions.png) | Corrected departmental Modify entries |
| [Final Sales NTFS Permissions](03-Sales-NTFS-Final-Permissions.png) | Sales folder ACL after additional hardening |
| [SMB Share Permissions](SMB-Share%20Permissions%20Verification.png) | Share-level Change and Full Control entries |
| [Sales Share Access](07-Sales-Access-Granted.png) | Successful network folder access |
| [IT Share Access](IT-Team%20acecss%20to%20r%2Cw%2Cm%20in%20IT%20folders%20only.png) | Successful IT share access |
| [Staff Access and Restriction](Staff%20dept%20acess%20%26%20restriction%20test.png) | Staff share access and Sales access denial |

## 7. Skills Demonstrated

This implementation demonstrates practical experience with Windows Server file sharing, Active Directory group-based access management, NTFS and SMB permissions, PowerShell administration and client-side access testing.

Troubleshooting strengthened my understanding of permission inheritance, security principal resolution and the importance of validating configuration changes.

---

[Return to Main Project README](../README.md)
