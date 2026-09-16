---
name: browser-tests
description: Generate or update automated browser test suites from end-user acceptance tests or user stories. Reuse the repository's existing test runner and browser automation framework when present; otherwise use the user-specified framework or default to Playwright.
argument-hint: End-user acceptance tests in the prompt or attached Markdown file, plus any preferred test runner or browser automation framework
---
# Browser Tests

Use this skill to generate a new automated browser test suite or update an existing suite from end-user acceptance tests, user stories, or a Markdown file supplied in the prompt.

This skill is intentionally agnostic to the application code language and web framework. Its job is to inspect the current repository, identify the established testing conventions, and add the smallest coherent set of browser tests needed to prove the requested user-facing behavior.

## When to Use

- Create browser tests from user stories or acceptance criteria
- Expand coverage of an existing browser test suite
- Add regression coverage for recently implemented UI behavior
- Review an existing suite and fill only uncovered acceptance criteria
- Update role-based, CRUD, navigation, or workflow coverage in a web application

## Inputs

Provide one or more of the following:

- User stories or acceptance tests directly in the prompt
- A Markdown file containing end-user acceptance tests or user stories
- The path to an existing browser test file or suite to extend
- The name of a preferred browser automation framework, if it should override the default
- The name of a preferred test runner, if it should override the default

Helpful context:

- Existing test commands, Makefile targets, or CI workflow names
- Authentication or role assumptions
- Seed data expectations
- Known selectors, test IDs, or page object patterns
- Whether the task is to create a new suite, update an existing suite, or close only specific coverage gaps

## Defaults

- Use the repository's existing browser automation framework when one is already present
- If the user explicitly names a browser automation framework, use that
- If no browser automation framework is present and none is specified, use Playwright
- Use the repository's existing test runner or browser-test execution pattern when one is already present
- If the user explicitly names a test runner, use that
- Reuse the existing browser-test directory structure, fixtures, helpers, setup, and commands whenever possible

## Procedure

1. Confirm the acceptance-test source.
	- Identify whether the user stories are in the prompt or in an attached Markdown file.
	- If the task is broad, restate the requested coverage slices before implementation.
2. Inspect the current test environment.
	- Identify the existing browser automation framework, test runner, test directories, fixtures, helpers, and execution commands.
	- Prefer the current stack over introducing a new one.
	- If no browser framework exists, use Playwright.
3. Map acceptance criteria to current coverage.
	- Determine which criteria are already covered, partially covered, or uncovered.
	- Prefer adding focused assertions to existing tests when that is cleaner than adding entirely new flows.
	- If coverage gaps are large, produce a short plan before editing.
4. Bound the scope before writing code.
	- Keep the work to one coherent feature slice, role slice, or approved plan.
	- Keep role-based suites pragmatic: verify distinct capabilities without duplicating another role's entire suite.
	- Use loops, parametrization, shared helpers, or page objects when they reduce repetition without hiding test intent.
5. Implement using repository conventions.
	- Match the current naming, fixture style, assertions, and file layout.
	- Prefer robust selectors that already exist in the app or tests.
	- Add the smallest helper abstraction that materially reduces duplication.
6. Validate the changes.
	- Run the narrowest relevant browser tests first.
	- Then run the existing broader browser-test command if one exists.
	- Follow the repository's existing setup sequence for local servers, test databases, or other prerequisites.
7. Report outcomes clearly.
	- State what was added or updated.
	- State which acceptance criteria are now covered.
	- State which requested criteria were already covered.
	- If tests reveal product bugs or missing behavior, report them separately from the test changes and do not silently change product behavior unless the user requested that.

## Decision Points

- If the repository already uses Playwright, Selenium, or Cypress, stay with that framework unless the user explicitly requests a migration.
- If the repository has no browser automation framework, use Playwright by default.
- If the repository already has a test runner or established browser-test command, reuse it instead of introducing a new one.
- If the acceptance criteria are too broad for one pass, split the work by role, feature, or workflow and plan first.
- If the user asks to update an existing suite, prefer extending it over rewriting it.
- If authentication, test data, or environment setup is unclear, inspect the repository first and ask only for the missing prerequisite that cannot be resolved locally.
- If a requested assertion depends on behavior not clearly implemented in the product, add the test only when the expected behavior is explicitly defined; otherwise report the mismatch.

## Output Format

Prefer this structure:

1. Environment and framework detection
2. Coverage plan or coverage delta
3. Test changes made
4. Verification performed
5. Product bugs or missing behavior found

When the task is planning-only, return:

- detected browser framework and test runner
- target files or likely target files
- acceptance-criteria-to-test mapping
- implementation order
- open risks or blockers

When the task is implementation, return:

- files updated
- helpers or parametrization added
- coverage gained
- validation results
- remaining gaps, if any

## Quality Bar

The work is complete when:

- the acceptance-test source is identified explicitly
- the existing browser automation framework and execution pattern are identified explicitly
- new coverage follows the repository's current test conventions
- duplicate coverage is minimized through shared helpers or parametrization where useful
- each requested acceptance criterion is covered, reported as already covered, or called out as blocked
- at least one executable validation step is performed when the environment allows it
- product bugs discovered through testing are reported separately from the test implementation

## Examples

Example invocation:

```text
Create or update browser tests from the attached user stories. Reuse the repo's existing browser-test framework and runner if present. Keep role-based coverage pragmatic, use shared helpers where helpful, and verify with the project's existing browser-test command.
```

Example follow-up:

```text
Inspect the current browser test suite and close only the uncovered acceptance criteria in the attached Markdown file. Prefer updating existing tests over adding new ones, and report any product bugs found during test execution without changing the application code.
```

Example with explicit framework override:

```text
Generate browser tests for the acceptance criteria in stories.md. The app language and web framework do not matter; inspect the repo and use the existing test runner if one is present. Use Selenium for browser automation, not the default.
```
