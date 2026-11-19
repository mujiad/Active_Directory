# Active Directory Helpdesk Lab

A hands-on home lab that simulates a real IT helpdesk. I built a small Active Directory environment for a fictional company (Contoso) and worked through eight realistic support tickets, solving each o[...]

## Lab Environment

| Component | Details |
| --- | --- |
| Domain | `corp.local` |
| Domain controller | `DC01` (Windows Server, Desktop Experience) |
| Client | `WIN-CLIENT` (Windows, domain-joined) |
| Tools used | Active Directory Users and Computers, Group Policy Management, Active Directory Administrative Center, File Explorer, RSAT |

### Directory Structure

```text
corp.local
└── Contoso
    ├── Sales
    ├── HR
    ├── IT
    ├── Groups
    ├── Workstations
    └── Disabled Accounts
```

Security groups: `GG_Sales`, `GG_HR`, and `GG_Helpdesk` are global groups; `DL_Sales_Read` is a domain-local group.

## Skills Demonstrated

- User account lifecycle: creation, unlock, password reset, and offboarding
- Group-based access control using AGDLP: Account, Global group, Domain Local group, and Permission
- Group Policy creation, linking, and troubleshooting
- Delegation of control using least privilege
- Fine-grained password policies
- Ticket documentation and verification

## Tickets

Each ticket was tested from the client as the affected user and documented with before-and-after screenshots.

## Ticket 1: Locked-Out User

**Problem:** A Sales user was locked out after multiple failed sign-in attempts.

**Solution:** Configured an account lockout policy, unlocked the account, and forced a password reset in Active Directory Users and Computers.

**Result:** The user signed in and was prompted to create a new password.

### Before

![User details](image/AD-user-details.PNG)

![Account lockout policy](image/lockout-policy.PNG)

![Account not locked out](image/account-not-lockedout.PNG)

![User signing in](image/user-signing-in.png)

### After

![User locked out](image/user-locked-out.png)

![Account locked out](image/account-lockout.PNG)

![User logged in after lockout](image/user-logged-after-locked-out.png)

## Ticket 2: New Hire Onboarding

**Problem:** HR requested a new user account, Sales access, and a home drive for a new employee.

**Solution:** Created the user by copying a Sales employee to inherit the appropriate group memberships, configured the job title and manager, and mapped an `H:` home drive to a hidden share.

**Result:** The new user signed in, was required to change the temporary password, and could access the `H:` drive.

### 1. Create the User Account

Created a user account for Peter Garcia in Active Directory Users and Computers by copying an existing Sales employee. This copied account inherited the Sales team memberships.

![Peter Garcia account membership](image/ticket2-member-of.PNG)

Next, signed in to the client as Peter Garcia. At this stage, the home folder was not available.

![Peter Garcia home folder before configuration](image/pgarcia-before-sharing.png)

### 2. Create the Home Folder Share

Created the home folder directory at `C:\Share\Home` on the domain controller.

Opened the folder properties, selected **Sharing**, and enabled **Advanced Sharing**. Named the share `Home$` to create a hidden share.

![Home folder advanced sharing configuration](image/ticket2-share-home.PNG)

Then opened **Permissions** and granted the appropriate access to the required users.

For this lab, the Everyone group was granted Full Control. In a production environment, permissions should normally be restricted to authorized users or groups.

![Home share permissions](image/ticket2-share-home-permission-full-control.PNG)

### 3. Configure the User's Home Folder

Opened Peter Garcia's account properties, selected the **Profile** tab, and mapped a home folder to the hidden share:

`\\AD-DC01\Home$\%username%`

The `%username%` variable automatically creates or connects to a folder named for the signed-in user.

![Home folder profile configuration](image/ticket2-connect.PNG)

### 4. Verify the Home Drive

Signed in again as Peter Garcia and verified that the home folder was available as the `H:` drive.

![Peter Garcia home folder after configuration](image/pgarcia-after-sharing.png) 

Ticket 3: Shared folder access

Problem: An HR user received "Access is denied" on the Sales share but needed read-only access. Solution: Created a domain local group DL_Sales_Read, added GG_HR to it, and granted Read & execute on the folder. No permissions were given to individual users. Result: User could open files but could not modify them.
