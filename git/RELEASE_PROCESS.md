# Release Process

## 1. Purpose

This document defines how a version becomes an actual released artifact.

Version-number meaning is defined by:

```text
git/VERSIONING_POLICY.md
```

This document owns:

* Release gates
* Release commits
* Tagging
* Release branches
* Release authority
* Pre-release publication
* GitHub Releases
* Package registries
* Artifacts
* Checksums
* Provenance
* Emergency releases
* Deployment rollback
* Corrective releases
* Release withdrawal/yanking
* Partial publication failures
* Post-release verification

---

# 2. Release Gates

Before any stable or published pre-release tag is pushed:

1. Working tree is clean.
2. Correct release branch is checked out.
3. Local branch is fully synchronized with the remote.
4. Required tests pass.
5. Linting passes.
6. Type checking passes when applicable.
7. Required security checks pass.
8. Release artifacts build successfully.
9. Basic installation/smoke testing passes.
10. Required migration testing passes.
11. Supported compatibility matrix passes.
12. User-facing documentation is updated.
13. `CHANGELOG.md` is updated.
14. Version source of truth contains the intended version.
15. Release notes are prepared.
16. Required release approval is obtained.

A release must not proceed while a required gate is failing.

---

# 3. Migration Gate

When a release changes:

* Database schema
* Persistent data
* Configuration format
* File format
* Stored state

test migration using representative existing data.

Do not test only a fresh installation.

When migration is irreversible, documentation must clearly state:

* Backup requirement
* Migration procedure
* Downgrade limitation
* Recovery procedure where possible

---

# 4. Compatibility Gate

If a project claims compatibility with multiple environments, CI should verify the supported matrix.

Example:

```text
Python 3.12
Python 3.13

Windows
Linux
macOS
```

Do not advertise compatibility that the release process does not verify where automated testing is practical.

---

# 5. Version Bump

The canonical version is updated in the release commit.

Example:

```text
0.3.0
  ↓
0.4.0
```

The same release commit should contain any final release-specific metadata required for that release, including the finalized changelog entry when applicable.

The Git tag points to this exact commit.

Projects may use automated/VCS-derived development versions if explicitly configured.

Otherwise, the canonical source remains at the most recently released version between releases.

---

# 6. Release Commit

A release commit represents the exact source state being released.

Example:

```text
chore(release): prepare v0.4.0
```

It may contain:

* Version bump
* Final changelog update
* Release metadata
* Required generated release files

Do not mix unrelated feature development into the release commit.

---

# 7. Tagging

Release tags are annotated.

Example:

```bash
git tag -a v1.4.0 -m "Release v1.4.0"
```

Pre-release:

```bash
git tag -a v1.4.0-rc.1 -m "Release v1.4.0-rc.1"
```

Push:

```bash
git push origin v1.4.0
```

Rules:

* Lowercase `v`
* Annotated tags
* Tag exact release commit
* No milestone suffix
* No build metadata
* Never tag a feature branch
* Published release tags are immutable

---

# 8. Tag Signing

Tags are unsigned by default unless the project requires signing.

Projects distributing security-sensitive or auto-updating software should consider signed tags and/or artifact signing.

The project must explicitly document when signing is mandatory.

Do not silently assume signing.

---

# 9. Release Branches

The default branch is the normal release source.

Create a maintenance release branch only when an older release line must continue receiving changes after the default branch has moved ahead.

Convention:

```text
release/<major>.<minor>
```

Example:

```text
main
    → developing 1.5.0

release/1.4
    → maintaining 1.4.x
```

Example maintenance release:

```text
release/1.4
    ↓
v1.4.1
    ↓
v1.4.2
```

Do not create a release branch for every normal release.

Keep the branch while that release line remains supported.

Remove it after that line reaches EOL according to:

```text
standards/SUPPORT_POLICY.md
```

---

# 10. Release Authority

Code-review ownership and release authority are separate responsibilities.

`CODEOWNERS` does not automatically determine who may publish a release.

Each project should identify the responsible role in its README or governance documentation.

Examples:

```text
Release Manager
Maintainer
Project Lead
```

Stable releases require human authorization.

CI may automate:

* Tests
* Builds
* Packaging
* Tag creation
* Artifact upload
* Registry publishing
* Release-note generation

but the decision to publish a stable release requires explicit approval unless the project has deliberately documented a different governance model.

---

# 11. Pre-Releases

Published pre-releases use real annotated tags.

Examples:

```text
v1.0.0-alpha.1
v1.0.0-beta.1
v1.0.0-rc.1
```

On GitHub they are Releases marked:

```text
Pre-release
```

They must not become `Latest`.

The project's required RC soak period may be specified in its project-specific release configuration.

If none is specified, the Engineering Playbook default is:

```text
48 hours
```

with:

* No release-candidate changes
* All release gates passing

before stable promotion.

---

# 12. Stable GitHub Release

A stable GitHub Release contains:

```text
Title

Summary

Highlights

Added

Changed

Fixed

Removed

Breaking Changes

Migration Information

Known Issues

Contributors
```

Not every heading must be present when empty.

Example:

```text
ResearchOS v0.4.0 — Workflow Foundation
```

Only stable versions are eligible for `Latest`.

---

# 13. Latest Release

`Latest` represents the newest stable release on the project's primary supported line.

Example:

```text
v2.0.0    ← Latest
```

If an older supported line receives:

```text
v1.9.7
```

publish it normally but do not replace `v2.0.0` as `Latest`.

The primary support line is defined by:

```text
standards/SUPPORT_POLICY.md
```

---

# 14. Artifacts

Platform-independent artifact:

```text
<project>-<version>.<ext>
```

Example:

```text
researchos-1.4.0.tar.gz
```

Platform-specific artifact:

```text
<project>-<version>-<platform>-<arch>.<ext>
```

Examples:

```text
researchos-1.4.0-windows-x64.zip
researchos-1.4.0-linux-x64.tar.gz
researchos-1.4.0-macos-arm64.tar.gz
```

Compatibility-specific projects may add compatibility information when required.

Example:

```text
mod-1.4.0+mc1.21.5-fabric.jar
```

---

# 15. Artifact Integrity

Directly distributed binary/archive release artifacts should include SHA-256 checksums.

Example:

```text
SHA256SUMS
```

or:

```text
researchos-1.4.0-linux-x64.tar.gz.sha256
```

Package registries that already provide integrity verification may use their ecosystem-native mechanism.

---

# 16. Provenance

Every release artifact must be traceable to exactly one source commit.

Record at minimum:

```text
Version
Git tag
Commit SHA
Build environment or workflow identifier where appropriate
```

This information may live in:

* Build manifest
* Artifact metadata
* `--version` output
* CI provenance record

The question:

> Which source commit produced this artifact?

must be answerable.

---

# 17. Reproducible Builds

Where the toolchain supports reproducible builds, rebuilding the same tag under the defined build environment should produce the same artifact.

Byte-identical reproducibility is preferred.

Functional equivalence is not called a reproducible build.

If deterministic output is currently impossible, document the known source of non-determinism.

---

# 18. Package Registries

Published registry versions are immutable.

Examples:

```text
PyPI
npm
crates.io
```

Never replace:

```text
1.4.0
```

with different contents.

If `1.4.0` is defective:

```text
1.4.0   ← withdraw/yank/deprecate
1.4.1   ← corrected release
```

Use ecosystem-specific withdrawal mechanisms where available.

---

# 19. Publication Order

For projects publishing to multiple destinations, define a deterministic publication sequence.

Recommended default:

```text
Build once
    ↓
Verify artifacts
    ↓
Create immutable tag
    ↓
Publish GitHub Release
    ↓
Publish package registry artifacts
    ↓
Verify each destination
```

The same verified artifacts should be promoted between destinations where tooling permits rather than independently rebuilt for every registry.

---

# 20. Partial Publication Failure

A release may partially succeed.

Example:

```text
Git tag          ✓
GitHub Release   ✓
PyPI             ✗
```

Do not create a new version merely because publication infrastructure failed if the released code/artifact itself is correct.

Instead:

1. Stop further publication.
2. Record the failed destination.
3. Determine whether any published artifact is defective.
4. If artifacts are correct, retry publication of the same immutable version.
5. Verify all destinations afterward.

Create a new PATCH only when the actual released software/artifact must change.

---

# 21. Post-Release Verification

A release is not complete immediately after upload.

After publication:

1. Confirm the Git tag resolves to the intended commit.
2. Confirm GitHub Release metadata.
3. Confirm required artifacts are downloadable.
4. Verify checksums where applicable.
5. Install/download from the actual public distribution channel.
6. Run a basic smoke test.
7. Confirm registry version metadata.
8. Confirm `Latest` status is correct.
9. Confirm changelog and release links work.

---

# 22. Definition of Release Complete

A release is complete when:

```text
Tag exists
+
GitHub Release exists
+
Required registries are published
+
Required artifacts are available
+
Post-release verification passes
+
CHANGELOG reflects the release
```

A tag alone does not constitute a completed release.

---

# 23. Failure Before Publication

If a tag was pushed but nothing was publicly released or published to a registry and a release-blocking problem is discovered, the tag may be deleted and re-created as an administrative correction.

Document the correction.

This is exceptional.

Once a version has been publicly distributed, do not move or reuse its tag.

---

# 24. Corrective Release

If a published release contains a defect but users are not in immediate danger:

```text
v1.4.0
   ↓
defect found
   ↓
fix
   ↓
v1.4.1
```

Never replace the existing `v1.4.0` artifact.

---

# 25. Deployment Rollback

Deployment rollback applies primarily to software where maintainers control deployment.

Example:

```text
Production: v1.4.0
        ↓
serious failure
        ↓
redeploy previous known-good v1.3.2
        ↓
investigate
        ↓
fix
        ↓
release v1.4.1
```

Rollback restores service while a permanent corrective release is prepared.

A rollback does not erase the existence of the bad release.

---

# 26. Distributed Packages

For libraries, downloadable applications, mods, SDKs, and other distributed software, maintainers generally cannot force users to roll back.

Instead:

```text
Bad release
    ↓
withdraw / yank / deprecate
    ↓
publish warning
    ↓
release corrected version
```

---

# 27. Release Withdrawal / Yanking

A published version that should no longer be used remains part of history.

Do not delete it merely to hide the mistake.

Where supported:

```text
npm     → deprecate
PyPI    → yank
```

For GitHub Releases:

* Clearly mark the release as not recommended
* Explain the problem
* Point users to the corrected version

Example:

```text
v1.4.0 — NOT RECOMMENDED

Known issue: ...
Upgrade to v1.4.1.
```

---

# 28. Serious Harm

If a release causes:

* Data loss
* Security exposure
* Corruption
* Severe production instability

take immediate action:

```text
Stop/reduce distribution
        ↓
Publish advisory
        ↓
Rollback deployment where possible
        ↓
Withdraw/yank affected package where appropriate
        ↓
Prepare emergency corrective release
        ↓
Provide recovery instructions
```

Never silently replace the affected release.

---

# 29. Emergency Releases

Security and production-critical releases may compress timelines but do not eliminate fundamental verification.

Never skip:

* Relevant tests
* Build verification
* Smoke testing
* Version verification
* Required release authorization

Documentation polish may follow quickly afterward when necessary, but minimum release and security information must be present.

---

# 30. Cancelled Releases

If a pre-release line is abandoned:

```text
v1.0.0-alpha.1
v1.0.0-beta.1
v1.0.0-beta.2
```

and `1.0.0` is never released:

* Keep published pre-release tags
* Keep their release records
* Record why development changed direction
* Do not rewrite history

Published pre-release identifiers are historical records.

---

# 31. Release Dates

Release dates use UTC.

Example:

```text
2026-08-05
```

Use the same date convention in:

* CHANGELOG
* GitHub Release
* Release metadata

---

# 32. Quick Reference

```text
Development complete
        ↓
Release gates
        ↓
Version + release commit
        ↓
Build
        ↓
Verify
        ↓
Annotated tag
        ↓
Pre-release?
├── Yes → GitHub Pre-release
└── No  → Stable GitHub Release
        ↓
Publish registries/artifacts
        ↓
Post-release verification
        ↓
RELEASE COMPLETE
```

If something goes wrong:

```text
Before publication
        ↓
Correct and re-cut exceptional unpublished tag

Published software defect
        ↓
Corrective PATCH

Deployment emergency
        ↓
Rollback + corrective release

Bad distributed package
        ↓
Yank/deprecate + corrective release

Security/data-loss emergency
        ↓
Advisory + rollback/withdrawal + emergency release
```
