# MITRE ATT&CK Mapping

| Technique | ID | Relevance |
|---|---|---|
| Phishing | T1566 | Described by the lab brief as the delivery mechanism; not directly visible in the supplied PCAP |
| User Execution: Malicious File | T1204.002 | Consistent with execution of a downloaded executable; exact execution telemetry is absent from PCAP |
| Input Capture: Keylogging | T1056.001 | Supported by the captured HawkEye keylogger data |
| Data from Local System | T1005 | Captured browser/account and email configuration data was exfiltrated |
| Application Layer Protocol: Mail Protocols | T1071.003 | SMTP was used for repeated external communication |
| Exfiltration Over C2 Channel | T1041 | Captured data was transferred over the malware's external communication channel |

> Mapping should be treated as an analytical assessment. The PCAP strongly supports the network/exfiltration observations, while host-level execution and initial phishing require additional lab evidence.
