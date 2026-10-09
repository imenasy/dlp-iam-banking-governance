# Privileged Access Management (PAM) & Session Governance Rules

## 1. Scope
Applies to all administrative access to District SACCO hypervisors, Core Banking Oracle/PostgreSQL databases, and network edge firewalls.

## 2. Hard Requirements
- **No Direct SSH/RDP:** Direct administrative access from workstations to production servers is strictly blocked by firewall zoning.
- **Mandatory Jump Host / Bastion Proxy:** All access must traverse the PAM proxy gateway (e.g., CyberArk / Wallix / Bastion).
- **Just-In-Time (JIT) Elevation:** Administrative privileges expire automatically after 120 minutes per approved change ticket number.
- **Complete Session Video & Keystroke Recording:** All SSH command history and RDP video sessions are recorded and archived in immutable WORM storage for a minimum of 365 days.

## 3. Disallowed Commands (Emergency Disconnect Triggered)
Executing the following commands inside a privileged administrative shell triggers immediate session kill and sends a critical P1 alert to the SOC:
* `rm -rf /` or recursive wiping commands
* `iptables -F` or flushing firewall rules
* `UPDATE ... SET balance = ...` (outside tracked CBS migrations)
* `useradd` or creating undocumented local root users
