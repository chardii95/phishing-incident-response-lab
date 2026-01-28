# Phishing Incident Response Lab

## Overview
This project documents a simulated **phishing-to-malware attack** conducted in a controlled Windows 11 lab environment. The objective was to replicate a realistic SOC Tier 1–2 incident, investigate attacker activity using host-based telemetry, and perform full incident response actions including identification, containment, eradication, and documentation.

The lab focuses on **hands-on detection and investigation**, not just theory.

---

## Scenario Summary
A user received a phishing email containing a malicious attachment disguised as an invoice. When executed, the attachment (`invoice.bat`) established persistence via the Windows Registry and attempted to maintain execution on system startup.

This activity was detected and investigated using **Sysmon** and **Windows Event Logs**, followed by remediation and validation.

---

## Tools & Technologies
- **Windows 11 (ARM) VM** running on macOS (UTM)
- **Sysmon (Sysinternals)** with custom configuration
- **Windows Event Viewer**
- **Command Prompt / Registry tools**
- **GitHub** for documentation and reporting

---

## Attack Flow (High Level)
1. Phishing attachment (`invoice.bat`) executed
2. Batch file created persistence via `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`
3. Payload executed from a public directory
4. Sysmon logged process creation and registry modification
5. Analyst identified malicious artefacts and persistence
6. Persistence and payload removed
7. System validated as clean

---

## Key Findings
- **Initial Execution:** `invoice.bat`
- **Persistence Mechanism:** Registry Run key (`InvoiceUpdater`)
- **Execution Path:** `C:\Users\Public\invoice.bat`
- **Relevant Sysmon Events:**
  - Event ID 1 – Process Creation
  - Event ID 13 – Registry Value Set
- **User Context:** NT AUTHORITY\SYSTEM / User session

---

## Incident Response Actions
- Identified malicious registry entry
- Deleted persistence key
- Removed malicious batch file
- Verified no further malicious activity
- Confirmed clean system state via logs

---

## Skills Demonstrated
- Phishing attack analysis
- Host-based log investigation
- Sysmon deployment and tuning
- Registry persistence detection
- Incident response lifecycle
- Evidence-based reporting
- SOC-style documentation

---

## Screenshots & Evidence
All investigation screenshots and logs are stored in the `/screenshots` directory and referenced during analysis.

---

## Disclaimer
This project was conducted **solely in a controlled lab environment** for educational and portfolio purposes. No real systems or users were harmed.

---

## Author
Anthony Chard  
Aspiring SOC Analyst | Cybersecurity Portfolio
