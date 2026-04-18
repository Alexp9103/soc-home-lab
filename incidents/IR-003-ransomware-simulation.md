# IR-003 — Ransomware Simulation & Incident Response

**Date:** 2026-04-18  
**Analyst:** Juan Alexander Alejo  
**Severity:** Critical  
**Status:** Contained (Lab Environment)  
**Agent:** kalitestpucmm (Kali Linux)  

---

## Summary

A ransomware attack was simulated on a Kali Linux endpoint monitored 
by Wazuh SIEM. The simulation replicated core ransomware behavior — 
mass file encryption (extension replacement) and ransom note deployment. 
Wazuh File Integrity Monitoring (FIM) detected the attack in real time, 
generating 19 alerts within 5 minutes including file deletion, file 
creation, and host-based anomaly detection events.

---

## Attack Simulation

### Ransomware behavior replicated
- 20 victim files created in /home/kalitestpucmm/documents/
- All files renamed from .txt to .encrypted (simulating encryption)
- Ransom note dropped: README_RANSOM.txt and /tmp/RANSOM_NOTE.txt

### Commands executed
```bash
mkdir -p /home/kalitestpucmm/documents
for i in $(seq 1 20); do echo "Sensitive data file $i" > file$i.txt; done
for f in *.txt; do mv "$f" "${f%.txt}.encrypted"; done
echo "YOUR FILES HAVE BEEN ENCRYPTED - SEND 1 BTC" > README_RANSOM.txt
```

---

## Detection

**Tool:** Wazuh SIEM — File Integrity Monitoring (FIM) realtime  
**Agent:** kalitestpucmm  
**Total alerts:** 19  
**Detection time:** < 2 minutes from attack start  

### Alerts triggered

| Rule ID | Description | Level | Count |
|---------|-------------|-------|-------|
| 553 | File deleted | 7 | Multiple |
| 554 | File added to system | 5 | Multiple |
| 510 | Host-based anomaly detection | 7 | 1 |
| 5502 | PAM login session closed | 3 | 1 |

### Detection pattern
Mass file deletions immediately followed by mass file additions 
with .encrypted extension — classic ransomware behavioral signature 
detectable via FIM realtime monitoring.

---

## Incident Response Plan

### Phase 1 — Identification (0-15 minutes)
- [ ] Confirm alert in SIEM — verify FIM rule 553/554 mass trigger
- [ ] Identify affected endpoint and user account
- [ ] Check process list for suspicious encryption processes
- [ ] Verify ransom note presence in common directories

### Phase 2 — Containment (15-30 minutes)
- [ ] Immediately isolate affected endpoint from network
- [ ] Disable user account to prevent further spread
- [ ] Block outbound C2 communication at firewall
- [ ] Snapshot VM/system before any changes
- [ ] Notify security team and management

### Phase 3 — Eradication (30-120 minutes)
- [ ] Identify ransomware strain via ransom note and file extension
- [ ] Search for persistence mechanisms (cron, startup, registry)
- [ ] Remove malicious process and associated files
- [ ] Scan all other endpoints for indicators of compromise (IOCs)
- [ ] Check for lateral movement to other systems

### Phase 4 — Recovery (2-24 hours)
- [ ] Restore files from last clean backup
- [ ] Verify backup integrity before restoration
- [ ] Rebuild endpoint from clean image if needed
- [ ] Re-enable network access after clean bill of health
- [ ] Monitor restored system closely for 48 hours

### Phase 5 — Lessons Learned (24-72 hours)
- [ ] Document full attack timeline
- [ ] Identify initial access vector
- [ ] Review backup frequency and coverage
- [ ] Implement additional FIM rules for early detection
- [ ] User awareness training on phishing/ransomware

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|--------|-----------|-----|
| Impact | Data Encrypted for Impact | T1486 |
| Defense Evasion | Masquerading — rename files | T1036 |
| Discovery | File and Directory Discovery | T1083 |
| Collection | Data from Local System | T1005 |

---

## Recommendations

1. Deploy FIM realtime monitoring on all endpoints
2. Set automated isolation on mass file change detection (>10 files in 60s)
3. Maintain offline backups — ransomware targets network-accessible backups
4. Implement application whitelisting to block unauthorized encryption tools
5. Deploy EDR solution for behavioral ransomware detection
6. User training — most ransomware enters via phishing

---

## Evidence

Screenshots: Wazuh dashboard showing 19 FIM alerts, file deleted/added pattern
