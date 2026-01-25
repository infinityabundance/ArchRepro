# ArchRepro – Reproducible System States for Arch Linux

[![License: GPL-3.0](https://img.shields.io/badge/License-GPL%203.0-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Arch Linux](https://img.shields.io/badge/Arch_Linux-1793D1?logo=archlinux&logoColor=white)](https://archlinux.org/)
[![Rust](https://img.shields.io/badge/Rust-000000?logo=rust&logoColor=white)](https://www.rust-lang.org/)
[![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)](https://www.python.org/)

**ArchRepro** is a lightweight, user-centric reproducibility layer built specifically for Arch Linux. It enables declarative system snapshots, deterministic package verification, and exact environment recreation — all while preserving Arch's rolling-release philosophy, minimalism, and user freedom.

Unlike full declarative distros (NixOS, Guix), ArchRepro does **not** replace pacman, makepkg, or the Arch Build System. Instead, it adds an optional, powerful reproducibility superstructure that works with — and enhances — the tools you already know and love.

## Why ArchRepro?

Arch Linux excels at giving users complete control and the latest software. But rolling releases introduce natural challenges:

- System states drift over time → hard to reproduce bugs or benchmarks
- AUR packages vary in build environment → difficult to verify exact binaries
- Collaborative work, CI/CD, scientific computing, and forensics demand bit-for-bit reproducibility
- Supply-chain trust becomes increasingly critical in 2026+

ArchRepro bridges these gaps without forcing a paradigm shift. It aims for:

- ~90–95% reproducible success rate on real-world systems (including many AUR packages)
- Sub-minute apply times for small-to-medium configurations
- Zero runtime overhead when not in use

## Core Features

- **Declarative system manifests** (YAML/TOML) — pin exact packages, versions, hashes, kernels, services, dotfiles, etc.
- **Deterministic rebuild verification** — extends makepkg with fixed timestamps, locales, and environment normalization
- **Lightweight snapshots** — using overlayfs / btrfs subvolumes / loop devices (no full container tax)
- **AUR-aware reproducibility** — optional sandboxed rebuilds with pinned sources & dependencies
- **Drift detection & auto-remediation** — compare live system vs. manifest
- **Cross-architecture & cross-host portability** — share manifests between x86_64, aarch64, even WSL
- **Integration hooks** — git, CI (GitHub Actions, GitLab CI), systemd generators, monitoring
- **Plugin ecosystem** — domain-specific extensions (ML/CUDA pinning, secure boot UKI repro, etc.)

## Quick Start

```bash
# Install from AUR (once available) or from git for now
yay -S archrepro-git

# Initialize a new manifest from current system (best-effort)
archrepro init --name my-stable-2026

# Edit the generated archrepro/my-stable-2026.repro.yaml
vim archrepro/my-stable-2026.repro.yaml

# Apply (idempotent — safe to run multiple times)
sudo archrepro apply my-stable-2026

# Create a lightweight snapshot (btrfs or overlayfs)
sudo archrepro snapshot create my-stable-2026 --backend btrfs

# Verify reproducibility of installed packages
archrepro verify --packages linux,mesa,vulkan-intel --rebuild

# Compare live system against manifest
archrepro diff my-stable-2026
