# ArchRepro Roadmap

This roadmap captures the **incremental path** toward a stable, high-confidence reproducibility layer for Arch Linux. Timelines are approximate and may shift as community feedback arrives.

---

## Guiding Themes
- **Determinism first**: stable builds and verifiable outputs.
- **Incremental adoption**: optional layers that do not disrupt existing Arch workflows.
- **Transparency**: clear provenance and explicit drift detection.
- **Extensibility**: plugin-based integrations for diverse workflows.

---

## Milestone 0.1 — Foundations (Current)
**Focus:** Core reproducibility primitives and CLI ergonomics.
- Manifest parsing and validation
- Deterministic makepkg wrapper
- Snapshot backends (btrfs, overlayfs)
- Basic diff + apply flows
- Prototype AUR rebuild sandbox

**Success Criteria**
- Deterministic rebuilds for ≥70% of tested packages
- CLI supports `init`, `apply`, `diff`, and `verify` end-to-end

---

## Milestone 0.2 — Reproducibility Database Integration
**Focus:** Verified packages and pinned dependency graphs.
- Integration with reproducible.archlinux.org metadata
- Hash pinning for official and AUR packages
- Dependency tree capture and lockfile support
- Improved error reporting and provenance summaries

**Success Criteria**
- Rebuild success ≥85% for official packages
- Manifest diffs highlight provenance mismatches

---

## Milestone 0.3 — Policy Enforcement at Boot
**Focus:** Early system validation and drift prevention.
- systemd generator to validate manifests at boot
- Optional blocking mode (fail boot on critical drift)
- Stable rollback integration with snapshots

**Success Criteria**
- Deterministic policy enforcement on reboot
- Zero manual intervention for rollbacks on failure

---

## Milestone 0.4 — UI + Workflow Automation
**Focus:** Usability and automation for non-experts.
- GUI configurator (Tauri or iced-rs)
- Guided onboarding workflow
- CI integration templates (GitHub Actions, GitLab CI)

**Success Criteria**
- One-click manifest creation and verification
- CI pipeline examples with reproducible artifacts

---

## Milestone 0.5 — Plugin System
**Focus:** Domain-specific extensions.
- Stable plugin API (Rust + Python)
- First-party plugins: ML stack pinning, secure boot, gaming profiles
- Signed plugin manifest support

**Success Criteria**
- 3+ plugins in active use
- Zero core changes required for adding a new plugin

---

## Milestone 1.0 — Stable Release
**Focus:** Production-ready reliability.
- Official AUR submission + packaging
- Backwards compatibility guarantees
- Comprehensive documentation and examples
- Security audit and reproducibility report

**Success Criteria**
- Rebuild success ≥95% for official packages
- Stable API and manifest schema
- Community adoption proof (≥3 independent users/organizations)

---

## Long-Term Research Track
- Formal reproducibility benchmarks
- Cross-distro reproducibility research
- Publication and academic collaboration

---

## Contribution Hooks
- Suggest new milestones in issues or PRs
- Provide reproducibility data from real systems
- Contribute plugins and domain-specific workflows
