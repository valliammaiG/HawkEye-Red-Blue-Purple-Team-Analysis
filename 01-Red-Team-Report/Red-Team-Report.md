# Red Team Report — HawkEye

## 1. Objective
Reconstruct the observed attack chain from the packet capture and explain how the malware reached the victim, executed, collected data, and exfiltrated it.

## 2. Attack Chain

### 2.1 Initial Delivery
The internship brief describes phishing as the delivery mechanism. However, the supplied PCAP begins after the victim is already communicating with the malicious infrastructure. No original phishing email is visible in this PCAP.

**Lab-context conclusion:** phishing may be the initial access mechanism, but the exact email, attachment, sender, and delivery event cannot be proven from this capture alone.

### 2.2 Payload Download
The victim `10.4.10.132` requested:

`GET /proforma/tkraw_Protected99.exe HTTP/1.1`

The HTTP `Host` header was:

`proforma-invoices.com`

The server responding to the request was `217.182.138.150`.

The server returned an executable with:
- Content-Type: `application/x-msdownload`
- Content-Length: `2025472` bytes

The extracted PE has:
- MD5: `71826ba081e303866ce2a2534491a2f7`
- SHA-256: `62099532750dad1054b127689680c38590033fa0bdfa4fb40c7b4dcb2607fb11`

### 2.3 Execution
The PCAP demonstrates that the downloaded executable was subsequently associated with malicious behavior, but it does not contain Windows process-creation telemetry. Therefore the exact command used to launch it cannot be established from the PCAP alone.

Do not claim a specific `cmd.exe`, PowerShell, Run key, or other command unless it is supported by the HawkEye lab's host evidence.

### 2.4 Credential Collection
The capture contains repeated outbound messages whose subject identifies the malware as a HawkEye keylogger/Reborn v9 and whose body contains captured browser/account credentials and email configuration data.

This is strong evidence of credential-stealing/keylogging functionality.

### 2.5 Exfiltration
The compromised host repeatedly connected to `23.229.162.69:587` and authenticated to an SMTP service before sending the captured data.

The traffic shows:
- SMTP `AUTH LOGIN`
- `MAIL FROM`
- `RCPT TO`
- `DATA`
- successful server acceptance

The messages were repeatedly sent at roughly ten-minute intervals, indicating automated collection and exfiltration.

## 3. Attack Timeline
1. Suspected phishing/delivery — lab context; not visible in supplied PCAP.
2. Victim downloads `tkraw_Protected99.exe` over HTTP.
3. Executable is present on the victim.
4. HawkEye collects credentials/keylogging data.
5. Malware authenticates to an external SMTP server.
6. Captured data is sent through SMTP.
7. The process repeats periodically.

## 4. Red Team Assessment
The attacker successfully achieved payload delivery, execution/implant activity, credential collection, and repeated exfiltration. The major weakness was the absence of effective controls preventing an untrusted executable download and/or detecting the subsequent unusual SMTP behavior.
