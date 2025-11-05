# Penetration Testing
# PROJECT_NAME
SSH Authentication Resilience — Hackathon Project

Author: Sneha D P 
Date: 2025-09-12  
Status: Hackathon prototype / lab demonstration / educational


⚠️ Legal & ethical notice (READ FIRST)

This repository documents a hackathon project and controlled lab research performed **only** against systems owned and explicitly authorized by the researcher. All experiments were carried out in an isolated lab environment. This project intentionally **omits** step-by-step offensive commands, scripts, or any material that could enable unauthorized access.

Do not use any part of this repository to test systems you do not own or have written permission to test. Unauthorized access to systems is illegal and unethical.


Project summary

**PROJECT_NAME** is a hackathon prototype that demonstrates how weak authentication controls and poor account hygiene can make SSH services vulnerable to automated credential-guessing at scale. The project focuses on detection, logging, and defensive recommendations — the objective is to help defenders identify suspicious activity patterns and harden systems, not to enable attacks.

During the hackathon I performed controlled experiments from an **Ubuntu Subsystem (WSL)** instance on my own machine and targeted only lab systems under my control. The repository contains sanitized findings, detection guidance, and hardening recommendations suitable for inclusion in a defensive playbook.

---

## Motivation

- Raise awareness of SSH authentication risks in real-world environments.
- Provide defenders with observable indicators and practical hardening steps.
- Produce a compact hackathon deliverable that demonstrates measurable outcomes (detection signals, log patterns, mitigation suggestions).

---

## High-level architecture / environment

> Replace the example names with your actual artifacts as needed.

- Attacker host: **Ubuntu Subsystem (WSL)** on a personal laptop (used only for lab work).
- Target host: isolated VM or container running an SSH server configured for demonstration.
- Network: isolated virtual network (no Internet exposure for the target).
- Snapshots/backups: VM snapshots were taken before testing and restored after experiments.

---

## Tools (high level, referenced for transparency)

This section lists tools referenced in the project for defensive and research context. No usage examples or command syntax are included.

- SSH server and client (protocol under test)
- Credential-guessing / automated testing tools (referenced conceptually only)
- System log inspection (`/var/log/*`, `journalctl`) and basic log parsing
- Defensive tools: `fail2ban` (or similar), firewall rules, SSH hardening options

---

## What I did (sanitized methodology)

This is a conceptual overview of the controlled experiments — intentionally non-actionable:

1. Provisioned an isolated target running an SSH service and an attacker environment using WSL.  
2. Executed controlled credential-guessing behavior inside the isolated environment to simulate automated attacks and observe system responses.  
3. Collected authentication logs and correlated events to extract detection patterns (timestamps, source IPs, frequency).  
4. Synthesized findings into detectable indicators and defensive recommendations.  
5. Restored snapshot and repeated with configuration changes to verify mitigations.

---

## Key (sanitized) findings

- Weak or default passwords drastically increase the likelihood of successful automated guesses when defensive controls are absent.
- Repeated failed authentication attempts create consistent, machine-readable log patterns that are useful for detection and alerting.
- Systems without rate limiting, account lockout, or automated containment are far more likely to be compromised by automated attacks.
- `sudo` invocations after suspicious authentication activity are a highly actionable signal for escalation and should be monitored.

> No passwords, logs, or offensive command output are included here.

---

## Demonstration / screenshots

A sanitized screenshot of the lab environment (attacker console) is included in this repo for presentation purposes. The screenshot shows a prototype test run from an **Ubuntu Subsystem (WSL)** instance.  
(You can add the screenshot file to the repo and reference it here; e.g., `demo/screenshot.png`.)

---

## Detection guidance (for defenders)

When instrumenting detection for SSH authentication risks, monitor for:

- Bursts of `Failed password` or `authentication failure` entries from a single source IP.
- Rapid sequential failures across many usernames originating from the same source.
- Significant deviation in SSH connection rate compared to the environment baseline.
- Correlation of failed authentication attempts followed by suspicious `sudo` or shell activity.

Recommended monitoring actions:

- Forward authentication logs to a centralized SIEM or log aggregator.
- Create alerts for high-frequency failed authentication patterns and for unusual `sudo` events following failed logins.
- Use thresholds relative to baseline traffic to reduce false positives.

---

## Hardening & mitigations (practical defensive steps)

Suggested defensive measures to reduce SSH attack surface:

- Enforce strong password and passphrase policies.
- Prefer SSH key-based authentication; disable password authentication where feasible.
- Implement rate limiting and connection throttling at the network or application layer.
- Use automated containment (e.g., `fail2ban` or equivalent) to block repeated offenders.
- Enable multi-factor authentication (MFA) for administrative accounts where supported.
- Restrict and audit `sudo` usage with least-privilege principles and regular log review.
- Harden `sshd` configuration (disable root login, restrict allowed users, set idle timeouts).
- Centralize authentication logs and integrate with alerting pipelines.



---

## How to responsibly reproduce (high-level)

If you wish to reproduce this study for learning or validation, follow responsible testing principles:

1. Only run experiments in isolated lab environments you control (local VMs, containers, or intentionally vulnerable images).
2. Use snapshots and backups so you can restore state after tests.
3. Limit scope and duration of tests; capture only the minimal evidence required.
4. Do not expose lab systems to the public internet while testing.
5. Share findings responsibly with owners when you identify vulnerabilities outside your lab.

---

## Next steps and optional deliverables I can add

If you want, I can prepare any of the following **without** including offensive details:

- A short slide deck summarizing the hackathon results and recommended mitigations.
- Redacted log samples annotated to show detection signals.
- Defensive `sshd_config` and `fail2ban` example configurations suitable for hardening (safe, non-offensive).
- An incident-response checklist focused on authentication brute-force events.

Tell me which of the above you want and I’ll prepare it in repo-ready format.

---

## License

This project is provided for educational and defensive purposes only. Use responsibly and ethically. The author accepts no liability for misuse or unauthorized testing.



