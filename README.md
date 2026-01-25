# ArchRepro – Reproducible System States for Arch Linux

[![License: GPL-3.0-or-later](https://img.shields.io/badge/License-GPL%203.0%20or%20later-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Arch Linux](https://img.shields.io/badge/Arch_Linux-1793D1?logo=archlinux&logoColor=white)](https://archlinux.org/)
[![Rust](https://img.shields.io/badge/Rust-000000?logo=rust&logoColor=white)](https://www.rust-lang.org/)
[![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)](https://www.python.org/)

ArchRepro is an optional, lightweight, Arch-native reproducibility layer for Arch Linux that makes rolling-release systems verifiable and repeatable — without replacing pacman, AUR, or the Arch Way.

Allows declarative system configuration, deterministic package verification, lightweight snapshots, and drift detection — while fully respecting Arch's rolling-release model, pacman ecosystem, AUR compatibility, and minimalist philosophy.

It is not a new distro or a replacement for pacman/makepkg. It is an enhancement layer you can adopt gradually (or ignore completely) until you need strong reproducibility guarantees.

Why ArchRepro Exists

Rolling releases give power and freshness, but also introduce reproducibility pain points:
- Frequent upstream updates cause natural system drift
- AUR packages often rebuild with slightly different environments → binaries vary
- Debugging "works on my machine" issues across collaborators is frustrating
- Scientific workflows, CI pipelines, forensic analysis, and compliance demand bit-for-bit (or near bit-for-bit) reproducibility
- Supply-chain attacks make verifying package provenance increasingly important

ArchRepro aims to solve these problems natively within the Arch ecosystem, targeting:
- 90–95%+ reproducible success rate on real-world setups (including many AUR packages)
- Sub-minute configuration apply times for typical desktops/servers
- Zero overhead when not actively used
- Easy onboarding for existing Arch users (low learning curve)

Core Features
- Declarative manifests (YAML/TOML) to pin packages, versions, hashes, kernel, services, filesystem snippets, users, groups, etc.
- Deterministic rebuild engine — wraps makepkg with fixed timestamps, locales, umask, build users, SOURCE_DATE_EPOCH, etc.
- Lightweight immutable-ish snapshots (btrfs subvolumes, overlayfs, or loop devices)
- AUR reproducibility support (sandboxed rebuilds with pinned sources & dependency trees)
- Live drift detection & (optional) auto-remediation
- Cross-host / cross-architecture manifest portability (x86_64 ↔ aarch64, physical ↔ VM ↔ WSL)
- Integration points: git, systemd generators, CI/CD, monitoring exporters
- Extensible via plugins (CUDA version pinning, secure boot UKI signing, container image generation, etc.)

Installation

From AUR (recommended once packaged)
```yay -S archrepro
# or paru -S archrepro
```
From source (current development method)
```
git clone https://github.com/yourusername/archrepro.git
cd archrepro
```
## Build Rust components
```
cargo build --release
```
## Set up Python CLI environment
```
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```
## Optional: symlink CLI for easy access
```
sudo ln -s "$(pwd)/target/release/archrepro-engine" /usr/local/bin/archrepro-engine
sudo ln -s "$(pwd)/src/cli/archrepro" /usr/local/bin/archrepro
```
See DEVELOPING.md for full developer setup, code hygiene, and commenting standards.
See ROADMAP.md for planned milestones and priorities.

Quick Start

## Generate a manifest capturing your current system state (best-effort)
```
archrepro init --name stable-2026.01
```
## Edit manifest (highly recommended)
```
vim archrepro/stable-2026.01.repro.yaml
```
## Apply configuration (idempotent)
```
sudo archrepro apply stable-2026.01
```
## Create a rollback-capable snapshot
```
sudo archrepro snapshot create stable-2026.01 --backend btrfs
```
## Check for drift
```
archrepro diff stable-2026.01
```
## Verify reproducibility of key packages
```
archrepro verify --packages linux,mesa,nvidia --rebuild --verbose
```
Minimal example manifest (my-laptop.repro.yaml):
apiVersion: archrepro/v1
name: workstation-2026
description: Hyprland + NVIDIA daily driver

kernel:
  package: linux-zen
  version: ">=6.12"

packages:
  official:
    - hyprland
    - waybar
    - firefox
    - neovim
    - git
  aur:
    - visual-studio-code-bin:
        hash: sha256:...
    - spotify:
        hash: sha256:...

filesystem:
  - path: /etc/hostname
    content: arch-workstation
  - path: /etc/locale.conf
    content: LANG=en_US.UTF-8

services:
  enabled:
    - NetworkManager
    - systemd-resolved
    - bluetooth
    - earlyoom

Security & Trust Model
- Manifests are plain text — easy to review & version-control
- Package verification uses upstream reproducible.archlinux.org data when available
- Rebuilds run in isolated chroots (using systemd-nspawn or bubblewrap)
- Hash pinning prevents silent upstream substitutions
- Optional signature verification of sources & final binaries (future)
- No telemetry, no phoning home, no root-level daemons by default

Project Status – January 2026

Achieved:
- CLI skeleton (manifest parse/apply/diff)
- Deterministic makepkg wrapper (Rust)
- Basic snapshot support (btrfs + overlayfs)
- Proof-of-concept AUR rebuild sandbox

Next milestones (2026):
- v0.2 – full AUR dependency pinning + verification database integration
- v0.3 – systemd generator for boot-time enforcement
- v0.4 – GUI configurator (Tauri or iced-rs)
- v0.5 – plugin system + first domain plugins (ML, gaming, server hardening)
- v1.0 – official AUR submission + packaging
- Research paper (target: USENIX Security / OSDI / Linux.conf.au)
- Book draft: Reproducible Arch – Deterministic Systems in a Rolling World

Comparison Table

Feature                        | ArchRepro       | NixOS           | Guix            | Distrobox/Toolbox | Vanilla Arch
-------------------------------|-----------------|-----------------|-----------------|-------------------|-------------
Rolling release native         | Yes             | No              | No              | Yes               | Yes
System-wide declarative        | Yes (optional)  | Yes (mandatory) | Yes (mandatory) | No                | No
pacman & AUR compatibility     | Native          | Emulated        | Emulated        | Layered           | Native
Build-time reproducibility     | Strong          | Excellent       | Excellent       | Weak              | Improving
Runtime performance overhead   | Near-zero       | Moderate–high   | Moderate–high   | Container tax     | Zero
Learning curve (for Arch users)| Low             | High            | High            | Medium            | —
Community momentum potential   | High (Arch base)| Very high       | Moderate        | Growing           | —

FAQ
Q: Will this slow down my system?
A: No — zero overhead unless you run archrepro apply, diff, or verify.

Q: Does it lock me into old packages forever?
A: No — manifests are advisory. You can keep updating normally and only enforce reproducibility when needed.

Q: What about AUR packages that fetch git HEAD?
A: Those are marked as "partial reproducibility". You can pin commit hashes or use --force-rebuild with pinned sources.

Q: Can I use this on servers / in CI?
A: Yes — especially valuable there. Manifests are git-friendly; verification can run in CI.

License
GPL-3.0-or-later

Select performance-critical components (rebuild engine, hashing utils) may be dual-licensed under MIT/Apache-2.0 in the future to allow easier reuse.

Contributing
We welcome bug reports, feature ideas, documentation, plugins, real-world use cases, and especially pull requests!

See CONTRIBUTING.md for guidelines.

---
ArchRepro — reproducibility that doesn’t force you to abandon the Arch Way.

Made with ♡ for the Arch community.
