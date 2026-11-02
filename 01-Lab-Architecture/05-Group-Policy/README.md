 Group Policy Management

This section documents the domain policies configured through Group Policy Management.

 Password Policy

Configured the Default Domain Policy with:

- Minimum password length: 12 characters
- Password history: 5 passwords
- Minimum password age: 1 day
- Maximum password age: 90 days
- Password complexity: Enabled
- Reversible password encryption: Disabled

 Network Drive Mapping

Created departmental Group Policy Objects:

- Sales-Mapped-Drive
- IT-Mapped-Drive
- Staff-Mapped-Drive

Configured drive mappings using:

User Configuration > Preferences > Windows Settings > Drive Maps

| Department | Drive Letter | Network Path |
|---|---|---|
| Sales | S: | \\DC01\Sales |
| IT | I: | \\DC01\IT |
| Staff | T: | \\DC01\Staff |

 Testing

Used Group Policy update and client-side checks to inspect mapped drives.

 Evidence

Screenshots of password policy settings, GPO configurations and mapped drives on PC01.
