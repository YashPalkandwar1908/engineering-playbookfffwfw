# Changelog Standard

## 1. Purpose

`CHANGELOG.md` is the permanent consumer-facing history of released changes.

It is not a copy of Git commit history.

---

# 2. Structure

```markdown
# Changelog

## [Unreleased]

## [1.2.0] - 2026-08-05

### Added

### Changed

### Fixed

### Removed

### Breaking Changes
```

Maintain `[Unreleased]` continuously.

At release time:

```text
[Unreleased]
      ↓
[1.2.0] - YYYY-MM-DD
      ↓
new empty [Unreleased]
```

Release dates use UTC.

---

# 3. Categories

Use:

```text
Added
Changed
Fixed
Removed
Breaking Changes
```

Omit empty categories.

### Added

New user-visible functionality.

### Changed

Changes to existing behavior that are not simple fixes.

### Fixed

Bug and security fixes.

### Removed

Removed functionality.

### Breaking Changes

Changes requiring consumer action.

Keep `Breaking Changes` last so they remain highly visible.

---

# 4. Writing Entries

Write for software consumers.

Do not paste commit messages.

Bad:

```text
fix(auth): resolve token refresh race (#482)
```

Good:

```text
Fixed a bug that could cause sessions to expire early under high load.
```

Use:

* Plain language
* One meaningful change per entry
* Consumer-visible effects
* Migration information where required

---

# 5. Internal Changes

Internal changes do not require detailed changelog entries unless they materially affect consumers.

Examples:

* Internal refactoring
* Test restructuring
* CI maintenance
* Build-script cleanup

They may be omitted from the consumer changelog.

If a tagged release consists entirely of internal maintenance, include a short entry such as:

```text
### Changed

- Internal maintenance and build improvements.
```

---

# 6. Documentation Changes

User-facing documentation corrections may be recorded under `[Unreleased]`.

Documentation-only work does not normally justify creating a software release by itself.

Release decisions are governed by:

```text
git/RELEASE_PROCESS.md
```

---

# 7. Breaking Changes

Every breaking change must contain:

* What changed
* What consumers must do
* Migration note or migration-guide link

Example:

```markdown
### Breaking Changes

- Renamed `server.host` to `server.bind_address`.
  See `docs/MIGRATION_1_TO_2.md`.
```

Migration-guide requirements are defined by:

```text
standards/DEPRECATION_POLICY.md
```

---

# 8. Pre-Releases

Published pre-releases receive their own changelog headings.

Example:

```markdown
## [1.0.0-beta.2] - 2026-08-03

### Fixed

- Fixed plugin discovery during startup.
```

Pre-release entries may be concise because the final stable release provides the polished summary.

---

# 9. Security Entries

Security fixes belong under:

```text
Fixed
```

unless they introduce a breaking change.

If disclosure is restricted, use a generic entry:

```text
- Fixed a security issue affecting authentication.
```

Do not publish exploit details before the approved disclosure time.

When disclosure becomes appropriate, the entry may be expanded and may reference:

```text
CVE
Security Advisory
Incident Report
```

Do not delay a security release merely to produce detailed public disclosure text.

---

# 10. GitHub Release Notes

GitHub Release notes and `CHANGELOG.md` serve different purposes.

```text
CHANGELOG
    ↓
Permanent categorized history

GitHub Release Notes
    ↓
Curated release presentation
```

GitHub Release notes may contain:

* Summary
* Highlights
* Upgrade guidance
* Known issues
* Contributor acknowledgements

Every substantive released change should remain traceable to the changelog.

---

# 11. Version Comparison Links

Version comparison links are recommended when the hosting platform supports them.

Example:

```markdown
[1.2.0]: https://github.com/org/repo/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/org/repo/compare/v1.0.0...v1.1.0
```

---

# 12. Example

```markdown
# Changelog

## [Unreleased]

### Added

- Added workflow cancellation support.

## [0.3.0] - 2026-08-05

### Added

- Added the plugin registry.
- Added workflow execution support.

### Changed

- Improved scheduler startup behavior.

### Fixed

- Fixed an event-dispatch race condition.

### Breaking Changes

- Renamed the workflow configuration key `steps` to `tasks`.
  See `docs/MIGRATION_0_2_TO_0_3.md`.

## [0.2.1] - 2026-07-20

### Fixed

- Fixed configuration loading on Windows.
```
