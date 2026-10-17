---
name: repo-onboarding
description: Onboard an unfamiliar software repository by inspecting dependencies, running it, analyzing failures, documenting its architecture, and producing an onboarding report. Use when a developer needs a structured first-time repository walkthrough.
---

# Repo Onboarding Skill — Version 2

## General Behavior

For every stage of this workflow:

- Before starting, print:
  `[Stage X] Starting: <stage name>`

- After completing it successfully, print:
  `[Stage X] Completed: <stage name>`

- If the stage fails or becomes blocked, clearly state:
  `[Stage X] Failed/Blocked: <reason>`

---

All permissions are valid only for the current onboarding run.

Even if:
- the same repository was onboarded before;
- the same dependencies were previously installed;
- the same fixes were previously approved;
- a previous Codex session already received permission;

ask for permission again whenever this workflow requires approval.

Never reuse approval from a previous run, session, or repository state.

---

Record the start time when Stage 0 begins.

At the end of the workflow, calculate and report the total elapsed wall-clock time, for example: Total elapsed time: 12m 34s

---

## 0. Repository Setup and Initial Review

Clone the repository to the PC and perform an initial structure review to determine how the project should be installed and run.

Identify:

- programming language(s);
- build system;
- package/dependency management system;
- important setup files;
- documented build and run procedure.

Do not perform a deep architecture analysis at this stage.

---

## 1. Identify Dependencies

Identify all required dependencies and their versions.

For each dependency, determine when possible:

- dependency name;
- required version;
- purpose;
- estimated download size;
- estimated installed size.

Also determine:

- total estimated disk space required;
- currently available disk space on the relevant filesystem.

If an exact dependency size cannot be determined reliably, clearly mark it as an estimate or unknown.

---

## 2. Dependency Approval

Show me the complete dependency list before installing anything.

Present:

- dependency;
- version;
- estimated download/install size;
- total estimated required space;
- currently available disk space.

Ask for permission before installing any dependencies.

Do not proceed until I approve.

---

## 3. Python Virtual Environment

If the project uses Python, create a separate `.venv` virtual environment inside the project.

Prefer keeping Python dependencies isolated from the system Python environment.

---

## 4. Install Dependencies

After receiving permission, install the approved dependencies.

Do not install additional dependencies that were not previously approved without explaining why they are needed and asking for permission.

If large package downloads repeatedly fail with timeouts, corrupted downloads, or hash mismatches, check possible VPN, proxy, or network issues before changing the environment or bypassing security checks.

Do not disable package integrity or security checks.

---

## 5. Build and Run

Build and run the project according to its official documentation and detected build system.

Verify whether:

- the build completes successfully;
- the application starts successfully;
- available tests can run successfully.

---

## 6. Error Analysis and Approval

If errors occur:

- briefly explain the cause;
- identify the affected component;
- propose a solution;
- explain what files, dependencies, configuration, or environment settings would need to change;
- ask for permission before applying the fix.

Do not make the proposed changes until I approve them.

---

## 7. Fix and Retry

After receiving permission:

- apply only the approved fix;
- rebuild or rerun the relevant part of the project;
- verify the result.

If another error appears, return to Stage 6.

Repeat this process until:

- the project runs successfully; or
- a problem appears that cannot be safely solved automatically.

Do not make unrelated changes while fixing errors.

---

## 8. Architecture Analysis

After the project can be successfully built and run, analyze its architecture.

For each important file, specify:

- file name and path;
- purpose;
- main classes;
- main functions;
- modules or libraries used;
- input data;
- output data;
- where the output data is sent;
- connections with other files or components.

Organize the architecture description in the logical execution order of the program, from entry points downward through the processing pipeline.

---

## 9. Final Report

After the entire workflow is complete, provide a short final report.

Include:

- repository setup status;
- dependency installation status;
- build status;
- run status;
- test status;
- unresolved errors or warnings;
- important files modified during onboarding;
- total elapsed time from the start of Stage 0 to the completion of the final report.

End with one clear overall result:

- `SUCCESS`
- `PARTIAL SUCCESS`
- `FAILED`

Briefly explain the reason for that status.

---

## Notes

- Do not modify the project's algorithmic logic without my permission.
- Do not use `sudo` without my permission.
- Do not modify files outside the current repository.
- Before installing dependencies or applying fixes, always explain the intended actions first.
- Do not make unrelated changes while solving a specific problem.
- Prefer isolated environments when possible.
- Do not bypass package integrity or security checks.

---
