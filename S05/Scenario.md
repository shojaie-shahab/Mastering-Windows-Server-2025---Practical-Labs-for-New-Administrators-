# 🏢 Project Assignment: IT Department Policy Requirements

## 👤 Client: Infrastructure & Security Management
## 🎯 Target Scope: 
- **User:** `s.shojaies`
- **Security Group:** `G_IT_Admins`
- **Organizational Unit:** `IT_Department`



## 📝 Scenario 1: Workstation Security & Access
**Requirement:** "We need to standardize the desktops for our IT staff. Specifically, we require a direct website shortcut to our internal portal (`https://portal.devlab.local`) to be present on their desktop. Additionally, as a critical security measure, we must prevent any data leakage through physical ports; therefore, all USB and removable storage devices must be completely blocked for these users."



## 📝 Scenario 2: Resource Automation
**Requirement:** "Our IT personnel, including s.shojaies, should have immediate access to their tools upon login. We need the network share `\\DC01\IT_Tools` to be automatically mapped to the **T:** drive. Furthermore, to avoid constant security warnings while they work, our server's IP address `192.168.10.10` must be recognized as a 'Trusted Site' in their system internet settings."



## 📝 Scenario 3: System Integrity & Management
**Requirement:** "To prevent accidental downtime, we must remove the ability for IT staff to shut down or restart their workstations from the Start Menu Power options. On the management side, we require our Domain Controllers to be synchronized using a **Central Store** for all Group Policy templates to ensure consistency across the entire infrastructure."
