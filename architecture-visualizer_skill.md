---
name: architecture-visualizer
description: Analyze an existing software repository and create a simplified architecture diagram and summary of its main runtime data flow. Use when the user requests an architecture visualization or an explanation centered on inputs, transformations, outputs, and participating source files.
---

## Purpose of This Visualization

This visualization is a simplified map of the program for a developer who is seeing the codebase for the first time.

Its purpose is to help the developer quickly understand:

- what data enters the program;
- what the program produces as its final output;
- which important source files participate in the main data-processing pipeline;
- how the input data is transformed step by step into the output data.

The visualization is intentionally simplified.

It is not intended to fully explain the program, replace reading the source code, or document every architectural detail.

The developer is expected to use this map only as an orientation tool and then inspect the source code personally to understand:

- implementation details;
- algorithms;
- classes and functions;
- configuration;
- control logic;
- edge cases;
- secondary components;
- internal behavior.

The goal is to reduce the time required to understand an unfamiliar codebase, not to replace the developer's own investigation.

Therefore, prioritize:

1. the main INPUT → processing files → OUTPUT pipeline;
2. readability;
3. correctness;
4. only the most important components.

Do not expand the diagram into a complete system architecture.


Create a simple Graphviz architecture diagram for the current Python project.

The goal is to show the main end-to-end data flow from the highest-level external input to the final external output.

First identify:

1. The highest-level external input data.
2. The final external output data.
3. The real `.py` files that participate in transforming the input into the output.

Build the diagram around this structure:

INPUT DATA
    ↓
file_a.py
    ↓
file_b.py
    ↓
file_c.py
    ↓
OUTPUT DATA

Layout requirements:

1. Use a top-to-bottom layout.

2. At the very top, create one ellipse containing the highest-level external input data.

   Show, when available:
   - data name as used in code;
   - data type;
   - short human-readable meaning.

3. At the very bottom, create one ellipse containing the final output data.

   Show, when available:
   - data name as used in code;
   - data type;
   - short human-readable meaning.

4. Between them, use rectangular blocks.

5. For Python projects, rectangular program blocks should represent only real `.py` files from the current repository that participate in the main input-to-output flow.

6. Inside every `.py` block show only:

   <file name>

   IN:
   <variable/topic name> : <type>

   OUT:
   <variable/topic name> : <type>

7. If there are multiple inputs or outputs, list only the important ones that cross the file/module boundary.

8. Use the exact variable, object, message, topic, or field names from the code whenever they can be determined reliably.

9. Use the most specific data type that can be determined reliably, for example:

   frame : np.ndarray
   detections : list[Detection]
   tracks : list[Track]
   image_msg : sensor_msgs/Image
   /detect_result : DetectResultMSG
   score : float

10. If the exact type cannot be determined confidently, write:

   <name> : unknown

   Do not invent types.

11. Do not show internal local variables that exist only inside functions.
    Show only data that enters or leaves the file/module as part of the main architecture flow.

12. Do not show:
   - classes;
   - functions;
   - implementation details;
   - purpose descriptions;
   - external libraries;
   - tests;
   - documentation;
   - `.ipynb` files;
   - requirements/setup files;
   - debug/visualization tools;
   unless they are genuinely part of the main runtime data flow.

13. Arrows must contain no text.
    They should only show where the output of one component goes next.

14. If the flow splits into multiple real `.py` files operating in parallel, place them side by side.

15. If parallel branches later merge, show the merge visually.

16. Do not duplicate the same `.py` file multiple times unless absolutely necessary to represent the real flow.

17. Verify that every `.py` file shown actually exists in the current repository.

18. Prioritize the real runtime input-to-output flow over repository structure.

19. The diagram should answer:

   - What data enters the program?
   - What is its exact name in the code?
   - What type is it?
   - Which `.py` file receives it?
   - What does that file output?
   - What is that output called in the code?
   - What type is it?
   - Where does it go next?
   - What final data does the program produce?

20. Keep the diagram as simple and readable as possible.

21. Generate:

   architecture.dot
   ARCHITECTURE.png
   ARCHITECTURE.svg

22. Do not modify project source code.

23. After generating the diagram, briefly report:
   - highest-level input;
   - final output;
   - `.py` files included in the main path;
   - any input/output names or types that could not be determined confidently.

Prioritize:
1. correctness;
2. exact code-level names;
3. correct data types;
4. clear input-to-output flow;
5. simplicity and readability.

Do not optimize for showing the entire repository.
Optimize for showing how real named data flows through the program.


## Architecture Summary

In addition to the Graphviz architecture diagram, create:

`ARCHITECTURE_SUMMARY.md`

The file must describe the same main runtime path shown in the diagram and follow the same top-to-bottom order.

For every important source file shown in the architecture diagram, include:

### `<file path>`

**Purpose:**  
One short sentence explaining what this file does in the overall program.

**Input:**  
`<exact variable/object/topic name> : <data type>`

Briefly explain what this input actually represents.

Use the earliest important input that enters this file from another part of the program.

**Output:**  
`<exact variable/object/topic name> : <data type>`

Briefly explain what this output actually represents.

Use the final important output that this file sends to another file/component or exposes as a final program result.

### Rules

- Follow the same execution/data-flow order as the architecture diagram.
- Use exact names from the source code whenever they can be determined reliably.
- Use the most specific reliable data type.
- If the name or type cannot be determined confidently, write `unknown`.
- Do not invent names or types.
- Keep every explanation short and beginner-readable.
- Do not list internal temporary variables.
- Do not list every function or class.
- Focus only on the main input/output interface of each important file.
- Keep this document compact enough to use as a quick architecture reference.
- do NOT include `main.py` or another entry-point file as a block in the final graphical diagram;
- do NOT include it in `ARCHITECTURE_SUMMARY.md`;
- use it only as supporting evidence for understanding the INPUT → processing files → OUTPUT flow.

The graphical diagram should begin with the actual external input data, not with the application entry-point file.

The purpose of the diagram is to show how data is transformed, not how the application is initialized.

Remember: this is a developer orientation map, not a complete architecture specification.
