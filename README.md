# Repository Understanding Skills

This repository contains Codex skills that help developers understand unfamiliar software projects quickly and accurately.

## Architecture Visualizer

Creates a simple, beginner-friendly view of a project's main data flow. It follows the real path from external input, through the most important source files, to the final output, then generates Graphviz diagrams and a compact architecture summary. Recently best works with C++ and Python projects.

**Use it when:** You want a quick visual map of how data moves through a repository without documenting every class, function, or subsystem.

**File:** [`architecture-visualizer.md`](architecture-visualizer.md)

## Architecture Audit

Checks an existing architecture diagram and summary against the actual source code. It identifies incorrect data flows, missing important stages, misleading interfaces, and unsupported assumptions while preserving the diagram's intended simplicity.

**Use it when:** You want to verify that an architecture visualization is trustworthy before sharing it or using it for onboarding.

**File:** [`architecture-audit.md`](architecture-audit.md)

## Repository Onboarding

Guides Codex through a structured first-time setup of an unfamiliar repository. It reviews the project, identifies dependencies, requests approval before installations or fixes, builds and runs the software, analyzes failures, documents the architecture, and produces a final onboarding report.

**Use it when:** You want help setting up, running, troubleshooting, and understanding a new codebase from start to finish.

**File:** [`repo-onboarding.md`](repo-onboarding.md)

## Suggested Workflow

1. Use **Repository Onboarding** to set up and explore a new project.
2. Use **Architecture Visualizer** to create a clear map of its main runtime data flow.
3. Use **Architecture Audit** to independently verify that map against the source code.
