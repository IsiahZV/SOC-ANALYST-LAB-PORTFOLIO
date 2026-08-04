# RAT Delivery

**Date:** 2026-08-04

## Executive Summary: Conversational Social Engineering & Unauthorized Remote Access

### 🌐 Incident Overview
An external threat actor initiated a highly targeted social engineering campaign against a high-value employee. The attacker built rapport through casual conversation via email before manipulating the victim into moving the communication to a video conferencing platform. 

The attacker sent a malicious link disguised as a meeting invitation. Upon interaction, the link executed a drive-by download that silently installed an unauthorized remote access tool (RAT). This instantly granted the attacker interactive command-and-control (C2) capability over the host.

### ‼️ Technical Impact
* **Initial Access**: Social Engineering / Phishing via trusted communication channels.
* **Execution**: User-executed malicious web link resulting in an unauthorized software installation.
* **Persistence & C2**: Immediate deployment of a remote management client bypassing standard host restrictions.

### 🔧 Response & Remediation
Execution of incident response plan immediately upon alert:
1. **Isolation**: Disconnected the affected host from the local network to prevent lateral movement.
2. **Remediation**: Wiped and re-imaged the asset to ensure complete eradication of the threat.
3. **Identity Containment**: Manual credential revocation via Active Directory (AD).
   * Rotated the affected user's domain and corporate credentials to invalidate any compromised session tokens.
   * Terminated active concurrent sessions across all corporate cloud services to prevent unauthorized access from secondary locations.
4. **Persistence Auditing**: Verified the user's Multi-Factor Authentication (MFA) configuration.
   * Inspected registered MFA devices in MS Admin to ensure the threat actor did not enroll unauthorized authentication methods or backup codes.

### 🦠🔬 Threat Intelligence & Malware Analysis
To verify the scope of the threat, behavioral and static analysis was performed using sandbox environments (Hybrid Analysis and VirusTotal):

* **Domain Reputation**: The malicious link utilized a newly registered domain (aged 5 days at the time of the incident). This aligns with adversary tactics of deploying short-lived infrastructure to bypass static web filters.
* **Sandbox Behavior**: Hybrid Analysis logs confirmed the URL initiated an unauthorized payload drop. The executable exhibited high-risk indicators, including process hollowing, scheduled task creation for persistence, and outbound connections to unclassified external IP addresses.
