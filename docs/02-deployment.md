# Phase 02 — Ubuntu Server Deployment

## VM Creation

Created via VMware Workstation using **Custom (advanced)** configuration (not Typical), to control every hardware setting explicitly rather than accept defaults.

- Virtual hardware: 2 vCPU, 4 GB RAM, 40 GB disk
- Network adapter: Host-only
- Installer source: Ubuntu Server 24.04.3 LTS ISO

## Installation Choices

| Setting | Choice | Reasoning |
|---|---|---|
| Hostname | `ubuntu-infra-lab` | Kept identical to the VM name to avoid confusion as the lab environment grows |
| Username | `sameh` (non-root, with sudo) | Following least-privilege practice — no direct root login |
| Disk partitioning | Use entire disk | Acceptable for a lab/portfolio environment; a production setup would separate `/var`, `/home`, etc. (to be explored in Phase 07 — Storage) |
| OpenSSH Server | Not installed at setup | Installed manually in Phase 05, to go through the full configuration process rather than accept it out of the box |
| Featured server snaps | None selected | Services will be added deliberately, phase by phase, instead of installed blindly |

Installation completed without errors.

## Initial System Verification

Ran a first round of checks after login:

```bash
sudo apt update
ip a
df -h
free -h
```

**Findings:**

- `apt update` failed — expected, since the lab network is Host-only with no internet path (confirms network isolation is working as designed).
- `ip a` initially showed no IPv4 address on `ens33`, only a link-local IPv6 address. Investigated whether the Host-only virtual network had DHCP enabled; after running `sudo dhclient ens33`, the interface received a valid address (`192.168.20.128/24`) from the Host-only network's own DHCP service — confirming the isolated network itself was functioning correctly.
- `free -h` confirmed 4 GB RAM visible to the system, as allocated.

## Storage Issue: LVM Free Space Not Allocated

`df -h` showed only **19 GB** available on `/`, despite a 40 GB disk being allocated.

### Diagnosis

```bash
lsblk
sudo vgs
sudo lvs
```

This showed:
- Disk (`sda`): 40 GB, partitioned into boot partitions + a 38 GB LVM physical volume
- Volume Group (`ubuntu-vg`): 38 GB total, but only **19 GB allocated** to the `ubuntu-lv` logical volume — the remaining 19 GB sat as unallocated free space inside the VG

This is the default behavior of the Ubuntu installer's "Use entire disk" option with LVM: it allocates roughly half the disk to the root logical volume by default, leaving the rest as free space inside the volume group (intended for future logical volumes, snapshots, etc.).

### Decision

Chose to extend `ubuntu-lv` to use the full 38 GB, rather than keep the free space reserved.

**Reasoning:** In a production environment, keeping free space in reserve is often the safer default — it provides a fast way to grow `/` if it fills unexpectedly (e.g. a log spike), without needing to add a new disk. In this lab/portfolio context, there's no production load competing for that safety margin, so using the full capacity up front avoids running into avoidable space constraints later in the project.

### Fix

```bash
sudo lvextend -l +100%FREE --resizefs /dev/ubuntu-vg/ubuntu-lv
```

`--resizefs` extends both the logical volume and the ext4 filesystem in a single command (online, no unmount/reboot required), avoiding the risk of extending the LV and forgetting to grow the filesystem in a separate step.

### Verification

```bash
df -h
sudo lvs
```

Confirmed `ubuntu-lv` grew from 19 GB to 38 GB, and `/` now reports the full capacity.

## Outcome

Phase 02 complete: VM deployed, network isolation verified, and storage fully allocated to the root filesystem.
