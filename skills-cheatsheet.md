# Skills Cheat Sheet — When to Use Which

Maintained reference. When you download a new skill, tell me "add <skill-id> to the cheat sheet" and I'll append it in the right category with a when-to-use line.

## How to invoke
- Explicit: "use the `skill-id` skill" (most reliable).
- Vague: describe the goal; I'll route to the best match.
- Keep 1-2 skills active at a time to avoid context bloat.

## Open Source / Git / GitHub
| Skill | When to use |
|---|---|
| `github-engineer` | Issues, PRs, reviews, Actions, releases via `gh` CLI |
| `git-intelligence` | History, blame, bisect, branch management, understanding changes |
| `pr-review` | Batch review PRs by complexity (XS→L), post comments |
| `pr-triage` | Audit open PRs, detect duplicates, risk assess |
| `issue-triage` | Audit open issues, categorize, detect duplicates |
| `codebase-explorer` | First pass on an unfamiliar repo: frameworks, entry points, architecture |
| `repomix-explorer` | Pack/explore a large or remote repo for structure overview |
| `ship` | Build, commit, push, version bump release cycle |

## Debugging & Quality
| Skill | When to use |
|---|---|
| `debugging-engineer` | Reproduce, diagnose, fix a bug systematically |
| `code-reviewer` | Serious review: correctness, security, perf, best practices |
| `code-simplifier` | Rust idiomatic simplification (RTK codebase) |
| `test-engineer` | Run/analyze/create unit, integration, API, E2E tests |
| `performance-engineer` | Frontend/backend bottleneck investigation |
| `security-auditor` | Auth, injection, XSS, vulnerability audit |
| `security-guardian` | CLI security: command injection, shell escaping, hooks (RTK) |
| `browser-qa-engineer` | Browser-based UI testing/verification |
| `accessibility-audit` | WCAG 2.1 audit on Webflow pages |

## Frontend / UI / Design
| Skill | When to use |
|---|---|
| `21st-ui-build` | Implement production UI in existing project |
| `21st-ui-explore` | Compare multiple UI directions/variants |
| `21st-ui-review` | Audit UI for a11y, responsive, UX issues |
| `design-taste-frontend` / `taste-skill` | Anti-slop art direction for pages/screens |
| `image-to-code` | Generate design image then implement to match |
| `web-design-guidelines` | Review UI against Web Interface Guidelines |
| `react-bits` | Add animated React Bits components |
| `vantajs` | WebGL animated backgrounds |
| `gsap-*` | GSAP animations (core, timeline, scrolltrigger, react, plugins, perf) |
| `lenis-*` | Lenis smooth scrolling (core, react, gsap, vue, framer, snap) |
| Design style skills (`minimal`, `clean`, `bold`, `neobrutalism`, `glassmorphism`, etc.) | Pick a visual aesthetic for a project |
| `anti-ui-slop` | Ground UI in real screens, avoid generic AI UI |

## Webflow
| Skill | When to use |
|---|---|
| `designer-tools` | Build/manage pages, elements, styles in Webflow Designer |
| `figma-to-webflow` | Translate Figma into Webflow |
| `cms-*` / `bulk-cms-update` | CMS collection setup/updates |
| `interactions` | IX3 GSAP animations |
| `safe-publish` | Publish with pre-checks + confirmation |
| `link-checker`, `site-audit`, `asset-audit` | Site health audits |
| `code-component-command`, `component-scaffold`, `convert-component` | React code components for Webflow |
| `flowkit-naming` | Flowkit CSS naming conventions |

## Automation / n8n
| Skill | When to use |
|---|---|
| `using-n8n-mcp-skills` | Entry point for any n8n work |
| `n8n-workflow-patterns` | Design workflow architecture |
| `n8n-node-configuration` | Configure node params correctly |
| `n8n-expression-syntax` | n8n `{{}}` expressions |
| `n8n-code-javascript` / `n8n-code-python` | Code nodes |
| `n8n-error-handling` | Retries, error branches, 4xx/5xx |
| `n8n-validation-expert` | Interpret validation errors |

## Job Search (Mercury toolkit)
| Skill | When to use |
|---|---|
| `job-scout` | Find roles at target companies |
| `resume-tailor` | Tailor resume/cover letter per role |
| `profile-optimizer` | Optimize LinkedIn for recruiters |
| `experience-bank` | Capture achievements quarterly |
| `linkedin-login` / `linkedin-post` | LinkedIn automation |

## Rust / RTK specific
| Skill | When to use |
|---|---|
| `rtk-tdd` / `tdd-rust` | TDD for Rust filter dev |
| `design-patterns` | Rust patterns for CLI filter modules |
| `performance` | CLI startup/memory/token benchmarks |
| `security-guardian` | CLI security |

## Docs / Architecture
| Skill | When to use |
|---|---|
| `project-architect` | Create/maintain architecture, decisions, conventions, API, DB docs |
| `report` | File an opencode issue with diagnostics |

## Desktop / Media / Other
| Skill | When to use |
|---|---|
| `flue-*` | Control desktop apps (Adobe, Blender, Figma, etc.) |
| `agent-browser` / `playwright-cli` | Browser automation |
| `email` | Send/read email via SMTP/IMAP |
| `brag-slim` | Generate launch video from a project/URL |

## Meta
| Skill | When to use |
|---|---|
| `opencode` | OpenCode itself: config, plugins, MCP, providers, CLI |
| `headroom` | Compress large tool output / reduce tokens |

---
## Maintenance rule
When you install a new skill, tell me: "add `<skill-id>` to the cheat sheet". I'll read its description and insert it into the right category with a one-line when-to-use. If it doesn't fit an existing category, I'll create a new one.
