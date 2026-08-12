# Incident Response Write-up: Suspicious Attachment Triage & Discrepancy Analysis

A documented technical triage of a phishing email containing a suspicious PDF attachment. This repository details the safe handling, hash extraction, and cryptographic analysis of an altered payload.

## Incident Overview
* **Initial Report:** Recipient received an email containing a suspicious 279 KB attachment named `678901.pdf`.
* **The Problem:** The local file size on disk registered as 0 bytes, creating a discrepancy with the email client's reported size.
* **The Verdict:** The original payload was successfully neutralized/stripped by automated perimeter defenses mid-transit, leaving an empty 0-byte file shell.

## Triage Workflow
1. **Isolation:** Established a remote session and immediately disabled File Explorer and Outlook preview panes to prevent auto-execution.
2. **Safe Extraction:** Downloaded the file explicitly via "Save As" without triggering system execution.
3. **Cryptographic Hashing:** Generated an SHA-256 fingerprint using Windows PowerShell:
   ```powershell
   Get-FileHash -Algorithm SHA256 "~\Downloads\678901.pdf"
   ```
4. **Threat Intelligence Lookup:** Queried the hash via VirusTotal and Hybrid Analysis.

## Key Findings & Analysis
* **The Hash Discrepancy:** The extracted hash mapped universally to a 0-byte (empty) file structure. This explains why automated engines marked the file "Clean" while community security analysts downvoted the hash (due to its association with malicious bundling and system template naming conventions like `INSTALL.LOG`).
* **Attacker Intent:** The original 279 KB size strongly implies a malicious payload was initially transmitted, likely aiming to execute an exploit or test email filter rules for follow-up phishing campaigns.

## Remediation Actions Taken
* [x] Disabled preview configurations.
* [x] Shift-Deleted the 0-byte payload stub from local storage.
* [x] Purged the source email from the mail client and cleared deleted folders.
* [x] Escallated the case details to the client's internal domain IT Administrators for server-side log review.
