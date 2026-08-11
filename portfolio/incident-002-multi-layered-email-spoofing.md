# Incident Report: Multi-Layered Email Spoofing & Mail Gateway Rule Conflict Mitigation

## Executive Summary
Investigated and remediated a targeted email impersonation attempt where an unauthorized server spoofed an external partner's domain (`partner-domain[.]com`) to target internal users. While the external domain properly maintained defensive DMARC records, a legacy configuration conflict within the internal Proofpoint mail gateway allowed the message to bypass intended quarantine blocks. 

The immediate threat was mitigated via defensive IP blocks in Microsoft 365, followed by a root-cause remediation of the conflicting mail routing rules.

---

## Technical Analysis & Identity Vector Failures

### 1. Indicators of Compromise (IoCs)
* **Source Infrastructure IP:** `154[.]127[.]53[.]78` (Defanged: `154[.]127[.]53[.]78`)
* **Spoofed Header Domain:** `partner-domain[.]com`
* **Network Owner:** Majestic Hosting Solutions, LLC / Spin Servers (AS space allocated under AFRINIC)

### 2. Authentication Evaluation
Upon reaching our perimeter, the message triggered multiple authentication failures due to the attacker's inability to control the victim domain's DNS or cryptographic keys:
* **SPF (Softfail):** The connection originated from `154[.]127[.]53[.]78`, which was absent from the partner domain's authorized sender list.
* **DKIM (None):** The message arrived with `header.d=none`, meaning no valid private key signature was present.
* **DMARC (Fail):** Because both SPF and DKIM failed domain alignment checks, the message failed DMARC. 

---

## Root Cause Analysis: The Gateway Disconnect

Although the partner domain published a protective `p=quarantine` policy, the message successfully reached a user-visible folder due to an internal rule priority conflict within our Proofpoint environment:

1. **The Restriction Rule:** A strict anti-spoofing policy was active, designed to capture and hold messages failing basic domain alignment.
2. **The Bypass Rule:** A legacy, overly broad transport bypass rule was operating simultaneously. 
3. **The Logic Failure:** The bypass rule executed with higher logical precedence than the restriction rule. Proofpoint identified the DMARC/SPF anomalies but was commanded by the internal bypass rule to skip standard quarantine protocols. 
4. **Downstream Delivery:** The message was handed down to Microsoft 365. Lacking strict drop instructions from the gateway, M365 placed the email into the user's visible folder.

---

## Remediation & Hardening Actions

### Phase 1: Tactical Containment
* **IP Network Blocking:** Executed an infrastructure-level block for `154[.]127[.]53[.]78` within the Microsoft 365 Admin Center (Tenant Allow/Block List - TABL) to neutralize the attacker's delivery vehicle without disrupting legitimate mail from the partner domain.
* **Upstream Abuse Reporting:** Submitted a formal infrastructure abuse notification to the hosting provider's compliance desk (`abuse@spinservers.com`) to initiate server de-provisioning.

### Phase 2: Gateway Hardening
* **Rule Audit & Deconfliction:** Audited the gateway's rule hierarchy. Deleted the legacy bypass rule as it was no longer necessary to implement. Ensured that the original proofpoint policy gained precedence.

---

## Key Takeaways
1. **Infrastructure Isolation Over Domain Blocks:** When dealing with real-world identities being impersonated, blocking the sending infrastructure (IP/AS) is the only way to safeguard users without stopping legitimate business communications.
2. **Beware of Configuration Creep:** Legacy bypass rules can silently blind active security features. Regular, proactive audits of mail routing logic and rule precedence are essential to maintaining edge defense integrity.
