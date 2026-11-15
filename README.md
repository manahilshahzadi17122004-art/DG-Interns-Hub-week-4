# DG Interns Hub - Week 4: SIEM in Practice

**Intern:** Manahil Shahzadi
**Topic:** Splunk, Sysmon and log ingestion
**Date:** 15 November 2026

## Goal
Ingest endpoint logs into a free SIEM, search them and turn attacker behaviour into alerts.

## Setup
- Windows laptop, 8 GB RAM
- Splunk Enterprise 10.6.1.1 with a custom index named `lab`
- Splunk Universal Forwarder sending Windows Security and Sysmon logs
- Wazuh was covered by theory and documentation only because of the hardware limit

## Folder structure
| Folder | Content |
|---|---|
| Notes | Q1 to Q8 theory answers (8 separate PDFs) |
| Writeups | TryHackMe writeups: Introduction to SIEM, Splunk: Exploring SPL |
| Findings | Screenshots and 3 triage notes |
| Detections | 5 SPL searches, custom Wazuh rule and a summary PDF |
| Cheat Sheet | SPL command table and SIEM alert table |

## Ingestion flow
Windows logs (Security + Sysmon) -> Universal Forwarder -> port 9997 -> Splunk indexer (index lab) -> Search head

## Detections (SPL)
| Detection | MITRE |
|---|---|
| Failed logons per user (4625) | T1110 |
| PowerShell creation (Sysmon ID 1) | T1059.001 |
| New user account (4720) | T1136.001 |
| SSH failures per source IP | T1110.001 |
| Rare parent-child process pairs | T1059 |

## Reports
- Week4_Report.pdf
- Week4_Presentation.pptx
