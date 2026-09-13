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

> Another alert appears, now pointing to 2[.]58[.]56[.]50, a potential C2 server address. Use the learned tools and services (e.g., VirusTotal and BPG.Tools) to answer the questions.

### What country does the malicious IP resolve to?

<img width="532" height="295" alt="image" src="https://github.com/user-attachments/assets/831be153-4a99-4a88-b5bf-c71b88f013be" />

- Netherlands


##


### what C2 server is hosted behind the IP?
> Looking at VirusTotal comments

<img width="524" height="158" alt="image" src="https://github.com/user-attachments/assets/b710cf6c-803c-4a53-947d-6f606fac8931" />

- Remcos


##


### What Autonomous System does the IP belong to? (Full name)

- 1337 Services GmbH


##


### What two tags does BPG.Tools attribute to the ASN? (Tag1, Tag2)

<img width="272" height="134" alt="image" src="https://github.com/user-attachments/assets/b1160926-0905-4b58-94c0-51a598b06df1" />

- Server Hosting, Tor Services

---

## SERVICE EXPOSURE

### What remote access service is exposed?

<img width="611" height="431" alt="image" src="https://github.com/user-attachments/assets/18acc93c-ed0b-4932-a9fe-7c3af268dc1f" />

- RDP


##


### How many ports have been identified as open on the server?

- 9


##


### One of the exposed services leaks an active C2 server!
> What is the name of that C2? (E.g., Cobalt Strike)

<img width="608" height="428" alt="image" src="https://github.com/user-attachments/assets/0ac4b3af-b93e-4cc4-bb5d-4ca31918cdc8" />

- AsyncRAT


##


### For how many days is the C2 server's certificate valid?

<img width="652" height="428" alt="image" src="https://github.com/user-attachments/assets/7f2cf9af-7728-48cf-a824-ed09a51368ad" />

- 3935

---

## CHALLENGE

> An APT group has just struck your company, and the Incident Response team is working through it. As an L1 analyst, you've volunteered to help and have been tasked with gathering everything you can on a malicious domain and identifying the infrastructure the malware relies on. The domain is: raytracingengine[.]com

### What IP does the domain resolve to?

For this approach, I'll begin with using NsLookup.io for the purpose of domain enrichment - gaining further information with the provided domain name.

<img width="452" height="269" alt="image" src="https://github.com/user-attachments/assets/a62bd420-2274-4e29-a013-238e6456223f" />

<img width="874" height="423" alt="image" src="https://github.com/user-attachments/assets/22cfbfde-e89b-4a06-a041-a8e7d316d269" />
> Here, the IP address will provide an answer different than what you see in the screenshot, I believe at the time this lab was posted, things can and possibly was subject to change given the upcoming answer. The IP address in this screenshot has been validated by cross-references via dnschecker.org as well

<img width="1127" height="518" alt="image" src="https://github.com/user-attachments/assets/c77bf6f9-4ac9-4f10-9745-db23435df531" />
> I'm too lazy to defang the answer at the moment so highlighted is the IP address below the IP address that I received as an answer from two other sources.


##


### What cloud provider did the attacker use? (E.g., Amazon Cloud)

The importance here is to take that initial IP Address, search for it either using IP enrichment tools or VirusTotal (my personal simple go-to), and find the AS / ASN there. 

The confusing part about this is that when prior research led the domain to be resolved at the IP 3[.]222[.]192[.]211, the answer demands for the other IP (35[.]188[.]105[.]97) to be used. I'll have to confirm whether this is confusion on my end or simply an error on THM's side.

<img width="1109" height="158" alt="image" src="https://github.com/user-attachments/assets/c6dc1c92-174c-4229-b739-e0cccbfb85cb" />

- Google Cloud


##


### What country is the malicious server located in? (E.g., France)

- United States


##


### When was the malicious domain name created? (E.g., 24.05.2026)

- 21.02.2026


##


### According to the exposed service, what is the attack server's OS?

<img width="566" height="646" alt="image" src="https://github.com/user-attachments/assets/6793761b-87e2-4334-aaa5-53cff11d719c" />
> Service exposure info from shodan.io

- Ubuntu (Linux)

---
