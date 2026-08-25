# Management Handover Report — HawkEye Incident

## Executive Summary
The HawkEye lab demonstrates a high-severity endpoint compromise. A workstation downloaded a malicious Windows executable from an external website and subsequently generated repeated outbound SMTP connections. The traffic contains evidence that the malware collected sensitive credentials and browser/account information and transmitted that information externally.

The incident should be treated as a credential-compromise event because sensitive authentication data was exposed.

## Severity
**High**

### Why?
- Malware executed/operated on a workstation.
- Credentials were collected.
- Sensitive data left the environment.
- The exfiltration repeated automatically.
- The attacker-controlled infrastructure was external.

## Three Key Recommendations

### 1. Deploy and actively monitor EDR
Use EDR to detect suspicious process execution, credential access, keylogging behavior, persistence, and unusual child processes.

### 2. Restrict untrusted downloads and outbound SMTP
Block or inspect executable downloads from untrusted sources. Prevent ordinary workstations from directly connecting to arbitrary external SMTP servers; route business email through approved infrastructure.

### 3. Strengthen credential and phishing defenses
Use secure email filtering, attachment sandboxing, user awareness training, MFA, and rapid credential-reset procedures after suspected compromise.

## Immediate Actions
1. Isolate the affected endpoint.
2. Reset credentials potentially exposed by the malware.
3. Block the identified malicious indicators.
4. Search enterprise telemetry for the same indicators.
5. Preserve forensic evidence.
6. Reimage the host if compromise cannot be confidently eradicated.

## Final Assessment
The HawkEye incident shows that network monitoring, endpoint detection, email security, and egress controls must work together. No single control is sufficient.
