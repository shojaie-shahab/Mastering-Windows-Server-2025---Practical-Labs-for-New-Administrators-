
# Chapter 10 Scenario: Server Hardening & Security Infrastructure

**Company Background:** Following a recent near-miss ransomware attack in the industry, the CISO of TechCorp has mandated a strict "Zero Trust and Endpoint Hardening" policy. The IT team must lock down all servers and endpoints using built-in Windows security features to prevent unauthorized access, lateral movement, and data theft.

---

## 🎯 Project Objectives

### Phase 1: Local Administrator Password Solution (Windows LAPS)
* **Goal:** Eliminate shared local administrator passwords across all machines to prevent hackers from using one compromised password to move laterally across the network (Pass-the-Hash attacks).
* **Solution:** Deploy **Windows LAPS** to automatically randomize local admin passwords and securely back them up to Active Directory (AD).

### Phase 2: Data at Rest Encryption (BitLocker)
* **Goal:** Protect sensitive data from physical theft. If a physical server or laptop is stolen, the hard drives must be unreadable without the correct decryption key.
* **Solution:** Enable **BitLocker Drive Encryption** using the TPM (Trusted Platform Module) chip and store the recovery keys in Active Directory.

### Phase 3: Traffic Control (Windows Defender Firewall)
* **Goal:** Restrict internal network traffic. For example, servers should only accept Remote Desktop (RDP) connections from the dedicated IT Admin subnet, blocking all other departments.
* **Solution:** Configure **Windows Defender Firewall with Advanced Security** via Group Policy (GPO) to restrict port 3389 (RDP).

### Phase 4: Threat Protection (Microsoft Defender & ASR)
* **Goal:** Protect the OS from malicious scripts and zero-day threats.
* **Solution:** Ensure **Windows Defender Antivirus** is active and enable **Attack Surface Reduction (ASR) rules** to block Office applications from creating child processes (a common malware technique).
