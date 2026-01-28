# Attack Timeline – Simulated Phishing-to-Malware Incident

## Overview
This document provides a complete attack timeline for a simulated phishing incident conducted in a controlled Windows 11 lab environment. The timeline captures attacker actions from initial execution through persistence, as well as SOC analyst detection, containment, eradication, and recovery activities.

All timestamps are logical (T0–Tn) and represent the sequence of events rather than exact clock times.

---

## T0 — Initial Access (User Execution)
- **Action:** User executed a malicious attachment disguised as an invoice  
- **Artifact:** `invoice.bat`  
- **Location:** `C:\Users\Public\invoice.bat`  
- **Context:** User-level execution  
- **MITRE ATT&CK:** T1204 – User Execution  

---

## T1 — Script Interpreter Execution
- **Action:** Batch file executed via Windows command interpreter  
- **Process Chain:** invoice.bat
→ cmd.exe
→ notepad.exe
- **Detection:**  
- Sysmon Event ID 1 – Process Creation  
- **Observation:** Parent-child process relationship confirmed  

- **MITRE ATT&CK:** T1059 – Command and Scripting Interpreter  

---

## T2 — Persistence Established
- **Action:** Registry-based persistence created to ensure execution on user logon  
- **Registry Path:** HKCU\Software\Microsoft\Windows\CurrentVersion\Run
- **Value Name:** `InvoiceUpdater`  
- **Value Data:** `C:\Users\Public\invoice.bat`  
- **Detection:**  
- Sysmon Event ID 13 – Registry Value Set  

- **MITRE ATT&CK:** T1547.001 – Registry Run Keys / Startup Folder  

---

## T3 — Detection & Alerting
- **Action:** SOC analyst reviewed host-based telemetry  
- **Findings:**
- Unauthorized registry Run key identified
- Payload located in publicly writable directory
- Persistence aligned with common malware techniques  
- **Classification:** Confirmed malicious activity  

---

## T4 — Investigation & Scoping
- **Action:** Analyst scoped the incident  
- **Scope Determined:**
- Single host affected
- No evidence of lateral movement
- No additional persistence mechanisms observed  
- **Data Sources:**
- Sysmon Event ID 1
- Sysmon Event ID 13

---

## T5 — Containment
- **Action:** Registry persistence removed to prevent re-execution  
- **Command Executed:**
```cmd
reg delete HKCU\Software\Microsoft\Windows\CurrentVersion\Run /v InvoiceUpdater /f
Result: Persistence neutralized

