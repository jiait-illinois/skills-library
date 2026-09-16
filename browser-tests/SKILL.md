---
name: browser-tests
description: Generate or update automated browser test suites from end-user acceptance tests or user stories. Reuse the repository's existing test runner and browser automation framework when present; otherwise use the user-specified framework or default to Playwright.
argument-hint: End-user acceptance tests in the prompt or attached Markdown file, plus any preferred test runner or browser automation framework
---
# Browser Tests

Use this skill to generate a new automated browser test suite or update an existing suite from end-user acceptance tests, user stories, or a Markdown file supplied in the prompt.

This skill is intentionally agnostic to the application code language and web framework. Its job is to inspect the current repository, find the established testing conventions, and add the smallest coherent set of browser tests that proves the requested user-facing behavior.

## When to Use

- Create browser tests from user stories or acceptance criteria
- Expand coverage of an existing browser test suite
- Add regression coverage for recently implemented UI behavior
- Review an existing suite and fill only uncovered acceptance criteria
- Update role-based, CRUD, navigation, or search coverage in a web application

## Defaults

- Use the repository’s existing browser automation framework when one is already present
- If the user explicitly names a browser automation framework, use that
- If no browser automation framework is present and none is specified, use Playwright
- Use the repository’s existing test runner or unit-testing framework conventions when that is how browser tests are already organized
- If the user explicitly names a test runner, use that
- If neither exists clearly in the repo, use the default runner paired with the chosen browser framework
- Reuse the existing browser-test folder structure, fixtures, helpers, and commands when possible

## Inputs

Provide one or more of the following:

- User stories or acceptance tests directly in the prompt
- A Markdown file containing end-user acceptance tests or stories
- The path to an existing browser test file or suite to extend
- The name of a preferred test runner or browser framework, if the default should be overridden

Helpful context:

- Existing test file paths
- Current browser test commands or Makefile targets
- Authentication or role assumptions
- Seed data expectations
- Known selectors, test IDs, or page object patterns
- Whether the task is to create a new suite, update an existing suite, or only close coverage gaps

## Procedure

1. Confirm the source of truth.
   - Identify whether the acceptance criteria are in the prompt or in an attached Markdown file.
   - If the task is large, restate the requested coverage slices before implementation.

2. Inspect the existing test environment.
   - Find the current test runner, browser automation framework, test directories, helper modules, fixtures, and commands.
   - Prefer the existing stack over introducing a new one.
   - If no browser automation framework exists, default to Playwright.

3. Map acceptance criteria to tests.
   - Build a coverage list from each acceptance criterion to one test, one assertion group, or one explicit gap.
   - Note which criteria are already covered and which need implementation.
   - Prefer updating existing tests when that is cleaner than adding a new file.

4. Bound the scope before writing code.
   - For broad requests, separate planning from implementation.
   - Keep role-based suites pragmatic: verify each role’s distinct capabilities without duplicating another role’s entire suite.
   - Use loops, parametrization, shared helpers, or page objects when they reduce repetition without hiding test intent.

5. Implement against the current conventions.
   - Match the repo’s assertion style, fixture style, naming, and folder layout.
   - Prefer robust selectors that already exist in the app or tests.
   - Avoid adding broad infrastructure unless the current repo clearly lacks what is needed.

6. Verify behavior.
   - Run the narrowest relevant browser tests first.
   - Then run the existing broader browser-test command if one exists.
   - If execution requires setup steps such as a local server, test database, or Makefile targets, follow the existing project workflow.

7. Report outcomes clearly.
   - State what was added or updated.
   - State which acceptance criteria are now covered.
   - Call out any requested criteria that were already covered.
   - If tests reveal product bugs or missing behavior, report them separately from test changes and do not silently change product behavior unless the user requested that.

## Review Rubric

For each requested acceptance criterion, check:

1. Is the user-facing behavior observable in a browser?
2. Is there an existing test that already covers it?
3. Can coverage be added with a focused assertion instead of a new end-to-end flow?
4. Does the proposed test use the repo’s current framework and helper patterns?
5. Is the verification deterministic and reasonably fast?
6. Does the suite avoid duplicating equivalent coverage across roles or pages?

## Decision Points

- If the user provides both stories and an existing failing suite, prefer updating the existing suite rather than rewriting it.
- If framework detection is ambiguous, inspect the repo before choosing.
- If the repo has Selenium, Cypress, or Playwright already, stay with that framework unless the user explicitly wants a migration.
- If the repo has no browser framework, use Playwright by default.
- If the existing unit-test runner cannot sensibly host browser tests, use the runner native to the selected browser framework and explain why.
- If the acceptance tests are too broad for one pass, propose a role-based or feature-based implementation order first.
- If authentication, test data, or environment setup is unclear, ask for the missing prerequisite only when inspection of the repo cannot resolve it.
- If a requested assertion would require changing product behavior, implement the test only if the expected behavior is already clearly defined; otherwise report the mismatch.

## Output Format

Prefer this structure:

1. Environment and framework detection
2. Coverage plan or coverage delta
3. Test changes made
4. Verification performed
5. Product bugs or missing behavior found

When the task is planning-only, return:

- detected framework and runner
- target files
- acceptance-criteria-to-test mapping
- execution plan
- open risks or blockers

When the task is implementation, return:

- files updated
- new helpers or parametrization added
- coverage gained
- validation results
- remaining gaps, if any

## Quality Bar

The work is complete when:

- the acceptance-test source is identified explicitly
- the existing test framework and browser automation stack are identified explicitly
- new coverage aligns with current repo conventions
- duplicate coverage is minimized through shared helpers or parametrization where useful
- each requested acceptance criterion is either covered, reported as already covered, or called out as blocked
- at least one executable validation step is performed when the environment allows it
- product bugs discovered through testing are reported separately from the test implementation

## Examples

Example invocation:

Create or update browser tests from the attached user stories. Reuse the repo’s existing browser-test framework and runner if present. Keep role-based coverage pragmatic, use shared helpers where helpful, and verify with the project’s existing browser-test command.

Example follow-up:

Inspect the current suite and close only the uncovered acceptance criteria in the attached Markdown file. Prefer updating existing tests over adding new ones, and report any product bugs found during test execution without changing the application code.
