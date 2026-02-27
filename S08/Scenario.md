# Internal Memo: Server Network Infrastructure & Security Audit



## 1. High Availability Implementation (NIC Teaming)
To prevent server downtime caused by a cable failure or switch port outage, you are required to "team" two physical network interfaces.

* **Team Name:** `ProductionTeam`
* **Member Adapters:** `Ethernet` and `Ethernet 1`
* **Configuration:** * **Teaming Mode:** `SwitchIndependent`
    * **Load Balancing:** `Dynamic`
* **Validation:** After configuration, initiate a continuous ping. Disconnect one of the physical interfaces to verify that the network remains stable without packet loss.


## 2. Web Service Accessibility & Firewall Verification
The development team has deployed a web service on **Port 80**. You must verify from a client machine that this port is accessible and not obstructed by the Windows Firewall.

* **Objective:** Perform a TCP connection test on Port 80.
* **Expected Outcome:** Confirmation of `TcpTestSucceeded`. 
* **Action Plan:** If the test fails, immediately audit and correct the **Inbound Rules** in the Windows Defender Firewall.



## 3. Active Session Monitoring & File Server Audit
Following recent security reports, I require a precise list of systems currently connected to the server. Extract the following reports:

* **Established Connections:** Provide a list of all IP addresses with active sessions, including the associated **Process ID (PID)**.
* **File Server Inspection:** Generate a detailed list of remote systems connected via **SMB**, including their IP addresses and computer names.
* **Open Files:** Identify exactly which sensitive files are currently being accessed or locked by remote users.

