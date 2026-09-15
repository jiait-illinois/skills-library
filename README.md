# Installation
**Recommended prerequisite**: Follow the GitHub CLI (`gh`) [installation instructions](https://github.com/cli/cli#installation) before using skills in this repo.

The `gh skill` command helps manage skill versions so you can install, update, and keep track of the skill set used by your environment. For more details, see the GitHub CLI documentation: [gh skill install](https://cli.github.com/manual/gh_skill_install).

For example, run the following command to install the [Illinois Web Toolkit skill](#il-toolkit):

```bash
gh skill install techservicesillinois/jiait-skills il-toolkit --agent github-copilot --scope project
```

# Disclaimer
<!-- TODO: Disclaim skills are deployed as is, skills will increase token usage,
results may vary depending on coding assistant & model used, dev responsibilities,
by using these skills or any modified form of them, you assume full responsibility for the generated code and acknowledge
that the maintainer(s) are not liable for any software defects present in code generated using these skills.
-->
# Skills
## [il-toolkit](il-toolkit/SKILL.md)
Official University of Illinois Web Toolkit framework and UI guidelines. Use this skill whenever generating, refactoring, or reviewing frontend code (HTML, CSS, JS) to keep output aligned with the Illinois brand and WCAG 2.2 Level AA accessibility requirements.

## [prompts-review](prompts-review/SKILL.md)
Reviews a JSON or CSV dataset of agent or GitHub Copilot prompts and evaluates prompt quality, context quality, scope, validation, model fit, and cost signals. Use when auditing prompt datasets, improving user prompts, scoring prompt engineering quality, or generating rewritten prompts and aggregate findings from structured rows or records.

Prompting guidelines are based on the following resources:
- [Best practices for using GitHub Copilot - GitHub Docs](https://docs.github.com/en/copilot/get-started/best-practices)
- [Best practices for using AI in VS Code](https://code.visualstudio.com/docs/agents/best-practices)
- [Fundamentals | Agentic Coding Handbook](https://tweag.github.io/agentic-coding-handbook/PROMPT_ENGINEERING/)