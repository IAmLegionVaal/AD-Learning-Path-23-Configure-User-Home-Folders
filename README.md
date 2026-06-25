# AD Learning Path 23 — Configure User Home Folders

## Goal
Create a dedicated home folder for each lab user and map it as drive `H:` through the user's Active Directory properties.

## Prerequisites
- Domain-joined file server `SRV01`
- Existing `Home$` SMB share
- Test user `ajohnson`
- Permission to manage the share and user properties

## Steps
1. On `SRV01`, create a folder named `D:\Home\ajohnson`.
2. In the folder Security properties, give `ajohnson` Modify access to the folder, subfolders, and files.
3. Keep the required operating-system and administrator recovery entries.
4. In AD Users and Computers, open `ajohnson` and select the **Profile** tab.
5. Under **Home folder**, select **Connect**, choose `H:`, and enter `\\SRV01\Home$\ajohnson`.
6. Sign out and back in as `ajohnson`.
7. Confirm `H:` appears, create a test file, and verify a different test user cannot open the folder.

## PowerShell validation
```powershell
Get-ADUser ajohnson -Properties HomeDrive,HomeDirectory |
    Select-Object SamAccountName,HomeDrive,HomeDirectory
Get-SmbShareAccess -Name 'Home$'
Get-Acl 'D:\Home\ajohnson' | Format-List Owner,AccessToString
```

## Expected result
- `HomeDrive` is `H:`.
- `HomeDirectory` is `\\SRV01\Home$\ajohnson`.
- The user can create and modify files in the assigned folder.
- Other standard users cannot access the folder.

## Evidence
Under `evidence/`, store the user properties, folder permission screenshot, mapped-drive screenshot, file test, cross-user access test, errors, corrective actions, and final pass/fail result. Remove credentials and real organizational data before committing.

## Troubleshooting
- Drive missing: verify the UNC path, user attributes, network access, and a fresh sign-in.
- Permission failure: compare share and folder permissions.
- Other users can browse the folder: review inherited permissions on the root and user folder.

## Cleanup
Clear the user's home-drive fields, review any test data, and remove only disposable lab content.

## Next module
`AD-Learning-Path-24-Create-a-Domain-Logon-Script`
