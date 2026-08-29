# Repository Maturity Model

A simple way to understand how a project should grow without adding unnecessary complexity too early.

## Stage 1 — Normal Project

Start with the basics:

```text
README.md
.gitignore
src/
tests/
docs/
```

Add `.env.example` or `SECURITY.md` when the project needs them.

The goal is to get the project working with a clean foundation.

## Stage 2 — Project Grows

As complexity increases, introduce what is actually needed:

```text
CI/CD
Better test coverage
Architecture documentation
Development documentation
Release management
CHANGELOG.md
```

Don't add these just because the project has reached a certain age. Add them when they solve a real problem.

## Stage 3 — Mature Project

The project should now have clear:

* Development practices
* Testing
* Documentation
* Security
* Releases
* Maintenance processes

The structure should support the project rather than slow it down.

## Stage 4 — Decide What Comes Next

A mature project can remain private or become public.

```text
                    MATURE PROJECT
                          │
                          ▼
                  Publish / Share?
                    /           \
                  NO             YES
                  │               │
                  ▼               ▼
               PRIVATE       OPEN SOURCE
                                  │
                                  ▼
                         CONTRIBUTING.md
                         CODE_OF_CONDUCT.md
                         Issue Templates
                         PR Template
                         Public Documentation
                         LICENSE
```

Open-source files should be added **when the project is actually being opened to outside users or contributors**.

## Core Principle

> **Start simple. Add structure when complexity justifies it.**

A project should grow because its needs grow — not because a checklist says it should.
