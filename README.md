# HawkEye Red Team / Blue Team / Purple Team Analysis

## Project Overview
This repository documents a defensive security analysis of the HawkEye malware lab using the supplied `stealer.pcap` packet capture.

The analysis is divided into:
- Red Team: attack reconstruction
- Blue Team: forensic evidence and detection/containment
- Purple Team: MITRE ATT&CK mapping and defensive gap analysis
- Handover: executive summary and recommendations

> **Evidence note:** The PCAP provides strong evidence of payload download and subsequent credential/keylogging-data exfiltration. It does **not** by itself prove the original phishing email or the exact local command used to execute the downloaded executable. Those points are therefore marked as lab-context/unsupported-by-PCAP rather than invented.

## Key Findings
- Victim/internal host: `10.4.10.132`
- Malware download server: `217.182.138.150`
- Downloaded file: `tkraw_Protected99.exe`
- Download URL: `http://proforma-invoices.com/proforma/tkraw_Protected99.exe`
- Downloaded PE MD5: `71826ba081e303866ce2a2534491a2f7`
- Downloaded PE SHA-256: `62099532750dad1054b127689680c38590033fa0bdfa4fb40c7b4dcb2607fb11`
- SMTP exfiltration server: `23.229.162.69:587`
- Exfiltration protocol: SMTP submission
- Repeated behavior: approximately every 10 minutes in the capture
- Captured content includes browser/account credentials and email configuration data, indicating keylogging/credential-stealing behavior.

## Scope
This repository contains analysis of the provided lab capture only. Credentials found in the capture are intentionally not reproduced.
