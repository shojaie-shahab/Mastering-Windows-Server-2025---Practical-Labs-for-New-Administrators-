

# Chapter 9: Remote Access Implementation & Verification Guide

**Objective:** Deploy and verify VPN, Always On VPN, DirectAccess, and Web Application Proxy (WAP) to provide secure remote access for the workforce.

---

## Phase 1: Traditional VPN (SSTP / IKEv2) & NPS
**Goal:** Set up a standard VPN for contractors using personal devices, authenticated via Network Policy Server (NPS).

### Step 1: Install Roles
Install the Routing and Remote Access (RRAS) and Network Policy Server (NPS) roles on your edge server.
```powershell
Install-WindowsFeature RemoteAccess, DirectAccess-VPN, NPAS -IncludeManagementTools

```

### Step 2: Configure the VPN Server

Initialize the Remote Access server to accept VPN connections.

```powershell
Install-RemoteAccess -VpnType Vpn

```

*Note: In the RRAS console, configure the server to hand out IP addresses from a static pool or DHCP.*

### Step 3: Configure NPS (Authentication)

Create a network policy to allow members of the "VPN_Users" Active Directory group to connect.

* Open **NPS Console**.
* Go to **Policies > Network Policies**.
* Create a rule: **Condition:** `User Groups = CONTOSO\VPN_Users`, **Action:** `Grant Access`, **EAP Type:** `PEAP-MSCHAPv2`.

### Verification (Client-Side)

On a remote Windows client, create and test the connection:

```powershell
Add-VpnConnection -Name "TechCorp VPN" -ServerAddress "vpn.techcorp.com" -TunnelType SSTP
# Connect to the VPN
rasdial "TechCorp VPN" [username] [password]

```

* **Success Criteria:** The command returns `Command completed successfully` and the client receives an internal IP address.

---

## Phase 2: Always On VPN (AOVPN)

**Goal:** Deploy a seamless, auto-connecting VPN for corporate-owned, domain-joined laptops.

### Step 1: Certificate Infrastructure (PKI)

Always On VPN requires certificates. You must configure your Internal CA to auto-enroll two certificates:

1. **Computer Certificate:** For the Device Tunnel (IKEv2).
2. **User Certificate:** For the User Tunnel (SSTP/IKEv2).

### Step 2: Create and Deploy the ProfileXML

Always On VPN is configured on clients using an XML file pushed via Intune, Microsoft Endpoint Manager, or PowerShell.
Create an XML file (`AOVPN_Profile.xml`) containing your routing and encryption settings.

Deploy it on the client using PowerShell:

```powershell
$ProfileXML = Get-Content -Path "C:\AOVPN_Profile.xml" -Raw
$WmiObj = [wmiclass]"\\.\root\cimv2\mdm\dmmap\MDM_VPNv2_01"
$WmiObj.CreateInstance("TechCorp_AOVPN", $ProfileXML)

```

### Verification (Client-Side)

Restart the client computer. Before the user logs in, the Device Tunnel should connect automatically.

```powershell
Get-VpnConnection -Name "TechCorp_AOVPN"

```

* **Success Criteria:** The `ConnectionStatus` shows as **Connected** without any user interaction.

---

## Phase 3: Legacy DirectAccess

**Goal:** Keep existing Windows 10 Enterprise clients connected seamlessly using DirectAccess.

### Step 1: Configure DirectAccess Prerequisites

DirectAccess requires an Active Directory Security Group for target clients and a Network Location Server (NLS) website to detect if a client is inside or outside the network.

* Create AD Group: `DA_Clients`
* Deploy NLS Website: `https://test-vpn.local`

### Step 2: Enable DirectAccess

Run the configuration command on the Remote Access server, targeting your external URL and client group.

```powershell
Install-RemoteAccess -VpnType DirectAccess -ClientGpoName "TechCorp.local\DirectAccess_Clients" -InternalInterface "Ethernet" -InternetInterface "Ethernet 2"

```

### Verification (Client-Side)

On a remote client belonging to the `DA_Clients` group, verify the DirectAccess connection state.

```powershell
Get-DAConnectionStatus

```

* **Success Criteria:** The Status returns **ConnectedRemotely** and the client can ping internal resources like a Domain Controller.

---

## Phase 4: Web Application Proxy (WAP)

**Goal:** Publish an internal web application securely to the internet using pre-authentication.

### Step 1: Install WAP Role

Install the WAP role on a server located in the DMZ.

```powershell
Install-WindowsFeature Web-Application-Proxy -IncludeManagementTools

```

### Step 2: Connect WAP to AD FS

Bind the WAP server to your internal Active Directory Federation Services (AD FS) server.

```powershell
Install-WebApplicationProxy -CertificateThumbprint "1A2B3C4D5E..." -FederationServiceName "adfs.techcorp.com"

```

### Step 3: Publish the Application

Publish an internal HR portal so external users can access it securely.

```powershell
Add-WebApplicationProxyApplication -Name "HR Portal" -ExternalPreauthentication ADFS -ExternalUrl "[https://test-vpn.com/](https://hr.techcorp.com/)" -BackendServerUrl "[https://internal-hr.techcorp.local/](https://internal-hr.techcorp.local/)" -ExternalCertificateThumbprint "1A2B3C4D5E..."

```

### Verification (External Client)

* Open a web browser on an external, non-connected device.
* Navigate to `https://test-vpn.com/`.
* **Success Criteria:** You are immediately redirected to the AD FS login page. After entering valid domain credentials, you are securely routed to the internal HR portal.

```

<FollowUp label="Want to cover troubleshooting for these technologies?" query="What are the most common troubleshooting steps and logs to check if VPN or DirectAccess fails to connect?"/>

