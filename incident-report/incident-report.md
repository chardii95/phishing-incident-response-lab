# Incident Report – Simulated Phishing Attack & Incident Response

## Incident ID
IR-PLAB-001

## Date
28/01/2026

## Analyst
Anthony Chard

## Environment
- Operating System: Windows 11 (ARM)
- Virtualization: UTM (macOS host)
- Monitoring Tools: Sysmon, Windows Event Viewer
- User Context: Standard user with local administrator access
- Environment Type: Isolated lab (non-production)

---

## 1. Executive Summary
A simulated phishing incident was conducted to replicate a real-world malware infection scenario. A malicious attachment disguised as an invoice was executed by a user, leading to the creation of a registry-based persistence mechanism. The activity was detected using Sysmon telemetry, investigated through log analysis, and fully remediated through containment and eradication actions. No lateral movement or further compromise was observed. The system was returned to a clean state.

---

## 2. Incident Description
The incident began when a user executed a suspicious batch file (`invoice.bat`) placed in a publicly writable directory. The script simulated attacker behaviour by establishing persistence via the Windows Registry Run key to ensure execution upon user logon. This activity was detected through host-based logging and investigated following standard SOC incident response procedures.

---

## 3. Timeline of Events

| Time (Approx.) | Event |
|---------------|-------|
| T0 | User executed `invoice.bat` from `C:\Users\Public` |
| T1 | Batch file executed via `cmd.exe` |
| T2 | Persistence added to registry Run key |
| T3 | Sysmon Event ID 13 logged registry modification |
| T4 | Analyst identified malicious persistence |
| T5 | Registry persistence removed |
| T6 | Malicious payload deleted |
| T7 | System verified as clean |

---

## 4. Detection & Analysis

### 4.1 Initial Detection
Suspicious behaviour was detected through Sysmon logging, specifically registry modification events indicating unauthorized persistence.

### 4.2 Log Evidence
- **Sysmon Event ID 1** – Process Creation  
  - Image: `cmd.exe` / `notepad.exe`  
  - Parent: `invoice.bat` execution chain  

- **Sysmon Event ID 13** – Registry Value Set  
  - Registry Path:
    ```
    HKCU\Software\Microsoft\Windows\CurrentVersion\Run\InvoiceUpdater
    ```
  - Data:
    ```
    C:\Users\Public\invoice.bat
    ```

### 4.3 Indicators of Compromise (IOCs)
- File:
