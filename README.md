# Engineering Playbook

A centralized repository for engineering standards, development workflows,
governance practices, and reusable project templates.

The Engineering Playbook is intended to provide a consistent and reusable
foundation for building, maintaining, and releasing software projects.

---

## Purpose

Software projects should not require engineering practices to be reinvented
from scratch every time.

This repository will gradually define practical standards and workflows for
software engineering while keeping those rules centralized, documented, and
reusable across projects.

The playbook is being developed incrementally. New standards, workflows, and
templates are added only when they are ready to be defined and reviewed.

---

## Explore the Playbook

The repository is organized into focused areas as the playbook grows.

### 🧰 Git Workshop

Versioning, Git practices, branching, releases, and related repository
conventions.

→ [Explore Git](git/)

The Git Workshop currently includes the project's versioning policy.

---

## Repository Foundation

The root of the repository provides the shared foundation:

- `README.md` — repository entry point and navigation.
- `LICENSE` — repository licensing terms.
- `.gitignore` — repository-level Git ignore rules.
- `.editorconfig` — baseline editor and file-formatting configuration.

New playbook areas are introduced incrementally as their policies and
documentation are established.

---

## Current Configuration

### Editor Configuration

The `.editorconfig` establishes baseline formatting rules:

- UTF-8 encoding
- LF line endings
- final newline enforcement
- spaces for indentation
- four-space default indentation
- trailing whitespace removal
- two-space indentation for YAML, YML, and JSON
- Markdown trailing whitespace preservation

### Git Ignore Rules

The root `.gitignore` currently excludes common:

- operating-system files
- IDE and editor files
- temporary files
- log files
- environment files
- local tooling directories
- archive files

Technology-specific ignore rules should be introduced only when required by
this repository or by future reusable templates.

---

## Development Approach

The Engineering Playbook is being built progressively.

The complete repository structure will not be created in advance. Instead,
standards, workflows, governance rules, templates, and examples will be added
as they are designed.

Each addition should have a clear purpose and should remain consistent with the
rules already established in the playbook.

---

## Documentation

Repository documentation should reflect the actual state of the project.

When a change introduces something that users of the playbook need to know,
the relevant documentation should be updated as part of the same change.

This README will therefore evolve alongside the repository.

---

## Status

**Early Development**

The initial repository foundation is established.

The playbook is now being expanded incrementally with its first documented
engineering policies.

Current foundation:

- README
- MIT License
- Git ignore configuration
- Editor configuration

Current playbook areas:

- 🧰 [Git Workshop](git/)

---

## License

This project is licensed under the [MIT License](LICENSE).

Copyright (c) 2026 Yash Palkandwar
