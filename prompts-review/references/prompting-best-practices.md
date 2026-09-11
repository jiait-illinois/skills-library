# Prompting Best Practices for CSV Reviews

This reference condenses the requested external guidance into a practical review rubric for prompt datasets.

## Source Themes

### GitHub Copilot Best Practices

Use these checks:

- Break complex tasks into smaller prompts when one prompt spans multiple deliverables.
- Be specific about the request, the requirements, and the expected output.
- Provide examples of inputs, outputs, or implementation patterns where useful.
- Give the model helpful context instead of assuming it knows the codebase or intent.
- Review and validate generated output rather than trusting it by default.

### VS Code Agent Best Practices

Use these checks:

- Pick the right interaction mode for the task: inline, chat, plan, or agent.
- Start with a bounded task and define an observable result.
- Provide relevant files, symbols, errors, constraints, and test commands.
- Include explicit verification or acceptance criteria.
- Avoid vague prompts such as "make this better".
- Tell the agent to ask clarifying questions when requirements are ambiguous.
- Keep context lean and remove irrelevant history.
- Separate planning from implementation for cross-cutting or complex work.
- Match the model to the task complexity and watch cost tradeoffs.

### Agentic Coding Handbook Prompt Engineering

Use these checks:

- Start with a clear action, expected output, and key constraints.
- Name the exact file, service, component, or system when possible.
- Treat the prompt like a work item handed to a junior developer: detailed but focused.
- Include code snippets, terminal output, or repository context when the task depends on them.
- Explain the why behind the change when that helps architectural decisions.
- Refine the prompt when the output is weak instead of patching around a bad instruction.
- Validate the result manually and with tests.

## Record-Level Scoring Heuristic

Use a simple three-level rating unless the user asks for numeric scoring.

Before scoring, exclude non-reviewable conversational records by default unless the user explicitly asks to review them. This includes skill invocations that begin with `/`, agent continuation or retry prompts such as `@agent Continue: ...` or `@agent Try Again`, and short plan addendums that do not stand on their own as work-item prompts.

- `strong`: clear, bounded, contextualized, and verifiable
- `mixed`: usable but missing important context, constraints, or checks
- `weak`: vague, oversized, under-specified, or not verifiable

## Common Anti-Patterns

Flag these consistently:

- Vague action verbs with no criteria, such as "improve", "fix", or "finish"
- Multiple major tasks combined into one prompt
- Missing file, symbol, data, or runtime context
- Missing acceptance criteria or no observable definition of done
- Asking for implementation without first planning a large change
- Repeating irrelevant instructions that inflate tokens without improving precision
- Mismatch between task complexity and selected mode or model
- Depending on hidden context from earlier turns without restating the needed facts

## Rewrite Strategy

When rewriting a weak prompt:

1. Preserve the original intent.
2. Narrecord the scope to one coherent task.
3. Add the relevant target surface, such as the file, function, symbol, or dataset.
4. Add constraints that materially affect the output.
5. Add an observable result, test, or review check.
6. Keep the prompt concise after adding the missing structure.

## Suggested Review Output Fields

For each reviewed record, prefer these fields:

- Identifier: record number or timestamp
- Rating: strong, mixed, weak
- Primary issue
- Secondary issues
- Evidence from the prompt text
- Suggested rewrite
- Notes on model, mode, or cost if supported by the record data

## Cautions

- Do not infer prompt quality from output quality alone; weak outputs can still follow a decent prompt if context was unavailable.
- Do not treat token count or AI credits as a defect by themselves; only flag them when the prompt structure likely caused waste.
- Do not overstate conclusions from a partial CSV excerpt or a sampled review.
- Do not label excluded conversational follow-ups as `weak`; report them separately as skipped or out of scope unless the user asked to include them.
