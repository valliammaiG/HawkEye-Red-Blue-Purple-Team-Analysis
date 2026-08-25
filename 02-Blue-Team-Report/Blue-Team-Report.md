# Blue Team Report — HawkEye

## 1. Incident Summary
The PCAP shows a compromised internal host communicating with external infrastructure to download an executable and later exfiltrate credential/keylogging data through SMTP.

## 2. Forensic Evidence / IOCs

| Type | Indicator |
|---|---|
| Internal victim | `10.4.10.132` |
| Payload server | `217.182.138.150` |
| Download domain | `proforma-invoices.com` |
| Payload path | `/proforma/tkraw_Protected99.exe` |
| Payload MD5 | `71826ba081e303866ce2a2534491a2f7` |
| Payload SHA-256 | `62099532750dad1054b127689680c38590033fa0bdfa4fb40c7b4dcb2607fb11` |
| SMTP server | `23.229.162.69:587` |
| Related domain observed in DNS | `macwinlogistics.in` |
| Public IP reported by malware | `173.66.146.112` |

The public IP `173.66.146.112` appears in the SMTP conversation as the host identity reported by the victim/malware. It should not be confused with the packet-level source IP, which is `10.4.10.132`.

## 3. Detection Opportunities

### Network
- Alert when a workstation downloads an executable over plain HTTP.
- Alert on access to known/suspicious newly observed domains.
- Alert when a workstation directly submits SMTP to an external server.
- Alert on repeated SMTP authentication and outbound message patterns from a user workstation.
- Monitor DNS for suspicious domains and unusual external destinations.

### Endpoint
- Monitor creation/execution of downloaded `.exe` files.
- Detect unsigned or suspicious executables launched from user-writable directories.
- Use EDR to monitor process trees, persistence, credential access, and keylogging behavior.

## 4. Containment and Removal
1. Isolate the affected endpoint from the network.
2. Block the observed malicious IP/domain indicators at appropriate controls.
3. Quarantine the identified executable by its hash.
4. Reset credentials that may have been captured.
5. Review email and browser accounts for unauthorized access.
6. Search other endpoints for the same hash, domain, IP, and SMTP destination.
7. Reimage the endpoint if malware persistence cannot be confidently removed.
8. Preserve the PCAP and endpoint evidence for investigation.

## 5. Example Sigma Rule — Suspicious External SMTP
This is a defensive starting point and should be adapted to the organization's telemetry:

```yaml
title: Workstation Direct External SMTP Submission
status: experimental
logsource:
  category: network_connection
detection:
  selection:
    destination_port: 587
  filter_internal_mail:
    destination_domain|contains:
      - "approved-mail-provider.example"
  condition: selection and not filter_internal_mail
level: medium
```

## 6. Example YARA Rule
A hash-only YARA rule is not useful by itself. A real production rule should use stable malware characteristics from the recovered sample. The sample hash above should instead be used directly in EDR/AV/hash-blocking controls.

## 7. Blue Team Conclusion
The strongest defensive opportunities were the executable download, direct external SMTP activity, repeated periodic exfiltration, and credential-stealing behavior. Endpoint telemetry plus network monitoring would substantially improve detection.
