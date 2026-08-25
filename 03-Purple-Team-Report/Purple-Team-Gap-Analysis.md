# Purple Team Report — Gap Analysis

## 1. Attack vs Defense

| Observed/Contextual Activity | MITRE ATT&CK | Defensive Control | Gap |
|---|---|---|---|
| Phishing delivery (lab context) | T1566 | Secure email gateway, attachment scanning, user training | Original email is not in PCAP, so exact sub-technique cannot be confirmed |
| User execution of malicious file (inferred from lab context) | T1204.002 | EDR, application control, SmartScreen, user training | No process telemetry in PCAP |
| Credential/keylogging collection | T1056.001 | EDR/behavior analytics, anti-malware | Credential collection was not prevented |
| Data collection from local sources | T1005 | EDR/DLP | Sensitive data was collected |
| SMTP-based command/control or transfer | T1071.003 | Egress filtering, mail proxy, network monitoring | Workstation could reach external SMTP |
| Exfiltration over an existing channel | T1041 | DLP, egress monitoring, EDR | Repeated outbound SMTP transfers were not blocked |

## 2. Main Security Gaps

### Gap 1 — Unrestricted executable download
The workstation retrieved a Windows executable from an external HTTP server. Downloading executable content over unencrypted HTTP should generate a high-confidence security alert.

### Gap 2 — Insufficient endpoint visibility
The PCAP cannot show the process that launched the executable. This indicates why endpoint telemetry is essential: network data alone cannot reliably establish process execution, parent-child relationships, persistence, or user context.

### Gap 3 — Direct outbound SMTP
The compromised workstation repeatedly connected to an external SMTP submission service. A normal employee workstation generally should not directly authenticate to arbitrary external SMTP servers.

### Gap 4 — Credential theft not detected
The captured outbound content contained browser/account credentials and email configuration data. EDR and behavioral detection should identify suspicious credential-access/keylogging activity.

## 3. Purple Team Improvement Plan
- Red Team should repeat the lab attack after each control change.
- Blue Team should confirm whether the download, execution, credential collection, and SMTP behavior generate alerts.
- Measure detection time and containment time.
- Tune SIEM/EDR/network rules based on observed false positives.
- Repeat the exercise until each major attack stage has a corresponding detection or prevention control.

## 4. Overall Assessment
The incident demonstrates a classic failure of layered defense: the initial payload was allowed onto the workstation, endpoint activity was not sufficiently visible, and outbound SMTP provided a convenient path for repeated data exfiltration.
