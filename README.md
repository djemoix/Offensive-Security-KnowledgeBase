# Offensive Security & Blue Team KnowledgeBase

**A practical, structured reference by Manish Verma**

A curated collection of study notes on offensive security, security operations, detection engineering, cloud enumeration and reporting. The repository combines **red-team methodology with blue-team visibility** to make security learning easier to navigate.

> **Usage:** Educational and authorized lab / assessment environments only. Techniques and commands may require adaptation for different operating systems, tool versions and scopes.

## Start here

| Section | Topic | Notes |
| --- | --- | --- |
| 00 | [Kali Linux Master](00-Kali-Linux-Master/kali-commands.md) | Linux commands and working environment |
| 01 | [Port-wise Enumeration](01-Port-Wise-Enumeration/port-cheatsheet.md) | Service-oriented reconnaissance references |
| 02 | [Web Exploitation](02-Web-Exploitation/burp-workflow.md) | Burp Suite workflow and web testing |
| 03 | [Active Directory](03-Active-Directory/README.md) | AD reference and legacy file locator |
| 04 | [Privilege Escalation](04-Privilege-Escalation/linux-privesc.md) | Linux privilege escalation checks |
| 05 | [Pivoting & Tunneling](05-Pivoting-Tunneling/pivoting-guide.md) | Network segmentation and tunneling notes |
| 06 | [Internal Network Assessment](06-Internal-Network-Attack/internal-attack-flow.md) | Internal lab assessment flow |
| 07 | [Advanced Red Team Notes](07-Red-Team-Advanced/red-team-notes.md) | Advanced techniques and context |
| 08 | [Bug Bounty](08-Bug-Bounty/bug-bounty-methodology.md) | Scope and methodology |
| 09 | [Web Security Deep Dive](09-Web-Deep-Dive/jwt-ssrf-oauth.md) | JWT, SSRF and OAuth references |
| 10 | [SOC Analyst](10-SOC-Analyst/soc-cheatsheet.md) | Event IDs, SIEM examples and response workflow |
| 11 | [Blue Team Detection](11-Blue-Team-Detection/detection-mapping.md) | Attack-to-detection mapping and hardening |
| 12 | [Cloud Security](12-Cloud-Security/cloud-enum.md) | Cloud assessment reference |
| 13 | [Report Templates](13-Report-Templates/pentest-report-template.md) | Reporting structure |
| 14 | [Interview Preparation](14-Interview-Preparation/interview-qa.md) | Knowledge checks and Q&A |

## Recommended learning paths

**SOC / defender:** Start with [SOC Analyst](10-SOC-Analyst/soc-cheatsheet.md), continue into [Blue Team Detection](11-Blue-Team-Detection/detection-mapping.md), and practice writing incident summaries.

**Penetration testing / OSCP preparation:** Start with [Kali Linux](00-Kali-Linux-Master/kali-commands.md), [Enumeration](01-Port-Wise-Enumeration/port-cheatsheet.md), [Web](02-Web-Exploitation/burp-workflow.md) and [Privilege Escalation](04-Privilege-Escalation/linux-privesc.md), always in authorized environments.

**Purple team:** Pair an approved simulated technique with appropriate logging, alerting, investigation and hardening.

## Documentation quality notes

- Commands are reference examples, **not** universal copy-and-run instructions.
- Detection logic is illustrative and may depend on log sources, audit configuration and environment-specific field names.
- Active Directory content includes a legacy filename/path preserved to avoid breaking existing links. Use the [AD index](03-Active-Directory/README.md) to locate it.
- No live client data, credentials, secrets or unauthorized target details belong in this repository.

## Related

- [OSCP-NOTES](https://github.com/djemoix/OSCP-NOTES) — focused learning notes.
- [Manish Verma's GitHub profile](https://github.com/djemoix).

## Legal & ethical use

Use these materials only on systems you own or are explicitly authorized to assess. Follow agreed scope, applicable law and responsible disclosure practices.
