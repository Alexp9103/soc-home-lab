# CTI-001 — Threat Intelligence Report: Scattered Spider

**Classification:** TLP:WHITE — Unrestricted Distribution  
**Date:** April 18, 2026  
**Analyst:** Juan Alexander Alejo  
**Version:** 1.0  
**Sector Relevance:** Financial Services, Retail, Telecommunications, Critical Infrastructure  

---

## Executive Summary

Scattered Spider is a financially motivated cybercriminal collective active since 2022, primarily composed of native English-speaking teenagers and young adults based in the United States and United Kingdom. Unlike traditional ransomware groups that rely on technical exploits, Scattered Spider's primary weapon is social engineering — manipulating human targets rather than attacking technical systems. Despite multiple arrests in 2024, the group remains active in 2026, having recently deployed DragonForce ransomware against major UK retailers and continuing to target financial services organizations globally.

This report provides a comprehensive threat profile for security operations teams in the financial sector, including TTPs, attack timeline, indicators of compromise, and defensive recommendations.

---

## Threat Actor Overview

| Attribute | Detail |
|-----------|--------|
| Also known as | UNC3944, Muddled Libra, Scatter Swine, Star Fraud, The Com |
| Active since | 2022 |
| Motivation | Financial gain — extortion, data theft, ransomware |
| Origin | United States, United Kingdom |
| Sophistication | High — social engineering experts |
| Primary targets | Financial services, retail, telecommunications, gaming, healthcare |
| Ransomware affiliates | ALPHV/BlackCat, RansomHub, DragonForce, Qilin |

---

## Attack Timeline

| Date | Event |
|------|-------|
| 2022 | Group emerges — focused on SIM swapping telecoms |
| Early 2023 | Pivots to ransomware and data extortion |
| June 2023 | Reddit breach — data listed on ALPHV leak site |
| Sept 2023 | MGM Resorts attack — $100M+ loss via helpdesk vishing |
| Sept 2023 | Caesars Entertainment attack — paid $15M ransom |
| Late 2023 | Targeted wave against financial services sector |
| 2024 | Multiple member arrests — Tyler Buchanan, Noah Urban |
| May 2024 | Targeted wave against food service companies |
| Aug 2024 | Transport for London attack |
| Q2 2024 | Adopted RansomHub and Qilin as new RaaS affiliates |
| April 2025 | Marks & Spencer attack via DragonForce ransomware |
| May 2025 | Multiple UK retailers targeted |
| June 2025 | FBI warns of expansion into airline sector |
| July 2025 | CISA updates advisory with new TTPs |
| Sept 2025 | UK national charged — 120 network intrusions, $115M ransom demands |

---

## Tactics, Techniques & Procedures (TTPs)

### Initial Access — The Human Attack

Scattered Spider almost never exploits software vulnerabilities for initial access. They attack people.

**Vishing (Voice Phishing)**
Attackers call IT helpdesks impersonating employees or contractors. Using publicly available information from LinkedIn and social media, they convincingly claim to be locked out of their account and request a password reset or MFA bypass. Native English fluency makes these calls highly convincing.

**Smishing (SMS Phishing)**
Victims receive SMS messages with links to fake Okta or VPN login pages. Credentials entered are captured in real time by the attacker.

**MFA Fatigue / Push Bombing**
Attackers who have stolen credentials flood the victim with MFA push notifications until the victim approves one out of frustration or confusion.

**SIM Swapping**
Attackers contact mobile carriers impersonating the victim, convince them to transfer the victim's phone number to an attacker-controlled SIM — capturing all SMS-based MFA codes.

**AI Voice Spoofing**
More recently, the group has used AI to clone victim voices, making impersonation calls even more convincing.

### Execution & Persistence

Once inside, Scattered Spider moves fast using legitimate tools — making detection difficult:

- **Remote access:** AnyDesk, TeamViewer, Fleetdeck.io, Ngrok tunneling
- **Remote monitoring:** Multiple RMM tools to maintain persistent access
- **Malware:** Spectre RAT (custom), Ave Maria RAT, WarzoneRAT
- **Living off the land:** Uses built-in OS tools to avoid triggering AV

### Lateral Movement & Collection

- Targets SaaS applications (Salesforce, Okta, Azure AD) for credential harvesting
- Exfiltrates PII, business data, and configuration files
- Moves to cloud environments for maximum impact
- Targets IT departments and helpdesk teams specifically

### Impact

- **Double extortion:** Encrypts files AND threatens to leak stolen data
- **Ransomware variants used:** DragonForce, RansomHub, ALPHV/BlackCat, Qilin
- **Average ransom demand:** Millions to tens of millions USD

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|--------|-----------|-----|
| Initial Access | Phishing — Spearphishing via Service | T1566.003 |
| Initial Access | Valid Accounts | T1078 |
| Credential Access | Multi-Factor Authentication Request Generation | T1621 |
| Credential Access | SIM Swapping | T1539 |
| Credential Access | OS Credential Dumping | T1003 |
| Persistence | Remote Access Software | T1219 |
| Defense Evasion | Use of Legitimate Tools (LOTL) | T1218 |
| Discovery | Cloud Service Discovery | T1526 |
| Collection | Data from Cloud Storage | T1530 |
| Exfiltration | Exfiltration Over Web Service | T1567 |
| Impact | Data Encrypted for Impact | T1486 |
| Impact | Financial Theft | T1657 |

---

## Indicators of Compromise (IOCs)

### Known Tools Used
- AnyDesk, TeamViewer, Fleetdeck.io (legitimate RMM tools — flag unusual use)
- Ngrok (tunneling — rarely legitimate in enterprise environments)
- Spectre RAT, Ave Maria RAT, WarzoneRAT

### Behavioral IOCs
- Helpdesk calls requesting urgent MFA bypass or password reset
- Multiple MFA push notifications in short timeframe
- New device enrollment on corporate accounts from unknown locations
- Okta or VPN login from unusual geography immediately after helpdesk call
- Mass file extension changes (ransomware indicator)
- Ngrok or other tunneling tool installation

### Infrastructure Patterns
- Lookalike domains mimicking corporate SSO pages
- Short-lived infrastructure — domains registered days before attack
- Fake Okta login pages hosted on typosquatted domains

---

## Financial Sector Specific Threat

Scattered Spider conducted a targeted wave against financial services in late 2023. Key risks for financial institutions:

- **Helpdesk impersonation** — financial firms have large IT support teams, creating more attack surface
- **SaaS exploitation** — financial firms use Salesforce, Okta, and similar platforms heavily targeted by the group
- **Regulatory exposure** — data breaches trigger PCI-DSS, SOX, and GDPR reporting obligations
- **Double extortion leverage** — customer financial data is highly sensitive, increasing ransom payment pressure
- **Cryptocurrency services** — a primary target given direct access to digital assets

---

## Defensive Recommendations

### Immediate Actions
1. Implement phishing-resistant MFA (hardware tokens, FIDO2) — eliminate SMS-based MFA
2. Train helpdesk staff to verify identity through out-of-band channels before any account changes
3. Establish a callback verification procedure — call the employee's known number, not one provided by the caller
4. Monitor for Ngrok, AnyDesk, and other RMM tool installations outside approved software list
5. Enable alerts for new device enrollment and logins from new geographies

### Detection Rules for SOC
- Alert on mass MFA push notifications to a single user within 10 minutes
- Alert on helpdesk ticket + password reset + new device enrollment within 24 hours
- Alert on Ngrok or tunneling tool execution on endpoints
- Alert on mass file extension changes (FIM rule 553/554 — as demonstrated in IR-003)
- Monitor for lookalike domain registrations matching your corporate SSO URLs

### Strategic Controls
- Implement Zero Trust Architecture — verify every access request regardless of network location
- Conduct quarterly social engineering awareness training
- Run tabletop exercises simulating helpdesk impersonation scenarios
- Review and restrict SaaS application permissions regularly
- Maintain immutable offline backups — ransomware targets network-accessible backups

---

## Intelligence Sources

- CISA Advisory AA23-320A (updated July 2025)
- FBI Joint Advisory — Scattered Spider TTPs
- Flashpoint Threat Intelligence — Scattered Spider Profile (2026)
- Google Cloud / Mandiant — UNC3944 Hardening Guidance
- Darktrace — Scattered Spider Evolving TTPs (2025)
- HHS HC3 Threat Actor Profile (2024)
- Huntress Threat Library

---

## Analyst Assessment

Scattered Spider represents a **high and persistent threat** to financial services organizations in 2026. Despite law enforcement actions resulting in multiple arrests, the group's decentralized structure and continuous recruitment through Telegram channels has prevented meaningful disruption. The adoption of AI voice spoofing in recent campaigns signals a significant evolution that will make traditional verbal verification procedures insufficient.

The group's pattern of sector-specific targeting waves — telecoms → finance → food services → retail — suggests financial services organizations should maintain elevated alert posture, particularly regarding helpdesk security controls. The combination of social engineering for initial access with ransomware for impact creates a threat that cannot be addressed through technical controls alone — security awareness and human verification procedures are equally critical.

**Confidence level:** High  
**Next review date:** July 2026
