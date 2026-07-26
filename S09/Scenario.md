# Chapter 9 Scenario: Secure Remote Access Infrastructure

**Company Background:** TechCorp has 300 employees. To support remote work safely, IT management needs to implement a secure remote access solution using four key Microsoft technologies: **VPN**, **Always On VPN**, **DirectAccess**, and **Web Application Proxy (WAP)**.

---

## 🎯 Project Objectives

### Phase 1: Traditional Remote Access VPN (SSTP / IKEv2)

* **Goal:** Allow external contractors using non-domain personal devices (BYOD) to securely connect to the internal network.
* **Solution:** Install the **Routing and Remote Access Service (RRAS)** role on Windows Server and enforce connection policies using **Network Policy Server (NPS)**.

### Phase 2: Modern Remote Access with Always On VPN

* **Goal:** Provide company-owned, domain-joined laptops with an automatic, seamless connection to the corporate network without user intervention.
* **Solution:** Deploy **Always On VPN** using two distinct tunnels:
* **Device Tunnel:** Connects before user login for domain authentication, password updates, and Group Policy updates.
* **User Tunnel:** Connects after user login to access file shares, internal apps, and intranet sites.



### Phase 3: Legacy Remote Access with DirectAccess

* **Goal:** Support legacy Windows 10 Enterprise devices with transparent, bi-directional connectivity to the office network.
* **Solution:** Configure **DirectAccess** on the Remote Access server using IP-HTTPS for reliable firewall traversal.

### Phase 4: Secure App Publishing with Web Application Proxy (WAP)

* **Goal:** Give employees secure access to internal web applications (e.g., HR Portal or Webmail) from the internet *without* connecting to a VPN or exposing internal web servers directly.
* **Solution:** Deploy a **Web Application Proxy (WAP)** server in the DMZ integrated with **Active Directory Federation Services (AD FS)** for multi-factor pre-authentication.


## 📋 Execution Plan

1. **Phase 1:** Install RRAS, configure SSTP VPN, and set up NPS rules.
2. **Phase 2:** Issue PKI Certificates, build the deployment XML profiles, and push Always On VPN via Intune/GPO.
3. **Phase 3:** Configure DirectAccess wizards and test external domain-joined clients.
4. **Phase 4:** Install WAP in the DMZ, connect to AD FS, and publish internal websites securely.

