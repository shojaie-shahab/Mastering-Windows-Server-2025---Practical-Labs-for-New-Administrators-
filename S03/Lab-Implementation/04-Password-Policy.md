# Lab 04: Implementing Fine-Grained Password Policies (FGPP)

## 🎯 Objective
In this lab, I implemented **Fine-Grained Password Policies (FGPP)** to apply different password restrictions to different sets of users within the same domain. This is critical for enhancing security for privileged accounts without forcing strict requirements on all employees.



## 🛠️ Step-by-Step Implementation

### 1. Open Active Directory Administrative Center (ADAC)
1. On **DC01**, go to **Server Manager** > **Tools**.
2. Select **Active Directory Administrative Center**.
3. In the left pane, switch to **Tree View** and navigate to:
   `devlab (local) -> System -> Password Settings Container`



### 2. Create a New Password Settings Object (PSO)
1. Right-click on **Password Settings Container**.
2. Select **New** > **Password Settings**.
3. Configure the following for the **IT Admins Policy**:
   - **Name:** `PSO_IT_Admins`
   - **Precedence:** `1` (Lower number means higher priority).
   - **Enforce minimum password length:** `15` characters.
   - **Password must meet complexity requirements:** `Enabled`.
   - **Account Lockout Policy:** `5` failed attempts.

### 3. Assign the Policy to a Group
1. In the same window, scroll down to the **Directly Applies To** section.
2. Click **Add**.
3. Type `G_IT_Admins` (The security group created in Lab 04).
4. Click **OK** and then **OK** again to save the policy.





## ✅ Verification

To verify that the policy is correctly applied to a specific user, run the following PowerShell command:

```powershell
# Check which PSO is applied to the user
Get-ADUserResultantPasswordPolicy -Identity s.shojaie
