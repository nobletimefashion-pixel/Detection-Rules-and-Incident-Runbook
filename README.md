# SOC Incident Response & Hardening - Task 3: Detection Engineering & Runbook

Repository for Task 3 of the SOC Analyst internship at HiveSpace Coworking, focusing on automated threat detection, Sigma rule drafting, and non-technical incident response procedures[cite: 1, 2].

---

##  Project Overview
As part of the security baseline implementation for the HiveSpace shared network (supporting 50+ freelancers)[cite: 1], this task establishes:
1. A **Sigma-compatible detection rule** targeting brute-force attacks (`MITRE ATT&CK T1110`) via Fail2ban logs.
2. A **step-by-step incident response runbook** designed to guide non-technical coworking staff on alert verification and escalation.
3. **Simulation evidence** proving the rule successfully triggers when Fail2ban blocks an unauthorized IP.

---

##  Deliverables Structure

```text
task-3-detection-and-runbook/
├── detection-rules/
│   └── fail2ban_brute_force.yml    # Sigma detection rule
├── runbooks/
│   └── incident_response_runbook.md # Staff escalation runbook
├── evidence/
│   └── fail2ban_simulation_log.png  # Terminal screenshot showing simulated failed SSH logins, fail2ban-client banning 192.0.2.50
└── README.md                        # Project documentation