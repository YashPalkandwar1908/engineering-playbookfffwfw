# Repository Standards

A practical baseline for keeping project repositories clean, consistent, and maintainable.

## Core Files

A typical project should have:

```text id="w9u8xs"
project-name/
│
├── README.md
├── .gitignore
│
├── src/
├── tests/
└── docs/
```

Add other files when the project needs them.

## Recommended Files

| File              | When to Use                                       |
| ----------------- | ------------------------------------------------- |
| `README.md`       | Every project                                     |
| `.gitignore`      | Every project                                     |
| `.env.example`    | When environment variables are used               |
| `SECURITY.md`     | When the project has meaningful security concerns |
| `CHANGELOG.md`    | When the project has releases or frequent changes |
| `LICENSE`         | When the project is distributed or made public    |
| `CONTRIBUTING.md` | When outside contributors are expected            |

## Keep the Repository Clean

Do not commit:

```text id="3i7b4m"
.env
API keys
passwords
secrets
temporary files
logs
build output
dependency folders
personal files
```

Use `.gitignore` and appropriate external storage for files that do not belong in version control.

## Documentation

Keep the root `README.md` focused on the project.

Move detailed material into:

```text id="5i1z9p"
docs/
```

Use additional documentation files when a topic becomes large enough to deserve its own document.

## GitHub Configuration

Add `.github/` when the project benefits from GitHub-specific features such as:

```text id="l9kq2v"
.github/
├── workflows/
├── ISSUE_TEMPLATE/
└── PULL_REQUEST_TEMPLATE.md
```

These are optional and should be introduced as the project grows.

## Project Growth

Start with the minimum structure needed.

```text id="g0n4la"
Start Simple
     │
     ▼
Add What You Need
     │
     ▼
Automate Repeated Work
     │
     ▼
Improve as the Project Grows
```

> **A repository should contain what the project needs, not everything a template can provide.**
