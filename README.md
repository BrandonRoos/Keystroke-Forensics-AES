# Keystroke Logging & Detection Study (AES-Encrypted)
<img src="https://img.shields.io/badge/-Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
<img src="https://img.shields.io/badge/-MITRE%20ATT%26CK%20T1056.001-E11B22?style=for-the-badge&logoColor=white" />

> **Educational security research.** This project studies how keystroke-logging malware behaves so it can be reliably detected and investigated. It is intended for use only on systems you own or are explicitly authorized to test. Do not deploy against any device without consent.

## Overview

Keystroke logging is one of the oldest and most common data-collection techniques used by malware — mapped to **MITRE ATT&CK T1056.001 (Input Capture: Keylogging)** under the Collection tactic. To detect a technique well, it helps to understand how it actually works.

This project implements a basic keystroke logger in Python that records input locally and encrypts its output with AES (via the `cryptography` library), then pairs that with the part that matters for a defender: **how this activity looks from a SOC's perspective and how to catch it.** The encryption piece mirrors a real-world tactic attackers use to obscure captured data at rest, which is exactly the kind of artifact an analyst needs to recognize during an investigation.

## What It Demonstrates

- How input-capture malware collects and stores data
- Why attackers encrypt captured data locally (to evade content-based detection and complicate forensics)
- The host and behavioral artifacts this activity generates
- How to map an observed technique to MITRE ATT&CK and build detection logic around it

## How a SOC Would Detect This

This is the core of the project — turning the technique into something defensible. The signals fall into a few buckets:

### Process & behavioral indicators
- An unrecognized process registering a global keyboard hook or continuously polling keyboard state is a strong behavioral signal. On Windows this typically means low-level keyboard hook APIs; on Linux it means a process reading from input devices it has no business touching.
- A script interpreter (e.g., `python.exe`) spawned from an unusual location or running persistently in the background warrants inspection.

### File system artifacts
- A steadily growing log file in a temp, hidden, or user-profile directory — especially one whose contents are encrypted or otherwise unreadable — is a classic collection artifact.
- File Integrity Monitoring (FIM) in a SIEM such as Wazuh can flag the creation and continuous modification of such a file.

### Endpoint telemetry (Sysmon + SIEM)
- **Sysmon Event ID 1 (Process Create):** detect interpreters or unsigned binaries launching from unexpected paths.
- **Sysmon Event ID 11 (File Create):** detect writes to suspicious output locations.
- **Sysmon Event ID 13 (Registry Modify):** detect persistence entries (e.g., Run keys) if the logger installs itself to survive reboot.
- Forwarding this telemetry to a SIEM and alerting on the combination — new process + persistent file growth + autostart entry — is far more reliable than any single indicator.

### Detection engineering takeaway
No single event proves a keylogger; the value is in correlation. A detection rule that fires on *the combination* of an unsigned/interpreted process, a continuously growing encrypted file, and a persistence mechanism produces a high-confidence alert with low noise.

## Project Files

- The logging script (capture + AES encryption of output)
- Supporting key/encryption handling

## Requirements

- Python 3.x
- The `cryptography` library (`pip install cryptography`)

## Ethical Use & Scope

This repository exists for learning and detection research. Keystroke logging without authorization is illegal in most jurisdictions. Use it only in a lab you control or with explicit written permission, and never against systems or people without consent.
