# Lab 02: Implementing an Additional Domain Controller (ADC)

## Objective
To ensure high availability and fault tolerance for the `devlab.local` domain by adding a second Domain Controller (DC02).

## Infrastructure Details
- **Hostname:** DC02
- **IP Address:** 192.168.10.11
- **Primary DNS:** 192.168.10.10 (Pointing to DC01)

## Steps
1. **Prepare OS:** Installed Windows Server 2022 and updated the hostname to DC02.
2. **Network Setup:** Configured static IP and pointed DNS to the primary DC.
![both](../Screenshots/13.PNG)
3. **Domain Join:** Successfully joined DC02 to `devlab.local`.
4. **Promotion:** Installed AD DS role and promoted the server as an additional DC in the existing forest.

## Verification
- Run `Get-ADDomainController -Filter *` in PowerShell to see both servers.
- Check **Active Directory Sites and Services** to ensure replication is active.
