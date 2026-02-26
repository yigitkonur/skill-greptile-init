# skill-greptile-init

Generate production-ready [Greptile](https://greptile.com) AI code review configuration for any repository.

```bash
npx skills add yigitkonur/skill-greptile-init
```

> Works with Claude Code, Cursor, Codex, Copilot, Windsurf, and [30+ other agents](https://skills.sh).

> **Companion skill:** [skill-devin-review-init](https://github.com/yigitkonur/skill-devin-review-init) — generates `REVIEW.md` for Devin Review. Use both on the same repo for dual coverage.

---

Point it at a repo, and it analyzes the structure, tech stack, documentation, and conventions — then produces tailored `.greptile/` configuration files with rules that catch real bugs, not noise.

## What It Does

1. **Explores your repo** — maps the directory structure, identifies frameworks, finds existing docs and schemas
2. **Decides the config strategy** — single config vs. cascading monorepo overrides, strictness levels per directory
3. **Engineers semantic rules** — rules that leverage Greptile's LLM understanding (not things ESLint can catch)
4. **Maps context files** — points the reviewer to architecture docs, API specs, database schemas
5. **Validates everything** — checks all JSON syntax, scope arrays, ignorePatterns format, and file existence before output

## Pre-Configured Rule Categories

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

These are starting points. The skill always analyzes your actual codebase and tailors rules to what it finds.

## Usage

Once installed, just ask your agent to set up Greptile:

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

## What Gets Generated

```
.greptile/
├── config.json    # Rules, strictness, filters, output format
├── rules.md       # Prose rules with good/bad code examples (when needed)
└── files.json     # Context files for the reviewer (when docs exist)
```

For monorepos, child `.greptile/config.json` files are placed in subdirectories that need different settings.

## Customization

The skill ships pre-configured for TypeScript backends, Next.js apps, Tauri desktop apps, and MCP servers. Customize for your own stacks:

- **`references/scenarios.md`** — Add or modify example configurations for your frameworks
- **`references/config-spec.md`** — Full parameter reference for Greptile features
- **`references/anti-patterns.md`** — Common mistakes and troubleshooting patterns
- **`SKILL.md`** — The main workflow and rule category table

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

## Key Design Decisions

- **Semantic over syntactic**: Every rule is designed for Greptile's LLM reasoning, not pattern matching. If ESLint/Prettier can catch it, the skill won't create a rule for it.
- **Scoped by default**: Every rule gets a `scope` array targeting only the directories where it matters.
- **Repository-first**: The skill always analyzes the actual repo structure before generating config. No generic boilerplate.
- **Validated output**: All JSON is checked for common Greptile anti-patterns (array scopes, newline-separated ignorePatterns, valid severity values, existing file paths) before output.

## License

MIT
