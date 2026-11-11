# Active Directory File Share Permissions Lab

**Tools:** Microsoft Azure, Windows Server, Active Directory, NTFS and share permissions

## Overview

I built a small Windows domain environment in Azure with a domain controller (**DC-1**) and a client PC (**Client-1**) to simulate a small-business network. I created shared folders, set permissions for different groups, and tested access as a standard user.

## What I Did

- Built a Windows domain environment in Azure with a domain controller and a client PC.
- Created and shared network folders and configured share and NTFS permissions for Domain Users, Domain Admins, and a custom security group.
- Created an **ACCOUNTANTS** security group in Active Directory Users and Computers and granted folder access through group membership instead of per-user permissions.
- Tested access from a standard user account, confirmed the expected allow and deny results, and troubleshot access issues involving permission inheritance, DNS, and sign-in refresh.

## Lab Setup

| Folder | Who has access | Permission |
|---|---|---|
| `read-access` | Domain Users | Read |
| `write-access` | Domain Users | Read/Write |
| `no-access` | Domain Admins only | Read/Write |
| `accounting` | ACCOUNTANTS group | Read/Write |

## Results

### 1. Folders shared on DC-1

![Shared folders on DC-1]("C:\Users\jolip\Pictures\Screenshots\fileShareLab\dc1AddedFolders.png")
*A standard user opens `\\dc-1` on Client-1 and sees the shared folders.*

### 2. Access denied on `no-access`

![Access denied on no-access]("C:\Users\jolip\Pictures\Screenshots\fileShareLab\limitedAccessUser.png")
*The standard user is blocked because only Domain Admins have access.*

### 3. Before and after joining the ACCOUNTANTS group

![Accounting denied before]("C:\Users\jolip\Pictures\Screenshots\fileShareLab\restrictedAccessAccounting.png")
*Before: access to `accounting` is denied.*

![Accounting opened after]("C:\Users\jolip\Pictures\Screenshots\fileShareLab\unristrictedAccessUserAccount.png")
*After: the user was added to ACCOUNTANTS, signed out and back in, and now has access.*

## What I Learned

- **Share permissions and NTFS permissions work together.** When they disagree, the stricter one wins.
- **Least privilege:** people should get only the access they need.
- **Groups save time.** Adding a user to a group is easier than editing every folder.
- **Inheritance matters.** Permissions pass down from parent folders, so it must be turned off when a folder needs to be locked down.
- **Group changes need a fresh sign-in.** Windows reads group memberships at login.
- **Always test as a standard user.** Admin accounts can hide permission problems.

## Troubleshooting Notes

- Changing permissions on the **C:\ drive** instead of the lab folders locked me out of Remote Desktop on DC-1. I fixed it with **Azure Run Command** and `icacls`.
- A folder only appears at `\\dc-1` if it is **shared**. I used `net share` to confirm.

## Skills Demonstrated

Active Directory, Windows Server, NTFS permissions, SMB file shares, security groups, least privilege, Azure VMs, troubleshooting

