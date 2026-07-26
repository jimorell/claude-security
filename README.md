# claude-security
A prompt to create a Claude Skill that can live in your repo and audit your code.

# Security Audit & Pentest Skill for Claude Code

A reusable, evidence-driven framework for creating a Claude Code skill that can perform comprehensive security audits and authorised penetration testing against an arbitrary local project folder.

The project is designed to reduce vague, checklist-only or unsupported AI security reviews. It requires structured reconnaissance, project-specific threat modelling, independent audit passes, evidence adjudication, explicit remediation approval, adversarial regression tests and a final verification gate.

> [!IMPORTANT]
> This project is intended only for defensive security work and authorised testing of systems you own or have explicit permission to assess.

## What this repository contains

This repository contains the creation prompt and supporting material for generating a reusable Claude Code skill named:

```text
security-audit-pentest
```

The generated skill is intended to work across projects such as:

* Web applications
* APIs and backend services
* Mobile and desktop applications
* Command-line tools
* Libraries and SDKs
* Infrastructure-as-code
* Serverless systems
* Data pipelines
* Monorepos
* Projects combining several of the above

It does not assume a particular language, framework, cloud provider, database, package manager, authentication system or testing framework.

## Why this exists

AI-assisted security reviews can easily become unreliable when they:

* Repeat generic OWASP advice without tracing the actual code
* Treat scanner output as proof
* Overstate severity without a concrete attack path
* Ignore areas where no vulnerability was found
* Invent test results or claim commands were run when they were not
* Allow multiple agents to influence one another before independent review
* Apply fixes before the user has approved them
* Declare a project “secure” despite incomplete coverage
* Lose state when a session is interrupted or compacted

This project defines a stricter operating model intended to make those failure modes harder.

## Core design

The generated skill operates as a resumable stage machine.

### Stage 0 — Bootstrap and authorisation

The skill:

* Resolves the local project root
* Records the current branch, commit and working-tree state
* Detects prior audit runs
* Inventories available tools and MCP integrations
* Establishes the authorised scope
* Separates static analysis, local code execution, active probing, network access and remote mutation permissions

Invoking the skill authorises read-only static inspection of the local project only. Active testing or access to external systems requires explicit authorisation.

### Stage 1 — Reconnaissance

The skill discovers the actual project architecture from repository evidence, including:

* Entry points and exposed interfaces
* Authentication and authorisation paths
* Tenant or workspace boundaries
* Data stores and queues
* File, process and deserialisation boundaries
* Secret-management patterns
* CI, deployment and infrastructure configuration
* Privileged operations and trust boundaries

### Stage 2 — Project-specific security guidance

The skill identifies which current standards and advisories apply to the project, such as:

* OWASP Top 10
* OWASP ASVS
* OWASP API Security Top 10
* OWASP mobile guidance
* CWE Top 25
* Language and framework security guidance
* Cloud and container hardening guidance
* Package advisories and current CVEs
* OAuth, OIDC and cryptographic guidance

General guidance must be converted into concrete checks against the discovered project.

### Stage 3 — Threat modelling

The skill creates project-specific abuse cases and maps them to:

* Assets
* Actors
* Entry points
* Trust boundaries
* Attack surfaces
* Preconditions
* Exploit hypotheses
* Potential impact
* Verification checks

### Stage 4 — Independent audit and adjudication

Each audit unit is reviewed through three distinct roles:

1. **Audit Agent A — implementation and data-flow review**
   Traces entry points, sinks, authentication, authorisation, tenant scoping, validation, storage, process execution and configuration.

2. **Audit Agent B — adversarial and abuse-case review**
   Looks for bypasses, malformed input, privilege escalation, cross-tenant access, races, replay, downgrade, parser differentials, insecure defaults and chained weaknesses.

3. **Review Agent — evidence adjudication**
   Receives both blind reports only after the initial passes are complete, opens the cited source directly, attempts to falsify findings and decides whether each claim is confirmed, plausible, rejected or requires a safe probe.

A finding cannot enter the remediation plan until it is confirmed.

### Stage 5 — Remediation plan and approval

The skill produces a plan explaining:

* What is vulnerable
* The concrete attack or failure scenario
* Where the change is required
* Why the change is necessary
* The proposed implementation
* Tests to add
* Compatibility and deployment implications
* Risks introduced by the fix
* Dependencies between fixes

The skill then stops and asks the user what to approve.

No application source, tests, dependencies, schema, infrastructure or CI configuration may be changed before approval.

### Stage 6 — Approved remediation and adversarial testing

For every approved finding, the skill must:

1. Add a non-vacuous security regression test
2. Run it against the vulnerable implementation
3. Confirm that it fails for the expected security reason
4. Preserve the vulnerable-state evidence
5. Apply the smallest coherent fix
6. Run the security test again
7. Confirm that it passes for the expected reason
8. Run relevant regression, lint, typecheck and build gates
9. Have the Review Agent validate the mitigation and test quality

### Stage 7 — Final verification and report

The skill produces a final report only after all completion gates are satisfied.

The report includes:

* Authorised scope
* Repository and revision state
* Architecture and threat-model summary
* Standards and sources consulted
* Findings and supporting evidence
* Areas examined and found sound
* Coverage gaps and unverified conclusions
* Changes made
* Security tests added
* Vulnerable-state and fixed-state results
* Deferred or blocked work
* Residual risk
* Operational follow-up
* Reproduction and verification commands

The skill must not describe a project as unconditionally “secure”.

## Evidence model

Each run creates a durable audit workspace:

```text
.security-audit/<run-id>/
├── scope.md
├── 00-state.md
├── 01-project-profile.md
├── 02-attack-surface.md
├── 03-applicable-standards.md
├── 04-threat-model.md
├── 05-audit-matrix.md
├── 06-findings.md
├── 07-coverage.md
├── 08-agent-adjudication.md
├── 09-remediation-status.md
├── 10-tool-capability-inventory.md
├── plan.md
├── report.md
├── commands.md
├── test-results.md
└── evidence/
    ├── evidence-manifest.md
    ├── agents/
    │   ├── agent-a/
    │   ├── agent-b/
    │   └── reviewer/
    └── mcp-activity.md
```

Findings, coverage records, test results and adjudication decisions reference stable evidence IDs.

The canonical skill remains reusable and immutable during an audit. Project-specific state is written only to the individual run directory.

## MCP and tool support

The generated skill can discover and use relevant MCPs and other integrations available to the current Claude Code session.

Possible uses include:

* Repository and code-hosting inspection
* Dependency and advisory research
* Database policy inspection
* Cloud and infrastructure configuration
* Container registries
* Logs and traces
* API specifications
* Browser or local application testing
* Existing issue and security-report review

Tool availability does not grant permission to access everything the tool can reach.

MCP output is treated as evidence, not proof. Material conclusions must be corroborated through repository evidence, authoritative advisories, safe local reproduction, direct configuration inspection or another independent source.

## Installation

Copy or clone the repository into a local working directory.

Use the supplied creation prompt with Claude Code from the repository in which you want the skill installed. Claude should first inspect the repository’s existing `.claude/skills/` conventions and binding project instructions.

The generated skill should be created at:

```text
.claude/skills/security-audit-pentest/
```

Review the proposed file tree and `SKILL.md` before approving creation of the remaining references, templates and scripts.

## Example requests

Once installed, requests may include:

```text
Run a security audit of this project.
```

```text
Threat-model this application and audit its authentication and authorisation paths.
```

```text
Perform an authorised local penetration test of this repository.
```

```text
Review tenant isolation and test for cross-workspace access.
```

```text
Find vulnerabilities in this API and produce a remediation plan before changing anything.
```

The exact invocation depends on the skill-discovery conventions supported by your Claude Code setup.

## Safety and authorised use

This project is intended for:

* Defensive security assessment
* Secure software development
* Authorised penetration testing
* Threat modelling
* Security regression testing
* Remediation planning and verification

Do not use it to:

* Test systems without permission
* Scan public, production, staging or third-party targets outside the approved scope
* Perform credential stuffing
* Establish persistence
* Exfiltrate data
* Create or deploy malware
* Damage systems or data
* Bypass legal, contractual or organisational controls

Users are responsible for obtaining appropriate authorisation and complying with applicable law.

## Limitations

This project does not provide:

* A guarantee that a project is vulnerability-free
* A substitute for qualified human security review
* A formal certification or compliance opinion
* Permission to test systems you do not own or control
* Assurance beyond the documented scope, evidence and tooling available during a run

Automated and AI-assisted analysis can produce both false positives and false negatives. Findings should be reviewed in context, and high-risk systems may require specialist manual testing.

## Repository status

This project is evolving. Interfaces, templates and internal artefact contracts may change before a stable release.

Until a stable version is published:

* Pin a commit or release before operational use
* Review generated skill changes before adopting them
* Treat changes to stage gates, evidence schemas and authorisation rules as security-sensitive
* Do not assume compatibility between audit runs created by different versions

## Contributing

Contributions are welcome, particularly improvements to:

* Evidence quality and traceability
* Agent isolation and adjudication
* Test validity and anti-vacuity controls
* Framework-specific security guidance
* Resumability and state validation
* Safe local testing patterns
* Documentation and examples

Before contributing:

1. Read [`CONTRIBUTING.md`](CONTRIBUTING.md).
2. Avoid including secrets, credentials, private source code or real vulnerability data.
3. Explain the failure mode addressed by the change.
4. Include tests or a structural walkthrough where applicable.
5. Preserve the project’s authorisation, evidence and approval boundaries.

By submitting a contribution, you agree that it is licensed under the Apache License, Version 2.0, on the same terms as the rest of the project.

## Reporting security issues

Do not open a public issue for a vulnerability that could put users at risk.

Follow the process in [`SECURITY.md`](SECURITY.md) to report security concerns privately.

## Licence

Copyright © 2026 James Morell.

Unless otherwise stated, this repository and all files within it are licensed under the Apache License, Version 2.0. See [`LICENSE`](LICENSE).

The project name and associated branding are not licensed for use in a way that suggests endorsement of modified or derivative versions.

## Acknowledgements

This project draws on established secure-development and application-security practices, including guidance from OWASP, CWE, NIST, language and framework maintainers, package ecosystems and cloud providers.

The generated skill is expected to consult current, authoritative sources relevant to each audited project rather than relying solely on embedded or model-memory guidance.

The project name and associated branding are not licensed for use in a
manner that suggests endorsement of modified or derivative versions.
