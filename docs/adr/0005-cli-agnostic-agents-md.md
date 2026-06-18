# CLI-agnostic instructions via a single AGENTS.md

`AGENTS.md` holds the canonical agent instructions. The CLI-specific entrypoints
(`CLAUDE.md`, `OPENCODE.md`, `GEMINI.md`) are thin wrappers that import it, and all CLIs
share the same `modes/*` files and `.{claude,opencode}/skills/*` definitions.

We centralized on one instruction file because career-ops targets many AI coding CLIs
(Claude Code, OpenCode, Gemini, Codex, Qwen, Copilot) via the open agent-skill standard.
Maintaining the full behavior spec separately per CLI would guarantee drift — a rule
fixed in one would rot in the others. The trade-off: genuinely CLI-specific differences
(headless invocation syntax, skill registration) still have to live in each wrapper, so
the boundary "shared behavior in `AGENTS.md`, CLI mechanics in the wrapper" must be held
consciously rather than collapsing everything into one file.
