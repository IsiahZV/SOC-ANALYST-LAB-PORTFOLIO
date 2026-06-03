# IP and Domain Threat Intel
**Date:** 2026-05-24

**Objective:**
- Understand IP and domain threat intelligence for a SOC.
- Geolocate IPs and interpret their Autonomous System Numbers (ASNs).
- Detect red-flag infrastructure via Shodan/Censys service banners.
- Assess reputation with various tools.
- Enrich domains with WHOIS age, DNS records, and certificate transparency.

##

**Scenario:**
It is Wednesday morning. The SOC has flagged two suspicious domains in phishing emails and three IP addresses in outbound proxy logs. You are tasked with triaging all seven artefacts, enriching them with context, and recommending actions with expiry.

- advanced-ip-sccanner[.]com
- 166[.]1[.]160[.]118
- 64[.]31[.]63[.]194
- 69[.]197[.]185[.]26
- 85[.]188[.]1[.]133

---

## IP BUILDING BLOCKS

### From the downloadable report, what are the IP addresses for the A Record associated with our flagged domain, advanced-ip-sccanner[.]com? Answer: IP-1, IP-2.

<img width="2186" height="1152" alt="image" src="https://github.com/user-attachments/assets/1ee739c5-26f1-4a99-85f7-c648d2646ecf" />

- 172.67.189.143,104.21.9.202


##


### What nameserver addresses are associated with the IP address? Defang the addresses.

The nameserver addresses are going to be located further down under "NS Records"

<img width="2192" height="1162" alt="image" src="https://github.com/user-attachments/assets/17c84231-0d55-4e5a-afa8-e8484591fa91" />

- jaziel[.]ns[.]cloudflare[.]com, summer[.]ns[.]cloudflare[.]com

**🧠 Why this is important:**

Performing this analysis helps analyst understand the infrastructure behind the malicious activity

> Chain
<img width="354" height="92" alt="image" src="https://github.com/user-attachments/assets/8cbfdd4b-e6bc-4b77-9603-968d0c1a9f91" />

> Example
<img width="264" height="96" alt="image" src="https://github.com/user-attachments/assets/e0329216-c1ad-4928-b39c-484c2e42d882" />


This analysis aids in infrastructure correlation, where if multiple flagged domains share the same DNS provider, nameserver, or patterns, the analyst may be able to presume that it belongs to the same attacker / malware operation. When Identifying shared nameservers, analyst can identify phishing sites, C2 domains, staging servers, and exfiltration infra.. It can also attacker-preffered providers. 

Because campaigns can utilize rotating IPs and other methods to evade detection, analyzing nameservers can reveal botnet behavior, domain rotation tactics, and fast-flux DNS. 


---


## IP ENRICHMENT: GEOLOCATION AND ASN

