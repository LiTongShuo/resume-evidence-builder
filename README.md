# Resume Evidence Builder

English | [中文](README.zh-CN.md)

[![Validate Evidence Skill](https://github.com/LiTongShuo-edu/resume-evidence-builder/actions/workflows/validate.yml/badge.svg)](https://github.com/LiTongShuo-edu/resume-evidence-builder/actions/workflows/validate.yml)

`resume-evidence-builder` is an Agent Skill for organizing project materials, public links, and verification records into a structured evidence ledger, then checking whether claims are traceable before writing. It supports portfolios, project summaries, results pages, and resumes where verified facts need to be distinguished from claims awaiting confirmation.

## Features

- Record projects, requirement mappings, public evidence, verification methods, and dates in a JSON ledger.
- Use strict mode to reject definitive claims based on unverified or explicitly excluded materials.
- Check whether public HTTP(S) links in the ledger are accessible.
- Reuse redacted examples for single and multiple projects.
- Follow the Agent Skill workflow for automated or manual evidence-based writing.

## Repository structure

| Path | Contents |
| --- | --- |
| [`SKILL.md`](./SKILL.md) | Skill instructions, workflow, and claim boundaries |
| [`references/evidence-ledger.schema.json`](./references/evidence-ledger.schema.json) | Ledger data conventions |
| [`references/example-learningplusplus-ledger.json`](./references/example-learningplusplus-ledger.json) | Single-project example |
| [`references/example-public-portfolio-ledger.json`](./references/example-public-portfolio-ledger.json) | Multi-project example including unverified and excluded materials |
| [`scripts/validate_evidence.py`](./scripts/validate_evidence.py) | Ledger structure, claim status, and public-link validator |
| [`tests/`](./tests/) | Automated validation tests |

## Quick start

The scripts use only the Python standard library. A virtual environment is recommended:

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe scripts\validate_evidence.py references\example-learningplusplus-ledger.json --strict-resume
.\.venv\Scripts\python.exe scripts\validate_evidence.py references\example-public-portfolio-ledger.json --strict-resume --check-public-links
.\.venv\Scripts\python.exe -m unittest discover -s tests -v
```

## Ledger statuses

| Status | Meaning |
| --- | --- |
| `verified` | Method and results are complete; cite within the verified scope. |
| `partially_verified` | Coverage is partial; limit the claim accordingly. |
| `unverified` | No verification evidence yet; do not present as a definitive result. |
| `excluded` | Explicitly excluded for privacy, authorization, or publication boundaries. |

With `--strict-resume`, validation fails if a claim marked for inclusion in a finished document is not `verified`. `--check-public-links` checks accessibility only; it does not determine whether a link supports a claim.

## Using the skill

[`SKILL.md`](./SKILL.md) defines the workflow from inventory and ledger creation to claim validation and final review. Install it as an Agent Skill to use this workflow for evidence-based writing.

## Tests and continuous integration

GitHub Actions creates a virtual environment, validates the example ledgers and public links, and runs automated tests covering multi-project ledgers, rejection of unverified claims, excluded materials, and link enumeration.

## Public data

Examples contain only public or redacted material needed to explain the structure and rules. For actual use, independently confirm permissions, privacy boundaries, and the verification behind each claim.
