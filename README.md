<div align="center">

<img src="https://drivershield.io/?r=favicon.svg" width="80" height="80" alt="DriverShield">

# DriverShield

### Windows Kernel Driver Vulnerability & Malware Analysis Platform

[![Website](https://img.shields.io/badge/Platform-drivershield.io-f0b429?style=for-the-badge)](https://drivershield.io)
[![API](https://img.shields.io/badge/API-Documentation-3b82f6?style=for-the-badge)](https://drivershield.io/?r=api)
[![Status](https://img.shields.io/badge/Status-Live-22c55e?style=for-the-badge)]([https://img.shields.io]())

---

**Automated threat intelligence for Windows kernel drivers (.sys)**

Detect vulnerabilities, malware patterns, rootkit indicators, and BYOVD (Bring Your Own Vulnerable Driver) attack vectors through deep static analysis, behavioral classification, and cross-referencing against known threat databases.

</div>

---

## What is DriverShield?

<img width="1401" height="863" alt="image" src="https://github.com/user-attachments/assets/a023cca8-bee9-42c9-8c85-f9f41a065d7b" />


DriverShield is a cloud-based analysis platform purpose-built for Windows kernel-mode drivers. Upload any `.sys` file and receive a comprehensive threat assessment covering code signing validation, dangerous API usage, IOCTL attack surfaces, rootkit behavior detection, known vulnerability matching, and behavioral risk scoring.

The platform is designed for **security researchers**, **SOC analysts**, **incident responders**, and **threat intelligence teams** who need fast, reliable driver-level analysis without setting up local toolchains.

> **Note:** DriverShield is an automated analysis tool that provides risk indicators and heuristic assessments. Results should be evaluated by qualified professionals and do not constitute a definitive security verdict. See [Disclaimer](#disclaimer) for details.

---

## Analysis Capabilities

### Core Engine — 14 Analysis Stages

| Stage | Description |
|:------|:------------|
| **Hash Identification** | MD5, SHA1, SHA256 fingerprinting with known-hash lookup |
| **PE Header Analysis** | COFF headers, section entropy, version info, subsystem validation |
| **Control Flow Integrity** | CFI/CFG, ASLR, DEP, integrity check detection with mitigations scoring |
| **Code Signing** | Authenticode validation — signer identity, chain of trust, expiration |
| **Import Analysis** | 80+ kernel APIs classified by risk level — flags dangerous functions commonly used in rootkits and privilege escalation |
| **IOCTL Extraction** | Device I/O control codes with threat classification and known exploit flags |
| **YARA Matching** | 12+ built-in behavioral signatures targeting rootkit patterns, process hiding, DKOM, SSDT hooking, and MBR/VBR manipulation |
| **VirusTotal** | Automated hash lookup with per-engine detection results |
| **LOLDrivers** | Cross-reference against the Living Off The Land Drivers database |
| **CVE Mapping** | 20+ known vulnerable drivers mapped to CVE entries with CVSS scores |
| **Behavioral Analysis** | Rule-based classifier detecting rootkit behavior, anti-AV evasion, persistence mechanisms, and privilege escalation patterns |
| **Symbolic Execution** | angr-based path exploration with taint analysis (optional) |
| **Sigma Rules** | Auto-generates 6+ SIEM detection rules per analyzed driver |
| **MITRE ATT&CK** | Maps driver behavior to 16 ATT&CK techniques including Defense Evasion and Privilege Escalation |

### Rootkit & Stealth Detection

DriverShield applies heuristic analysis to identify common rootkit indicators in kernel drivers:

- **Direct Kernel Object Manipulation (DKOM)** — Detects patterns associated with process/thread list unlinking
- **SSDT / IDT Hooking** — Flags imports and code patterns related to system service descriptor table modification
- **I/O Request Packet (IRP) Hooking** — Identifies file system and network filter driver patterns that may indicate interception
- **Registry Callback Abuse** — Detects registration of notification callbacks commonly used for persistence and hiding
- **MBR/VBR Access Patterns** — Flags low-level disk access associated with bootkits
- **Anti-Forensics** — Identifies timestamp manipulation, log clearing, and trace removal techniques
- **Process & File Hiding** — Detects behavioral patterns associated with hiding processes, files, or network connections from user-mode tools

> These detections are heuristic-based and may produce false positives. Legitimate security software, anti-cheat systems, and system monitoring tools can trigger similar patterns. Always validate findings in context.

### Risk Scoring

Dynamic weighted composite score (0-100) incorporating:

```
VirusTotal (22%)  |  API Risk (18%)  |  YARA (14%)  |  IOCTLs (13%)
CVE (10%)  |  LOLDrivers (9%)  |  Signatures (5%)  |  Dynamic (5%)  |  Packing (4%)
```

Verdicts: **Clean** (0-29) · **Suspicious** (30-59) · **Vulnerable** (60-79) · **Malicious** (80-100)

---

## API

DriverShield provides a REST API for automated driver analysis workflows.

### Hash Lookup

```bash
curl -s "https://drivershield.io/?r=apilookup&sha256=HASH"
```

### Upload & Scan

```bash
curl -X POST "https://drivershield.io/?r=api/upload" \
  -H "X-Driver-MD5: YOUR_TOKEN" \
  -F "file=@driver.sys"
```

> Full API documentation: [drivershield.io/api](https://drivershield.io/?r=api)

---

## Embeddable Badge

Show the analysis status of any driver on your website or README:

```html
<img src="https://drivershield.io/?r=badge&sha256=DRIVER_SHA256" alt="DriverShield Badge">
```

---

## Use Cases

**Incident Response** — Quickly assess whether a driver found during forensic investigation exhibits known threat characteristics or exploitable vulnerabilities.

**SOC Triage** — Integrate hash lookups into SIEM/SOAR workflows via the API to auto-enrich driver-related alerts with risk context.

**Threat Intelligence** — Track BYOVD campaign tooling, map driver artifacts to MITRE ATT&CK, and generate Sigma detection rules for proactive defense.

**Vulnerability Research** — Analyze IOCTL attack surfaces, dangerous kernel API usage, rootkit-associated patterns, and missing exploit mitigations.

**Supply Chain Validation** — Verify code signing integrity, check against LOLDrivers, and cross-reference CVE databases before deploying drivers in production environments.

---

## Contact & Sponsorship

- **Platform**: [drivershield.io](https://drivershield.io)
- **Example Scan Results**: [drivershield.io]([https://drivershield.io](https://drivershield.io/DS-B41E612383F5/58a74dceb2022cd8a358b92acd1b48a5e01c524c3b0195d7033e4bd55eff4495))
- **API Docs**: [drivershield.io/api](https://drivershield.io/?r=api)
- **Contact**: [drivershield.io/contact](https://drivershield.io/?r=contact)
- **Sponsor**: [drivershield.io/sponsor](https://drivershield.io/?r=sponsor)

### Support the Project

DriverShield is independently developed and maintained. If you find it useful, consider supporting its development:

**Bitcoin:** `19XiTXwVeh9hsY6dGbStCvYN6k3mAgEXHd`

---

## Disclaimer

DriverShield is provided **"as is"** for informational and research purposes only.

Analysis results are automated heuristic assessments and **do not constitute a definitive security verdict**, legal opinion, professional advice, or certification of any kind. The platform performs **static analysis only** — uploaded files are never executed, run, or detonated.

**False positives and false negatives are expected.** Legitimate software including anti-virus products, anti-cheat engines, hardware monitoring utilities, and system management tools routinely use privileged kernel operations that may trigger elevated risk scores. A high score does not mean a driver is malicious, and a low score does not guarantee a driver is safe.

Users are solely responsible for interpreting results and any actions taken based on them. The platform does not make accusations of wrongdoing against any software vendor, developer, or organization. Risk indicators reflect automated pattern matching against known threat characteristics and do not imply intent.

Users must ensure they have the legal right to upload and analyze any files submitted. DriverShield does not knowingly facilitate intellectual property theft, unauthorized reverse engineering, or any unlawful activity.

DriverShield, its developer, and any associated parties shall not be held liable for any direct, indirect, incidental, consequential, or special damages arising from the use of or inability to use this service, including but not limited to damages for loss of data, business interruption, or reputational harm.

This platform is **not a substitute** for professional security audits, penetration testing, code review, or legal compliance assessments. Organizations with regulatory requirements should engage qualified professionals for formal evaluation.

By using DriverShield, you acknowledge that you have read, understood, and agree to these terms.

---

<div align="center">

**Developed by Tolga SEZER**

*DriverShield — Vulnerability & Malware Analysis*

</div>
