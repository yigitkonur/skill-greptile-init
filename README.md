# skill-greptile-initializer

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that generates production-ready [Greptile](https://greptile.com) AI code review configuration for any repository.

Point it at a repo, and it analyzes the structure, tech stack, documentation, and conventions — then produces tailored `.greptile/` configuration files with rules that catch real bugs, not noise.

## What It Does

Instead of writing generic boilerplate, this skill:

1. **Explores your repo** — maps the directory structure, identifies frameworks, finds existing docs and schemas
2. **Decides the config strategy** — single config vs. cascading monorepo overrides, strictness levels per directory
3. **Engineers semantic rules** — rules that leverage Greptile's LLM understanding (not things ESLint can catch)
4. **Maps context files** — points the reviewer to architecture docs, API specs, database schemas
5. **Validates everything** — checks all JSON syntax, scope arrays, ignorePatterns format, and file existence before output

## Pre-Configured Rule Categories

The skill ships with deep knowledge of these stacks and their common pitfalls:

| Stack | What It Catches |
|---|---|
| **TypeScript Backend** | Raw SQL, missing error handling, business logic in controllers, unstructured logging |
| **Next.js Website** | Missing metadata/SEO, raw `<img>` tags, unnecessary client components, waterfall fetches, no ISR strategy |
| **Next.js Dashboard** | Missing auth on routes, unvalidated server actions, RBAC bypass, env var exposure in client components |
| **Tauri Desktop App** | Panics in IPC commands, Mutex locks across await, shell injection, path traversal, sensitive data logging |
| **MCP Server (TypeScript)** | Schema/registry mismatch, missing error boundaries, secrets in responses, no AbortSignal propagation |
| **mcp-use Framework (Python)** | Leaked connections, missing timeouts, hardcoded secrets, tool call error swallowing |
| **Python Django** | N+1 queries, missing permission_classes, raw SQL |
| **Go Microservice** | Missing error wrapping, no context propagation, goroutine leaks |
| **React Frontend** | API contract mismatches, design system bypass, direct DOM manipulation |
| **Monorepo** | Cascading configs with per-package strictness and disabled rules |

These are starting points. The skill always analyzes your actual codebase and tailors rules to what it finds — it never copies scenarios verbatim.

## Installation

### Using npx (Recommended)

```bash
npx add-skill yigitkonur/skill-greptile-initializer
```

### Git Clone

Clone this repo into your project's `.claude/skills/` directory:

```bash
# From your project root
mkdir -p .claude/skills
git clone https://github.com/yigitkonur/skill-greptile-initializer.git .claude/skills/greptile-config
```

### Manual Copy

Download the `SKILL.md` and `references/` folder into `.claude/skills/greptile-config/`:

```
your-project/
└── .claude/
    └── skills/
        └── greptile-config/
            ├── SKILL.md
            └── references/
                ├── config-spec.md
                ├── anti-patterns.md
                └── scenarios.md
```

For more details on installing and customizing Claude Code skills, see [skill-snapshot-to-nextjs](https://github.com/yigitkonur/skill-snapshot-to-nextjs) which covers the full installation workflow.

## Usage

Once installed, just ask Claude Code to set up Greptile for your repo:

```
Set up Greptile for this repo
```

```
Configure AI code review for our Next.js dashboard
```

```
I want Greptile to catch security issues in our MCP server — analyze the codebase and generate the config
```

```
We're a monorepo with packages/api and packages/web — set up Greptile with stricter rules on the API
```

The skill triggers on mentions of Greptile, AI code review, PR review configuration, or anything about setting up automated code review rules.

## What Gets Generated

Depending on your repo, the skill produces:

```
.greptile/
├── config.json    # Rules, strictness, filters, output format
├── rules.md       # Prose rules with good/bad code examples (when needed)
└── files.json     # Context files for the reviewer (when docs exist)
```

For monorepos, child `.greptile/config.json` files are placed in subdirectories that need different settings.

## Customization

This skill comes pre-configured with rule categories I use most — TypeScript backends, Next.js apps, Tauri desktop apps, and MCP servers. You can (and should) customize it for your own stacks:

- **`references/scenarios.md`** — Add or modify example configurations for your frameworks
- **`references/config-spec.md`** — Full parameter reference if you need to add new Greptile features
- **`references/anti-patterns.md`** — Common mistakes and troubleshooting patterns
- **`SKILL.md`** — The main workflow and rule category table

Clone the repo and edit these files to match your team's conventions.

## Skill Structure

```
.claude/skills/greptile-config/
├── SKILL.md                          # Main skill (184 lines, always loaded)
├── references/
│   ├── config-spec.md                # Complete parameter reference (280 lines)
│   ├── anti-patterns.md              # Validation + troubleshooting (148 lines)
│   └── scenarios.md                  # 11 example configs (920 lines)
└── evals/
    └── evals.json                    # 9 test cases for skill evaluation
```

The skill uses progressive disclosure — `SKILL.md` is always in context (~184 lines), while reference files are loaded on demand when the skill needs detailed specs or example patterns.

## Key Design Decisions

- **Semantic over syntactic**: Every rule is designed for Greptile's LLM reasoning, not pattern matching. If ESLint/Prettier can catch it, the skill won't create a rule for it.
- **Scoped by default**: Every rule gets a `scope` array targeting only the directories where it matters.
- **Repository-first**: The skill always analyzes the actual repo structure before generating config. No generic boilerplate.
- **Validated output**: All JSON is checked for common Greptile anti-patterns (array scopes, newline-separated ignorePatterns, valid severity values, existing file paths) before output.

## License

MIT
