# Phase 3: Active Session Monitoring & File Server Audit

**Objective:** To identify all active inbound network connections and audit file-level access to ensure server security, verify resource integrity, and identify unauthorized access.



## Step 1: Auditing Established Network Connections
The primary goal is to identify every remote IP address that has an active "handshake" with the server. We utilize the `netstat` utility to filter for **ESTABLISHED** sessions and map them to their respective internal Process IDs (PID).

### Execution Command:
```dos
netstat -ano | findstr ESTABLISHED
```

### Interpretation of Results:

1. Foreign Address: Displays the IP address of the remote system currently connected to your server.

2. PID (Process ID): The unique identifier of the service or application handling that connection.

3. Pro-Tip: To identify the specific application name using a PID, run:
```dos 
tasklist /fi "pid eq [PID_Number]"
```

## Step 2: File Server Inspection (SMB Sessions)

For servers hosting shared folders, it is critical to identify who is connected via the SMB (Server Message Block) protocol. This reveals the hostnames and usernames of active remote participants.

Execution Command (PowerShell):
``` PowerShell
Get-SmbSession | Select-Object ClientComputerName, ClientUserName, NumOpens, SecondsIdle
```

### Key Audit Metrics:
1. ClientComputerName: The source network address or hostname of the remote connection.

2. ClientUserName: The specific user account used to authenticate the session.

3. NumOpens: The number of files or handles currently held open by the remote user.

4. SecondsIdle: Indicates how long the user has been inactive (critical for identifying stale or "ghost" sessions).


## Step 3: Identifying Open and Locked Files
Beyond session tracking, we must identify the exact files being accessed. This is crucial for tracking access to sensitive data or troubleshooting "File in Use" errors.

Execution Command (PowerShell):
``` PowerShell
Get-SmbOpenFile | Select-Object ClientComputerName, Path, ShareName
```

### Data Analysis:
1. Path: The full local directory path to the file currently being read or modified.

2. ShareName: The specific network share name through which the user gained access.

