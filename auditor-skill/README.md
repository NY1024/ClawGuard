# ClawGuard Auditor (CG-A)

ClawGuard Auditor is an enterprise-grade security kernel and vetting tool designed for the OpenClaw agent ecosystem. It provides comprehensive defense-in-depth by combining pre-installation static analysis with continuous runtime supervision.

## 🌟 Why ClawGuard Auditor?

Autonomous AI agents are powerful, but executing third-party skills and dynamic code introduces significant supply-chain and execution risks. ClawGuard Auditor solves this by acting as both a **firewall** and an **auditor**.

By intelligently combining the deep static analysis concepts of traditional SAST tools, the strict provenance checking of vetting checklists, and the continuous capability-based interception of a security kernel, ClawGuard provides unparalleled protection.

## ✨ Core Capabilities

### 1. Pre-Flight Vetting (SAST & Heuristics)
Before any skill is installed or executed, ClawGuard Auditor scans the source code:
- **Malware Signatures**: Detects reverse shells, obfuscated payloads (Base64/Hex), and malicious dependency typosquatting.
- **Execution Risks**: Flags dangerous system calls (`eval()`, `exec()`, `os.system`).
- **Provenance Validation**: Evaluates the trustworthiness of the skill's source (e.g., GitHub stars, author reputation) to apply the correct level of scrutiny.

### 2. Runtime Supervision & Capability Tokens
ClawGuard doesn't stop at installation. It monitors the agent's actions in real-time:
- **Capability Enforcement**: Skills must operate within a strict conceptual permission model (`CAP_FS_READ`, `CAP_NET_EGRESS`, etc.). Attempts to exceed these capabilities are blocked.
- **Zero-Trust Exfiltration Defense**: Prevents sensitive data (SSH keys, `.env` files, personal documents) from being read or transmitted over the network without explicit out-of-band user approval.
- **Prompt Injection Protection**: Intercepts and blocks conversational attempts to bypass security, reveal system prompts, or disable the auditor.

### 3. Structured Risk Classification
Actions and skills are categorized into four distinct Risk Tiers (Safe to Critical). This ensures that standard operations proceed smoothly while high-risk actions (like modifying system files or accessing credentials) are hard-blocked or require human intervention.

## 🚀 How to Use

Simply load this Skill into your OpenClaw environment. It automatically establishes itself as a persistent supervisor.

To explicitly vet a new skill or repository, ask the agent:
> *"Use ClawGuard to audit the skill located at `./downloads/new-tool`"*

The Auditor will read the files, map the required capabilities, hunt for red flags, and output a standardized **ClawGuard Audit Report** detailing the Risk Tier and the final Guardian Verdict.
