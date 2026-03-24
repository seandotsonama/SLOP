# SLOP v1.3
## Simple Language of Prompting

*A self-bootstrapping session metalanguage for LLM interaction*
*Sean Dotson, Mar 23, 2026 - sean@automationama.com*
https://github.com/seandotsonama/SLOP

---

## What Is SLOP

SLOP is a plain-text prompt schema designed to be pasted at the start of any LLM session.
It defines its own rules inline, so the model learns the language from the document itself.
No external tooling, no SDK, no compiler required.

The design borrows XML-adjacent tag conventions that modern LLMs already parse well, and
adds a control-flow and variable layer that existing prompt frameworks omit. It is intentionally
simple, model-agnostic, and usable in any plain-text prompt environment.

---

## The Case for a Standardized Prompting Language in AI

AI is non-deterministic. Small changes in wording can produce different outputs. That variability is acceptable in exploration, but it becomes a liability in production where consistency matters.

A standardized prompting language reduces that variability by enforcing structure. Instead of free-form prompts, inputs follow a defined format with clear roles, constraints, and expected outputs. The objective is not better prompts, it is repeatable results.

This is the same principle that made manufacturing scale. Controlled inputs lead to predictable outputs. AI behaves the same way.

At a minimum, a standardized prompt defines:
	•	role, what the AI is acting as
	•	task, what must be done
	•	input, the data provided
	•	rules, constraints and exclusions
	•	output format, exact structure required

This shifts prompting from conversation to interface.

The advantages are practical. Outputs become more consistent across users and sessions. Reliability improves because results are less dependent on individual phrasing. Teams move faster since they are not rewriting prompts each time. When something breaks, it is easier to isolate whether the issue is in the task, input, or rules. Most importantly, it allows AI to scale across a team instead of relying on a few individuals who know how to “talk to it.”

There are tradeoffs. Structure limits flexibility, which makes it less useful for creative or exploratory work. There is an upfront cost to defining standards and training teams. Standards require maintenance and versioning. It also does not eliminate randomness, it only reduces it. There is a risk of over-structuring, where teams optimize for format instead of outcome.

In practice, this approach works best where outputs need to be repeatable and usable downstream, engineering documentation, sales workflows, operational reporting, and analysis. It is less effective where ambiguity is part of the process.

The shift is straightforward. AI stops being something you interact with casually and becomes something you operate systematically. That is what makes it usable in real systems.

For a deeper example, including SLOP (Simple Language of Prompting):
https://automationnavigator.substack.com/p/ai-slop-simple-language-of-prompting

---

## Color Legend

Each SLOP syntax category is color-coded throughout this document for quick visual scanning.

- ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) Block delimiters `[[ BLOCK ]]`
- ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) Control keywords `!KEYWORD!`
- ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) Mode declarations `|MODE|`
- ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) Variable commands `SET / RECALL / DEFAULT:`
- ![#E67E22](https://placehold.co/15x15/E67E22/E67E22.png) Variable references `{VAR_NAME}`
- ![#95A5A6](https://placehold.co/15x15/95A5A6/95A5A6.png) Inline emphasis `**bold**` / `*italic*` / `` `backtick` ``
- ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) Step declarations `# STEP N:`

---

## Syntax Overview

SLOP uses seven visually distinct syntax conventions. Each maps to a different type of instruction.

| | Convention | Syntax | Purpose |
|---|---|---|---|
| ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) | Block delimiters | `[[ BLOCK ]] / [[ /BLOCK ]]` | Wrap distinct sections of a prompt |
| ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) | Control keywords | `!KEYWORD!` | Alter execution flow or set conditions |
| ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) | Mode declarations | `\|MODE\|` | Set the model's operational posture |
| ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) | Variable commands | `SET / RECALL / DEFAULT:` | Manage session state |
| ![#E67E22](https://placehold.co/15x15/E67E22/E67E22.png) | Variable references | `{VAR_NAME}` | Substitute named values inline |
| ![#95A5A6](https://placehold.co/15x15/95A5A6/95A5A6.png) | Inline emphasis | `**bold**` / `*italic*` / `` `backtick` `` | Signal priority within any block |
| ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) | Step declarations | `# STEP N: [title]` | Define ordered execution units |

---

## Schema Header

Paste this block at the top of any session to activate SLOP. The model reads it once and
applies the rules for the remainder of the session.

```
[[ SLOP v1.3 ]]

You are now operating under SLOP v1.3 (Simple Language of Prompting).
Read and internalize this entire block before responding to anything.
Apply these rules for the remainder of the session unless explicitly instructed otherwise.

PARSING ORDER
Process prompt content in this priority order:
  1. VARS block — load all session variables first
  2. ROLE block — set persona and context
  3. CONSTRAINTS block — hard limits that override all other instructions
  4. STEP declarations — identify all steps and set initial state to PENDING
  5. TASK block — what to do within the current step
  6. INPUT block — the data or content to work with
  7. OUTPUT-FORMAT block — how to structure the response

HEADING WEIGHT
  # H1  = session-level directive or STEP declaration. Highest weight.
  ## H2 = major task or phase boundary within a step.
  ### H3 = sub-task, parameter, or condition.
  #### H4 = detail, example, or clarification. Lowest weight.

INLINE EMPHASIS
  **bold**   = hard requirement. Non-negotiable.
  *italic*   = soft guidance. Apply unless overridden.
  `backtick` = literal string or exact output. Reproduce exactly.

BLOCK DELIMITERS
  Blocks wrap distinct sections. Always include an opening and closing tag.
  Blocks are processed according to PARSING ORDER above, not document position.

CONTROL KEYWORDS
  !STOP!             = Do not proceed unless the stated condition is true. Halt and report if false.
  !GATE!             = Evaluate a condition. If false, pause and ask the user to resolve before continuing.
  !GATE! GOTO: STEP N = If the gate condition is met, jump to the specified step. Skipped steps
                        are marked BYPASSED and may be resumed later.
  !CONFIRM!          = Pause and ask the user to approve before executing the next block.
  !ASSUME!           = Treat the stated condition as true without requiring user confirmation.
  !LOOP!             = Repeat the enclosing block until !DONE! is received or a !GATE! condition is met.
  !DONE!             = Terminate an active !LOOP!.
  !IGNORE!           = Suppress a default model behavior for this session.

MODE DECLARATIONS
  Declare one of the following as a standalone line. Default is |EXECUTE| if none is declared.
  |EXECUTE| = Perform the task. Minimal meta-commentary.
  |PLAN|    = Produce a plan or outline only. Do not execute yet.
  |REVIEW|  = Critique only. Do not rewrite unless explicitly asked.
  |ITERATE| = Expect multiple passes. Hold session context and track changes.
  |EXPLAIN| = Prioritize explanation over output.
  |DRAFT|   = Produce a clearly marked first-pass output for review.

STEP EXECUTION RULES
  Steps are declared as top-level H1 headings: # STEP N: [title]
  Steps must be executed in sequential order.
  You may not advance to the next step until all conditions in the current step are satisfied.
  You may not skip a step except via !GATE! GOTO:.
  Steps carry one of four states: PENDING, ACTIVE, COMPLETE, BYPASSED.
  A step marked BYPASSED may be resumed using RESUME: STEP N THEN GOTO: STEP N
  or RESUME: STEP N THEN CONTINUE.

SUB-STEP RULES
  Sub-steps use decimal notation: # STEP N.M: [title]
  Sub-steps execute sequentially within their parent step.
  A parent step is COMPLETE only when all its sub-steps are COMPLETE.
  Sub-steps carry the same four states: PENDING, ACTIVE, COMPLETE, BYPASSED.
  All control keywords work identically inside sub-steps.
  !GATE! GOTO: may target any step or sub-step (e.g. STEP 3.2 or STEP 5).
  If a jump skips remaining sub-steps, those sub-steps are marked BYPASSED.
  Sub-steps are optional. Steps without internal phases do not require them.

VARIABLE COMMANDS
  SET [VAR] = [value]    Store a named value for use throughout this session.
  RECALL [VAR]           Read and apply the current value of a named variable.
  DEFAULT: [VAR] = [val] Use this value if the variable is not explicitly set.

VARIABLE REFERENCES
  Use {VAR_NAME} anywhere in a prompt or block to substitute the current value.

[[ /SLOP v1.3 ]]
```

---

## ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) Block Delimiters

Blocks create explicit boundaries between different types of content. This prevents the model
from mixing instructions with data, or context with constraints.

The closing tag uses a forward slash: `[[ /BLOCK ]]`

| | Block | Purpose |
|---|---|---|
| ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) | `[[ VARS ]]` | Session variable definitions. Always loaded first. |
| ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) | `[[ ROLE ]]` | Persona, expertise level, or perspective to adopt. |
| ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) | `[[ TASK ]]` | The specific action to perform. One task per block preferred. |
| ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) | `[[ CONSTRAINTS ]]` | Hard limits. These override TASK instructions if there is conflict. |
| ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) | `[[ INPUT ]]` | Raw material: text, data, code, documents, paste content. |
| ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) | `[[ OUTPUT-FORMAT ]]` | Structure, length, and style of the response. |
| ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) | `[[ EXAMPLE ]]` | Reference sample. Do not execute. Model the output after this. |
| ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) | `[[ NOTE ]]` | Low-weight background context. Apply if relevant, do not prioritize. |
| ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) | `[[ SLOP v1.3 ]]` | Schema header. Contains the full rule set for the session. |

---

## ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) Control Keywords

Control keywords alter execution flow. They are always written in uppercase wrapped in
exclamation marks. They can appear inside any block or as standalone lines.

| | Keyword | Behavior |
|---|---|---|
| ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) | `!STOP!` | Halt execution if a condition is not met. Report why. |
| ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) | `!GATE!` | Pause and ask the user to resolve a condition before continuing. |
| ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) | `!GATE! GOTO: STEP N` | If the gate condition is met, jump to the specified step. All skipped steps are marked BYPASSED. |
| ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) | `!CONFIRM!` | Pause and ask the user to explicitly approve before proceeding. |
| ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) | `!ASSUME!` | Declare something true to prevent unnecessary clarification questions. |
| ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) | `!LOOP!` | Repeat the enclosing block on each pass until `!DONE!` or a `!GATE!` is met. |
| ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) | `!DONE!` | Terminate an active `!LOOP!`. |
| ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) | `!IGNORE!` | Suppress a default model behavior for this session. |

---

## ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) Step System

### Purpose

Steps impose a strict sequential execution order on a session. They are the highest-level
organizational unit in SLOP, sitting above blocks and tasks. Use them when a workflow has
discrete phases that must be completed in order, with conditions that must be satisfied before
advancing.

### Declaration

Steps are declared as top-level H1 headings. The step number must be sequential starting at 1.
A short descriptive title should follow the step number.

```
# STEP 1: Gather Requirements
# STEP 2: Validate Inputs
# STEP 3: Generate Draft
# STEP 4: Review and Approve
# STEP 5: Deliver Output
```

### Sub-Steps

Sub-steps break a parent step into ordered internal phases using decimal notation.
Use them when a step contains multiple sequential operations that benefit from independent
state tracking, gating, or status reporting.

#### Sub-Step Declaration

Sub-steps are declared as H1 headings with decimal numbering. The parent step number
comes first, followed by a dot and the sub-step number starting at 1.

```
# STEP 2: Venture Retrieval
# STEP 2.1: Fetch Task List
# STEP 2.2: Filter Scored Ventures
# STEP 2.3: Present Selection
```

#### Sub-Step Rules

- Sub-steps execute sequentially within their parent step.
- A parent step is COMPLETE only when all of its sub-steps are COMPLETE.
- Sub-steps carry the same four states as top-level steps: PENDING, ACTIVE, COMPLETE, BYPASSED.
- All control keywords (![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!STOP!`, ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!GATE!`, ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!GATE! GOTO:`) work identically inside sub-steps.
- ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!GATE! GOTO:` may target any step or sub-step (e.g. `STEP 3.2` or `STEP 5`).
- If a jump skips remaining sub-steps, those sub-steps are marked BYPASSED.
- Sub-steps are optional. Steps without internal phases do not require them.

#### STATUS with Sub-Steps

When sub-steps are present, the STATUS report shows them indented under their parent.

```
STEP 1: Reviewer Identification     [COMPLETE]
STEP 2: Venture Retrieval           [ACTIVE]
  STEP 2.1: Fetch Task List         [COMPLETE]
  STEP 2.2: Filter Scored Ventures  [ACTIVE]
  STEP 2.3: Present Selection       [PENDING]
STEP 3: Proposal Retrieval          [PENDING]
```

### Step States

Each step carries one of four states at any point in the session.

| | State | Meaning |
|---|---------|---|
| ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) | PENDING | Not yet started. Waiting for prior steps to complete. |
| ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) | ACTIVE | Currently executing. Conditions not yet fully satisfied. |
| ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) | COMPLETE | All conditions satisfied. Ready to advance to the next step. |
| ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) | BYPASSED | Skipped via `!GATE! GOTO:`. Resumable by explicit instruction. |

The model should track and report the current state of each step when asked, or when a
state change occurs.

### Advancement Rules

- Steps must be executed in order: STEP 1, STEP 2, STEP 3, and so on.
- You may not advance to the next step until the current step is marked COMPLETE.
- A step is COMPLETE when all conditions, tasks, ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!GATE!` checks, and ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!CONFIRM!` approvals within it have been satisfied.
- You may not skip a step except via an explicit ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!GATE! GOTO: STEP N` instruction.
- If a step contains a ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!STOP!` condition that evaluates to false, the step cannot be marked COMPLETE until the condition is resolved.

### Non-Sequential Jumps via ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!GATE! GOTO:`

A ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!GATE! GOTO:` instruction inside a step can redirect execution to any other step when
a condition is met. This is the only mechanism for non-sequential step execution.

All steps between the current step and the destination step are marked BYPASSED.
BYPASSED steps are not deleted. They retain their content and can be resumed later.

```
# STEP 2: Validate Client Type

[[ TASK ]]
Determine whether the client is an existing account or a new prospect.
!GATE! GOTO: STEP 5 if client is flagged as existing in the INPUT block.
[[ /TASK ]]
```

In this example, if the condition is met, STEP 3 and STEP 4 are marked BYPASSED and
execution moves directly to STEP 5.

### Resuming a Bypassed Step

A BYPASSED step can be resumed at any point using the RESUME command. The user must
specify where execution should go after the resumed step completes.

```
RESUME: STEP 3 THEN GOTO: STEP 6
RESUME: STEP 4 THEN CONTINUE
```

THEN CONTINUE resumes sequential execution from the resumed step onward.
THEN GOTO: STEP N jumps to the specified step after the resumed step completes.

### Step Status Report

At any point the user can request a status report using:

```
STATUS
```

The model will respond with the current state of all steps in the session.

Example response format:

```
STEP 1: Gather Requirements     [COMPLETE]
STEP 2: Validate Client Type    [COMPLETE]
STEP 3: Prepare Proposal        [BYPASSED]
STEP 4: Internal Review         [BYPASSED]
STEP 5: Client Presentation     [ACTIVE]
```

---

## ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) Mode Declarations

Mode declarations set the model's operational posture for the session or task. Declare one
as a standalone line anywhere in the prompt. The most recent declaration takes effect.
Default is `|EXECUTE|` if none is declared.

| | Mode | Behavior |
|---|---|---|
| ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) | `\|EXECUTE\|` | Perform the task. Minimal explanation unless the output requires it. |
| ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) | `\|PLAN\|` | Produce a plan or outline only. Do not produce the deliverable. |
| ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) | `\|REVIEW\|` | Critique only. Do not rewrite unless explicitly asked. |
| ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) | `\|ITERATE\|` | Expect multiple passes. Track and reference prior outputs. |
| ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) | `\|EXPLAIN\|` | Prioritize explanation over deliverable. |
| ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) | `\|DRAFT\|` | Produce a clearly marked first-pass output for review. |

---

## ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) Variable System

Variables give the model persistent named values to reference across a session without
requiring the user to repeat information. They also allow prompts to be reused as templates
by swapping variable values.

### Commands

| | Command | Purpose |
|---|---|---|
| ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) | `SET [VAR] = [value]` | Store a named value for the session. |
| ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) | `RECALL [VAR]` | Read and apply the current value of the named variable. |
| ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) | `DEFAULT: [VAR] = [value]` | Use this value if the variable has not been explicitly set. |

### Variable Types

| | Type | Description |
|---|---|---|
| ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) | Static | Set once, used throughout. |
| ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) | Dynamic | Updated mid-session with `SET`. New value applies immediately. |
| ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) | Computed | Derived from other variables. References other ![#E67E22](https://placehold.co/15x15/E67E22/E67E22.png) `{VAR_NAME}` values. |

### Variable Block

```
[[ VARS ]]
CLIENT        = "Acme Robotics"
TONE          = "direct, professional"
OUTPUT_FORMAT = "markdown with headers"
AUDIENCE      = "PE firm operating partners"
CONTEXT       = "industrial packaging line automation"
MAX_LENGTH    = 600

[[ /VARS ]]
```

### ![#E67E22](https://placehold.co/15x15/E67E22/E67E22.png) Inline Reference

Use ![#E67E22](https://placehold.co/15x15/E67E22/E67E22.png) `{VAR_NAME}` anywhere in a prompt or block to substitute the current value.

```
Draft a summary for {CLIENT} describing the ROI case for robotic palletizing.
Tone: {TONE}. Audience: {AUDIENCE}. Length: {MAX_LENGTH}.
```

### Dynamic Update

Call ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) `SET` at any point mid-session to change a variable. All subsequent references
to that variable will use the new value.

```
SET TONE = informal
SET AUDIENCE = plant floor operators
```

---

## ![#95A5A6](https://placehold.co/15x15/95A5A6/95A5A6.png) Inline Emphasis

| | Marker | Meaning | Use For |
|---|---|---|---|
| ![#95A5A6](https://placehold.co/15x15/95A5A6/95A5A6.png) | `**bold**` | Hard requirement. Non-negotiable. | Compliance rules, format mandates, critical outputs. |
| ![#95A5A6](https://placehold.co/15x15/95A5A6/95A5A6.png) | `*italic*` | Soft guidance. Apply unless overridden. | Tone suggestions, style preferences, context notes. |
| ![#95A5A6](https://placehold.co/15x15/95A5A6/95A5A6.png) | `` `backtick` `` | Literal string. Reproduce exactly. | Variable names, file paths, required output strings. |

---

## Heading Hierarchy

| Level | Syntax | Weight | Use For |
|---|---|---|---|
| H1 | `#` | Highest | ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) STEP declarations and session-level directives |
| H2 | `##` | High | Major task phase or top-level section within a step |
| H3 | `###` | Medium | Sub-task, parameter definition, conditional rule |
| H4 | `####` | Low | Example, clarification, optional detail |

---

## Full Session Template

```
[[ SLOP v1.3 ]]
{paste schema header here}
[[ /SLOP v1.3 ]]

[[ VARS ]]
CLIENT        =
TONE          =
AUDIENCE      =
OUTPUT_FORMAT =
CONTEXT       =
MAX_LENGTH    =
[[ /VARS ]]

[[ ROLE ]]

[[ /ROLE ]]

[[ CONSTRAINTS ]]
- **
- *
- !IGNORE!
[[ /CONSTRAINTS ]]

|EXECUTE|

# STEP 1: [title]

[[ TASK ]]

[[ /TASK ]]

[[ INPUT ]]

[[ /INPUT ]]

[[ OUTPUT-FORMAT ]]

[[ /OUTPUT-FORMAT ]]

# STEP 2: [title]

# STEP 2.1: [sub-step title]

[[ TASK ]]

[[ /TASK ]]

# STEP 2.2: [sub-step title]

[[ TASK ]]

[[ /TASK ]]

# STEP 3: [title]

[[ TASK ]]

[[ /TASK ]]
```

---

## Model Compatibility

| Model | Compatibility | Notes |
|---|---|---|
| Claude Sonnet / Opus | Excellent | XML-adjacent syntax parses cleanly. Block hierarchy, control keywords, and step tracking reliable. |
| GPT-4o | Good | Respects block structure. Control keywords and variable substitution reliable. Step state tracking generally solid. |
| GPT-4-turbo | Good | Similar to GPT-4o. Occasional drift on ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!LOOP!` and step state across long sessions. |
| Gemini 1.5 Pro | Moderate | Follows structure generally. Step state tracking less consistent without explicit STATUS calls. |
| Llama 3 / Mistral | Variable | Smaller models may lose step state across long sessions. Recommend explicit STATUS calls at each step boundary. |
| Local / fine-tuned | Unknown | Validate manually. Step state tracking is the most likely failure point on smaller models. |

### Known Limitations

- Variables do not persist across separate sessions. Each session requires a fresh ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `[[ VARS ]]` block.
- Step state is held in the model's context window. Very long sessions may cause state drift on smaller models. Use STATUS calls to force re-evaluation if behavior seems inconsistent.
- ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!LOOP!` behavior depends on the model's ability to track iteration state. Use explicit ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!GATE!` conditions for better reliability on smaller models.
- In multi-agent systems, include the schema header in each agent's system prompt individually rather than assuming it propagates across agents.

---

## Changelog

| Version | Date | Changes |
|---|---|---|
| v1.0 | 2026-03-23 | Initial release as SPL. Core schema, variable system, control keywords, execution modes, block types. |
| v1.1 | 2026-03-23 | Renamed to SLOP (Simple Language of Prompting). Control keywords wrapped in `!KEYWORD!`. Mode declarations changed to `\|MODE\|` with no prefix. Double brackets retained for blocks. |
| v1.2 | 2026-03-23 | Added STEP system. Sequential execution units with four states: PENDING, ACTIVE, COMPLETE, BYPASSED. Added ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!GATE! GOTO:` for non-sequential jumps. Added RESUME command with THEN GOTO: and THEN CONTINUE options. Added STATUS command for step state reporting. |
| v1.3 | 2026-03-24 | Added sub-step system. Decimal notation (`STEP N.M`) for ordered internal phases within a parent step. Sub-steps inherit all step rules, states, and control keywords. Parent step is COMPLETE only when all sub-steps are COMPLETE. ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!GATE! GOTO:` may target sub-steps directly. STATUS report shows sub-steps indented under parent. |

---

*SLOP sits in the gap between lightweight prompt frameworks like CO-STAR and RISEN,
and full programmatic tools like DSPy and Microsoft POML. It is designed to be
self-bootstrapping, model-agnostic, and usable in any plain-text prompt environment
without external tooling.*
