# SOC Home Lab

A personal Security Operations Center (SOC) lab built to develop hands-on experience in threat detection, incident response, and security monitoring.

## Lab Architecture

| Component | Details |
|-----------|---------|
| SIEM | Wazuh 4.x (OVA on VirtualBox) |
| Endpoint | Windows 11 (Wazuh Agent) |
| Host Machine | Windows 11, 16GB RAM |
| Network | Bridged adapter — 10.0.0.x |

## Skills Demonstrated

- SIEM deployment and configuration (Wazuh)
- Windows and Linux endpoint agent enrollment
- Real-time alert monitoring and triage
- Penetration testing (Metasploit, Nmap, Netcat)
- Vulnerability assessment and exploitation (CVE-2011-2523)
- Credential harvesting and password cracking (John the Ripper)
- Ransomware simulation and behavioral detection
- File Integrity Monitoring (FIM) realtime detection
- Incident documentation and IR playbook writing
- MITRE ATT&CK framework mapping
- PCI-DSS compliance mapping

## Incidents

| ID | Title | Severity | Date |
|----|-------|----------|------|
| IR-001 | Administrator Account Lockout via Brute Force | Medium | 2026-04-17 |
| IR-002 | vsftpd 2.3.4 Backdoor Exploitation + Credential Harvesting | Critical | 2026-04-17 |
| IR-003 | Ransomware Simulation & Incident Response Plan | Critical | 2026-04-18 |

## Tools Used

- Wazuh SIEM
- VirtualBox
- Wireshark (in progress)
- Windows Event Viewer
- PowerShell

## About

Built by Juan Alexander Alejo — final-year Telecommunications Engineering student at PUCMM, Dominican Republic. Certified Junior Blue Team Analyst (Security Blue Team).
