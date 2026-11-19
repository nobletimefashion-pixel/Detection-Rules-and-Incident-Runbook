# HiveSpace Coworking - Security Incident Response Runbook
**Document ID:** HS-SOC-RB-003  
**Target Audience:** Front-Desk, Administrative, and Non-Technical Staff  
**Effective Date:** November 2026  
**Security Lead:** Khan Ibrahim  

---

##  Purpose
This runbook provides clear, step-by-step instructions for non-technical staff at HiveSpace Coworking on how to handle automated security alerts triggered on the management server (specifically when **Fail2ban** blocks an unauthorized IP address for suspected brute-force attempts).

---

##  Incident Workflow: Step-by-Step Guide

### Step 1: Locate and Record the Alert
* **What happens:** When an automated threat (such as repeated failed login attempts) targets the HiveSpace management server, Fail2ban automatically blocks the offending IP address and logs the event.
* **Staff Action:** 
  1. Note the exact **IP address** flagged in the alert notification or dashboard.
  2. Note the **timestamp** (date and time) when the block occurred.
  3. *Verification:* You have successfully written down or screenshotted the IP and timestamp.

### Step 2: Identify Internal vs. External IP Source
* **Why:** Knowing whether the blocked IP comes from inside the HiveSpace building or from the public internet determines whether it is likely a member on-site or an external scanner.
* **Staff Action:**
  1. Look at the first two numbers of the blocked IP address recorded in Step 1:
     * **Internal HiveSpace LAN:** Starts with `192.168.` or `10.` (originating from a device connected to the coworking Wi-Fi or wired network).
     * **External Internet:** Starts with any other numbers (originating from outside the building).
  2. *Verification:* Record whether the IP is **Internal** or **External** in your escalation note.

### Step 3: Check for On-Site Lockout Reports
* **Why:** If the IP is internal, a coworking member or staff member may have just locked themselves out by mistyping their SSH or server credentials.
* **Staff Action:**
  1. Check if any member or staff member at the front desk or on the coworking support chat has just reported trouble connecting or being locked out.
  2. Do **not** attempt to open system log files or unblock the IP yourself; deep log inspection for targeted user accounts is handled by the Security Lead after escalation.
  3. *Verification:* Note whether a current on-site member claimed the lockout or if no one reported an issue.

### Step 4: Determine Action (Clear vs. Escalate)
* **Option A: Do Not Escalate (False Positive / Tenant Mistake)**
  * If the IP belongs to a registered tenant who confirms they locked themselves out due to a mistyped password, verify their identity, assist them with a password reset, and notify the security lead during normal hours.
* **Option B: Escalate Immediately (Potential Attack)**
  * If the IP address is completely unknown, originates from an external foreign network, or if brute-force attempts persist, **immediately escalate to the Security Lead**.
  * *Escalation Contact:* **Khan Ibrahim (Security Lead)** via the designated secure communication channel.
  * *What to Provide:* Send the recorded IP address, timestamp, and a brief description of the alert.
  * *Verification:* The security lead acknowledges receipt of the escalation.

### Step 5: Post-Escalation Actions & Handoff
* **Ownership Handoff:** Once escalated, **Khan Ibrahim (Security Lead)** assumes full ownership of the incident investigation and technical remediation.
* **What the Staff Member Does While Waiting:**
  1. **Do not unblock the IP:** Leave the Fail2ban block active; never manually lift a ban or alter firewall settings.
  2. **Monitor the Front Desk & Network:** Keep an eye out for any tenants reporting sudden Wi-Fi/network drops or strange login prompts, and log any unusual physical visitor activity if the IP originated from the local coworking subnet.
  3. **Maintain Communication:** Keep the alert dashboard open and relay any new repeat alerts from the same subnet to the Security Lead.
* **What the Security Lead Checks First:**
  1. **Verify Active Containment:** Confirm via `fail2ban-client status sshd` and UFW rules that the offending IP is actively dropped at the firewall level.
  2. **Audit Authentication Logs:** Inspect `/var/log/auth.log` or `journalctl -u sshd` around the reported timestamp to verify whether any login attempts succeeded prior to the ban (`Accepted publickey` or `Accepted password`).
  3. **Scope & Threat Intelligence:** Check if the IP is internal (a compromised tenant device on the HiveSpace LAN) or external (automated internet scanner), and apply a permanent firewall block or isolate the local port if malicious persistence is confirmed.
  4. **Closure:** Update the staff member once the threat is contained and document the incident summary.

---

##  Emergency Escalation Contacts
* **Security Lead:** Khan Ibrahim
* **Internal SOC Channel:** HiveSpace Security Operations
* **Support Desk:** support@hivespace.local