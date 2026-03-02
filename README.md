# skill-greptile-init

> **other skills by [@yigitkonur](https://github.com/yigitkonur):**
> [testing mcp servers](https://github.com/yigitkonur/skill-mcp-server-tester) · [extracting design dna from dashboards](https://github.com/yigitkonur/skill-design-soul-saas) · [converting saved webpages to next.js](https://github.com/yigitkonur/skill-snapshot-to-nextjs) · [generating devin review config](https://github.com/yigitkonur/skill-devin-review-init) · [mcp server for searching skills](https://github.com/yigitkonur/mcp-skills-as-context) · [tauri observability & mcp bridge](https://github.com/yigitkonur/skill-tauri-mcp-install)

a claude code skill that generates `.greptile/config.json` and `.greptile/rules.md` for [greptile](https://greptile.com) ai code review. analyzes your codebase and writes review rules tuned to your actual stack, patterns, and architecture.

## what it does

greptile uses a `.greptile/` directory in your repo to configure its automated code reviews. this skill reads your code — not just the framework name — and generates:

- **config.json** — strictness level, comment types, ignore patterns, branch inclusion, status checks, and per-rule definitions with id, scope, severity, and natural language descriptions
- **rules.md** — extended review guidelines with good/bad code examples, pattern explanations, and nuanced rules that don't fit in json

## what goes in config.json

each rule gets:

- **id** — a kebab-case identifier like `mcp-tool-input-validation`
- **rule** — natural language description of what to check and why
- **scope** — file glob patterns where the rule applies (e.g., `src/mcp/tools/**/*.ts`)
- **severity** — high, medium, or low

plus global settings: strictness (1-3), comment types (logic, syntax, style), ignore patterns, branch filters, summary/issues section configuration.

## what goes in rules.md

the extended rules that need code examples and longer explanations. each rule has a good and bad code block showing the pattern in your actual stack. this is where nuance lives — things like "never hold a mutex guard across an await point" with concrete before/after examples.

## scenario coverage

the skill includes reference scenarios for:

- typescript backend api (express/fastify + prisma)
- next.js marketing website (app router + cms)
- next.js dashboard / admin panel
- python django rest api
- tauri desktop app (rust + typescript)
- mcp server (typescript)
- production mcp server (middleware pipelines, token budgets)
- python agent using mcp-use
- monorepo with scoped configs

these are references the agent uses to understand patterns, not templates that get copy-pasted. everything adapts to what it actually finds in your code.

## usage

```
set up greptile for this repo
```

```
generate greptile review config for this project
```

```
create .greptile config with rules for our codebase
```

## install

```bash
npx skills add yigitkonur/skill-greptile-init
```

> works with claude code, cursor, codex, copilot, windsurf, and [30+ other agents](https://skills.sh).

## license

mit
