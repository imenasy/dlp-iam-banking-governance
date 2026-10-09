markdown
# Data Loss Prevention (DLP) & Hardening Governance Stack

Implementation patterns for stopping sensitive financial data exfiltration and enforcing CIS Benchmark baselines across centralized District SACCO servers.

## Highlights
- **Forcepoint DLP Classifiers:** Regex and algorithmic validation for Rwanda National IDs (16 digits) and bank account numbers.
- **Linux OS Hardening Scripts:** Automated verification against CIS Level 1 & Level 2 server profiles.
- **Zero-Trust IAM:** Least privilege and PAM jumpbox requirements for database and core system access.



*hardening/rhel9_cis_baseline_audit.sh*:

bash
#!/usr/bin/env bash
# CIS Benchmark Automated Verification - Core Banking Server Baseline
echo "== Checking RHEL9 CIS Security Controls for SACCO Infrastructure =="

# 1. Verify SSH Protocol and Root Login
SSHD_ROOT=$(sshd -T 2>/dev/null | grep -i '^permitrootlogin' | awk '{print $2}')
if [ "$SSHD_ROOT" = "no" ]; then
    echo "[PASS] SSH Root Login Disabled"
else
    echo "[FAIL] Root login enabled via SSH - Violates CIS benchmark"
fi

# 2. Verify Auditd Daemon Status
if systemctl is-active --quiet auditd; then
    echo "[PASS] auditd service is active"
else
    echo "[FAIL] auditd is stopped! Required for BNR forensic auditability"
fi

# 3. Check Core Dump Disabling
if grep -q "fs.suid_dumpable = 0" /etc/sysctl.conf /etc/sysctl.d/* 2>/dev/null; then
    echo "[PASS] Core dumps disabled"
else
    echo "[WARN] Core dumps not explicitly disabled in sysctl"
fi



---
