# Windows 11 STIG Implementation – LDAP Client Signing Requirements

## Overview
This lab focused on identifying, validating, remediating, and automating a Windows 11 DISA STIG finding using Tenable Vulnerability Management and PowerShell.

The selected STIG for this remediation was:

- STIG ID: WN11-SO-000210
- Vulnerability ID: V-253463
- Rule ID: SV-253463r991589_rule

---

# STIG Description
"The system must be configured to the required LDAP client signing level."

The objective of this lab was to:
- Identify the failed STIG through Tenable compliance auditing
- Manually remediate the finding
- Validate remediation success through rescanning
- Revert the remediation to reproduce the failed state
- Automate the remediation using PowerShell
- Validate successful remediation through rescanning

---

# Technologies Used
- Windows 11
- Tenable Vulnerability Management
- PowerShell
- Local Security Policy
- DISA Windows 11 STIG
- Windows Security Configuration
- Compliance Auditing

---

# Skills Demonstrated
- Vulnerability Management
- STIG Compliance Implementation
- Security Policy Hardening
- PowerShell Automation
- Compliance Validation
- Security Remediation Workflows
- Windows Security Administration
- Registry & Policy Configuration
- Tenable Compliance Auditing

---

# Initial Vulnerability Scan

A Windows 11 STIG compliance scan was performed using Tenable Vulnerability Management with the DISA Windows 11 STIG audit policy enabled.

The initial scan identified the following failed STIG:

- WN11-SO-000210 – LDAP client signing requirements

The system was initially configured incorrectly, causing the compliance audit to fail.

## Screenshot 1 – Initial Failed STIG Scan
<img width="1013" height="297" alt="Initial_Failed_Scan" src="https://github.com/user-attachments/assets/b84a97f5-7fea-4b47-9733-b48b0cc6fe9b" />


---

# Manual Remediation

## Manual Configuration Path

The remediation was performed manually through Local Security Policy.

Navigation Path:

```text
Local Security Policy
→ Security Settings
→ Local Policies
→ Security Options
→ Network security: LDAP client signing requirements
```

## Screenshot 2 – Navigating to Local Security Policy
<img width="880" height="706" alt="Local_Security_Policy" src="https://github.com/user-attachments/assets/4b3c3fc2-fc11-4132-bf7e-b76420e1739d" />


---

# Incorrect Configuration (Failed State)

The system was originally configured as:

```text
None
```

This insecure configuration caused the STIG compliance scan to fail.

## Screenshot 3 – LDAP Client Signing Set to "None"
<img width="879" height="713" alt="Client Signing Set to None" src="https://github.com/user-attachments/assets/3bfa02fe-1a0b-464b-8ebc-75645c4efe16" />


---

# Manual Remediation (Correct Configuration)

The policy was manually changed to:

```text
Negotiate signing
```

After applying the configuration, the system policy was updated and rescanned.

## Screenshot 4 – LDAP Client Signing Set to "Negotiate signing"
<img width="480" height="552" alt="Negotiate Signing " src="https://github.com/user-attachments/assets/b4e5cbcf-eb35-46b0-8cc6-d3c5e0e454b1" />


```

---

# Validation Scan – Manual Remediation

A rescan was performed after the manual remediation.

Result:
- STIG Status: PASSED

This validated that the manual remediation was successful.

## Screenshot 5 – Passed Scan After Manual Remediation
<img width="1013" height="296" alt="Passed Scan" src="https://github.com/user-attachments/assets/e0f9dde7-4e94-4814-a001-eafe6f091637" />


```

---

# Reverting the Manual Remediation

To validate the remediation lifecycle, the policy was reverted back to the insecure state.

The setting was changed from:

```text
Negotiate signing
```

Back to:

```text
None
```

Another Tenable compliance scan was performed.

Result:
- STIG Status: FAILED

This confirmed the vulnerability could be reproduced successfully.

## Screenshot 6 – Failed Scan After Reverting Configuration
<img width="1013" height="297" alt="Failed Scan" src="https://github.com/user-attachments/assets/c0b86b06-5f79-4b62-91ec-4a1a66df2208" />


```

---

# PowerShell Remediation

After validating the failed state again, the remediation was automated using PowerShell.

## PowerShell Remediation Script

```powershell
Set-ItemProperty `
-Path "HKLM:\SYSTEM\CurrentControlSet\Services\LDAP" `
-Name "LDAPClientIntegrity" `
-Value 1

gpupdate /force
```

## Screenshot 7 – PowerShell Remediation Execution
<img width="1242" height="475" alt="PowerShell Remediation Execution" src="https://github.com/user-attachments/assets/9b9cd695-92d5-4ad5-b18d-96b79d7cb534" />


```

---

# Final Validation Scan

A final Tenable compliance scan was executed after the PowerShell remediation.

Result:
- STIG Status: PASSED

This validated successful automated remediation of the vulnerability.

## Screenshot 8 – Final Passed Scan After PowerShell Remediation
<img width="1017" height="76" alt="Passed_Scan" src="https://github.com/user-attachments/assets/d35e7b76-d6b3-4fe5-b5a8-52146340f8ea" />


```

---

# PowerShell Remediation Explanation

The script:
- Configures the LDAP client signing requirement
- Sets the LDAPClientIntegrity registry value
- Applies updated security policy settings using gpupdate

Registry Location:

```text
HKLM:\SYSTEM\CurrentControlSet\Services\LDAP
```

Registry Value:

```text
LDAPClientIntegrity = 1
```

---

# Lessons Learned

This lab demonstrated the full vulnerability remediation lifecycle including:
- Identifying failed STIG findings
- Understanding DISA STIG requirements
- Performing manual remediation
- Validating compliance success
- Reproducing failed states for testing
- Automating remediations with PowerShell
- Validating automated remediation through rescanning

The lab also reinforced the importance of:
- Compliance auditing
- Configuration validation
- Repeatable remediation workflows
- Security automation

---

# Outcome

Successfully remediated and automated the following DISA Windows 11 STIG finding:

- WN11-SO-000210 – LDAP Client Signing Requirements

The system successfully transitioned from:

```text
FAILED → MANUAL FIX → PASSED → REVERTED → FAILED → POWERSHELL FIX → PASSED
```

This project demonstrates practical vulnerability management and security hardening experience using Windows security controls, PowerShell automation, and Tenable compliance auditing.
