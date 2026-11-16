# Active Directory Helpdesk Lab
 
A hands-on home lab that simulates a real IT helpdesk. I built a small Active Directory environment for a fictional company (Contoso) and worked through eight realistic support tickets, solving each one using only the Windows Server graphical tools (no scripts).
 
## Lab Environment
 
| Component | Details |
| --- | --- |
| Domain | `corp.local` |
| Domain controller | `DC01` (Windows Server, Desktop Experience) |
| Client | `WIN-CLIENT` (Windows, domain-joined) |
| Tools used | Active Directory Users and Computers, Group Policy Management, Active Directory Administrative Center, File Explorer, RSAT |
 
### Directory structure
 
```
corp.local
└── Contoso
    ├── Sales
    ├── HR
    ├── IT
    ├── Groups
    ├── Workstations
    └── Disabled Accounts
```
 
Security groups: `GG_Sales`, `GG_HR`, `GG_Helpdesk` (global) and `DL_Sales_Read` (domain local).
 
## Skills Demonstrated
 
- User account lifecycle: creation, unlock, password reset, offboarding
- Group-based access control (AGDLP: Account, Global group, Domain Local group, Permission)
- Group Policy creation, linking and troubleshooting
- Delegation of control with least privilege
- Fine-grained password policies
- Ticket documentation and verification
## Tickets
 
Each ticket was tested from the client as the affected user, then documented with before and after screenshots.

## Ticket 1: Locked-Out User

**Problem:** A Sales user was locked out after failed sign-in attempts.
**Solution:** Configured an account lockout policy, then unlocked the account and forced a password reset in Active Directory Users and Computers.
**Result:** User signed in and was prompted to set a new password.

### Before

![User details](image/AD-user-details.PNG)

![Account lockout policy](image/lockout-policy.PNG)

![Account not locked out](image/account-not-lockedout.PNG)

![User logging in](image/user-signing-in.png)

### After

![User locked out](image/user-locked-out.png)

![Account locked out](image/account-lockout.PNG)

![User logged in after lockout](image/user-logged-after-locked-out.png)
