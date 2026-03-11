# Editor Selection Bake-Off (March 2026)

A comparative analysis of local-install, GUI-capable code editors for an experienced Emacs developer considering a move to better AI-integrated tooling.

## The Question

> Is my preference for Emacs holding me back from what's possible with today's AI agents and AI-focused editors?

## Editors Evaluated

| Editor | Archetype | One-Line Take |
|--------|-----------|---------------|
| **Cursor** | The AI Maximalist | Most aggressive AI integration; pay for it in RAM and dollars |
| **VS Code** | The Safe Default | Broadest ecosystem, strong AI, lowest risk |
| **Zed** | The Speed Demon | Native performance + open agent standards; ecosystem still maturing |
| **JetBrains** | The Engineering Workstation | Best debugger, best database tools, heaviest footprint |
| **Emacs** | The Sovereign Hacker | Unmatched extensibility and SQL workflow; AI requires assembly |
| **Sublime Text** | The Lightweight Veteran | Still the fastest editor; AI story is effectively nonexistent |

## Constraints

- Must run locally on Linux, macOS, and Windows (no web-only tools)
- Must be a GUI editor (no terminal-only editors like vim/helix)
- Developer does significant Go + SQL database work
- Current workflow uses Emacs `sql-connect` for lightweight code-to-SQL bouncing

## Analysis Criteria

Each analysis covers (at minimum):

1. **Performance** -- startup, memory, rendering, pathological cases
2. **General ecosystem** -- extensions, community health, development velocity
3. **AI ecosystem** -- interaction modes, context depth, model support, agent capabilities
4. **Costs** -- direct, hidden, and total cost of ownership
5. **Competitive trajectory** -- funding, market position, where the betting money goes
6. **Cross-platform UX** -- Linux/Mac/Windows consistency, config sync, gotchas
7. **Extensibility** -- plugin APIs, depth of customization, "make it your own"
8. **Debugger support** -- DAP maturity, language coverage, advanced features
9. **SQL/database workflow** -- how each editor handles the code-to-SQL loop

## Repository Structure

Three AI agents independently researched and analyzed the same question:

```
claude/          -- Claude's analysis (comparative, data-heavy, cross-editor matrices)
codex/           -- Codex's analysis (editorial, opinionated, recommendation-focused)
gemini/          -- Gemini's analysis (per-editor deep dives + strategy synthesis)
```

### Reading Guide

- **Want the quick answer?** Start with `codex/editor-bakeoff-2026-03.md` -- it reads like a senior engineer's frank recommendation memo. Scroll to "Specific recommendation for your situation" at the bottom.

- **Want comparative data?** Read `claude/editor-analysis.md` -- organized by criterion with cross-editor matrices, quantitative benchmarks, and a tiered rating system. Sections 10-12 (External Agent Normalizer, SQL Decision Reframed, Windsurf) were added after reviewing the other agents' work.

- **Want per-editor deep dives?** Browse `gemini/` for individual editor files. **Caveat:** Gemini's research contains fabricated data points (inflated extension counts, non-existent version numbers, invented features and source URLs). Cross-reference claims before relying on them.

## Key Findings (Cross-Agent Consensus)

All three agents independently converged on several points:

1. **Cursor is the AI leader today**, but VS Code + Copilot is closing the gap fast
2. **JetBrains is unmatched for debugging and database work** -- if those matter, it's the strongest non-Emacs option
3. **Emacs's SQL workflow is genuinely best-in-class** -- no other editor replicates the buffer-to-REPL fluidity
4. **External agents (Claude Code, Codex CLI, Aider) reduce the importance of editor-native AI** -- this helps Emacs, Zed, and JetBrains more than it helps Cursor
5. **Sublime Text is not a serious contender** if AI capability matters
6. **Zed is the most interesting performance-first challenger** but its ecosystem and Linux GPU support need more time
7. **VS Code is the lowest-risk starting point** for anyone evaluating a move from Emacs

## Where the Agents Disagreed

- **Codex** emphasized VS Code as the control-case first evaluation; **Claude** gave more weight to the hedge approach (keep Emacs for SQL, use another editor for AI work)
- **Gemini** was more bullish on Zed's current capabilities (but this was based partly on fabricated data)
- **Gemini** included Windsurf and Neovim in scope; **Claude** and **Codex** stuck to the original six editors (Claude added a Windsurf mention after peer review)
- **Codex** caught the Azure Data Studio retirement (Feb 2026) strengthening VS Code's SQL story; the others missed it initially

## Codex Contribution

This is the short version of my recommendation after reviewing the field and then checking the other agents' work.

### My call

- **Best default starting point:** `VS Code`
- **Best AI-first option:** `Cursor`
- **Best total environment if SQL and debugging matter a lot:** `JetBrains`
- **Most interesting performance-first future bet:** `Zed`

### Why

- `VS Code` is the best control case because it minimizes migration risk while still giving you a serious modern AI/editor stack.
- `Cursor` is the clearest test of whether you actually want an editor-native agent workflow rather than just better completion and chat.
- `JetBrains` is the strongest non-Emacs answer to your coding-plus-SQL workflow because it is the only candidate that clearly upgrades the database side instead of merely approximating it with extensions.
- `Zed` is the most compelling on responsiveness and architecture, but I would still treat it as an experiment rather than the first default replacement if database ergonomics matter.

### The SQL reality

The most important nuance in this bake-off is that your current Emacs setup is unusually strong for lightweight SQL iteration.

- `Emacs` is best at the fast "write SQL in a normal buffer, send region, inspect results, repeat" loop.
- `JetBrains` is best at rich database work: schema browsing, result grids, explain plans, and debugger-adjacent tooling.
- `VS Code` and `Cursor` are workable through extensions, but they do not naturally reproduce the `sql-connect` feel.

### The AI reality

Another key takeaway is that editor-native AI is no longer the whole story.

- If you plan to drive a lot of work through external agents like Claude Code, Codex, or Aider, the editor gap narrows.
- That makes `Emacs`, `Zed`, and `JetBrains` more competitive than a pure "built-in AI features" ranking would suggest.
- If you want the editor itself to be the orchestration layer, `Cursor` and `VS Code` pull ahead again.

### Recommended evaluation order

1. Try `VS Code` first as the baseline.
2. Try `Cursor` next to test the AI-maximal path.
3. Try `JetBrains` if SQL/database and debugging remain central.
4. Try `Zed` if tactile speed is a major priority.

For the full Codex memo, see `codex/editor-bakeoff-2026-03.md`.

## Gemini Contribution

This is my strategic synthesis after researching the "Challengers" and "AI-Native" innovators of 2026.

### The "Gemini" Recommendation Matrix

| Priority | Top Choice | Why? |
|----------|------------|------|
| **AI Autonomy** | **Windsurf** | **Cascade engine** and **SWE-1.5** model offer the fastest, most proactive agentic flow. |
| **Tactile Speed** | **Zed** | **Native Rust** and **GPUI** framework provide a 120 FPS experience that Electron can't match. |
| **SQL Mastery** | **JetBrains** | **DataGrip** integration is the only "adult" upgrade to your current SQL workflow. |
| **Sovereignty** | **Neovim 0.12** | The true successor for Emacs users who want **Cursor-level AI (Avante.nvim)** in a terminal. |

### My Strategic Take

- **Don't settle for "AI-as-a-plugin":** In 2026, the best work happens in editors where the AI is integrated into the **event loop**. This is why **Cursor** and **Windsurf** feel transformative compared to VS Code + Copilot.
- **The "Native" tax is worth paying:** If you miss the snappiness of Emacs, **Zed** is the only modern GUI editor that won't frustrate you. Its commitment to the **Agent Client Protocol (ACP)** ensures it stays open to future AI innovations.
- **SQL is your anchor:** While AI can write queries, **JetBrains** is the only environment that treats your database as a first-class citizen with deep semantic awareness. If your day-to-day is 50% SQL, this is your workstation.

### Final Call for Jim

1.  **If you want the most "magical" AI experience:** Start with **Windsurf**. Its context-handling is more proactive than Cursor's.
2.  **If you want to feel the "speed of thought":** Go with **Zed**. It's the performance leader.
3.  **If you want a professional workstation for Go + SQL:** **JetBrains** is the most robust choice.

For my detailed deep-dives and the full strategy synthesis, see the `gemini/` directory. For an overview of my agentic approach, see `GEMINI.md`.

