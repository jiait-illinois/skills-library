---
name: prompts-review
description: 'Review a JSON or CSV dataset of agent or Copilot prompts and evaluate prompt quality, context quality, scope, validation, model fit, and cost signals. Use when auditing prompt datasets, improving user prompts, scoring prompt engineering quality, or generating rewritten prompts and aggregate findings from structured rows or records.'
argument-hint: 'Path to a JSON or CSV file in the workspace, or a pasted JSON/CSV snippet to review'
---

# Prompt Review

Use this skill to review a JSON or CSV dataset where each row or record represents a user prompt or prompt-response pair from an agent session.

Typical fields:

```text
prompt_id,request_text,response_text,model,mode,prompt_tokens,completion_tokens,ai_credits,timestamp
```

The schema can vary. The minimum useful input is a field containing the prompt text, usually `request_text`. In either JSON or CSV, expect fields such as `prompt_id`, `request_text`, `response_text`, `model`, `mode`, `prompt_tokens`, `completion_tokens`, `ai_credits`, and `timestamp` when available.

## When to Use

- Audit a batch of prompts for prompt engineering quality
- Compare prompt quality across models or modes
- Identify why prompts are producing weak or expensive results
- Produce rewritten prompts that are more specific, bounded, and testable
- Summarize recurring prompt anti-patterns across a dataset

## Inputs

Provide one of the following:

- A JSON or CSV file path in the workspace
- A pasted JSON or CSV excerpt
- A description of the schema if field names differ from the example

Helpful fields:

- `prompt_id`: useful for stable row or record identification in reports
- `request_text`: the prompt to evaluate
- `response_text`: useful for checking whether the prompt gave the model enough direction
- `model`: useful for model-fit observations
- `mode`: useful for checking whether the interaction type matches the task
- `prompt_tokens`, `completion_tokens`, `ai_credits`: useful for efficiency and cost observations
- `timestamp`: useful for sequencing or trend analysis

## Procedure

1. Confirm the usable schema.
	- Identify whether the input is JSON or CSV.
	- Identify which field contains the prompt text.
	- Note any missing but optional columns.
	- If the dataset is large, state whether you will review all rows or a sample.
2. Pre-filter non-reviewable conversational rows.
	- Ignore rows that are not meaningful standalone work-item prompts unless the user explicitly asks to review them.
	- Exclude skill invocations that begin with `/`, such as `/prompts-review ...`.
	- Exclude agent continuation or retry prompts, such as `@agent Continue: ...` and `@agent Try Again`.
	- Exclude short plan addendums that only amend an existing plan or checklist without enough standalone task context.
	- If excluded rows are common, mention the exclusion rule in the dataset summary and optionally report a skipped-row count.
3. Establish the review frame.
	- Treat each row primarily as a prompt-quality review.
	- If `response_text` exists, also assess whether the prompt was likely to produce a verifiable, high-quality result.
	- If token or credit columns exist, include efficiency observations without over-weighting them.
4. Evaluate each remaining prompt against the rubric in [prompting-best-practices.md](./references/prompting-best-practices.md).
	- Score or label each prompt consistently.
	- Prefer concrete findings over generic advice.
5. Produce row-level findings.
	- Identify the strongest and weakest aspects of the prompt.
	- Quote or summarize the specific wording that caused the issue.
	- Suggest a revised prompt when the improvement is clear.
6. Produce dataset-level findings.
	- Identify repeated anti-patterns.
	- Group issues such as vague asks, oversized scope, weak constraints, missing verification, thin context, or poor model/mode fit.
	- Call out cost drivers such as broad prompts, repeated restatement, or missing boundaries when supported by the data.
7. Return an actionable output.
	- Include a concise executive summary.
	- Include row-level results in a compact table when practical.
	- Include a short prioritized recommendation list.
	- Include 2-5 exemplar rewrites if they would teach the pattern better than abstract guidance.

## Review Rubric

For each prompt, inspect these dimensions:

1. Task clarity
	- Is the action explicit?
	- Is the desired output format or deliverable clear?
2. Scope control
	- Is the task bounded to one coherent unit of work?
	- Does it avoid broad or multi-project asks unless explicitly intended?
3. Context quality
	- Does the prompt provide the relevant files, symbols, errors, data shape, or environment constraints?
	- Does it point to the right implementation surface?
4. Constraints and standards
	- Does it specify language, framework, coding constraints, or process constraints where needed?
	- Does it avoid ambiguous words like "improve" or "fix" without criteria?
5. Verification and acceptance criteria
	- Does it define a test, observable result, or completion check?
	- Can the model verify its own output?
6. Decomposition and sequencing
	- For complex work, does it split planning from implementation or break work into smaller steps?
7. Tool, mode, and model fit
	- Does the task appear suited for chat, inline, plan, or agent mode?
	- Is the chosen model or mode mismatched with the apparent task complexity?
8. Cost and context efficiency
	- Is the prompt concise without omitting essential context?
	- Does it avoid irrelevant history or repeated instructions?
9. Safety and privacy hygiene
	- Does it avoid exposing secrets or asking the model to infer hidden requirements?
10. Rewrite leverage
	- Is there a small wording change that would materially improve results?

## Decision Points

- If `request_text` is missing or ambiguous, stop and ask which field contains the prompt text.
- If rows appear to be skill invocations, retry prompts, continuation prompts, or plan-only addendums, exclude them by default instead of rating them `weak` unless the user explicitly asks to review those prompt types.
- If `response_text` is missing, continue with prompt-quality review only.
- If token or credit fields are missing, skip efficiency scoring instead of guessing.
- If prompts span unrelated categories, segment the dataset before drawing aggregate conclusions.
- If the JSON or CSV dataset is too large for exhaustive review, sample deliberately and state the sampling rule.
- If a prompt is actually a follow-up in a larger conversation, note that the row may understate the available context.

## Output Format

Prefer this structure:

1. Dataset summary
2. Common findings
3. Row-by-row review
4. Rewritten prompt examples
5. Recommended next experiments

If the user requests the row-by-row review as a file output, default that file to JSON unless they explicitly ask for another format.

When rows are excluded by the default pre-filter, mention that clearly and do not count them in weak-prompt totals unless the user asked for those prompts to be reviewed.

For row-by-row review, use these compact fields:

- `prompt_id`, row number, or timestamp
- Overall rating such as `strong`, `mixed`, or `weak`
- Exact prompt text (not summarized)
- Main issue
- Why it matters
- Suggested rewrite or fix

## Quality Bar

The review is complete when:

- The prompt-bearing field is identified explicitly
- Excluded non-reviewable rows are separated from reviewed rows
- Every reviewed row has at least one concrete finding
- Aggregate patterns are separated from one-off issues
- Rewrites preserve the original user intent instead of changing the task
- Claims about cost, model fit, or output quality are tied to available columns
- The final advice is prioritized and actionable

## Examples

Example invocation:

```text
/prompts-review Review prompts in prompts.json. Use request_text as the primary prompt field, treat response_text as optional evidence, score each row against prompt engineering best practices, and give me the top five rewrite opportunities.
```

Example follow-up:

```text
Review this JSON excerpt and group the prompts by anti-pattern. Flag vague prompts, prompts missing acceptance criteria, and prompts that should have been split into plan then implement. If you save the row-by-row review to a file, use JSON.
```