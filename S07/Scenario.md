## Scenario 1: Securing Internal Websites and Linux Platforms

**Current Status:** We have several internal websites running on Windows servers (IIS) and various management dashboard services on Linux (NGINX). When employees access these sites, browsers display "Your connection is not private" errors.

**Requirements:**
1. **Deploy an Internal CA:** Establish a private Certificate Authority so our certificates are no longer individual "Self-signed" entities but are trusted by our own corporate root.
2. **Issue SSL Certificates:** Generate certificates for internal websites (e.g., `panel.devlab.local`).
3. **Cross-Platform Compatibility:** Provide a solution to convert the Windows-issued certificates into a format compatible with our **Linux servers** using OpenSSL.



## Scenario 2: Passwordless Authentication and Automated Computer Enrollment

**Current Status:** Our number of clients has grown significantly. I want company computers to be identified via digital certificates when connecting to the network, rather than requiring manual username/password entry every time. Furthermore, the administrative team does not have the time to manually install certificates on hundreds of individual machines.

**Requirements:**
1. **Create a Certificate Template:** Design a new template specifically for domain computers with appropriate permissions.
2. **Implement Auto-enrollment:** Configure a Group Policy (GPO) so that as soon as a computer joins the network, it automatically requests and installs the certificate from the CA.
3. **Automate Lifecycle Management:** Configure the system so that when certificates approach their expiration date, they **Auto-renew** without any manual intervention from the administrators.