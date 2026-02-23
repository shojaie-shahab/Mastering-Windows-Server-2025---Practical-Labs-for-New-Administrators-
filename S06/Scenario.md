# 📂 Project Phase: File Management & Infrastructure Security

This document outlines the business requirements for Scenario 1 and Scenario 2, focusing on data availability and security hardening.



## 📝 Scenario 1: Unified Access & Data Redundancy (DFS Implementation)

**Requirement (IT Director):** "We have a major problem: our users are confused because files are scattered across multiple servers (e.g., `\\DC01\Sales`, `\\DC02\Marketing`, etc.). Also, if one server goes down, the department stops working.

I want you to implement a **Domain-Based DFS Namespace** called `\\devlab.local\CompanyData`. All department folders should be accessible under this single path. Furthermore, I need you to set up **DFS Replication (DFSR)** between our two servers so that if one fails, the data remains accessible and synchronized. Ensure that IT staff have a mapped drive **(S:)** pointing to this new namespace via GPO."


## 📝 Scenario 2: Secure File Access & NTFS Permissions Hardening

**Requirement (Security Auditor):** "During our recent audit, we found that file permissions are a mess. We need to enforce a **'Least Privilege'** model.

1.  **Group-Based Access:** Stop using individual user accounts for folder permissions—**Use Groups only!** Create a structured permission system for the 'Finance' folder where the `Finance_Managers` group has **'Modify'** rights, but `Finance_Staff` can only **'Read'** files.
2.  **Inheritance Control:** We have a sensitive folder where **Inheritance** must be disabled to prevent accidental access from higher-level administrators or parent folders.
3.  **Secure External Transfer:** Set up a secure **FTP Server (FTPS)** on our Windows Server to allow our external partners to upload reports safely, ensuring that each partner is restricted to their own home directory (User Isolation)."
