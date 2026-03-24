# SLOP v1.4
## Simple Language Of Prompting

*A self-bootstrapping session metalanguage for LLM interaction*
*Sean Dotson, Mar 23, 2026 - sean@automationama.com*
https://github.com/seandotsonama/SLOP

---

## What Is S.L.O.P. (Simple Language Of Prompting)

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
- role, what the AI is acting as
- task, what must be done
- input, the data provided
- rules, constraints and exclusions
- output format, exact structure required

This shifts prompting from conversation to interface.

The advantages are practical. Outputs become more consistent across users and sessions. Reliability improves because results are less dependent on individual phrasing. Teams move faster since they are not rewriting prompts each time. When something breaks, it is easier to isolate whether the issue is in the task, input, or rules. Most importantly, it allows AI to scale across a team instead of relying on a few individuals who know how to "talk to it."

There are tradeoffs. Structure limits flexibility, which makes it less useful for creative or exploratory work. There is an upfront cost to defining standards and training teams. Standards require maintenance and versioning. It also does not eliminate randomness, it only reduces it. There is a risk of over-structuring, where teams optimize for format instead of outcome.

In practice, this approach works best where outputs need to be repeatable and usable downstream, engineering documentation, sales workflows, operational reporting, and analysis. It is less effective where ambiguity is part of the process.

The shift is straightforward. AI stops being something you interact with casually and becomes something you operate systematically. That is what makes it usable in real systems.

For a deeper example, including SLOP (Simple Language of Prompting):
https://automationnavigator.substack.com/p/ai-slop-simple-language-of-prompting

---

## Repository Structure

```
SLOP/
├── README.md                                    ← this file (v1.4 spec + documentation)
├── SLOP-v1.2.slp                                ← legacy spec (v1.2)
├── SLOP-v1.3.slp                                ← legacy spec (v1.3, full color reference)
├── SLOP-v1.4.slp                                ← current spec (v1.4, Dark/Light colors)
├── examples/
│   └── Venture_Reviewer_V200_SLOP.slp           ← complete 7-step example (26KB)
└── vscode-slop/
    ├── package.json
    ├── language-configuration.json
    ├── README.md
    ├── syntaxes/
    │   └── slop.tmLanguage.json                 ← TextMate grammar (v1.4 bracket regex)
    └── themes/
        ├── slop-dark.json
        └── slop-light.json
```

---

## Bracket Types

SLOP v1.4 uses three distinct bracket types. Each shape has exactly one meaning.

| Bracket | Name | Purpose | Example |
|---|---|---|---|
| `[[ ]]` | Double brackets | Block delimiters | `[[ TASK ]]` ... `[[ /TASK ]]` |
| `[ ]` | Single brackets | Variable names in commands | `SET [VMODE] = 1`, `RECALL [REVIEWER_FIELD_ID]` |
| `{ }` | Curly braces | Inline variable substitution | `Draft a summary for {CLIENT}` |

In schema definitions (the header block), angle brackets `< >` denote placeholders:
`SET [<n>] = <value>` means "replace `<n>` with your variable name and `<value>` with the actual value."

In actual usage: `SET [VMODE] = 1`

---

## Color Legend

Each SLOP syntax category is color-coded throughout this document for quick visual scanning.

| Category | Syntax | Dark | Light |
|---|---|---|---|
| Block delimiters | `[[ BLOCK ]]` | ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `#1589F0` | ![#1270C0](https://placehold.co/15x15/1270C0/1270C0.png) `#1270C0` |
| Control keywords | `!KEYWORD!` | ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `#f03c15` | ![#C0392B](https://placehold.co/15x15/C0392B/C0392B.png) `#C0392B` |
| Mode declarations | `\|MODE\|` | ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) `#9B59B6` | ![#8E44AD](https://placehold.co/15x15/8E44AD/8E44AD.png) `#8E44AD` |
| Variable commands | `SET / RECALL / DEFAULT:` | ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) `#2ECC71` | ![#27AE60](https://placehold.co/15x15/27AE60/27AE60.png) `#27AE60` |
| Variable references | `{VAR_NAME}` | ![#E67E22](https://placehold.co/15x15/E67E22/E67E22.png) `#E67E22` | ![#D35400](https://placehold.co/15x15/D35400/D35400.png) `#D35400` |
| Inline emphasis | `**bold**` / `*italic*` / `` `backtick` `` | ![#95A5A6](https://placehold.co/15x15/95A5A6/95A5A6.png) `#95A5A6` | ![#7F8C8D](https://placehold.co/15x15/7F8C8D/7F8C8D.png) `#7F8C8D` |
| Step declarations | `# STEP N:` | ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) `#F1C40F` | ![#B7950B](https://placehold.co/15x15/B7950B/B7950B.png) `#B7950B` |
| Comments | `//` | ![#555577](https://placehold.co/15x15/555577/555577.png) `#555577` | ![#95A5A6](https://placehold.co/15x15/95A5A6/95A5A6.png) `#95A5A6` |

---

## Syntax Overview

SLOP uses seven visually distinct syntax conventions. Each maps to a different type of instruction.

| Convention | Syntax | Purpose | Dark | Light |
|---|---|---|---|---|
| Block delimiters | `[[ BLOCK ]] / [[ /BLOCK ]]` | Wrap distinct sections of a prompt | ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `#1589F0` | ![#1270C0](https://placehold.co/15x15/1270C0/1270C0.png) `#1270C0` |
| Control keywords | `!KEYWORD!` | Alter execution flow or set conditions | ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `#f03c15` | ![#C0392B](https://placehold.co/15x15/C0392B/C0392B.png) `#C0392B` |
| Mode declarations | `\|MODE\|` | Set the model's operational posture | ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) `#9B59B6` | ![#8E44AD](https://placehold.co/15x15/8E44AD/8E44AD.png) `#8E44AD` |
| Variable commands | `SET / RECALL / DEFAULT:` | Manage session state | ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) `#2ECC71` | ![#27AE60](https://placehold.co/15x15/27AE60/27AE60.png) `#27AE60` |
| Variable references | `{VAR_NAME}` | Substitute named values inline | ![#E67E22](https://placehold.co/15x15/E67E22/E67E22.png) `#E67E22` | ![#D35400](https://placehold.co/15x15/D35400/D35400.png) `#D35400` |
| Inline emphasis | `**bold**` / `*italic*` / `` `backtick` `` | Signal priority within any block | ![#95A5A6](https://placehold.co/15x15/95A5A6/95A5A6.png) `#95A5A6` | ![#7F8C8D](https://placehold.co/15x15/7F8C8D/7F8C8D.png) `#7F8C8D` |
| Step declarations | `# STEP N: [title]` | Define ordered execution units | ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) `#F1C40F` | ![#B7950B](https://placehold.co/15x15/B7950B/B7950B.png) `#B7950B` |

---

## Schema Header

Paste this block at the top of any session to activate SLOP. The model reads it once and
applies the rules for the remainder of the session.

```
[[ SLOP v1.4 ]]

You are now operating under SLOP v1.4 (Simple Language of Prompting).
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
  SET [<n>] = <value>    Store a named value for use throughout this session.
  RECALL [<n>]           Read and apply the current value of a named variable.
  DEFAULT: [<n>] = <value> Use this value if the variable is not explicitly set.

VARIABLE REFERENCES
  Use {VAR_NAME} anywhere in a prompt or block to substitute the current value.

[[ /SLOP v1.4 ]]
```

---

## Block Delimiters

Blocks create explicit boundaries between different types of content. This prevents the model
from mixing instructions with data, or context with constraints.

The closing tag uses a forward slash: `[[ /BLOCK ]]`

| Block | Purpose | Dark | Light |
|---|---|---|---|
| `[[ VARS ]]` | Session variable definitions. Always loaded first. | ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `#1589F0` | ![#1270C0](https://placehold.co/15x15/1270C0/1270C0.png) `#1270C0` |
| `[[ ROLE ]]` | Persona, expertise level, or perspective to adopt. | ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `#1589F0` | ![#1270C0](https://placehold.co/15x15/1270C0/1270C0.png) `#1270C0` |
| `[[ TASK ]]` | The specific action to perform. One task per block preferred. | ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `#1589F0` | ![#1270C0](https://placehold.co/15x15/1270C0/1270C0.png) `#1270C0` |
| `[[ CONSTRAINTS ]]` | Hard limits. These override TASK instructions if there is conflict. | ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `#1589F0` | ![#1270C0](https://placehold.co/15x15/1270C0/1270C0.png) `#1270C0` |
| `[[ INPUT ]]` | Raw material: text, data, code, documents, paste content. | ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `#1589F0` | ![#1270C0](https://placehold.co/15x15/1270C0/1270C0.png) `#1270C0` |
| `[[ OUTPUT-FORMAT ]]` | Structure, length, and style of the response. | ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `#1589F0` | ![#1270C0](https://placehold.co/15x15/1270C0/1270C0.png) `#1270C0` |
| `[[ EXAMPLE ]]` | Reference sample. Do not execute. Model the output after this. | ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `#1589F0` | ![#1270C0](https://placehold.co/15x15/1270C0/1270C0.png) `#1270C0` |
| `[[ NOTE ]]` | Low-weight background context. Apply if relevant, do not prioritize. | ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `#1589F0` | ![#1270C0](https://placehold.co/15x15/1270C0/1270C0.png) `#1270C0` |
| `[[ SLOP v1.4 ]]` | Schema header. Contains the full rule set for the session. | ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `#1589F0` | ![#1270C0](https://placehold.co/15x15/1270C0/1270C0.png) `#1270C0` |

---

## Control Keywords

Control keywords alter execution flow. They are always written in uppercase wrapped in
exclamation marks. They can appear inside any block or as standalone lines.

| Keyword | Behavior | Dark | Light |
|---|---|---|---|
| `!STOP!` | Halt execution if a condition is not met. Report why. | ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `#f03c15` | ![#C0392B](https://placehold.co/15x15/C0392B/C0392B.png) `#C0392B` |
| `!GATE!` | Pause and ask the user to resolve a condition before continuing. | ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `#f03c15` | ![#C0392B](https://placehold.co/15x15/C0392B/C0392B.png) `#C0392B` |
| `!GATE! GOTO: STEP N` | If the gate condition is met, jump to the specified step. All skipped steps are marked BYPASSED. | ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `#f03c15` | ![#C0392B](https://placehold.co/15x15/C0392B/C0392B.png) `#C0392B` |
| `!CONFIRM!` | Pause and ask the user to explicitly approve before proceeding. | ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `#f03c15` | ![#C0392B](https://placehold.co/15x15/C0392B/C0392B.png) `#C0392B` |
| `!ASSUME!` | Declare something true to prevent unnecessary clarification questions. | ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `#f03c15` | ![#C0392B](https://placehold.co/15x15/C0392B/C0392B.png) `#C0392B` |
| `!LOOP!` | Repeat the enclosing block on each pass until `!DONE!` or a `!GATE!` is met. | ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `#f03c15` | ![#C0392B](https://placehold.co/15x15/C0392B/C0392B.png) `#C0392B` |
| `!DONE!` | Terminate an active `!LOOP!`. | ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `#f03c15` | ![#C0392B](https://placehold.co/15x15/C0392B/C0392B.png) `#C0392B` |
| `!IGNORE!` | Suppress a default model behavior for this session. | ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `#f03c15` | ![#C0392B](https://placehold.co/15x15/C0392B/C0392B.png) `#C0392B` |
| `RESUME: STEP N THEN GOTO: STEP M` | Resume a BYPASSED step, then jump to a specified step. | ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `#f03c15` | ![#C0392B](https://placehold.co/15x15/C0392B/C0392B.png) `#C0392B` |
| `RESUME: STEP N THEN CONTINUE` | Resume a BYPASSED step, then continue sequentially. | ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `#f03c15` | ![#C0392B](https://placehold.co/15x15/C0392B/C0392B.png) `#C0392B` |
| `STATUS` | Request a state report of all steps in the session. | ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `#f03c15` | ![#C0392B](https://placehold.co/15x15/C0392B/C0392B.png) `#C0392B` |

---

## Step System

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
- All control keywords (`!STOP!`, `!GATE!`, `!GATE! GOTO:`) work identically inside sub-steps.
- `!GATE! GOTO:` may target any step or sub-step (e.g. `STEP 3.2` or `STEP 5`).
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

| State | Meaning | Dark | Light |
|---|---|---|---|
| PENDING | Not yet started. Waiting for prior steps to complete. | ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) `#F1C40F` | ![#B7950B](https://placehold.co/15x15/B7950B/B7950B.png) `#B7950B` |
| ACTIVE | Currently executing. Conditions not yet fully satisfied. | ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) `#F1C40F` | ![#B7950B](https://placehold.co/15x15/B7950B/B7950B.png) `#B7950B` |
| COMPLETE | All conditions satisfied. Ready to advance to the next step. | ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) `#F1C40F` | ![#B7950B](https://placehold.co/15x15/B7950B/B7950B.png) `#B7950B` |
| BYPASSED | Skipped via `!GATE! GOTO:`. Resumable by explicit instruction. | ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) `#F1C40F` | ![#B7950B](https://placehold.co/15x15/B7950B/B7950B.png) `#B7950B` |

The model should track and report the current state of each step when asked, or when a
state change occurs.

### Advancement Rules

- Steps must be executed in order: STEP 1, STEP 2, STEP 3, and so on.
- You may not advance to the next step until the current step is marked COMPLETE.
- A step is COMPLETE when all conditions, tasks, `!GATE!` checks, and `!CONFIRM!` approvals within it have been satisfied.
- You may not skip a step except via an explicit `!GATE! GOTO: STEP N` instruction.
- If a step contains a `!STOP!` condition that evaluates to false, the step cannot be marked COMPLETE until the condition is resolved.

### Non-Sequential Jumps via `!GATE! GOTO:`

A `!GATE! GOTO:` instruction inside a step can redirect execution to any other step when
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

## Mode Declarations

Mode declarations set the model's operational posture for the session or task. Declare one
as a standalone line anywhere in the prompt. The most recent declaration takes effect.
Default is `|EXECUTE|` if none is declared.

| Mode | Behavior | Dark | Light |
|---|---|---|---|
| `\|EXECUTE\|` | Perform the task. Minimal explanation unless the output requires it. | ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) `#9B59B6` | ![#8E44AD](https://placehold.co/15x15/8E44AD/8E44AD.png) `#8E44AD` |
| `\|PLAN\|` | Produce a plan or outline only. Do not produce the deliverable. | ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) `#9B59B6` | ![#8E44AD](https://placehold.co/15x15/8E44AD/8E44AD.png) `#8E44AD` |
| `\|REVIEW\|` | Critique only. Do not rewrite unless explicitly asked. | ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) `#9B59B6` | ![#8E44AD](https://placehold.co/15x15/8E44AD/8E44AD.png) `#8E44AD` |
| `\|ITERATE\|` | Expect multiple passes. Track and reference prior outputs. | ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) `#9B59B6` | ![#8E44AD](https://placehold.co/15x15/8E44AD/8E44AD.png) `#8E44AD` |
| `\|EXPLAIN\|` | Prioritize explanation over deliverable. | ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) `#9B59B6` | ![#8E44AD](https://placehold.co/15x15/8E44AD/8E44AD.png) `#8E44AD` |
| `\|DRAFT\|` | Produce a clearly marked first-pass output for review. | ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) `#9B59B6` | ![#8E44AD](https://placehold.co/15x15/8E44AD/8E44AD.png) `#8E44AD` |

---

## Variable System

Variables give the model persistent named values to reference across a session without
requiring the user to repeat information. They also allow prompts to be reused as templates
by swapping variable values.

### Commands

| Command | Purpose | Dark | Light |
|---|---|---|---|
| `SET [<n>] = <value>` | Store a named value for the session. | ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) `#2ECC71` | ![#27AE60](https://placehold.co/15x15/27AE60/27AE60.png) `#27AE60` |
| `RECALL [<n>]` | Read and apply the current value of the named variable. | ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) `#2ECC71` | ![#27AE60](https://placehold.co/15x15/27AE60/27AE60.png) `#27AE60` |
| `DEFAULT: [<n>] = <value>` | Use this value if the variable has not been explicitly set. | ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) `#2ECC71` | ![#27AE60](https://placehold.co/15x15/27AE60/27AE60.png) `#27AE60` |

### Variable References

| Syntax | Purpose | Dark | Light |
|---|---|---|---|
| `{VAR_NAME}` | Substitute the current value of a named variable inline. | ![#E67E22](https://placehold.co/15x15/E67E22/E67E22.png) `#E67E22` | ![#D35400](https://placehold.co/15x15/D35400/D35400.png) `#D35400` |

### Variable Types

| Type | Description |
|---|---|
| Static | Set once, used throughout. |
| Dynamic | Updated mid-session with `SET`. New value applies immediately. |
| Computed | Derived from other variables. References other `{VAR_NAME}` values. |

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

### Inline Reference

Use `{VAR_NAME}` anywhere in a prompt or block to substitute the current value.

```
Draft a summary for {CLIENT} describing the ROI case for robotic palletizing.
Tone: {TONE}. Audience: {AUDIENCE}. Length: {MAX_LENGTH}.
```

### Dynamic Update

Call `SET` at any point mid-session to change a variable. All subsequent references
to that variable will use the new value.

```
SET [TONE] = informal
SET [AUDIENCE] = plant floor operators
```

---

## Inline Emphasis

| Marker | Meaning | Use For | Dark | Light |
|---|---|---|---|---|
| `**bold**` | Hard requirement. Non-negotiable. | Compliance rules, format mandates, critical outputs. | ![#FFFFFF](https://placehold.co/15x15/FFFFFF/FFFFFF.png) `#FFFFFF` | ![#1A1A2E](https://placehold.co/15x15/1A1A2E/1A1A2E.png) `#1A1A2E` |
| `*italic*` | Soft guidance. Apply unless overridden. | Tone suggestions, style preferences, context notes. | ![#95A5A6](https://placehold.co/15x15/95A5A6/95A5A6.png) `#95A5A6` | ![#7F8C8D](https://placehold.co/15x15/7F8C8D/7F8C8D.png) `#7F8C8D` |
| `` `backtick` `` | Literal string. Reproduce exactly. | Variable names, file paths, required output strings. | ![#BDC3C7](https://placehold.co/15x15/BDC3C7/BDC3C7.png) `#BDC3C7` | ![#566573](https://placehold.co/15x15/566573/566573.png) `#566573` |

---

## Heading Hierarchy

| Level | Syntax | Weight | Use For | Dark | Light |
|---|---|---|---|---|---|
| H1 | `#` | Highest | STEP declarations and session-level directives | ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) `#F1C40F` | ![#B7950B](https://placehold.co/15x15/B7950B/B7950B.png) `#B7950B` |
| H2 | `##` | High | Major task phase or top-level section within a step | ![#d4d4d4](https://placehold.co/15x15/d4d4d4/d4d4d4.png) `#d4d4d4` | ![#2C3E50](https://placehold.co/15x15/2C3E50/2C3E50.png) `#2C3E50` |
| H3 | `###` | Medium | Sub-task, parameter definition, conditional rule | ![#AAAACC](https://placehold.co/15x15/AAAACC/AAAACC.png) `#AAAACC` | ![#5D6D7E](https://placehold.co/15x15/5D6D7E/5D6D7E.png) `#5D6D7E` |
| H4 | `####` | Low | Example, clarification, optional detail | ![#777799](https://placehold.co/15x15/777799/777799.png) `#777799` | ![#7F8C8D](https://placehold.co/15x15/7F8C8D/7F8C8D.png) `#7F8C8D` |

---

## Full Session Template

```
[[ SLOP v1.4 ]]
{paste schema header here}
[[ /SLOP v1.4 ]]

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

## Examples

### Venture Reviewer (complete, production-grade)

[`examples/Venture_Reviewer_V200_SLOP.slp`](examples/Venture_Reviewer_V200_SLOP.slp) is a full 7-step SLOP prompt that guides a reviewer through a structured venture evaluation using ClickUp and Google Drive integrations.

| Step | Name | Sub-steps | Features Used |
|---|---|---|---|
| 1 | Reviewer Identification | none | `!STOP!`, `SET`, identity map via comments |
| 2 | Venture List Retrieval | 2.1, 2.2, 2.3 | `!GATE!`, `!STOP!`, `!IGNORE!`, ClickUp API, `RECALL` |
| 3 | Proposal Retrieval | 3.1, 3.2, 3.3 | `!GATE! GOTO:`, BYPASSED sub-steps, Google Drive API, fallback chain |
| 4 | Mode Selection | none | `!STOP!`, `SET`, three-mode branching |
| 5 | Evaluation | 5.1, 5.2, 5.3 | `!LOOP!`, `!DONE!`, `!GATE!`, conditional branches on `{VMODE}`, web search, `INPUT` block |
| 6 | Final Summary | 6.1, 6.2 | `OUTPUT-FORMAT`, `!LOOP!` adjustment gate, score recalculation |
| 7 | Save Results | 7.1, 7.2 | `!CONFIRM!`, ClickUp write, `!GATE! GOTO: STEP 2` for loop-back, variable reset |

This example demonstrates most SLOP features in a real workflow: sequential steps, sub-steps, conditional branching, loops, gates, variable state management, API integrations, and session looping.

---

## Model Compatibility

| Model | Compatibility | Notes |
|---|---|---|
| Claude Sonnet / Opus | Excellent | XML-adjacent syntax parses cleanly. Block hierarchy, control keywords, and step tracking reliable. |
| GPT-4o | Good | Respects block structure. Control keywords and variable substitution reliable. Step state tracking generally solid. |
| GPT-4-turbo | Good | Similar to GPT-4o. Occasional drift on `!LOOP!` and step state across long sessions. |
| Gemini 1.5 Pro | Moderate | Follows structure generally. Step state tracking less consistent without explicit STATUS calls. |
| Llama 3 / Mistral | Variable | Smaller models may lose step state across long sessions. Recommend explicit STATUS calls at each step boundary. |
| Local / fine-tuned | Unknown | Validate manually. Step state tracking is the most likely failure point on smaller models. |

### Known Limitations

- Variables do not persist across separate sessions. Each session requires a fresh `[[ VARS ]]` block.
- Step state is held in the model's context window. Very long sessions may cause state drift on smaller models. Use STATUS calls to force re-evaluation if behavior seems inconsistent.
- `!LOOP!` behavior depends on the model's ability to track iteration state. Use explicit `!GATE!` conditions for better reliability on smaller models.
- In multi-agent systems, include the schema header in each agent's system prompt individually rather than assuming it propagates across agents.

---

## VS Code Extension

The `vscode-slop/` directory contains a VS Code extension with syntax highlighting and two color themes (SLOP Dark, SLOP Light) for `.slp` files. See [`vscode-slop/README.md`](vscode-slop/README.md) for install instructions.

---

## Changelog

| Version | Date | Changes |
|---|---|---|
| v1.0 | 2026-03-23 | Initial release as SPL. Core schema, variable system, control keywords, execution modes, block types. |
| v1.1 | 2026-03-23 | Renamed to SLOP. Control keywords wrapped in `!KEYWORD!`. Mode declarations changed to `\|MODE\|`. |
| v1.2 | 2026-03-23 | Added STEP system with four states. Added `!GATE! GOTO:`, RESUME, and STATUS commands. |
| v1.3 | 2026-03-24 | Added sub-step system with decimal notation (`STEP N.M`). Parent COMPLETE requires all sub-steps COMPLETE. `!GATE! GOTO:` targets sub-steps. VS Code extension with Dark and Light themes. |
| v1.4 | 2026-03-24 | Variable names in commands now wrapped in brackets: `SET [VAR] = value`, `RECALL [VAR]`, `DEFAULT: [VAR] = value`. Three distinct bracket types: `[[ ]]` blocks, `[ ]` variable names, `{ }` substitution. Schema definition placeholders use angle brackets `<n>`. Completed Venture Reviewer example (7 steps, 26KB). |

---

*SLOP sits in the gap between lightweight prompt frameworks like CO-STAR and RISEN,
and full programmatic tools like DSPy and Microsoft POML. It is designed to be
self-bootstrapping, model-agnostic, and usable in any plain-text prompt environment
without external tooling.*
