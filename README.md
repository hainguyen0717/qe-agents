# qe-agents

A collection of QE agents and skills for GitHub Copilot. Starting with a Playwright E2E test script creation agent — more agents from the workflow to follow.

The agent reads your project structure and generates test scripts that match your existing conventions. The skill provides the Playwright patterns the agent draws from.

## Structure

```
agents/
  e2e-runner.md       # Agent definition
skills/
  e2e-testing/
    SKILL.md          # Pattern library
```

## How It Works

- **Agent** (`e2e-runner`) — the decision-maker. Reads the project, plans test journeys, generates scripts, handles flaky tests, validates CI.
- **Skill** (`e2e-testing`) — the pattern library. POM templates, config, flaky strategies, artifact management, CI/CD setup.

The agent draws from the skill. When patterns change, update the skill once — the agent picks it up automatically.

## Setup (GitHub Copilot in VS Code)

1. Copy `agents/e2e-runner.md` to `.github/agents/e2e-runner.agent.md` in your project
2. Copy `skills/e2e-testing/SKILL.md` to `.github/prompts/skills/e2e-testing/SKILL.md` in your project
3. Reload VS Code — the agent will appear in Copilot chat

> Paths may vary depending on your VS Code and Copilot version. See the [GitHub Copilot customization docs](https://code.visualstudio.com/docs/copilot/copilot-customization) if the agent doesn't appear.

## Usage

Provide your test scenarios directly — the agent follows them exactly:

```
@e2e-runner create tests for the login flow:
- Happy path: valid credentials → redirect to dashboard
- Failed login: wrong password → error message shown
- Locked account: 5 failed attempts → lockout screen
```

Other prompts:

```
@e2e-runner update the search tests — the input selector changed
@e2e-runner identify flaky tests in tests/e2e/auth/
```

## Adapt It

This is a concise starting point. Extend the skill with:

- Your project-specific locator conventions
- Auth fixture patterns
- Custom reporters
- Environment-specific config

**No `data-testid` attributes in your app?** Add the [Playwright MCP server](https://github.com/microsoft/playwright-mcp) so the agent can inspect the live UI directly for locators before writing scripts:

```json
// .vscode/mcp.json
{
  "servers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

## Credits

Adapted from [everything-claude-code](https://github.com/affaan-m/everything-claude-code)
