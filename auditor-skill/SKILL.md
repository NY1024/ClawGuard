# ClawGuard Skill Auditor (CG-SA)

## Overview
**ClawGuard Skill Auditor** is a comprehensive, enterprise-grade security vetting tool for OpenClaw and other autonomous agents. Before an agent is allowed to execute or install a third-party Skill, this tool performs deep static analysis (SAST) and behavioral heuristics to identify malicious intent, data exfiltration risks, privilege escalation attempts, and supply chain vulnerabilities.

Unlike basic vetters that only check for required metadata or simple keywords, `clawguard-skill-auditor` acts as an intelligent firewall, simulating the execution context and flagging high-risk code patterns.

## Features & Capabilities

1. **Deep Code Inspection (SAST):**
   - Detects dangerous function calls (`eval()`, `exec()`, `os.system()`, `subprocess.Popen(shell=True)`).
   - Identifies obfuscated code (e.g., Base64 encoded payloads, hex-encoded strings).
   - Flags unsafe file operations targeting sensitive directories (`~/.ssh`, `/etc/passwd`, `.env`, `.aws/credentials`).

2. **Network Activity & Data Exfiltration Analysis:**
   - Scans for hardcoded IP addresses, suspicious domains, and untrusted API endpoints.
   - Detects patterns associated with reverse shells (`bash -i >& /dev/tcp/...`, `nc -e /bin/sh`).
   - Identifies potential telemetry theft or unauthorized data uploads.

3. **Privilege Escalation Detection:**
   - Alerts on scripts attempting to use `sudo`, modify permissions (`chmod 777`), or alter ownership (`chown`).
   - Checks for persistence mechanisms (modifying `cron` jobs, `.bashrc`, or systemd services).

4. **Dependency Supply Chain Analysis:**
   - Parses `requirements.txt`, `package.json`, or `setup.py`.
   - Flags known malicious typo-squatted packages or outdated libraries with high-severity CVEs.

## How to Use

The auditor can be invoked by the agent to analyze a local directory containing a Skill or a remote repository.

### Command Syntax

```bash
/audit-skill <path_to_skill_directory_or_repo_url> [--strict]
```

### Examples

**Audit a local Skill directory:**
```bash
/audit-skill /Users/elwood/Desktop/downloaded-skills/suspicious-tool
```

**Audit a remote repository (requires `--strict` mode for zero-trust execution):**
```bash
/audit-skill https://github.com/unknown-author/openclaw-helper --strict
```

## Audit Report Format

When the audit completes, CG-SA outputs a structured JSON report and a human-readable summary:

```json
{
  "target": "suspicious-tool",
  "status": "REJECTED",
  "risk_score": 95,
  "findings": [
    {
      "severity": "CRITICAL",
      "category": "Remote Code Execution",
      "file": "main.py",
      "line": 42,
      "description": "Detected usage of 'os.system' with unsanitized user input."
    },
    {
      "severity": "HIGH",
      "category": "Data Exfiltration",
      "file": "utils/network.js",
      "line": 15,
      "description": "Found hardcoded connection to untrusted domain: telemetry.unknown-server.com"
    }
  ],
  "recommendation": "DO NOT INSTALL. The code contains critical vulnerabilities that allow arbitrary code execution and unauthorized data transmission."
}
```

## Installation & Configuration

To integrate this auditor into your OpenClaw environment:

1. Copy this directory (`clawguard-skill-auditor`) into your OpenClaw skills path.
2. In your OpenClaw configuration, set `clawguard-skill-auditor` as a mandatory pre-flight hook for the `install` command.

---
*Built with 🛡️ by the ClawGuard Security Team.*
