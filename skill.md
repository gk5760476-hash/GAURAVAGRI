---
name: evidence-loop
description: >-
  Use this skill to implement features, fix bugs, or audit components in agricultural
  economics data dashboards using the Evidence-First Development Loop from THE UNPROMPTED.
---

# Evidence-First Development Loop Skill

This custom skill enforces the **Evidence-First Development Loop** described in *"THE UNPROMPTED • Open Source Edition"* to ensure rigorous, evidence-backed software development for agricultural policy visualization tools.

---

## The 10-Phase Development Loop

For any feature modification, bug fix, or audit in this workspace, the agent must execute these ten phases sequentially:

### 1. Discover (Inventory what exists)
*   **Action:** Search and list all active files, configuration settings, and data arrays.
*   **Rules:** Do not assume code structures exist. Use `list_dir` and `grep_search` to verify.
*   **Verification:** Maintain an inventory of data sources (e.g., CSVs, manual arrays) and code entry points (`app.py`).

### 2. Reconstruct (Describe current behavior)
*   **Action:** Describe current behavior *only* using verifiable facts from the code.
*   **Rules:** 
    *   Trace variables from data definition to visualization rendering.
    *   Separate facts from inferences. Mark undocumented assertions as `UNKNOWN`.

### 3. Classify (State evaluation)
*   **Action:** Classify each component or data point into one of these states:
    *   `VERIFIED_IMPLEMENTED` (matches original paper and works)
    *   `PARTIAL` (incomplete implementation or missing states)
    *   `DUPLICATED` (multiple data arrays representing same paper metrics)
    *   `INCONSISTENT` (differences between NSSO data and Rawal's adjustments)
    *   `DEAD_OR_UNWIRED` (unused functions, code block placeholders)
    *   `STUB_OR_MOCK` (hardcoded simulation curves instead of calculated ones)
    *   `MISSING` (lacking necessary metrics like Gini trends)
    *   `UNKNOWN` (insufficient evidence)

### 4. Define Target (Specify canonical states)
*   **Action:** Set absolute expectations for target state:
    *   Identify the canonical data owner (e.g., specific state arrays).
    *   Define equations/state machines (e.g., Gini calculation formula).
    *   Establish visual and functional boundaries.

### 5. Plan (Dependency-ordered packages)
*   **Action:** Break the work down into incremental, dependency-ordered packages.
*   **Rules:** Always implement backend/data logic before designing the visual dashboard interface.

### 6. Implement (Bounded step execution)
*   **Action:** Execute edits one bounded package at a time.
*   **Rules:** Prioritize code correctness and security. Avoid writing multi-layer changes simultaneously.

### 7. Verify (Runtime and data integrity validation)
*   **Action:** Validate the execution using automated and manual scripts:
    *   Run syntax checkers (`python -m py_compile app.py`).
    *   Launch local servers and verify console logs/outputs.
    *   Confirm mathematical outputs against published text.

### 8. Adversarial Review (Try to break the results)
*   **Action:** Identify and test potential failure states:
    *   *Slider boundary conditions:* What happens when the policy slider is set to 0, 100, or negative values?
    *   *Missing data handling:* What happens if a state is missing data?
    *   *Responsiveness:* Does the layout break on small screens?

### 9. Record (Update findings and handoff)
*   **Action:** Document changes, ADRs (Architecture Decision Records), and write the session handoff.
*   **Structure:**
    *   **Goal:** What was changed.
    *   **Files changed:** Complete absolute paths.
    *   **Tests run & Results:** Output logs.
    *   **Residual risks:** Identified compromises or limits.

### 10. Operate (Monitoring and maintenance)
*   **Action:** Monitor resource load and runtime behavior. Establish alerts for runtime errors.

---

## Specific Guidelines for Python/Streamlit Dashboards

1.  **Data Integrity:**
    *   All econometric data must be stored in immutable structures (e.g., Pandas DataFrames or frozen dictionaries) at the module level.
    *   Do not let slider interactions mutate the baseline dataset.
2.  **Visual Uniformity:**
    *   Ensure all charts use consistent color mappings (e.g., matching colors for specific states or indicators across all figures).
    *   Use Streamlit's container system (`st.container`, `st.columns`) to structure the glassmorphic cards layout.
3.  **Error Boundaries:**
    *   Wrap all interactive calculations in try-except blocks to catch division-by-zero or numeric overflow errors.
