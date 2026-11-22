# Group Policy Management

## 1. Summary

Configured and verified Group Policy in a Windows Server 2022 Active Directory environment to manage domain password settings and automate departmental network drive mappings.

Implemented password requirements through the Default Domain Policy and created three departmental Group Policy Objects (GPOs) for Sales, IT and Staff.

Used Group Policy Management, PowerShell and Windows command-line tools to configure policies, investigate incorrect GPO targeting and verify the final configurations on a domain-joined Windows 10 workstation.

Successfully corrected GPO linking and confirmed each department received its intended network drive.

## 2. Overview

The simulated business required centralised password management and automated access to departmental network resources.

Group Policy was used to manage security settings and distribute network drive configurations based on departmental Organisational Units (OUs).

| Component | Configuration |
|---|---|
| Domain Controller | DC01 |
| Client Workstation | PC01 |
| Active Directory Domain | adlab.test |
| Server OS | Windows Server 2022 |
| Client OS | Windows 10 Pro |
| Management Tool | Group Policy Management Console |
| Features | Password Policy, Group Policy Preferences, Drive Maps |

## 3. Configuration and Implementation

### 3.1 Domain Password Policy

Configured password requirements through the Default Domain Policy.

**Configuration path:**

`Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy`

| Policy Setting | Configuration |
|---|---|
| Minimum Password Length | 12 characters |
| Password History | 5 passwords remembered |
| Minimum Password Age | 1 day |
| Maximum Password Age | 90 days |
| Password Complexity | Enabled |
| Reversible Password Encryption | Disabled |

These settings establish consistent domain password requirements and help limit password reuse.

Verified the configured values through Group Policy Management Editor.

**Evidence:** [Password Policy Settings](Pwd%20policy%20settings.png)

### 3.2 Departmental Network Drive Mapping

Created three GPOs using Group Policy Preferences to automate departmental network drive connections.

**Configuration path:**

`User Configuration > Preferences > Windows Settings > Drive Maps`

| Department | GPO Name | Drive Letter | Network Path |
|---|---|---|---|
| Sales | Sales-Mapped-Drive | S: | `\\DC01\Sales` |
| IT | IT-Mapped-Drive | I: | `\\DC01\IT` |
| Staff | Staff-Mapped-Drive | T: | `\\DC01\Staff` |

Configured each drive mapping using the following settings:

- Action: Create
- Location: Corresponding departmental SMB share
- Drive Letter: Assigned departmental drive
- Reconnect: Enabled
- Label: Departmental shared drive

Following troubleshooting, confirmed each drive-mapping GPO was linked to its corresponding departmental OU.

| Organisational Unit | Linked GPO |
|---|---|
| Sales | Sales-Mapped-Drive |
| IT | IT-Mapped-Drive |
| Staff | Staff-Mapped-Drive |

The Default Domain Policy remained linked at the domain level.

This OU-based configuration applies departmental drive-mapping policies to users within the corresponding organisational units.

**Evidence:** [IT Drive Mapping Configuration](IT-Mapped-Drive.png)

## 4. Testing and Verification

Performed client-side verification on PC01 using domain accounts from all three departments.

### Verification Commands

```cmd
whoami
gpupdate /force
net use
gpresult /r /scope user
```

These commands were used to identify the signed-in account, refresh policy settings, inspect network drive connections and verify applied user GPOs.

### Final Results

| Department | Domain Account | Applied GPO | Verified Drive | Status |
|---|---|---|---|---|
| Sales | ADLAB\asmith | Sales-Mapped-Drive | S: | Passed |
| IT | ADLAB\tsole | IT-Mapped-Drive | I: | Passed |
| Staff | ADLAB\dbrown | Staff-Mapped-Drive | T: | Passed |

Final client tests confirmed:

- Sales received `S:` mapped to `\\DC01\Sales`.
- IT received `I:` mapped to `\\DC01\IT`.
- Staff received `T:` mapped to `\\DC01\Staff`.
- Each account received only its corresponding departmental drive-mapping GPO.
- Outdated mappings from the previous configuration were removed.

### Verification Evidence

| Department | Network Drive Verification | Applied GPO Results |
|---|---|---|
| Sales | [Drive Verification](Sales-Drive-Verification.png) | [GPO Results](Sales-GPO-Results.png) |
| IT | [Drive Verification](IT-Drive-Verification.png) | [GPO Results](IT-GPO-Results.png) |
| Staff | [Drive Verification](Staff-Drive-Verification.png) | [GPO Results](Staff-GPO-Results.png) |

**Result: PASSED**

All three departmental GPO targeting and network drive mapping tests were successfully verified.

## 5. Troubleshooting and Problem Solving

### Issue: Incorrect Departmental GPO Targeting

During initial testing, domain accounts received all three network drive mappings regardless of their department.

Running `gpresult /r /scope user` revealed that multiple departmental GPOs were applying to the same user.

### Investigation

Inspected GPO links through Group Policy Management and identified that the departmental drive-mapping GPOs were linked at the domain level.

These domain-level links caused the policies to apply more broadly than intended.

### Resolution

1. Removed the unnecessary domain-level GPO links without deleting the GPOs.
2. Confirmed each drive-mapping GPO was linked to its corresponding departmental OU.
3. Removed outdated network connections using:

```cmd
net use * /delete /y
```

4. Refreshed Group Policy using:

```cmd
gpupdate /force
```

5. Signed out and back in before repeating the client-side tests.

### Verification

Used `whoami`, `net use` and `gpresult /r /scope user` to verify the corrected behaviour.

Confirmed each departmental account received only its assigned drive and departmental GPO.

**Result: RESOLVED**

**Evidence:** [Original Network Drive Mapping Issue](Mapped-Drive-Network-Verification.png)

This investigation demonstrated the importance of understanding GPO inheritance and scope, recognising persistent network connections and validating policy application from the client workstation.

## 6. Skills Demonstrated

This implementation demonstrates practical experience with:

- Windows Server Group Policy administration.
- Domain password policy configuration.
- GPO creation, linking and management.
- OU-based policy targeting.
- Automated network drive mapping through Group Policy Preferences.
- Client-side verification using `gpresult` and `net use`.
- Troubleshooting incorrect GPO scope and persistent drive mappings.
- Technical documentation and configuration verification.

The project strengthened my understanding of centralised Windows administration and the relationship between Active Directory organisational structure, Group Policy processing and client configuration.

---

[Return to Main Project README](../README.md)
