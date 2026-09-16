# Installation
**Recommended prerequisite**: Follow the GitHub CLI (`gh`) [installation instructions](https://github.com/cli/cli#installation) before using skills in this repo.

The `gh skill` command helps manage skill versions so you can install, update, and keep track of the skill set used by your environment. For more details, see the GitHub CLI documentation: [gh skill install](https://cli.github.com/manual/gh_skill_install).

For example, run the following command to install the [Illinois Web Toolkit skill](#il-toolkit):

```bash
gh skill install techservicesillinois/jiait-skills il-toolkit --agent github-copilot --scope project
```

# Disclaimer
These skills are provided as is, without warranties or guarantees of performance, suitability, or correctness.

Using these skills may increase token usage and related costs. Output quality, behavior, and reliability may vary depending on the coding assistant (eg. GitHub Copilot), model, system instructions, workspace context, and any local modifications to the skills.

Developers are responsible for reviewing, verifying, securing, and adequately testing all generated or modified code before using it in any environment. This includes confirming correctness, accessibility, security, compliance, and fitness for the intended use case.

By using these skills, or any modified form of them, you assume full responsibility for the generated code and acknowledge that the maintainer(s) of this repository are not liable for any software defects, failures, losses, or other issues present in code generated with these skills.

# Skills
## [browser-tests](browser-tests/SKILL.md)
Generate or update automated browser test suites from end-user acceptance tests or user stories. Reuse the repository's existing test runner and browser automation framework when present; otherwise use the user-specified framework or default to Playwright.

## [il-toolkit](il-toolkit/SKILL.md)
Official University of Illinois Web Toolkit framework and UI guidelines. Use this skill whenever generating, refactoring, or reviewing frontend code (HTML, CSS, JS) to keep output aligned with the Illinois brand and WCAG 2.2 Level AA accessibility requirements.

## [prompts-review](prompts-review/SKILL.md)
Reviews a JSON or CSV dataset of agent or GitHub Copilot prompts and evaluates prompt quality, context quality, scope, validation, model fit, and cost signals. Use when auditing prompt datasets, improving user prompts, scoring prompt engineering quality, or generating rewritten prompts and aggregate findings from structured rows or records.

Prompting guidelines are based on the following resources:
- [Best practices for using GitHub Copilot - GitHub Docs](https://docs.github.com/en/copilot/get-started/best-practices)
- [Best practices for using AI in VS Code](https://code.visualstudio.com/docs/agents/best-practices)
- [Fundamentals | Agentic Coding Handbook](https://tweag.github.io/agentic-coding-handbook/PROMPT_ENGINEERING/)