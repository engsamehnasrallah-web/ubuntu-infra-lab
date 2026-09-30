# Phase 01 — Requirements & Infrastructure Planning

## Objective

Build a realistic, fully isolated Ubuntu Server LTS infrastructure lab inside VMware, to practice server administration, networking, security, and automation the way it would be done on a real infrastructure project — not as disconnected command exercises.

## Scope & Boundaries

- This lab is **completely independent** from any physical home server. No connection, no shared network, no dependency in either direction.
- A future, separate phase ("Ubuntu Home Server Infrastructure") will later apply these skills to a real physical server. That phase is explicitly out of scope here.

## Virtual Machine Specification

| Resource | Allocation |
|---|---|
| vCPU | 2 |
| RAM | 4 GB |
| Disk | 40 GB |
| OS | Ubuntu Server 24.04.3 LTS |
| Hostname | `ubuntu-infra-lab` |
| VM Name | `ubuntu-infra-lab` |

## Network Design

**Decision: Host-only network**

The lab network is configured as an isolated Host-only virtual network rather than NAT or Bridged.

**Reasoning:**
- NAT would let the VM reach the internet, but still ties it indirectly to the host's network — not the isolation this lab is meant to have.
- Bridged would put the VM directly on the physical home network, which breaks the "completely independent" requirement.
- Host-only gives full isolation by default, while still allowing a temporary NAT adapter or switch later (e.g. for `apt` updates) without changing the lab's default posture.

This decision was tested in Phase 02: `sudo apt update` failed as expected with no internet path, while `ip a` confirmed a valid DHCP-assigned address from the isolated Host-only network — confirming the isolation works as intended.

## Naming Convention

- VM and hostname kept identical (`ubuntu-infra-lab`) to avoid confusion once more VMs or lab projects exist side by side.

## Success Criteria

- A fully configured, secured, and monitored Ubuntu server with a tested backup/recovery process
- Documentation reflecting what was actually implemented, not a generic tutorial
- A GitHub repository that reads as a real infrastructure engineering project
