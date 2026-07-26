
---

## 📋 Step-by-Step Implementation & Verification Guide

### Phase 1: Windows LAPS Implementation
**Step 1: Update AD Schema and Grant Permissions**
First, prepare Active Directory to store the LAPS passwords. (Assuming Windows Server 2022 / Windows 11 built-in LAPS).
```powershell
Update-LapsADSchema -Confirm:$false
Set-LapsADComputerSelfPermission -Identity "OU=Workstations,DC=techcorp,DC=local"

```

**Step 2: Configure LAPS via Group Policy**
Create a GPO linked to your Workstations/Servers OU and configure:

* **Path:** `Computer Configuration > Administrative Templates > System > LAPS`
* Enable **Configure password backup directory** (Choose Active Directory).
* Enable **Password Settings** (Set length, complexity, and rotation age).

**Verification (Admin-Side):**
To verify that the password was successfully rotated and saved to AD, run this PowerShell command using an IT Admin account:

```powershell
Get-LapsADPassword -Identity "ClientPC-01"

```

* **Success Criteria:** The command returns the current randomized plaintext password for the local admin account.

---

### Phase 2: BitLocker Drive Encryption

**Step 1: Install the Feature and Encrypt the Drive**
Install the BitLocker feature on the server/client and encrypt the OS drive (`C:`), backing up the recovery key to AD.

```powershell
Install-WindowsFeature BitLocker -IncludeAllSubFeature -IncludeManagementTools -Restart

```

*(After reboot)*

```powershell
Enable-BitLocker -MountPoint "C:" -EncryptionMethod XtsAes256 -UsedSpaceOnly -RecoveryPasswordProtector -ActiveDirectoryBackup

```

**Verification (Local-Side):**
Check the encryption status of the drive:

```powershell
manage-bde -status C:

```

* **Success Criteria:** The output shows **Protection Status: Protection On** and **Percentage Encrypted: 100%**.

---

### Phase 3: Windows Defender Firewall

**Step 1: Block Unauthorized RDP Access**
Instead of allowing RDP from anywhere, restrict Port 3389 so only the IT Subnet (`192.168.100.0/24`) can connect.

```powershell
New-NetFirewallRule -DisplayName "Restrict Inbound RDP to IT Subnet" -Direction Inbound -LocalPort 3389 -Protocol TCP -Action Allow -RemoteAddress 192.168.100.0/24

```

*Note: Ensure the default "Allow All" RDP rule is disabled.*

**Verification (Network-Side):**
Attempt to open an RDP session to the server from a machine outside the `192.168.100.0/24` subnet.

* **Success Criteria:** The connection times out and fails. Running `Get-NetFirewallRule -DisplayName "Restrict Inbound RDP to IT Subnet"` confirms the rule is active.

---

### Phase 4: Microsoft Defender & Attack Surface Reduction (ASR)

**Step 1: Enable Defender and ASR Rules**
Ensure real-time protection is on, and apply a specific ASR rule to block Office apps from spawning child processes (Rule GUID: `d4f940ab-401b-4efc-aadc-ad5f3c50688a`).

```powershell
Set-MpPreference -DisableRealtimeMonitoring $false
Set-MpPreference -AttackSurfaceReductionRules_Ids d4f940ab-401b-4efc-aadc-ad5f3c50688a -AttackSurfaceReductionRules_Actions Enable

```

**Verification (System-Side):**
Verify that the Defender preferences have successfully applied the ASR rule:

```powershell
Get-MpPreference | Select-Object -ExpandProperty AttackSurfaceReductionRules_Ids

```

* **Success Criteria:** The GUID `d4f940ab-401b-4efc-aadc-ad5f3c50688a` appears in the output, confirming the rule is active and protecting the system.
