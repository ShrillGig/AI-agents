---
name: architecture-audit
description: Independently audit an existing repository architecture diagram and summary against source code, verify the simplified main input-to-output data flow, and produce an evidence-based ARCHITECTURE_AUDIT.md with an optional correction prompt. Use when the user asks to validate, review, or audit artifacts created by the architecture-visualizer skill. Do not use to create an architecture diagram from scratch or to modify project source code.
---

# Architecture Auditor

## Purpose of This Audit

This skill independently verifies an existing architecture visualization against the actual source code of the current repository.

The architecture being audited is intentionally a simplified developer orientation map.

Its purpose is to help a developer who is seeing an unfamiliar codebase for the first time quickly understand:

- what important data enters the program;
- what important data leaves the program;
- which main source files transform or transfer that data;
- how the main input is transformed step by step into the main output.

The visualization is intentionally simplified.

It is NOT intended to:

- fully document the software architecture;
- replace reading the source code;
- show every runtime component;
- show every control path;
- show every class or function;
- show initialization and lifecycle logic;
- show the complete call graph;
- show every configuration detail;
- show every secondary subsystem;
- replace the developer's own investigation of the source code.

The developer is expected to use the architecture as an orientation map and then inspect the actual source code personally.

Therefore, the auditor must evaluate the architecture according to this intended level of abstraction.

Do not penalize the diagram merely for being simplified.

A missing component is an architecture error only when omitting it makes the main:

INPUT → processing files → OUTPUT

pipeline incorrect, misleading, internally inconsistent, or materially harder to understand.

The primary goal of this audit is to verify:

1. plausibility;
2. integrity;
3. correctness;
4. consistency;

of the simplified data-flow map produced by the `architecture-visualizer`.

Prefer a simple but truthful architecture map over a complete but overcrowded system diagram.

---

## Expected Architecture Style

The architecture being audited should approximately follow:

INPUT
  ↓
important processing file
  ↓
important processing file
  ↓
important processing file
  ↓
OUTPUT

Parallel branches and merges are acceptable when they are necessary to represent the main data transformation correctly.

The architecture is intentionally NOT:

- a complete system architecture;
- a complete runtime architecture;
- a control-flow graph;
- a call graph;
- a dependency graph;
- a thread/concurrency diagram;
- a lifecycle diagram.

Do not require it to become one.

Do not recommend adding:

- GUI components;
- configuration loaders;
- logging;
- initialization logic;
- lifecycle management;
- threads;
- orchestration-only components;
- secondary subsystems;

unless their omission causes the displayed INPUT → OUTPUT transformation itself to become incorrect or materially misleading.

---

## Entry-Point Rule

The architecture-visualizer intentionally does not show application entry-point files such as:

- `main.py`;
- `__main__.py`;
- equivalent bootstrap or application entry files;

unless explicitly required by the user.

The auditor MAY and SHOULD inspect these files internally when useful.

Entry-point files can be used as evidence to determine:

- where the main data enters the runtime;
- which processing component receives it;
- the real order of important processing files;
- which branches are active;
- where the final output is produced or transmitted.

However:

- the absence of `main.py` from the diagram is NOT an error;
- the absence of `main.py` from `ARCHITECTURE_SUMMARY.md` is NOT an error;
- do not recommend adding `main.py` merely because it initializes or orchestrates the application;
- do not report missing entry-point/bootstrap files unless their omission causes the shown data pipeline itself to be factually incorrect.

Use entry-point files as a source of truth for verifying the pipeline.

Do not require them to appear in the visualization.

---

## General Principle

Do not trust the existing architecture diagram or architecture summary by default.

Treat them as claims that must be verified against the real source code.

First independently reconstruct only the amount of the real program necessary to verify the simplified main INPUT → OUTPUT pipeline.

Then compare that reconstruction against the existing architecture representation.

Do not simply reason from the existing diagram.

Do not assume that something is correct because another AI generated it.

At the same time, do not reconstruct the entire software architecture unless that is necessary to verify a specific claim.

---

## 1. Locate Architecture Files

Look for:

- `architecture.dot`
- `ARCHITECTURE_SUMMARY.md`

Optionally inspect:

- `ARCHITECTURE.png`
- `ARCHITECTURE.svg`

Prefer `architecture.dot` over the rendered image when verifying:

- nodes;
- edges;
- labels;
- inputs;
- outputs;
- relationships.

If the architecture files cannot be found, clearly report that the audit cannot proceed.

Do not generate a new architecture diagram.

---

## 2. Identify the Real Main Data Pipeline

Inspect the repository source code independently.

Determine only what is necessary to reconstruct the main end-to-end INPUT → OUTPUT data pipeline represented by the architecture.

Identify:

- highest-level external input;
- relevant runtime entry point(s), for internal verification;
- important processing files;
- important data passed between components;
- important intermediate artifacts;
- final external output.

Intermediate artifacts may include:

- files;
- checkpoints;
- datasets;
- serialized messages;
- ROS topics/messages;
- sockets;
- database records;
- other cross-component data.

Use entry points and orchestration code internally when necessary to verify the real order of the data pipeline.

Do not require those control files to appear in the architecture unless they directly participate in the represented data transformation.

Do not perform a complete line-by-line analysis of the entire repository unless necessary.

Prioritize the main path represented by the existing architecture.

---

## 3. Verify Files

For every source-file block shown in the architecture:

Check that:

- the file actually exists;
- it really participates in the represented main data pipeline;
- its position in the pipeline is reasonable;
- its relationship with neighboring blocks is plausible.

Also check whether an omitted processing file causes the shown main data pipeline to become incorrect or misleading.

Do NOT require every important runtime file to appear.

Do NOT report helper, orchestration, bootstrap, GUI, lifecycle, configuration, or secondary files as missing merely because they exist or are important to the complete application.

Only report a missing file when it is necessary to understand how the main input data is transformed into the main output data.

---

## 4. Verify Connections

For every architecture edge, verify that the represented relationship exists in the real program.

Determine whether the connection plausibly represents:

- direct data flow;
- file/artifact flow;
- message/topic communication;
- another real cross-component transfer.

Specifically check for false direct connections.

Example:

If the architecture shows:

`train.py -> sample.py`

but the real interface is:

`train.py -> ckpt.pt -> sample.py`

report this if omitting `ckpt.pt` materially changes understanding of the pipeline.

Do not collapse important intermediate artifacts when doing so would misrepresent how data actually moves.

However, harmless simplifications are acceptable when they preserve the correct meaning of the pipeline.

---

## 5. Distinguish Data Flow From Code Dependency

Pay special attention to the difference between:

DATA FLOW

and

CODE DEPENDENCY.

For example:

`train.bin -> train.py -> ckpt.pt -> sample.py`

represents data flow.

But:

`model.py -> train.py`

may only mean that `train.py` imports or uses code from `model.py`.

Do not treat a code dependency as runtime data flow unless actual important data crosses that boundary in the represented direction.

Report architecture relationships that materially confuse these concepts.

Do not penalize harmless visual simplification when the resulting pipeline remains truthful and understandable.

---

## 6. Verify Inputs

For every important file shown in the architecture, verify its displayed input.

Check:

- input name;
- input type;
- whether the input actually reaches that component;
- whether it is relevant to the represented main pipeline.

Prefer exact names from source code when they can be determined reliably.

If the architecture uses a simplified logical name rather than an exact variable name, determine whether the simplification is still accurate.

Do not report harmless simplification as an error.

Do not require every secondary input to be shown.

Focus on the important input relevant to the main pipeline.

---

## 7. Verify Outputs

For every important file shown in the architecture, verify its displayed output.

Check:

- output name;
- output type;
- whether the output actually leaves the represented component;
- where the important output goes next;
- whether an important intermediate artifact has been omitted.

For example, distinguish an internal variable such as:

`train_ids`

from a real cross-component artifact such as:

`train.bin`.

Prefer the representation that best helps a developer understand the real interface between important processing stages.

Do not require every secondary or debugging output to appear.

---

## 8. Verify Data Types

Verify important data types shown in the architecture.

Use source-code evidence whenever possible.

If an exact type cannot be confidently determined, it is acceptable for the architecture to use:

`unknown`

Do not invent a more specific type.

Only report a type problem when there is sufficient evidence that the displayed type is incorrect or materially misleading.

Harmless generalization of a type should normally be classified as MINOR or ignored.

---

## 9. Verify Highest-Level Input and Final Output

Check whether the top-level INPUT represents the real external input of the represented main workflow.

Check whether the bottom-level OUTPUT represents the real final result of that workflow.

Report if:

- the displayed input is wrong;
- the architecture begins after an essential data transformation;
- the displayed output is actually only an intermediate result;
- the architecture ends before the meaningful external result;
- the selected active pipeline uses a different primary input.

If the program supports multiple possible inputs but the diagram intentionally represents one valid main path, do not automatically classify the absence of every alternate path as an error.

Only report an alternate input when its omission makes the diagram misleading for the runtime being represented.

---

## 10. Check Important Branches and Merges

Check whether the represented main data pipeline contains important:

- processing branches;
- parallel transformations;
- merges;
- alternate processing paths;

that are absent from the architecture.

Only report them when their omission materially affects understanding of how the main input becomes the main output.

Do not require every runtime branch or secondary subsystem to be represented.

Do not require control-only branches that do not materially change the represented data transformation.

---

## 11. Preserve Intended Simplicity

The auditor must actively preserve the intended simplicity of the architecture.

Do NOT recommend expanding the diagram merely to make it more complete.

Before reporting a missing component, ask:

> Does the developer need this component on the map to correctly understand how the main input becomes the main output?

If the answer is no, do not report its absence as an architecture problem.

A technically incomplete but truthful and useful overview is acceptable.

A complete but overcrowded diagram is not the target of this system.

---

## 12. Evidence Requirement

Every reported issue must be supported by evidence from the repository.

Whenever possible provide:

- file path;
- relevant function/class/section;
- line number or line range when reliably available;
- short explanation of what the code actually does.

Do not report speculative issues as confirmed errors.

If something cannot be confidently verified, explicitly mark it:

`UNCERTAIN`

and explain why.

---

## 13. Severity Levels

Classify findings into:

### CRITICAL

The simplified architecture fundamentally misrepresents the main INPUT → OUTPUT pipeline.

Examples:

- wrong main input;
- wrong main output;
- major processing stages shown in the wrong order;
- nonexistent major data-flow connection;
- omission of a processing stage that makes the represented transformation fundamentally false.

Do NOT classify intentional omission of:

- `main.py`;
- bootstrap logic;
- GUI;
- lifecycle;
- configuration;
- orchestration;
- logging;
- secondary architecture;

as CRITICAL unless that omission makes the represented data pipeline itself incorrect.

### IMPORTANT

The overall main pipeline is understandable, but an important interface, transformation, or relationship is materially inaccurate.

Examples:

- important missing checkpoint/file/message;
- incorrect important input;
- incorrect important output;
- confusing a significant code dependency with data flow;
- materially incorrect data type;
- omission of an important processing stage that changes understanding of the transformation.

### MINOR

Small inaccuracies that do not significantly affect understanding of the main pipeline.

Examples:

- slightly imprecise naming;
- harmless type generalization;
- secondary output omitted;
- minor interface simplification.

Do not inflate minor differences into major problems.

---

## 14. Architecture Summary Verification

If `ARCHITECTURE_SUMMARY.md` exists, verify that it agrees with:

- the simplified Graphviz architecture;
- the actual source code at the same level of abstraction.

For every file represented in the summary, check:

- Purpose;
- Input;
- Input description;
- Output;
- Output description.

The summary should describe the same simplified pipeline as the diagram.

Do not require it to document files intentionally excluded from the diagram, including `main.py`.

Report contradictions between:

- diagram and summary;
- summary and source code.

---

## 15. Token and Work Efficiency

The audit should be targeted.

Do not fully re-document the repository.

Do not recreate the entire architecture.

Do not recreate `ARCHITECTURE.md`.

Do not inspect unrelated:

- tests;
- documentation;
- notebooks;
- examples;
- tooling;
- experimental code;

unless they are needed to verify a specific architecture claim.

If the repository is large, inspect progressively:

1. architecture files;
2. runtime entry point(s), only for verification;
3. files shown in the architecture;
4. their direct data-related dependencies;
5. additional source files only when needed to verify a claim.

Focus effort on verifying the simplified INPUT → OUTPUT map.

---

## 16. Overall Result Rules

Create one of the following results:

### PASS

The simplified architecture accurately represents the important main INPUT → OUTPUT pipeline.

Minor omissions caused by intentional simplification are acceptable.

### PASS WITH MINOR ISSUES

The main pipeline is trustworthy, but small inaccuracies exist that do not materially affect understanding.

### PASS WITH IMPORTANT ISSUES

The main concept and pipeline remain useful, but one or more important interfaces, transformations, or relationships should be corrected.

### FAIL

Use FAIL only when the simplified architecture cannot be trusted as an orientation map because one or more CRITICAL issues fundamentally misrepresent the main INPUT → OUTPUT pipeline.

Do not assign FAIL merely because the architecture is incomplete compared with the full software system.

---

## 17. Output Report

Create:

`ARCHITECTURE_AUDIT.md`

Use this structure:

# Architecture Audit

## Overall Result

One of:

- PASS
- PASS WITH MINOR ISSUES
- PASS WITH IMPORTANT ISSUES
- FAIL

Add a short explanation focused specifically on whether the simplified map is trustworthy for initial developer orientation.

---

## Critical Issues

For every issue:

### Issue 1 — short title

**Architecture claims:**
<what the existing architecture says>

**Actual behavior:**
<what the source code shows>

**Why this matters for the simplified map:**
<why the difference materially affects understanding of the INPUT → OUTPUT pipeline>

**Evidence:**
`path/to/file.py`
<function/section/line reference>

**Recommended correction:**
<smallest correction needed to make the simplified map accurate>

---

## Important Issues

Use the same format.

---

## Minor Issues

Use the same format.

---

## Verified Correct Elements

Briefly list important parts of the simplified architecture that were independently verified as correct.

Examples:

- main external input;
- major processing order;
- important intermediate interfaces;
- final output;
- important data types.

---

## Uncertain Elements

List anything that could not be verified confidently.

Do not treat uncertainty as a confirmed architecture error.

---

## Final Assessment

Briefly answer:

- Is the simplified architecture trustworthy as a first-orientation map?
- Is the main input correct?
- Is the main output correct?
- Is the main INPUT → processing → OUTPUT flow correct?
- Are the important files shown appropriate?
- Are the important interfaces sufficiently accurate?
- Has the diagram remained appropriately simple?

Keep the report concise.

---

## 18. Architecture Visualizer Correction Prompt

After completing the audit, check whether there is at least one:

- CRITICAL issue;
- IMPORTANT issue.

If at least one CRITICAL or IMPORTANT issue exists, create:

`ARCHITECTURE_FIX_PROMPT.md`

If the audit contains only:

- MINOR issues;
- UNCERTAIN elements;
- or no issues;

do NOT create `ARCHITECTURE_FIX_PROMPT.md`.

Minor issues alone do not justify modifying the architecture.

---

### Purpose of the Correction Prompt

`ARCHITECTURE_FIX_PROMPT.md` must contain a ready-to-use prompt for the `architecture-visualizer` skill.

The developer should be able to copy the contents directly into Codex.

The correction prompt must be based only on confirmed CRITICAL and IMPORTANT findings from the current audit.

Do not include:

- MINOR issues;
- UNCERTAIN findings;
- speculative improvements;
- unrelated architecture changes.

The purpose is to make the smallest necessary corrections while preserving the simple orientation-map philosophy.

---

### Required Correction Prompt Behavior

The generated prompt must instruct the `architecture-visualizer` to:

1. Use the `architecture-visualizer` skill.

2. Read:
   - `architecture.dot`;
   - `ARCHITECTURE_SUMMARY.md`;
   - `ARCHITECTURE_AUDIT.md`.

3. Correct only the confirmed CRITICAL and IMPORTANT issues.

4. For every requested correction, clearly explain:
   - what the current architecture shows;
   - what the source code actually does;
   - why the current representation is materially inaccurate;
   - the minimal correction required.

5. Preserve all architecture elements that the audit verified as correct.

6. Do not redesign unrelated parts of the diagram.

7. Preserve the simple architecture philosophy:

   INPUT
     ↓
   important processing files
     ↓
   OUTPUT

8. Preserve useful parallel branches or merges when they are necessary for the main data pipeline.

9. Do not turn the visualization into:
   - a complete system architecture;
   - a call graph;
   - a complete control-flow graph;
   - a thread architecture;
   - a lifecycle diagram;
   - a full dependency graph.

10. Do not add:
   - `main.py`;
   - `__main__.py`;
   - bootstrap files;
   - GUI;
   - configuration loaders;
   - logging;
   - lifecycle components;
   - orchestration-only components;

   merely because the auditor inspected them.

11. Add a previously omitted component only when it is necessary to correct a confirmed CRITICAL or IMPORTANT error in the main INPUT → OUTPUT pipeline.

12. Preserve simplicity and readability.

13. Update:
   - `architecture.dot`;
   - `ARCHITECTURE.png`;
   - `ARCHITECTURE.svg`;
   - `ARCHITECTURE_SUMMARY.md`.

14. Do not modify project source code.

15. After making the corrections, briefly report exactly what changed.

---

### Required Structure of ARCHITECTURE_FIX_PROMPT.md

Use approximately this format:

# Architecture Visualizer Correction Prompt

Use the `architecture-visualizer` skill.

The current architecture was independently checked by the
`architecture-auditor`.

Correct only the confirmed CRITICAL and IMPORTANT issues below.

Do not redesign unrelated parts of the architecture.

## Issue 1 — <short title>

Current architecture:

<what is currently shown>

Actual behavior:

<what the source code actually does>

Required correction:

<the smallest precise correction required>

## Issue 2 — <short title>

Current architecture:

<...>

Actual behavior:

<...>

Required correction:

<...>

## Constraints

- Preserve all verified-correct parts of the architecture.
- Keep the visualization simple and readable.
- Preserve the INPUT → processing files → OUTPUT philosophy.
- Do not add `main.py` or bootstrap files merely for orchestration.
- Do not expand the diagram into a complete runtime architecture.
- Do not make unrelated improvements.
- Do not modify project source code.

Update:

- `architecture.dot`
- `ARCHITECTURE.png`
- `ARCHITECTURE.svg`
- `ARCHITECTURE_SUMMARY.md`

Afterward, briefly report exactly what changed.

---

### Correction Prompt Principle

Recommend the smallest changes necessary to eliminate confirmed CRITICAL and IMPORTANT inaccuracies.

Do not use the correction process as an opportunity to redesign, expand, or beautify unrelated parts of the architecture.

---

## 19. No Automatic Fixes

Do not modify:

- `architecture.dot`;
- `ARCHITECTURE_SUMMARY.md`;
- `ARCHITECTURE.png`;
- `ARCHITECTURE.svg`;
- project source code;

during the audit.

The auditor only:

- analyzes;
- reports;
- optionally creates `ARCHITECTURE_FIX_PROMPT.md`.

The auditor must NOT directly fix the architecture.

If corrections are needed, they should be performed later by the `architecture-visualizer` skill.

---

## 20. Final Console Summary

After completing the audit, print a short summary containing:

- overall audit result;
- number of CRITICAL issues;
- number of IMPORTANT issues;
- number of MINOR issues;
- number of UNCERTAIN elements;
- path to `ARCHITECTURE_AUDIT.md`;
- architecture correction prompt status.

For example:

Overall result: PASS WITH IMPORTANT ISSUES
Critical issues: 0
Important issues: 2
Minor issues: 0
Uncertain elements: 0
Audit report: ARCHITECTURE_AUDIT.md
Correction prompt: ARCHITECTURE_FIX_PROMPT.md

Or:

Overall result: PASS WITH MINOR ISSUES
Critical issues: 0
Important issues: 0
Minor issues: 2
Uncertain elements: 0
Audit report: ARCHITECTURE_AUDIT.md
Correction prompt: not required

---

## Final Principle

The primary objective is NOT to determine whether the diagram contains every part of the program.

The primary objective is to determine whether a developer can safely use the simplified diagram as a quick map for understanding the main data pipeline before personally studying the source code.

The architecture should remain intentionally simple.

The auditor should protect both:

- correctness;
- simplicity.

Prioritize:

1. correctness of the main INPUT → OUTPUT pipeline;
2. plausibility;
3. integrity;
4. consistency with source code;
5. evidence from real source code;
6. correct important interfaces;
7. preservation of diagram simplicity;
8. minimal correction scope;
9. efficiency;
10. concise reporting.

Do not modify project source code.
