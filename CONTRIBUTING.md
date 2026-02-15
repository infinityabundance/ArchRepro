# Contributing to ArchRepro

Thank you for your interest in contributing to ArchRepro! This document provides guidelines for contributing to the project.

---

## Project Status

⚠️ **Current Phase:** Pre-Alpha / Documentation & Planning

ArchRepro is currently in the early planning and foundation phase. While comprehensive documentation exists, implementation work is just beginning. This is an **excellent time to contribute** as you can help shape the project from the ground up!

See [IMPLEMENTATION_STATUS.md](IMPLEMENTATION_STATUS.md) for detailed information about what exists vs what needs building.

---

## How to Contribute

### For Developers

#### 1. Getting Started
```bash
git clone https://github.com/yourusername/archrepro.git
cd archrepro

# Read these documents first:
cat README.md
cat DEVELOPING.md
cat IMPLEMENTATION_STATUS.md
cat TODO.md
```

#### 2. Pick a Task
- Check [TODO.md](TODO.md) for a complete list of tasks
- Look for issues labeled `good-first-issue` or `help-wanted`
- Start with Phase 0 (Foundation) tasks if you're new
- Comment on an issue to claim it before starting work

#### 3. Development Workflow
```bash
# Create a feature branch
git checkout -b feature/your-feature-name

# Make your changes
# ... edit code ...

# Run tests (when they exist)
cargo test
python -m pytest

# Run linters (when configured)
cargo fmt
cargo clippy

# Commit with clear messages
git commit -m "feat: add manifest parser for kernel section"

# Push and create PR
git push origin feature/your-feature-name
```

#### 4. Pull Request Guidelines
- **One feature per PR** - keep changes focused and reviewable
- **Write tests** - all new code should have tests
- **Update documentation** - if behavior changes, docs must too
- **Follow DEVELOPING.md standards** - especially commenting guidelines
- **Link to issues** - reference related issues in PR description

### For Non-Developers

Even if you don't write code, you can still contribute:

#### Documentation
- Fix typos or unclear explanations
- Add examples and use cases
- Improve installation instructions (once code exists)
- Write tutorials or guides

#### Testing & Feedback
- Test on different Arch setups (once alpha is available)
- Report bugs with detailed reproduction steps
- Share use cases and requirements
- Provide feedback on UX and CLI design

#### Community
- Answer questions in discussions
- Help triage issues
- Write blog posts or make videos
- Spread the word about the project

---

## Code Standards

### Rust Code
- Use `rustfmt` for formatting (run `cargo fmt`)
- Use `clippy` for linting (run `cargo clippy`)
- Avoid `unwrap()` and `expect()` in production code
- Document all public functions with doc comments
- Write unit tests for all logic

### Python Code
- Follow PEP 8 style guide
- Use type hints for all function signatures
- Use Black for formatting (when configured)
- Write docstrings for all functions
- Prefer explicit over implicit

### Commenting
See [DEVELOPING.md](DEVELOPING.md) for extensive commentary guidelines. Key points:
- Explain **WHY**, not just **WHAT**
- Document constraints (determinism, security, compatibility)
- Mark workarounds with TODO and removal conditions
- Include rationale for non-obvious decisions

### Commit Messages
Use conventional commits format:
```
type(scope): brief description

Longer explanation if needed...

Fixes #123
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only
- `test`: Adding tests
- `refactor`: Code restructuring
- `perf`: Performance improvements
- `chore`: Maintenance tasks

---

## Testing Requirements

### Unit Tests
- Required for all new logic
- Test happy paths and error cases
- Use descriptive test names
- Mock external dependencies

### Integration Tests
- Required for CLI commands
- Test end-to-end workflows
- Use fixtures for test data
- Clean up after tests

### Reproducibility Tests
- Critical for build-related code
- Verify deterministic output
- Test with multiple runs
- Document any known non-determinism

---

## Security Considerations

ArchRepro deals with system-level operations. All contributions must consider security:

- **Never trust user input** - validate everything
- **Avoid privilege escalation** - minimize root operations
- **Sandbox dangerous operations** - isolate builds and rebuilds
- **Verify checksums** - don't trust network data
- **Document security assumptions** - explain threat model
- **Report vulnerabilities privately** - see SECURITY.md (when created)

---

## Documentation Standards

### README.md
- Keep concise and high-level
- Focus on user benefits
- Accurate project status (no vaporware claims!)
- Link to detailed docs for deep dives

### Code Documentation
- All public APIs must have doc comments
- Include examples in documentation
- Document error conditions
- Explain parameters and return values

### User Documentation
- Write for Arch users with varying skill levels
- Provide examples for common use cases
- Include troubleshooting sections
- Keep language clear and precise

---

## Communication

### GitHub Issues
- Use for bug reports and feature requests
- Search existing issues before creating new ones
- Provide reproduction steps for bugs
- Be specific about feature requirements

### GitHub Discussions
- Use for questions and general discussion
- Share use cases and ideas
- Help other users
- Discuss design decisions

### Code Reviews
- Be constructive and respectful
- Focus on code, not the person
- Explain reasoning for suggestions
- Be open to feedback on your own code

---

## License

By contributing to ArchRepro, you agree that your contributions will be licensed under the GPL-3.0-or-later license.

Some performance-critical components may be dual-licensed MIT/Apache-2.0 in the future. Contributors will be consulted before any license changes.

---

## Recognition

All contributors will be:
- Listed in the contributors section
- Mentioned in release notes for significant contributions
- Acknowledged in the project documentation

---

## Getting Help

- Read existing documentation first
- Search issues and discussions
- Ask in GitHub Discussions
- Tag maintainers if urgent

---

## Code of Conduct

### Our Standards
- Be respectful and inclusive
- Welcome newcomers
- Accept constructive criticism
- Focus on what's best for the project
- Show empathy toward others

### Unacceptable Behavior
- Harassment or discrimination
- Trolling or personal attacks
- Publishing private information
- Other unprofessional conduct

### Enforcement
Violations may result in:
1. Warning
2. Temporary ban
3. Permanent ban

Report violations to project maintainers.

---

## Roadmap and Priorities

See [ROADMAP.md](ROADMAP.md) for planned milestones.

Current priorities (February 2026):
1. **Phase 0:** Project structure and build system
2. **Phase 1:** Manifest parser and validator
3. **Phase 2:** Basic package management
4. **Phase 3:** Deterministic builds

Focus contributions on current phase tasks for maximum impact.

---

## Thank You!

ArchRepro aims to bring strong reproducibility to Arch Linux while respecting the Arch Way. Your contributions help make this vision a reality.

Every contribution matters - whether it's code, documentation, testing, or feedback. Thank you for being part of this project!

---

*For technical details, see [DEVELOPING.md](DEVELOPING.md)*  
*For task tracking, see [TODO.md](TODO.md)*  
*For project status, see [IMPLEMENTATION_STATUS.md](IMPLEMENTATION_STATUS.md)*
