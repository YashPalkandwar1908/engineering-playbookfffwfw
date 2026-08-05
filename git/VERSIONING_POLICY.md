# Versioning Policy

## 1. Purpose

This document defines how project versions are numbered and what those version numbers mean.

This policy covers:

* Semantic Versioning
* MAJOR, MINOR, and PATCH classification
* `0.x` development versions
* Pre-release versions
* Public compatibility guarantees
* Development milestones versus versions
* Compatibility targets and platform variants
* Multi-package and monorepo versioning

Release execution is defined separately in:

```text
git/RELEASE_PROCESS.md
```

---

# 2. Separate Systems

Planning, development, versioning, releasing, and maintenance are related but separate systems.

```text
PROJECT PLANNING
        │
        │ GitHub Projects
        │ GitHub Milestones
        │ Issues
        ▼
DEVELOPMENT
        │
        │ Commits
        │ Branches
        │ Internal checkpoints
        ▼
PRE-RELEASE
        │
        │ alpha
        │ beta
        │ release candidate
        ▼
STABLE RELEASE
        │
        │ Semantic Version
        │ Git tag
        │ GitHub Release
        ▼
MAINTENANCE
        │
        │ Patch releases
        │ Backports
        │ Security fixes
        ▼
END OF LIFE
```

A GitHub Milestone is not a version.

A Git commit is not a version.

A compatibility target is not the project version.

A platform-specific artifact is not automatically a separate project version.

---

# 3. Semantic Versioning

Projects follow Semantic Versioning:

```text
MAJOR.MINOR.PATCH
```

Examples:

```text
0.1.0
0.2.0
1.0.0
1.4.2
2.0.0
```

The canonical project version does not contain a `v` prefix:

```text
1.4.2
```

Git tags use:

```text
v1.4.2
```

Do not use leading zeros.

Invalid:

```text
01.04.02
v01.4.0
```

This Engineering Playbook standardizes on Semantic Versioning rather than Calendar Versioning (CalVer) for project versions.

Projects requiring a fundamentally different versioning model must document that exception explicitly.

---

# 4. Single Source of Truth

Every independently versioned package must have exactly one authoritative source for its version.

Examples:

```text
pyproject.toml
package.json
Cargo.toml
```

Example:

```toml
[project]
version = "0.3.0"
```

The Git tag must match this value:

```text
Canonical version: 0.3.0
Git tag:           v0.3.0
```

The source-of-truth file determines the project version.

Release automation should verify the version before publishing.

---

# 5. MAJOR Version

Increment MAJOR when a stable public contract is broken.

Example:

```text
1.7.3
  ↓
2.0.0
```

Breaking changes include:

* Removing public APIs
* Changing public API signatures incompatibly
* Renaming required configuration keys
* Breaking database or schema compatibility
* Removing documented CLI commands or flags
* Breaking documented file formats
* Breaking documented HTTP/RPC contracts
* Dropping supported runtimes
* Dropping supported operating systems
* Dropping supported architectures
* Removing previously supported behavior consumers depend on

Example:

```text
1.4.0

get_user(id)

↓

2.0.0

get_user(user_id, organization_id)
```

If existing consumers must change their code to continue working, the change is normally breaking.

---

# 6. MINOR Version

Increment MINOR for backward-compatible functionality.

Example:

```text
1.4.0
  ↓
1.5.0
```

Examples:

* New feature
* New module
* New endpoint
* New optional configuration
* New plugin
* New command that does not break existing commands
* New AI capability
* New backward-compatible API

Existing consumers should continue working without modification.

---

# 7. PATCH Version

Increment PATCH for backward-compatible fixes.

Example:

```text
1.5.2
  ↓
1.5.3
```

Examples:

* Bug fixes
* Security fixes that preserve compatibility
* Performance improvements with unchanged behavior
* Dependency updates with no public behavioral change
* Internal implementation corrections

Documentation, test, CI, and internal refactoring changes do not normally require an independent release.

They may be included in the next appropriate release.

A project may still publish a PATCH release for an important documentation-only or operational correction when distributing that correction independently is useful to consumers.

---

# 8. Change Classification

When classification is unclear, ask:

> Does anything a consumer could reasonably depend on change?

| Change                                        | Classification      |
| --------------------------------------------- | ------------------- |
| Internal refactor, same public behavior       | No independent bump |
| Performance improvement, same behavior        | PATCH               |
| Bug fix                                       | PATCH               |
| New backward-compatible feature               | MINOR               |
| New optional API                              | MINOR               |
| Dependency upgrade with no visible effect     | PATCH               |
| Dependency upgrade changing public behavior   | MINOR or MAJOR      |
| Security fix preserving compatibility         | PATCH               |
| Security fix requiring public incompatibility | MAJOR               |
| Deprecating an existing API                   | MINOR               |
| Removing a stable deprecated API              | MAJOR               |
| Dropping supported runtime                    | MAJOR               |
| Dropping supported OS                         | MAJOR               |
| Breaking configuration format                 | MAJOR               |
| Breaking documented file format               | MAJOR               |

If one release contains several kinds of changes, the highest-impact classification wins.

Example:

```text
10 bug fixes
3 features
1 breaking API change
```

Result:

```text
MAJOR
```

## Dependency Examples

A dependency's own version change does not automatically determine the project's version change.

Classify the effect on the project's public contract.

Example:

```text
Dependency:

OpenSSL 3.0
    ↓
OpenSSL 3.1

No consumer-visible compatibility change
    ↓
PATCH
```

Another dependency upgrade might expose new backward-compatible functionality:

```text
Dependency upgrade
        ↓
New backward-compatible project capability
        ↓
MINOR
```

A runtime-support change can be breaking:

```text
Before:

Node.js >=20

After:

Node.js >=22
```

If Node.js 20 was previously supported and is now removed:

```text
MAJOR
```

The dependency version itself is not the deciding factor.

The impact on consumers is.

---

# 9. `0.x` Development Versions

Versions below `1.0.0` represent initial development.

Example:

```text
0.1.0
0.2.0
0.3.0
...
1.0.0
```

SemVer does not define a detailed breaking-change bump policy for `0.x`.

This Engineering Playbook therefore uses the following convention:

| Change                      | Version |
| --------------------------- | ------- |
| Backward-compatible fix     | PATCH   |
| Backward-compatible feature | MINOR   |
| Breaking change             | MINOR   |

Example:

```text
0.4.0 → 0.4.1

Backward-compatible bug fix
```

Example:

```text
0.4.1 → 0.5.0

New functionality and/or breaking development change
```

PATCH releases under `0.x` must remain backward compatible.

This is an Engineering Playbook convention and is intentionally stricter than SemVer's general guidance for initial development.

---

# 10. Declaring `1.0.0`

Do not release `1.0.0` merely because development has existed for a long time.

Release `1.0.0` when:

* The public contract is intentionally defined
* The project has meaningful real usage
* Breaking changes are no longer routine
* Core functionality is considered stable
* Required documentation exists
* Project-wide Definition of Done requirements are satisfied
* Maintainers are willing to preserve compatibility according to SemVer
* Maintainers are committed to maintaining backward compatibility going forward

Before that point, remain under `1.0.0`.

`1.0.0` represents both a technical milestone and a compatibility commitment to consumers.

---

# 11. Public Contract

SemVer compatibility guarantees apply to the project's documented public surface.

Examples:

* Public APIs
* Published CLI commands
* Published CLI flags
* Documented configuration
* Documented file formats
* Documented HTTP APIs
* Documented RPC contracts

Undocumented implementation details are not part of the public contract unless explicitly declared otherwise.

Code being technically accessible or exported does not by itself make that behavior a supported public contract.

Detailed public/private API rules and deprecation behavior are defined in:

```text
standards/DEPRECATION_POLICY.md
```

---

# 12. Pre-Release Versions

Pre-release versions are public versions that are not yet considered stable.

Supported stages:

```text
alpha
beta
rc
```

Format:

```text
1.0.0-alpha.1
1.0.0-alpha.2

1.0.0-beta.1
1.0.0-beta.2

1.0.0-rc.1
1.0.0-rc.2

1.0.0
```

## Alpha

Alpha releases are incomplete and unstable.

Example:

```text
1.0.0-alpha.1
```

Use them for:

* Early testing
* Internal testing
* Collaborator testing
* Experimental integration

## Beta

Beta releases are feature-complete or close to feature-complete but may still contain significant bugs.

Example:

```text
1.0.0-beta.1
```

## Release Candidate

A release candidate is believed to be suitable for stable release.

Example:

```text
1.0.0-rc.1
```

After RC begins:

* No new planned features
* Only fixes
* Documentation corrections
* Release-blocking changes

If a bug is found:

```text
1.0.0-rc.1
       ↓
1.0.0-rc.2
```

Do not use:

```text
1.0.1
```

because `1.0.0` has not yet become stable.

---

# 13. Pre-Release Ordering

Version precedence follows SemVer.

Example:

```text
1.0.0-alpha.1
<
1.0.0-alpha.2
<
1.0.0-alpha.10
<
1.0.0-beta.1
<
1.0.0-rc.1
<
1.0.0
```

Numeric identifiers are compared numerically.

Do not rely on simple string sorting.

---

# 14. Allowed Pre-Release Identifiers

Use only:

```text
alpha.N
beta.N
rc.N
```

Examples:

```text
1.0.0-alpha.1
1.0.0-beta.3
1.0.0-rc.2
```

Do not invent identifiers such as:

```text
1.0.0-final
1.0.0-m3
1.0.0-beta-final
1.0.0-rc-final2
```

---

# 15. Milestones Are Not Versions

Development milestones are planning and delivery checkpoints.

Examples:

```text
Milestone 1 — Repository Foundation
Milestone 2 — Configuration System
Milestone 3 — Event Bus
```

Do not convert them into version tags such as:

```text
v0.1.0-m1
v0.1.0-m2
v0.1.0-m3
```

Instead:

```text
GitHub Milestone
        +
Issues
        +
Merge commit SHA
```

provides the development checkpoint.

If a milestone needs to produce an installable version for external testing, create a pre-release:

```text
v0.1.0-alpha.1
```

Milestone workflow rules belong in:

```text
workflow/MILESTONE_BASED_CODE_DELIVERY.md
```

---

# 16. Compatibility Targets

A compatibility target is separate from the project's own version.

Examples:

```text
Minecraft 1.21.5
Python 3.12
Node.js 22
Java 21
```

Do not automatically encode these into the project version.

Example:

```text
Project version:
1.4.0

Minecraft compatibility:
1.21.5

Loader:
Fabric
```

These are three different facts.

---

# 17. Platform and Artifact Variants

Different artifacts may represent the same project release.

Example:

```text
Project version:

1.4.0
```

Artifacts:

```text
project-1.4.0-linux-x64.tar.gz
project-1.4.0-windows-x64.zip
project-1.4.0-macos-arm64.tar.gz
```

Minecraft-style example:

```text
mod-1.4.0+mc1.21.5-fabric.jar
mod-1.4.0+mc1.21.5-neoforge.jar
```

The project version remains:

```text
1.4.0
```

Platform and compatibility information belongs primarily to artifact identity.

---

# 18. Build Metadata

SemVer supports build metadata such as:

```text
1.2.0+build.42
```

For Engineering Playbook projects, build metadata is not stored in the canonical project version and is not used in release Git tags.

It may be used for artifact/build identification when appropriate.

Example:

```text
project-1.2.0+build.42.tar.gz
```

Build metadata does not affect SemVer version precedence.

For example:

```text
1.2.0+build.1
```

and:

```text
1.2.0+build.99
```

have the same SemVer precedence.

Build metadata identifies builds; it does not make one version newer than the other under SemVer precedence rules.

This restriction on canonical versions and release tags is an Engineering Playbook convention, not a SemVer requirement.

---

# 19. Multi-Platform Projects

By default, platform variants share one project version.

Example:

```text
1.4.0
├── Fabric
├── NeoForge
└── Common
```

Do not silently create unrelated version histories for platforms.

If platform implementations become independently maintained products, the project must explicitly document independent versioning.

---

# 20. Multi-Package and Monorepo Versioning

A repository must explicitly choose one of two models.

## Synchronized Versioning

All packages share one version.

Example:

```text
core       1.4.0
server     1.4.0
frontend   1.4.0
```

Use when packages are tightly coupled and normally ship together.

## Independent Versioning

Each package has its own version.

Example:

```text
core       1.4.0
sdk        2.1.0
plugin     0.8.0
```

Each independently versioned package must have:

* Its own source of truth
* Its own changelog or clearly separated changelog section
* Its own release notes or clearly separated release-note section
* Its own release lifecycle
* Its own tag namespace

Recommended tags:

```text
core-v1.4.0
sdk-v2.1.0
plugin-v0.8.0
```

Package compatibility rules belong in:

```text
standards/DEPENDENCY_MANAGEMENT.md
```

---

# 21. Software, API, and Schema Versions

These are separate concepts.

Example:

```text
Application version: 3.4.0
API version:         v2
Database schema:     7
```

Do not assume:

```text
Application 3.4.0
=
API v3
=
Schema 3
```

Each version system must be documented independently.

---

# 22. Related Policies

| Concern                            | Authoritative Document                      |
| ---------------------------------- | ------------------------------------------- |
| Version meaning and classification | `git/VERSIONING_POLICY.md`                  |
| Milestone planning and delivery    | `workflow/MILESTONE_BASED_CODE_DELIVERY.md` |
| Release execution                  | `git/RELEASE_PROCESS.md`                    |
| Changelog format                   | `standards/CHANGELOG_STANDARD.md`           |
| Public API deprecation             | `standards/DEPRECATION_POLICY.md`           |
| Supported versions and EOL         | `standards/SUPPORT_POLICY.md`               |
| Dependency compatibility           | `standards/DEPENDENCY_MANAGEMENT.md`        |
| Testing requirements               | `standards/TESTING_STANDARD.md`             |
| Completion requirements            | `workflow/DEFINITION_OF_DONE.md`            |

---

# 23. Policy Precedence

Project-specific requirements may override this generic policy when the exception is explicitly documented.

Silence is not an override.

When two Engineering Playbook documents overlap, the document specifically responsible for that subject is authoritative.

Examples:

```text
VERSIONING_POLICY.md
→ determines whether a change is PATCH, MINOR, or MAJOR

RELEASE_PROCESS.md
→ determines how that version is released

DEPRECATION_POLICY.md
→ determines how an API is deprecated

SUPPORT_POLICY.md
→ determines how long the version is maintained
```

---

# 24. Quick Reference

```text
Bug fix
    ↓
PATCH

Backward-compatible feature
    ↓
MINOR

Breaking stable public contract
    ↓
MAJOR

Breaking change during 0.x
    ↓
MINOR

Documentation only
    ↓
Usually no independent release

Internal milestone
    ↓
NO VERSION

External unstable test build
    ↓
alpha / beta / rc

Stable release
    ↓
MAJOR.MINOR.PATCH
```

---

# 25. Completion Status

`VERSIONING_POLICY.md` is complete for its defined responsibility.

Future changes should be driven by new requirements or by resolving documented conflicts with other Playbook policies, not by incremental stylistic refinement.

Further guidance on releases, support, security, milestones, changelogs, deprecation, testing, and dependency management belongs in their respective policy documents.
