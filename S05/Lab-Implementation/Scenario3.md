# 🛠️ Step-by-Step Implementation: Scenario 03 (System Integrity & Central Store)

## 📋 Task Overview
- **Objective:** Remove Shutdown/Restart options and establish a GPO Central Store.
- **Target User:** `s.shojaies` (IT_Staff Group)
- **Infrastructure:** All Domain Controllers (Central Store synchronization).



## ⚙️ Execution Steps

### Step 1: Create the Central Store (Infrastructure Level)
*This step ensures all Domain Controllers use the same set of GPO templates.*

1.  Log in to the **Domain Controller** as an Administrator.
2.  Navigate to the `SYSVOL` shared folder: `C:\Windows\SYSVOL\sysvol\devlab.local\Policies`.
3.  Create a new folder named **`PolicyDefinitions`**.
4.  Copy all files and folders from the local machine's PolicyDefinitions folder (`C:\Windows\PolicyDefinitions`) into the new folder you just created in `SYSVOL`.
5.  **Verification:** Open the Group Policy Management Editor. Under Administrative Templates, it should now say: *"Policy definitions (ADMX files) retrieved from the Central Store."*

### Step 2: Create GPO for Power Restrictions
1.  Open **Group Policy Management**.
2.  Right-click on the **IT_Department** OU and select **"Create a GPO in this domain, and Link it here..."**.
3.  Name it: `GPO_IT_Power_Restrictions`.

### Step 3: Remove Shutdown, Restart, Sleep, and Hibernate
1.  Right-click the GPO and select **Edit**.
2.  Navigate to: `User Configuration` > `Policies` > `Administrative Templates` > `Start Menu and Taskbar`.
3.  Find the setting: **Remove and prevent access to the Shut Down, Restart, Sleep, and Hibernate commands**.
4.  Set it to **Enabled**.
5.  Click **OK**.

### Step 4: Security Filtering
1.  In the GPMC **Scope** tab for this GPO, remove `Authenticated Users`.
2.  Add the `IT_Staff` security group.
3.  In the **Delegation** tab, ensure `Authenticated Users` has **Read** access.



## 🧪 Verification & Testing

### 1. Central Store Check
- **Action:** Open any GPO for editing.
- **Expected Result:** In the left pane, click on **Administrative Templates**. In the right pane, verify it says "retrieved from the Central Store".

### 2. Power Options Check
- **Action:** Log in to the client as `s.shojaies` and run `gpupdate /force`.
- **Action:** Click the **Start Menu**, then the **Power icon**.
- **Expected Result:** The options for "Shut down" and "Restart" should be missing. Only "Disconnect" or "Sign out" should be available.

