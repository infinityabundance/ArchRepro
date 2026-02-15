# ArchRepro TODO Tracker

**Purpose:** Track all stub, TODO, and missing components from documentation  
**Updated:** 2026-02-15

---

## Critical Path Items (Must Do First)

### Project Foundation
- [ ] **Create .gitignore** - Prevent committing build artifacts
- [ ] **Create Cargo.toml** - Rust project manifest
- [ ] **Create requirements.txt** - Python dependencies list
- [ ] **Fix my-laptop.repro.yaml** - Syntax errors on line 16
- [ ] **Update README.md** - Accurate project status section
- [ ] **Create src/ directory structure** - Base for all code

**Estimated Time:** 2-4 hours  
**Blocker For:** Everything else

---

## Phase 0: Infrastructure (Week 1-2)

### Directory Structure
- [ ] Create `src/`
- [ ] Create `src/engine/`
- [ ] Create `src/cli/`
- [ ] Create `tests/`
- [ ] Create `tests/unit/`
- [ ] Create `tests/integration/`
- [ ] Create `tests/fixtures/`
- [ ] Create `examples/`
- [ ] Create `docs/`
- [ ] Create `.github/workflows/`

### Build Configuration
- [ ] Write Cargo.toml with dependencies
- [ ] Write requirements.txt with Python deps
- [ ] Create Cargo.lock via cargo build
- [ ] Create .rustfmt.toml
- [ ] Create rust-toolchain.toml
- [ ] Create Makefile or justfile
- [ ] Create .gitignore

### CI/CD
- [ ] Create .github/workflows/ci.yml
- [ ] Create .github/workflows/release.yml
- [ ] Set up automated testing
- [ ] Set up linting checks
- [ ] Set up security scanning

### Documentation
- [ ] Create CONTRIBUTING.md
- [ ] Create CHANGELOG.md
- [ ] Update README with accurate status
- [ ] Add API documentation structure

**Estimated Time:** 1-2 weeks  
**Deliverable:** Buildable (empty) project

---

## Phase 1: Manifest System (Week 3-4)

### Schema Definition
- [ ] Define ManifestV1 struct in Rust
- [ ] Define KernelConfig struct
- [ ] Define PackageList struct
- [ ] Define FilesystemConfig struct
- [ ] Define ServicesConfig struct
- [ ] Define UsersConfig struct
- [ ] Define GroupsConfig struct

### Parser Implementation
- [ ] Implement YAML parser (serde_yaml)
- [ ] Implement TOML parser (serde_toml)
- [ ] Add format auto-detection
- [ ] Add schema version handling
- [ ] Handle malformed input gracefully

### Validation
- [ ] Validate apiVersion field
- [ ] Validate kernel package names
- [ ] Validate package names (official)
- [ ] Validate package names (AUR)
- [ ] Validate file paths
- [ ] Validate service names
- [ ] Validate username format
- [ ] Validate group names
- [ ] Validate hash formats (sha256, sha512)

### CLI Command: init
- [ ] Implement `archrepro init` skeleton
- [ ] Detect current kernel
- [ ] Detect installed packages
- [ ] Detect enabled services
- [ ] Detect filesystem state
- [ ] Generate manifest YAML
- [ ] Write manifest to file

### Tests
- [ ] Test valid manifest parsing
- [ ] Test invalid manifest rejection
- [ ] Test schema version validation
- [ ] Test package name validation
- [ ] Test hash format validation
- [ ] Test manifest generation

**Estimated Time:** 2 weeks  
**Deliverable:** Working manifest parser + basic init command

---

## Phase 2: Package Management (Week 5-7)

### Pacman Integration
- [ ] Implement package query functions
- [ ] Implement package installation
- [ ] Implement package removal
- [ ] Handle package conflicts
- [ ] Handle missing dependencies
- [ ] Transaction management

### Manifest Application
- [ ] Parse manifest to package list
- [ ] Compute diff (desired vs actual)
- [ ] Generate installation plan
- [ ] Execute installation plan
- [ ] Handle errors gracefully
- [ ] Log all changes

### CLI Command: apply
- [ ] Implement `archrepro apply` skeleton
- [ ] Add --dry-run flag
- [ ] Add --verbose flag
- [ ] Add --force flag
- [ ] Progress indicators
- [ ] Error reporting

### CLI Command: diff
- [ ] Implement `archrepro diff` skeleton
- [ ] Show added packages
- [ ] Show removed packages
- [ ] Show version changes
- [ ] Show config file changes
- [ ] Colored output

### Tests
- [ ] Test package installation
- [ ] Test diff computation
- [ ] Test apply idempotency
- [ ] Test error handling

**Estimated Time:** 3 weeks  
**Deliverable:** Working apply and diff commands for official packages

---

## Phase 3: Deterministic Builds (Week 8-10)

### Environment Control
- [ ] Implement SOURCE_DATE_EPOCH handling
- [ ] Set fixed locale (LC_ALL=C)
- [ ] Set fixed timezone (TZ=UTC)
- [ ] Set fixed umask (0022)
- [ ] Set fixed build user
- [ ] Control PATH
- [ ] Control environment variables

### Makepkg Wrapper
- [ ] Wrap makepkg binary
- [ ] Inject environment variables
- [ ] Control filesystem access
- [ ] Log all build steps
- [ ] Capture build artifacts
- [ ] Compute hashes

### Build Sandbox
- [ ] Implement systemd-nspawn wrapper
- [ ] Create minimal container image
- [ ] Mount build directories
- [ ] Network isolation
- [ ] Cleanup after build

### Hash Verification
- [ ] Compute package hashes
- [ ] Compare with manifest
- [ ] Store hash database
- [ ] Report mismatches

### Tests
- [ ] Test deterministic builds
- [ ] Test environment isolation
- [ ] Test hash computation
- [ ] Test sandbox cleanup

**Estimated Time:** 3 weeks  
**Deliverable:** Deterministic rebuild capability

---

## Phase 4: Snapshot System (Week 11-12)

### Btrfs Backend
- [ ] Detect btrfs filesystem
- [ ] Create subvolume snapshots
- [ ] List snapshots
- [ ] Delete snapshots
- [ ] Restore from snapshot
- [ ] Handle errors

### Overlayfs Backend
- [ ] Create overlay mounts
- [ ] Manage lower/upper/work dirs
- [ ] List overlays
- [ ] Delete overlays
- [ ] Merge changes
- [ ] Cleanup

### Loop Device Backend
- [ ] Create loop device
- [ ] Mount filesystem
- [ ] Create snapshots
- [ ] Restore snapshots
- [ ] Cleanup

### CLI Command: snapshot
- [ ] Implement `archrepro snapshot create`
- [ ] Implement `archrepro snapshot list`
- [ ] Implement `archrepro snapshot delete`
- [ ] Implement `archrepro snapshot restore`
- [ ] Add --backend flag
- [ ] Progress indicators

### Tests
- [ ] Test btrfs snapshots
- [ ] Test overlayfs snapshots
- [ ] Test snapshot restore
- [ ] Test error handling

**Estimated Time:** 2 weeks  
**Deliverable:** Working snapshot system

---

## Phase 5: AUR Support (Week 13-15)

### AUR Integration
- [ ] Implement AUR API client
- [ ] Query package info
- [ ] Download PKGBUILDs
- [ ] Download sources
- [ ] Verify signatures

### Dependency Resolution
- [ ] Build dependency graph
- [ ] Resolve AUR dependencies
- [ ] Resolve official dependencies
- [ ] Handle circular dependencies
- [ ] Generate build order

### AUR Rebuild
- [ ] Clone AUR repo
- [ ] Pin commit hash
- [ ] Verify sources
- [ ] Build in sandbox
- [ ] Install package
- [ ] Clean up

### Tests
- [ ] Test AUR package fetch
- [ ] Test dependency resolution
- [ ] Test rebuild process
- [ ] Test source verification

**Estimated Time:** 3 weeks  
**Deliverable:** AUR package support

---

## Phase 6: Verification (Week 16-17)

### Verification Engine
- [ ] Rebuild packages
- [ ] Compare hashes
- [ ] Report differences
- [ ] Integration with reproducible.archlinux.org
- [ ] Generate reports

### Drift Detection
- [ ] Scan system state
- [ ] Compare with manifest
- [ ] Detect unmanaged packages
- [ ] Detect config changes
- [ ] Report drift

### CLI Command: verify
- [ ] Implement `archrepro verify`
- [ ] Add --packages flag
- [ ] Add --rebuild flag
- [ ] Add --verbose flag
- [ ] Progress indicators
- [ ] Detailed reports

### Tests
- [ ] Test rebuild verification
- [ ] Test drift detection
- [ ] Test report generation

**Estimated Time:** 2 weeks  
**Deliverable:** Working verification system

---

## Phase 7: Polish (Week 18-20)

### Documentation
- [ ] Complete API documentation
- [ ] Write tutorials
- [ ] Create video demos
- [ ] Update README with real status
- [ ] Create migration guide

### Performance
- [ ] Profile hot paths
- [ ] Optimize manifest parsing
- [ ] Optimize package operations
- [ ] Reduce memory usage
- [ ] Parallel operations

### Security
- [ ] Security audit
- [ ] Fuzzing tests
- [ ] Privilege escalation review
- [ ] Input validation review
- [ ] Dependency audit

### Packaging
- [ ] Create PKGBUILD for AUR
- [ ] Test installation
- [ ] Test upgrades
- [ ] Submit to AUR
- [ ] Monitor feedback

**Estimated Time:** 3 weeks  
**Deliverable:** Production-ready v0.1

---

## Future Phases (Post v0.1)

### Milestone 0.2
- [ ] Plugin system design
- [ ] Plugin API definition
- [ ] First-party plugins
- [ ] Plugin discovery
- [ ] Plugin security

### Milestone 0.3
- [ ] systemd generator
- [ ] Boot-time enforcement
- [ ] Rollback integration
- [ ] Emergency mode

### Milestone 0.4
- [ ] GUI configurator (Tauri)
- [ ] Visual manifest editor
- [ ] Dashboard
- [ ] Notification system

### Milestone 1.0
- [ ] Stability guarantees
- [ ] API compatibility
- [ ] Full test coverage
- [ ] Security certification
- [ ] Production deployments

---

## Known Issues

### Example Manifest (my-laptop.repro.yaml)
- **Line 16:** Invalid YAML syntax
  ```yaml
  # Current (BROKEN):
  - visual-studio-code-bin: hash: sha256:...
  
  # Should be:
  - visual-studio-code-bin:
      hash: sha256:...
  ```

### README.md
- **Lines 139-144:** False claims about "Achieved" features
- **Lines 46-64:** Installation instructions for non-existent code
- **Lines 69-92:** Quick Start commands that don't work

### Documentation
- Missing CONTRIBUTING.md (referenced in README line 187)
- Missing CHANGELOG.md (referenced in DEVELOPING.md line 41)

---

## Statistics

### Current State
- **Total TODOs:** ~200+
- **Completed:** 0 (0%)
- **Lines of Code:** 0
- **Test Coverage:** N/A (no tests)
- **Documentation Coverage:** 100% (aspirational)

### Project Completion
- **Phase 0:** 0%
- **Phase 1:** 0%
- **Phase 2:** 0%
- **Phase 3:** 0%
- **Phase 4:** 0%
- **Phase 5:** 0%
- **Phase 6:** 0%
- **Phase 7:** 0%
- **Overall:** 0%

### Time Estimates
- **To v0.1 Alpha:** 8-12 weeks
- **To v0.2 Beta:** 4-6 months
- **To v1.0 Stable:** 6-12 months

---

## Priority Matrix

### P0 (Critical - Do First)
1. Fix documentation accuracy
2. Create project structure
3. Set up build system
4. Fix example manifest

### P1 (High - Core Features)
1. Manifest parser
2. Package management
3. Deterministic builds
4. Basic CLI

### P2 (Medium - Important Features)
1. Snapshot system
2. AUR support
3. Verification system

### P3 (Low - Nice to Have)
1. GUI
2. Plugins
3. Advanced features

---

*This TODO list represents the complete gap between documentation and implementation.*
