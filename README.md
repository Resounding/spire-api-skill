# spire-api-skill

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill for building C#/.NET applications against the Spire ERP REST API. When installed, Claude automatically uses this skill's knowledge of Spire API conventions, endpoints, authentication patterns, and C# code patterns whenever you're working on Spire integrations.

## What is a Claude Code skill?

Claude Code skills are markdown-based extensions that give Claude domain-specific knowledge and workflows. Each skill is a `SKILL.md` file containing instructions that Claude follows when the skill is relevant to your task. Skills can be scoped per-project or installed globally for all projects.

## Installation

### Option 1: Personal skill (available in all your projects)

Clone this repo into your personal Claude skills directory:

```cmd
git clone https://github.com/Resounding/spire-api-skill.git %USERPROFILE%\.claude\skills\spire-api
```

### Option 2: Project skill (available only in a specific project)

From your project's root directory:

```cmd
mkdir .claude\skills
git clone https://github.com/Resounding/spire-api-skill.git .claude\skills\spire-api
```

### Option 3: Git submodule (for version-tracked projects)

From your project's root directory:

```cmd
mkdir .claude\skills
git submodule add https://github.com/Resounding/spire-api-skill.git .claude\skills\spire-api
```

## Verification

After installation, start a Claude Code session and ask something like:

> How do I query customers from the Spire API?

Claude should respond using Spire-specific patterns (Basic auth, `HttpClientHandler` with `AllowAutoRedirect = false`, the `SpireListResponse<T>` wrapper, etc.) without you needing to explain them.

## What's included

- **SKILL.md** -- Core skill definition with API fundamentals, C# patterns, filtering, pagination, and common conventions
- **examples.md** -- C# code examples for common Spire workflows
- **docs/** -- Detailed API reference for each Spire module:
  - Customers, Sales Orders, Invoices, Inventory, Purchasing
  - Accounts Payable/Receivable, General Ledger
  - Production, Payroll, Job Costing, CRM, Email, and more

## License

See [LICENSE](LICENSE) for details.
