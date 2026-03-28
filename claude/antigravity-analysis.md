# Google Antigravity: Addendum to Editor Comparative Analysis

*Added March 2026. Based on web research, official documentation, community reviews, and the antigravity.google product page.*

**Context:** This is a supplemental analysis for the [main editor comparison](editor-analysis.md). Google launched Antigravity in public preview in November 2025 — an "agent-first" IDE built on a VS Code fork. It enters the bake-off as a seventh contender.

---

## Table of Contents

1. [What It Is](#1-what-it-is)
2. [Performance](#2-performance)
3. [General Ecosystem](#3-general-ecosystem)
4. [AI Ecosystem](#4-ai-ecosystem)
5. [Costs](#5-costs)
6. [Competitive Trajectory](#6-competitive-trajectory)
7. [Cross-Platform UX](#7-cross-platform-ux)
8. [Extensibility](#8-extensibility)
9. [Debugger Support](#9-debugger-support)
10. [SQL & Database Workflow](#10-sql--database-workflow)
11. [Comparative Positioning](#11-comparative-positioning)

---

## 1. What It Is

Antigravity is Google's answer to Cursor. It is a VS Code fork — like Cursor — but with a distinct design philosophy: **agent-first, not AI-assisted**. Where Cursor augments a human editing loop with AI, Antigravity positions the editor as a "Mission Control" for autonomous agents that plan, code, test, and verify work independently.

The key architectural difference is the three-surface agent model: agents get dedicated access to the **Code Editor**, **Terminal**, and **Browser**. The browser access is what sets Antigravity apart — agents can navigate web pages, fill forms, take screenshots, and record themselves testing the app. This is integrated at the platform level, not bolted on via extensions.

### Key Concepts

- **Mission Control / Manager Surface**: A dashboard showing which agents are working on which tasks, with real-time artifact capture
- **Artifacts**: Task lists, implementation plans, screen recordings, and test evidence produced by agents
- **Fast Mode**: Skips agent planning for simple edits — "vibe coding" mode for quick iteration
- **Multi-agent parallelism**: Up to 5 simultaneous agents on separate tasks

---

## 2. Performance

### Raw Numbers (Estimated)

| Metric | Antigravity |
|--------|-------------|
| Cold startup | ~1.5-2s |
| Idle RAM | 250-500 MB |
| Working RAM (agents active) | 1-4 GB+ |
| Rendering | Chromium (VS Code fork) |
| 120fps capable | No |

### Assessment

Antigravity inherits VS Code's Electron baseline — neither fast nor slow for editing. The real performance story is **agent execution overhead**. Community reports note 3-5 second freezes when agents start tasks, and resource consumption climbs significantly with multiple concurrent agents. This is worse than Cursor's agent overhead and much worse than Zed or Sublime.

**For your workflow**: If tactile responsiveness matters to you (and as an Emacs user, it likely does), Antigravity will feel sluggish. It is not designed to feel fast — it is designed to let agents work fast on your behalf.

### Where in the Performance Tier List

Antigravity slots below Cursor:

1. **Zed** / **Sublime Text** — effectively instant
2. **Emacs daemon** — instant after initial start
3. **VS Code** — noticeable but acceptable
4. **Cursor** — VS Code + AI overhead
5. **Antigravity** — VS Code + heavier agent orchestration overhead
6. **JetBrains** — genuinely slow startup + indexing

---

## 3. General Ecosystem

### VS Code Fork Inheritance

Like Cursor, Antigravity inherits most of VS Code's extension ecosystem. Being a VS Code fork means themes, keybindings, and many extensions work out of the box. The same caveat applies: Microsoft-proprietary extensions (C/C++, Pylance, C# Dev Kit, Remote Development) are not available.

### Antigravity-Specific Ecosystem

Antigravity adds:
- **MCP Store**: A searchable registry for MCP servers (Google Cloud services, databases, third-party integrations). "Click to install" setup for MCP connections.
- **Artifact ecosystem**: Agents produce structured artifacts (plans, recordings, test evidence) — a new primitive that doesn't exist in other editors.

### Assessment

The ecosystem is comparable to Cursor's: broad VS Code compatibility minus Microsoft proprietary extensions, plus Google-specific additions (Cloud SQL, BigQuery, Spanner MCP integrations). The MCP Store is a nice touch but is Google Cloud-centric — it's most useful if you're already in the Google ecosystem.

| Editor | Extensions/Packages | Primary Registry |
|--------|-------------------|------------------|
| VS Code | ~50,000+ | VS Code Marketplace |
| Cursor | Inherits VS Code (minus MS-proprietary) | Open VSX Registry |
| **Antigravity** | **Inherits VS Code (minus MS-proprietary) + MCP Store** | **Open VSX + MCP Store** |

---

## 4. AI Ecosystem

This is Antigravity's reason to exist. Here's how it compares to the field.

### AI Feature Comparison (Antigravity vs. Key Competitors)

| Capability | Antigravity | Cursor | VS Code + Copilot |
|-----------|-------------|--------|-------------------|
| Inline completions | ✅ Gemini 3 Pro | ✅ Custom model | ✅ Copilot |
| Next Edit Suggestions | ✅ | ✅ | ✅ |
| Inline edit | ✅ | ✅ Cmd+K | ✅ Ctrl+I |
| Chat with project context | ✅ | ✅ Deep RAG indexing | ✅ @workspace |
| Agent mode (multi-file, autonomous) | ✅ (primary interface) | ✅ Composer Agent | ✅ Agent Mode |
| Parallel agents | ✅ Up to 5 | ✅ Up to 8 (worktrees) | ✅ Multi-agent |
| Background/async agents | ✅ | ✅ | ✅ Copilot Coding Agent |
| **Agent browser control** | **✅ (unique — native)** | ❌ | ❌ |
| **Agent screen recording** | **✅ (unique — agents record test evidence)** | ❌ | ❌ |
| MCP support | ✅ (MCP Store) | ✅ | ✅ |
| Codebase semantic indexing | ✅ | ✅ Custom embedding model | ⚠️ Improving |
| Local/offline AI | ❌ | ⚠️ Limited | ⚠️ Via extensions |
| BYOK (bring your own key) | ⚠️ (limited — credit system) | ❌ (credit system) | ❌ (subscription) |
| External agent integration | ⚠️ (Gemini-centric) | ❌ | ✅ (any agent via MCP) |

### Model Availability

| Provider | Antigravity |
|----------|-------------|
| Gemini (Google) | ✅ Gemini 3.1 Pro (High/Low), Gemini 3 Flash |
| Claude (Anthropic) | ✅ Claude Sonnet 4.6, Claude Opus 4.6 |
| GPT (OpenAI) | ✅ GPT-OSS 120B |
| Local/Ollama | ❌ |
| BYOK (any provider) | ❌ (credit system) |

### What's Genuinely Different

**Browser as agent surface.** No other editor gives agents native browser control. Cursor and VS Code agents can modify files and run terminal commands, but Antigravity agents can also open a browser, navigate to `localhost:3000`, fill out a form, verify the output, and record a screen capture as proof. This is architecturally closer to what you'd build with Playwright + Claude Code than what any other IDE offers natively.

**Artifact-centric workflow.** Agents produce structured artifacts (task lists, implementation plans, screen recordings) rather than just code diffs. This makes agent work more auditable and reviewable — you can see what the agent planned, what it tried, and what it verified.

### What's Not Different Enough

**Agent quality still depends on model quality.** Gemini 3 Pro scores 76.2% on SWE-bench Verified — competitive but not category-leading. The agent orchestration is novel; the underlying intelligence is comparable to what you get with Claude in Cursor or VS Code.

**No local/offline AI.** If model privacy or offline work matters, Antigravity is a non-starter. Cursor and VS Code at least offer limited local model support through extensions.

### AI Ecosystem Verdict

Antigravity's browser-integrated agent architecture is genuinely innovative. The "agent plans, codes, tests in browser, records proof" loop is something no other editor offers natively. But the underlying model capability (Gemini 3 Pro) is not a clear winner over Claude or GPT alternatives available in Cursor and VS Code. The innovation is in orchestration, not intelligence.

---

## 5. Costs

### Pricing Tiers

| Tier | Cost | What You Get |
|------|------|-------------|
| Free Preview | $0 | Rate-limited Gemini 3 Pro, all features |
| Pro | $20/mo | Higher rate limits, priority access |
| Ultra | $250/mo | Maximum throughput, advanced models |

### Cost Comparison

| Editor | Effective Monthly (with AI) |
|--------|---------------------------|
| VS Code + Copilot Pro | $10 |
| Zed Pro | $10 |
| Cursor Pro | $20 |
| **Antigravity Pro** | **$20** |
| JetBrains All Products + AI Pro | $24-45 |
| Antigravity Ultra | $250 |
| Cursor Ultra | $200 |

### Hidden Costs and Concerns

**Credit system opacity.** Like Cursor, Antigravity uses a credit-based system. The free tier's "generous rate limits" are not clearly published. After the free preview honeymoon, Google introduced subscription tiers, triggering significant community backlash. The credit pricing has been called "opaque" by reviewers.

**Rate limiting in practice.** Multiple community reports indicate that high-reasoning models feel throttled, and actual rate limits are more restrictive than advertised. This is the same pattern Cursor experienced in mid-2025 — initial generosity followed by tightening as costs become real.

**Google Cloud lock-in potential.** The MCP Store's tight integration with Google Cloud services (BigQuery, Cloud SQL, Spanner, AlloyDB) is convenient if you're on GCP. If you're on AWS or Azure, the database integration story is weaker. The MCP Toolbox for Databases is open-source, which mitigates this somewhat.

### Costs Verdict

At $20/mo Pro, Antigravity is priced identically to Cursor and double VS Code + Copilot Pro. The free tier is attractive for evaluation but unreliable for daily use (rate limits). The Ultra tier at $250/mo is the most expensive option in the entire bake-off.

---

## 6. Competitive Trajectory

### The Google Factor

**Funding:** Effectively unlimited. Google/Alphabet has the deepest pockets of any company in this space. Antigravity doesn't need to worry about runway.

**Distribution:** Google has massive developer reach through Android Studio, Firebase, Google Cloud, and the wider Google ecosystem. If Antigravity gets integrated into Google Cloud Workstations or Cloud Shell, it could achieve rapid enterprise adoption.

**Model advantage/risk:** Antigravity is built around Gemini. If Gemini 3.x continues to improve, Antigravity benefits disproportionately. If Gemini falls behind Claude or GPT, Antigravity's "agent-first" value proposition weakens. The multi-model support (Claude, GPT-OSS) hedges this risk but Gemini is clearly the first-class citizen.

**History of abandonment:** Google has a well-earned reputation for killing products. Cloud Code, Android Studio Plugin (original), multiple chat apps, Stadia, etc. Developers are rightly cautious about building workflows around Google tools. This is Antigravity's biggest strategic weakness — not capability, but trust.

### Competitive Position

Antigravity competes most directly with:
- **Cursor** — same archetype (AI-maximalist VS Code fork), same price point, different philosophy (human-augmented vs. agent-first)
- **Windsurf** — another VS Code fork with agentic ambitions (Cascade engine), slightly cheaper ($15/mo)
- **VS Code + Copilot** — Microsoft's integrated offering, cheaper, broader ecosystem

Against JetBrains, Zed, Emacs, and Sublime, Antigravity is playing a different game entirely. Those editors optimize for different things (debugging depth, speed, sovereignty, lightweight editing). Antigravity optimizes for agent autonomy.

### Trajectory Assessment

**Antigravity** — Rising (explosive launch, uncertain staying power). The biggest open question is whether Google sustains investment or eventually subsumes this into a broader product (Cloud Workstations, Project IDX successor, Firebase Studio).

---

## 7. Cross-Platform UX

### Platform Support

| Platform | Status |
|----------|--------|
| macOS | ✅ Good |
| Windows | ✅ Good |
| Linux | ✅ Good (glibc >= 2.28 required) |

Linux requirements: glibc >= 2.28, glibcxx >= 3.4.25 — Ubuntu 20+, Debian 10+, Fedora 36+, RHEL 8+. Arch Linux satisfies these easily.

### Assessment

As a VS Code fork, Antigravity inherits reasonable cross-platform support. No specific Linux GPU issues have been reported (unlike Zed). No AppImage distribution complaints (unlike Cursor). Config sync status is unclear — likely file-based like Cursor rather than Microsoft's built-in sync.

### Where in the Cross-Platform Tier List

Comparable to Cursor — functional everywhere, not best-in-class anywhere:

1. **VS Code** — gold standard (built-in sync + remote dev)
2. **Sublime Text** — remarkably consistent, portable configs
3. **JetBrains** — consistent but sync issues
4. **Emacs** — great Linux/Mac, rough Windows
5. **Antigravity** / **Cursor** — functional, no robust sync
6. **Zed** — great Mac, GPU issues on Linux

---

## 8. Extensibility

### Architecture

Antigravity inherits VS Code's TypeScript/JS extension API. Extensions from Open VSX work. The MCP Store adds a second extension point specifically for AI agent tool integrations.

### What Antigravity Adds Beyond VS Code

- **MCP server marketplace**: Browse and install MCP servers that give agents access to external services (databases, cloud APIs, monitoring)
- **Agent behavior customization**: Configure how agents plan, execute, and verify work (though the depth of this customization is unclear from current documentation)

### What It Doesn't Have

- No equivalent to Emacs's unlimited runtime modification
- No equivalent to JetBrains's deep AST/PSI access
- No Rust/WASM sandboxing like Zed
- Same Microsoft-proprietary extension gap as Cursor

### Extensibility Rating

| Editor | How Easy to Start | How Deep Can You Go |
|--------|------------------|---------------------|
| **Antigravity** | **Very easy (VS Code compatible)** | **Moderate (API boundaries + MCP Store)** |

Comparable to Cursor: easy to start because of VS Code heritage, moderate depth, with the MCP Store as a differentiator for AI-oriented extensibility.

---

## 9. Debugger Support

### Assessment

No specific debugger innovations have been highlighted in Antigravity's marketing or reviews. As a VS Code fork, it inherits VS Code's DAP-based debugging infrastructure — which is very good.

| Feature | Antigravity |
|---------|-------------|
| DAP support | ✅ (inherited from VS Code) |
| Zero-config for common languages | ✅ (inherited) |
| Conditional breakpoints | ✅ |
| Inline variable display | ✅ |
| Thread management | ✅ |
| Remote debugging | ⚠️ (no Remote Development extension — MS-proprietary) |
| Memory/heap views | ❌ |
| Smart step into | ❌ |

### Where in the Debugger Tier List

Same tier as Cursor — inherited VS Code debugging, solid but not JetBrains-level:

1. **JetBrains** — best-in-class
2. **VS Code** — very good, universal DAP
3. **Cursor** / **Antigravity** — inherited from VS Code, solid
4. **Zed** — functional, maturing
5. **Emacs** — functional, text-based UI
6. **Sublime Text** — basic

### For Go Specifically

Delve via dlv-dap will work the same as it does in VS Code. No advantage or disadvantage over Cursor here.

---

## 10. SQL & Database Workflow

This is where Antigravity has an interesting — and Google-tilted — story.

### MCP Toolbox for Databases

Google provides pre-built MCP servers for:
- **Cloud SQL** (PostgreSQL, MySQL, SQL Server)
- **AlloyDB** (PostgreSQL)
- **Spanner**
- **BigQuery**

These are installable from the MCP Store with "click to install" setup. The MCP Toolbox handles authentication and connection pooling. Agents can query databases directly without leaving the IDE.

### What This Means in Practice

The Antigravity SQL workflow is fundamentally different from both Emacs's REPL approach and JetBrains's GUI approach:

**Antigravity's model:** Describe what data you want → agent writes SQL → agent runs it via MCP → agent shows you results → agent iterates if needed.

This is the "AI-native SQL" approach. You don't write SQL; you describe intent. The agent handles the query lifecycle.

### Assessment

| Capability | Antigravity |
|-----------|-------------|
| Schema-aware SQL completion | ⚠️ (via MCP, agent-mediated) |
| Visual schema browser | ❌ |
| ER diagrams | ❌ |
| Editable result grids | ❌ |
| EXPLAIN visualization | ❌ |
| Send region to REPL | ❌ |
| SQL buffer = full editor | ❌ |
| AI-powered SQL | ✅ (primary interface) |
| Multiple simultaneous connections | ✅ (via MCP) |
| Google Cloud database integration | ✅ (best in class for GCP) |
| Non-Google database support | ⚠️ (MCP Toolbox is open-source, community MCP servers exist) |

### For Your Workflow

**Blunt assessment:** Antigravity does not replicate your Emacs `sql-connect` workflow at all. There is no REPL. There is no "write SQL in a buffer, send region." The entire interaction model is agent-mediated — you tell the agent what you want, not the database.

If you're on Google Cloud, the MCP integration is slick. If you're on AWS/Azure or self-hosted databases, the story is weaker.

This is the same fundamental trade-off as Cursor's MCP approach, but with better out-of-the-box Google Cloud support. JetBrains DataGrip and Emacs sql-mode remain far superior for direct, human-driven SQL work.

---

## 11. Comparative Positioning

### Summary Ratings (Antigravity Added to Main Matrix)

| Dimension | Antigravity | Cursor | VS Code | Zed | JetBrains | Emacs | Sublime |
|-----------|-------------|--------|---------|-----|-----------|-------|---------|
| **Performance** | C (agent overhead) | C+ | B | A- | C | B+ | A+ |
| **General Ecosystem** | B+ (VS Code fork + MCP Store) | A- | A+ | C+ | A- | B+ | B- |
| **AI Ecosystem** | A (browser+agent innovation) | A+ | A | B+ | B+ | B- | D |
| **Cost (value)** | B- (opaque credits) | B- | A+ | A+ | B | A+ | A |
| **Trajectory** | B+ (Google backing, abandonment risk) | A+ | A | A- | B+ | C+ | C- |
| **Cross-Platform** | B (functional everywhere) | B- | A | B | B+ | B+ | A- |
| **Extensibility** | B+ (VS Code + MCP Store) | B+ | A | B- | A- | A+ | B |
| **Debugger** | B+ (inherited) | B+ | B+ | B- | A+ | C+ | C- |
| **SQL/Database** | C+ (GCP-centric, agent-mediated) | C+ | B | D | A+ | A | C |

### The Antigravity Archetype

**Google Antigravity** — *The Agent Dispatcher*

Best for: Developers who want to dispatch autonomous agents and review their work rather than write code directly. Full-stack prototyping where the "agent plans, builds, tests, and proves it works" loop is valuable. Google Cloud-heavy workflows where MCP integrations shine.

Weakest: Tactile editing feel (agent freezes, heavy resource use), SQL/database workflow for humans (entirely agent-mediated), Google abandonment risk, opaque credit pricing, no local/offline AI, Gemini dependency.

### Where Antigravity Fits in the Decision Framework

**If your primary question is "should I try Antigravity instead of Cursor?":**
They're different bets. Cursor augments *your* coding with AI assistance. Antigravity delegates coding *to* agents that you supervise. If you're an experienced developer who wants to stay in the loop (and your Emacs background suggests you are), Cursor's human-augmented model is likely a better fit. If you want to experiment with "describe the feature, let agents build it, review the result," Antigravity is the most coherent expression of that idea.

**If your primary question is "does Antigravity change the recommendation order?":**
No. The recommended evaluation order remains:

1. **VS Code** — baseline, lowest risk
2. **Cursor** — AI-maximalist path
3. **JetBrains** — if SQL/debugging are central
4. **Zed** — if speed is a priority

Antigravity is worth a look after Cursor if you want to compare human-augmented (Cursor) vs. agent-first (Antigravity) approaches to AI-assisted development. The free preview makes evaluation painless. But it doesn't displace any of the top four recommendations because:

- It has no SQL REPL workflow (critical for you)
- It has Google's abandonment track record as a strategic risk
- Its pricing trajectory (free → opaque credits → backlash) mirrors Cursor's worst moments
- The agent-first paradigm is genuinely novel but unproven for sustained daily use

---

## Sources

- [Google Developers Blog: Build with Google Antigravity](https://developers.googleblog.com/build-with-google-antigravity-our-new-agentic-development-platform/)
- [InfoWorld: A first look at Google's new Antigravity IDE](https://www.infoworld.com/article/4096113/a-first-look-at-googles-new-antigravity-ide.html)
- [Techmeme: Google debuts Antigravity](https://www.techmeme.com/251118/p29)
- [Google Codelabs: Getting Started with Google Antigravity](https://codelabs.developers.google.com/getting-started-google-antigravity)
- [Google Cloud Blog: Connect Antigravity to Data Cloud services](https://cloud.google.com/blog/products/data-analytics/connect-google-antigravity-ide-to-googles-data-cloud-services)
- [Hacker News discussion](https://news.ycombinator.com/item?id=45967814)
- [DevRadar: Google Antigravity vs Windsurf 2026](https://devradar.dev/compare/google-antigravity-vs-windsurf-2026)
- [Community reviews on Medium](https://medium.com/@ihor.sasovets/my-experience-with-the-new-antigravity-ide-from-google-a1d9bbd2a301)
