# Developing ArchRepro

This document describes engineering standards, code hygiene expectations, and the **commenting guidelines** used to keep ArchRepro understandable and reliable as it grows.

## Goals
- Keep the system **reproducible**, **reviewable**, and **safe to evolve**.
- Make it easy for new contributors to understand *why* a choice was made, not just *what* it does.
- Maintain high-quality documentation and low technical debt.

---

## Code Hygiene Standards

### 1) Project Structure
- Keep core engine logic in `src/engine/` and CLI surfaces in `src/cli/`.
- Isolate platform-specific logic behind explicit modules (e.g., `src/platform/arch/`).
- Keep YAML/TOML schema definitions next to their parsers and validators.

### 2) Dependencies
- Prefer **standard library** features before adding new crates or Python packages.
- Every dependency must be justified in code review and documented in `README.md` or `DEVELOPING.md`.
- Avoid optional dependencies unless they provide a measurable benefit.

### 3) Determinism & Reproducibility
- Any build-time or runtime **timestamp** usage must be centralized and explicitly controlled.
- Avoid nondeterministic ordering; always sort inputs and outputs before hashing or comparison.
- Do not rely on system locale or filesystem order (use explicit ordering rules).

### 4) Error Handling & Diagnostics
- All errors must include actionable context: **what failed, where, and why it matters**.
- Prefer structured error types over stringly-typed errors.
- A failure should always point to a user-fixable cause when possible.

### 5) Testing Philosophy
- Tests are **required** for parsing, validation, and diff logic.
- Deterministic build wrappers must include a reproducibility regression test.
- Prefer **fast, isolated** tests; avoid tests that depend on the network by default.

### 6) Documentation Hygiene
- Any new CLI flag or configuration option must be documented in README or CLI help.
- Behavior changes require a changelog entry (when `CHANGELOG.md` exists).
- Docs should explain **tradeoffs** and why a choice was made.

---

## Commenting Standards (Extensive Commentary)

ArchRepro uses **explicit, thoughtful commentary** to ensure reviewers understand the *intent* and *constraints* behind decisions. Comments are not just for *what* the code does — they explain *why it must be done that way*.

### When to Comment
1. **Non-obvious logic**
   - Anything that requires extra context or special knowledge should be commented.
2. **Reproducibility constraints**
   - If you are forcing deterministic behavior (timestamps, umask, locales), explain the rationale.
3. **Security-sensitive logic**
   - Any code related to verification, hashing, or isolation needs comments explaining threat assumptions.
4. **Performance tradeoffs**
   - If you chose a slower but deterministic method, document that decision.
5. **Temporary workarounds**
   - Any hack or workaround must include a TODO with a clear removal condition.

### Comment Types to Use
- **Module-level comments** for the high-level purpose and boundaries.
- **Function docstrings** describing inputs/outputs/side-effects.
- **Inline rationale comments** explaining tricky lines and ordering requirements.

### Examples

#### Module Comment
```rust
//! Deterministic build wrapper for makepkg.
//!
//! We explicitly control time, locale, umask, and build UID to guarantee
//! bit-for-bit reproducibility across hosts with the same inputs.
```

#### Function Docstring
```rust
/// Rebuilds a package with deterministic environment variables.
///
/// # Why
/// This method enforces SOURCE_DATE_EPOCH and fixed locale settings so that
/// build output hashes are stable across different machines.
///
/// # Inputs
/// - `pkg`: Package metadata and source constraints.
///
/// # Returns
/// - `BuildResult` with hashes and diagnostics.
```

#### Inline Rationale
```rust
// We sort file listings before hashing so filesystems with different
// directory iteration order produce the same digest.
let files = collect_files(root)?.sorted();
```

#### YAML Manifest Comment Example
```yaml
# NOTE: The kernel is pinned to reduce reproducibility drift.
# We accept a range only for critical security updates.
kernel:
  package: linux-zen
  version: ">=6.12"
```

### Comment Quality Checklist
- Does the comment explain **why**, not just **what**?
- Does it highlight **constraints** (determinism, security, compatibility)?
- Would a new contributor understand the reasoning without asking a maintainer?

---

## Coding Standards by Language

### Rust
- Use `rustfmt` for formatting.
- Prefer `Result<T, Error>` with rich error types.
- Avoid `unwrap()`/`expect()` in non-test code.
- Document every public function and module.

### Python
- Use type hints for public functions.
- Prefer dataclasses for structured configuration.
- Avoid dynamic typing when representing schemas.

### YAML/TOML Schema
- All fields must be documented with examples.
- Defaults must be explicit and reproducible.

---

## Recommended Review Checklist
- [ ] Determinism preserved (timestamps, locale, ordering).
- [ ] Comments explain **why** and reference constraints.
- [ ] Errors include actionable context.
- [ ] Docs updated for new user-facing changes.

---

## How to Add New Features
1. Draft a short proposal in `ROADMAP.md` or a GitHub issue.
2. Add schema changes + validation first.
3. Add deterministic behavior enforcement.
4. Add tests + docs + commentary.

---

If you are uncertain about any of these standards, **ask for clarification early**.
