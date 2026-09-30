# Ubuntu Server Infrastructure Lab

A hands-on Ubuntu Server LTS infrastructure lab built from scratch in VMware — covering deployment, Linux administration, networking, remote access, services, storage, security hardening, monitoring, backup/recovery, and automation.

This lab is designed and documented as a real infrastructure project rather than a collection of isolated exercises: every phase follows Requirements → Implementation → Testing → Documentation.

> **Note:** This lab is completely independent from any physical/production server. It runs fully isolated inside VMware and is not connected to any real home infrastructure.

## Goals

- Practice real-world Linux server administration end to end
- Build a realistic, isolated infrastructure environment (not just run commands)
- Document design decisions, not just steps — including mistakes and how they were diagnosed
- Produce a portfolio-quality project that reflects actual infrastructure/DevOps thinking

## Tech Stack

- **Hypervisor:** VMware Workstation
- **OS:** Ubuntu Server 24.04.3 LTS
- **Networking:** Isolated Host-only virtual network
- **Storage:** LVM-managed disk layout

## Project Status

🟢 In progress — see [`docs/`](./docs) for phase-by-phase documentation.

| Phase | Status |
|---|---|
| 01 — Requirements & Planning | ✅ Done |
| 02 — Deployment | ✅ Done |
| 03 — Linux Administration Fundamentals | 🔜 Next |
| 04–15 | ⏳ Planned |

## Documentation

- [`docs/01-requirements.md`](./docs/01-requirements.md) — Goals, architecture, VM specs, network design
- [`docs/02-deployment.md`](./docs/02-deployment.md) — Installation, initial configuration, storage fix

## Repository Structure

```
ubuntu-infra-lab/
├── README.md
├── docs/           # Phase-by-phase documentation
├── scripts/        # Automation scripts (added from Phase 11 onward)
└── .gitignore
```
