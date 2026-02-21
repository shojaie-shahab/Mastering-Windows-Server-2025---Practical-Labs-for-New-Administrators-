# 🏢 Project Scenario: Resilient IP Addressing

## 🗣️ Client Requirement
"We need a foolproof IP management system for our office. Each department should get IPs from a specific range. Most importantly, we cannot afford any downtime; if our primary DHCP server fails, the second one must take over immediately without disconnecting the users."


## 🎯 Project Goals
- Create a specific **DHCP Scope** (192.168.10.100 - 200).
- Implement **DHCP Failover (Load Balance)** between DC01 and DC02.
- Ensure clients receive Gateway and DNS settings automatically.

---

# 🏢 Project Scenario: Internal Name Resolution

## 🗣️ Client Requirement
"Our staff finds it difficult to remember IP addresses. We want to be able to reach any PC or Server by its name (e.g., 'Win11-PC' or 'FileServer'). Also, we need a dedicated internal zone to manage all our resources centrally."


## 🎯 Project Goals
- Create a **Forward Lookup Zone** for `devlab.local`.
- Register a Windows 11 PC in DNS.
- Verify connectivity using **Hostnames** instead of IP addresses.