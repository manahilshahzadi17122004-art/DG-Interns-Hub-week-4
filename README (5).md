# Week 4 - SIEM in Practice: Splunk, Wazuh and Log Ingestion
**Intern:** Hashim Ali  |  **Program:** DG Interns Hub  |  **Date:** 15 November 2026

## 1. Goal
Ingest endpoint logs into a free SIEM, search them and turn attacker behaviour into alerts.

## 2. My setup
| Item | Detail |
|---|---|
| Machine | Windows laptop, 8 GB RAM |
| SIEM | Splunk Enterprise 10.6.1.1 (installed on the host, D: drive) |
| Forwarder | Splunk Universal Forwarder 10.6.1.1 |
| Sysmon | v15.22 with the SwiftOnSecurity config |
| Index | `lab` |
| Hardware note | Splunk was built fully. Wazuh needs 4-8 GB more RAM, so Wazuh is covered by theory and documentation only, as the task allows. |

## 3. Folder structure
```
DG Interns Hub / Week 4
|-- Notes/        Q1 to Q8 theory answers (8 separate PDFs)
|-- Writeups/     TryHackMe writeups (Introduction to SIEM, Splunk: Exploring SPL)
|-- Findings/     Screenshots, triage notes
|-- Detections/   5 SPL searches and the custom Wazuh rule
|-- Cheat Sheet/  SPL command table and SIEM alert table
|-- Week4_Report.pdf
|-- Week4_Presentation.pptx
|-- README.md
```

## 4. Hands-on summary
| Task | What I did | Evidence (Findings folder) |
|---|---|---|
| 1. Install Splunk | Installed Splunk Enterprise, created index `lab` | task1_splunk-login-page.png, task1_lab-index-empty.png |
| 2. Ingest logs | Installed Sysmon and Universal Forwarder, receiving port 9997, inputs.conf for Security and Sysmon | task2_forwarder-config.png, task2_receiving-port-9997.png, task2_search-index-lab.png, task2_sysmon-authlog-events.png, task2_sysmon-installed.png |
| 3. Wazuh | Theory and documentation only (hardware limit) | Notes/Q4 |
| 4. Attack to alert | Not completed on this machine (Kali/Ubuntu VM not run because of RAM) | - |
| 5. Detections | 5 SPL searches, 2 scheduled alerts, dashboard, Wazuh rule written | task5_search*.png, Detections folder |
| 6. Triage notes | 3 notes (failed logons, PowerShell, new user) | triage_note1 to triage_note3 |
| 7. Cheat sheets | SPL table (20 entries), alert table (14 entries) | Cheat Sheet folder |

## 5. Ingestion flow
`Windows (Security + Sysmon logs) -> Universal Forwarder (inputs.conf) -> port 9997 -> Splunk indexer (index lab) -> search head (Search & Reporting)`

## 6. Detections
| # | Detection | MITRE | File |
|---|---|---|---|
| 1 | Failed logons per user (4625) | T1110 | search1_failed_logons_4625.spl |
| 2 | PowerShell process creation (Sysmon ID 1) | T1059.001 | search2_powershell_sysmon1.spl |
| 3 | New user account (4720) | T1136.001 | search3_new_user_4720.spl |
| 4 | SSH failures per source IP | T1110.001 | search4_ssh_failures_per_ip.spl |
| 5 | Rare parent-child process pairs | T1059 | search5_rare_parent_child.spl |

Custom Wazuh rule: `Detections/local_rules.xml` (rule ID 100100, SSH brute force, T1110). It was written but not tested with wazuh-logtest because Wazuh was not installed.

## 7. Problems I solved
- Splunk website login failed while downloading the forwarder, fixed by resetting the Splunk.com password.
- The forwarder was installed in `D:\Program Files`, so inputs.conf had to be placed there.
- Sysmon events did not arrive until the forwarder service ran as Local System.
- Sysmon fields (Image, CommandLine) were not extracted, so I used `rex` on `_raw`.
- Splunk's own `splunk-powershell.exe` created noise, so I filtered it out (tuning).

## 8. TryHackMe
Introduction to SIEM and Splunk: Exploring SPL, both written up in the Writeups folder.

## 9. LinkedIn
Concept post and weekly post (with dashboard screenshot, tagging DG Interns Hub).
