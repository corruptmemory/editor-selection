# Editor & IDE Comparative Analysis

*Compiled March 2026. All data sourced from web research, official documentation, community surveys, and financial reporting.*

**Editors analyzed:** Cursor, Zed, Sublime Text, VS Code, Emacs, JetBrains IDEs, Google Antigravity

---

## Table of Contents

1. [Performance](#1-performance)
2. [General Ecosystem](#2-general-ecosystem)
3. [AI Ecosystem](#3-ai-ecosystem)
4. [Costs](#4-costs)
5. [Competitive Trajectory](#5-competitive-trajectory)
6. [Cross-Platform UX](#6-cross-platform-ux)
7. [Extensibility](#7-extensibility)
8. [Debugger Support](#8-debugger-support)
9. [SQL & Database Workflow](#9-sql--database-workflow)
10. [The External Agent Normalizer](#10-the-external-agent-normalizer)
11. [The SQL Decision, Reframed](#11-the-sql-decision-reframed)
12. [Notable Omission: Windsurf](#12-notable-omission-windsurf)
13. [Final Comparative Matrix](#13-final-comparative-matrix)

---

## 1. Performance

### Raw Numbers

| Metric | Sublime Text | Zed | Emacs (native-comp) | VS Code | Cursor | JetBrains |
|--------|-------------|-----|---------------------|---------|--------|-----------|
| Cold startup | <0.5s | ~0.12s | 1-4s (daemon: <0.1s) | 0.8-1.2s | ~1.2s | ~10s |
| Idle RAM | ~95 MB | ~142 MB | 50-200 MB | 150-200 MB | 200-400 MB | 2-4 GB |
| Working RAM | 100-200 MB | 200-400 MB | 100-400 MB | 400 MB-3 GB | 500 MB-7 GB+ | 2-6 GB+ |
| Rendering | Custom (OpenGL 4.1) | GPUI (Vulkan/Metal/DX11) | Native toolkit | Chromium | Chromium | JVM Swing |
| 120fps capable | Yes | Yes | No | No | No | No |

### Startup Tier List

1. **Zed** (~120ms) and **Sublime** (<500ms) — effectively instant
2. **Emacs daemon** (<100ms for new frames) — instant *after* initial daemon start
3. **VS Code** (~1s) and **Cursor** (~1.2s) — noticeable but acceptable
4. **Emacs cold** (1-4s) — varies wildly with config complexity
5. **JetBrains** (~10s + minutes of indexing) — genuinely slow

### Pathological Cases (Where Things Break Down)

| Scenario | Worst | Problematic | Fine |
|----------|-------|-------------|------|
| Very long lines (>5K chars) | **Emacs** (100% CPU, fundamental display engine limitation) | VS Code, Cursor (sluggish) | Sublime, Zed, JetBrains |
| Large files (>100 MB) | JetBrains (not designed for this) | VS Code, Cursor, Emacs | **Sublime** (handles gracefully), Zed |
| Memory leaks over time | **Cursor** (7 GB+ reported), VS Code (3 GB+) | JetBrains (steady high baseline) | Sublime, Zed, Emacs |
| Large monorepo (100K+ files) | Zed (extension gaps) | VS Code (extension overhead) | **JetBrains** (built for this after indexing), Sublime |

### Performance Verdict

**Zed and Sublime** are in a class of their own for raw speed — both use native code with GPU-accelerated rendering. Zed has a caveat: on **Linux with Intel integrated GPUs or GNOME**, there are active performance issues (high idle GPU/CPU usage, Vulkan selection problems). These are being fixed but are real today.

**Emacs** is fast for normal editing with native compilation + tree-sitter, but the long-lines problem is a fundamental architectural limitation that will likely never be fixed. The daemon pattern eliminates startup concerns.

**VS Code and Cursor** are "good enough" Electron apps. Cursor runs heavier due to its AI indexing infrastructure, with documented memory leaks pushing to 7 GB+ in extended sessions.

**JetBrains** is the heaviest by far. The tradeoff is intentional: deep semantic indexing costs resources but enables features no lightweight editor can match. If you have 16+ GB RAM, the weight is tolerable. On 8 GB machines, JetBrains can be painful.

---

## 2. General Ecosystem

### Ecosystem Size

| Editor | Extensions/Packages | Primary Registry | Language |
|--------|-------------------|------------------|----------|
| VS Code | ~50,000+ | VS Code Marketplace | TypeScript/JS |
| JetBrains | ~8,860 | JetBrains Marketplace | Kotlin/Java |
| Emacs | ~6,000+ | MELPA/ELPA/NonGNU ELPA | Emacs Lisp |
| Sublime Text | ~6,000 | Package Control | Python |
| Zed | ~1,000 | zed.dev/extensions | Rust/WASM |
| Cursor | Inherits VS Code (minus MS-proprietary) | Open VSX Registry | TypeScript/JS |

### Community Health

| Editor | Dev Survey Usage (2025) | Core Team Size | Release Cadence | Trend |
|--------|------------------------|----------------|-----------------|-------|
| VS Code | 75.9% | Large (Microsoft) | Monthly | Dominant, still growing |
| JetBrains (IntelliJ) | 27.1% | ~2,125 employees | 3x/year | Stable, slight growth |
| Cursor | ~17.9% | ~250 | Quarterly major | Explosive growth |
| Emacs | 4.2% | 3 maintainers + community | ~Annual major | Flat (but ecosystem is vibrant) |
| Sublime Text | <2% (not in top list) | 2-3 people | Irregular | Declining |
| Zed | ~1-2% | Growing startup | Weekly builds | Strong upward |

### Ecosystem Quality Assessment

**VS Code** wins by sheer volume and momentum. Every language, framework, and tool has a VS Code extension. The ecosystem is so dominant that Cursor literally forks it.

**JetBrains** has fewer plugins but higher average quality — the more complex development requirements filter out low-effort extensions. The built-in feature set also reduces the need for plugins.

**Emacs** has a uniquely productive ecosystem relative to its community size. Packages like Magit (best Git interface in any editor), Org-mode (nothing comparable elsewhere), and TRAMP (transparent remote editing) are genuine "killer apps" that keep experienced developers on the platform. The community is in what's been called a "Second Golden Age" — more active core development than in the past decade.

**Sublime Text** has a mature but shrinking ecosystem. Package Control works well, but the community is migrating to VS Code. The 2-3 person team means development is sustainable but not fast.

**Zed** has the smallest ecosystem — ~1,000 extensions — but compensates with extensive built-in features (collaboration, AI, terminal, Git, debugging). Extensions are sandboxed WASM modules: secure and cross-platform by nature, but the Rust/WASM barrier to entry slows ecosystem growth. **Update (March 2026):** The ACP Registry launch (January 2026) adds a second ecosystem dimension — installable external agents (Claude Code, Codex CLI, Gemini CLI, etc.) that integrate as first-class citizens, partially compensating for the smaller extension count.

**Cursor** inherits most of VS Code's ecosystem but lost access to Microsoft-proprietary extensions (C/C++, Pylance, C# Dev Kit, Remote Development) in April 2025 when Microsoft blocked non-VS Code editors from the official marketplace.

---

## 3. AI Ecosystem

This is the section that matters most for your decision. Here's every dimension of AI capability across all six editors.

### AI Feature Matrix

| Capability | Cursor | VS Code + Copilot | Zed | JetBrains | Emacs | Sublime |
|-----------|--------|-------------------|-----|-----------|-------|---------|
| Inline completions | ✅ Custom model (<200ms) | ✅ Copilot | ✅ Zeta (own model) + Copilot | ✅ AI Assistant + local | ✅ copilot.el | ⚠️ LSP-copilot (basic) |
| Next Edit Suggestions | ✅ | ✅ (Feb 2025) | ✅ | ✅ | ❌ | ❌ |
| Inline edit (select→describe→rewrite) | ✅ Cmd+K | ✅ Ctrl+I | ✅ Inline Assistant | ✅ AI Chat | ✅ gptel | ❌ |
| Chat with project context | ✅ Deep RAG indexing | ✅ @workspace | ✅ Agent Panel | ✅ Semantic indexes | ⚠️ Manual context (gptel) | ❌ |
| Chat with buffer/selection | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ OpenAI plugin |
| Agent mode (multi-file, autonomous) | ✅ Composer Agent | ✅ Agent Mode | ✅ Agent Panel | ✅ Junie (IDE + CLI) | ⚠️ gptel-agent, emigo | ❌ |
| Parallel agents | ✅ Up to 8 (worktrees) | ✅ Multi-agent orchestration | ❌ | ❌ (Air preview: yes) | ❌ | ❌ |
| Background/async agents | ✅ | ✅ Copilot Coding Agent (cloud) | ❌ | ❌ | ❌ | ❌ |
| Codebase semantic indexing | ✅ Custom embedding model | ⚠️ Improving | ❌ | ✅ Built-in indexes | ❌ | ❌ |
| MCP support | ✅ | ✅ | ✅ | ✅ | ✅ (mcp.el) | ❌ |
| Local/offline AI | ⚠️ Limited | ⚠️ Via extensions | ✅ Ollama | ✅ Local models free | ✅ Ollama via ellama/gptel | ❌ |
| External agent integration | ❌ | ✅ (any agent via MCP) | ✅ ACP protocol (Claude Code, Codex, etc.) | ✅ Air supports ACP | ✅ Claude Code (terminal) | ❌ |

### Context Awareness Depth

This is where the real differences emerge:

1. **Cursor** — deepest codebase context. Custom embedding model indexes your entire repository. Advertises 200K token context (usable: 70-120K). The AI understands project structure, not just open files.

2. **JetBrains** — unique advantage: AI leverages the same deep semantic indexes used for refactoring and navigation. This means the AI understands type hierarchies, method signatures, and call graphs at a level Cursor can't match.

3. **VS Code + Copilot** — @workspace provides repository-level context. 2x retrieval improvements since September 2025 (8x smaller index, 37.6% better retrieval). Gap with Cursor is narrowing.

4. **Zed** — context from open files and project structure, but no semantic indexing like Cursor or JetBrains. **Update:** Zeta2 (March 2026) added LSP-based context retrieval to edit predictions — the model now sees types and definitions of symbols around the cursor, a significant improvement though still not full codebase indexing.

5. **Emacs** — primarily manual context management. gptel lets you include specific files/regions. No automatic project-wide indexing. The Claude Code terminal integration is the strongest AI workflow in Emacs.

6. **Sublime Text** — minimal. Basic completion and chat only. No project context awareness.

### Model Availability

| Provider | Cursor | VS Code Copilot | Zed | JetBrains | Emacs (gptel) |
|----------|--------|----------------|-----|-----------|---------------|
| Claude (Anthropic) | ✅ | ✅ (Pro+) | ✅ | ✅ | ✅ |
| GPT (OpenAI) | ✅ | ✅ | ✅ | ✅ | ✅ |
| Gemini (Google) | ✅ | ✅ | ✅ | ✅ | ✅ |
| DeepSeek | ✅ | ⚠️ Via extensions | ✅ | ❌ | ✅ |
| Ollama (local) | ⚠️ Limited | ⚠️ Via extensions | ✅ | ✅ | ✅ |
| BYOK (any provider) | ❌ (credit system) | ❌ (Copilot subscription) | ✅ | ✅ (Junie CLI) | ✅ |

### The Claude Code Factor

An important consideration: **Claude Code is a terminal-based tool that works independently of your editor**. This means:

- In **Emacs**: First-class integration via claude-code.el, eat/vterm terminal backends. Multiple dedicated packages. Featured at EmacsConf 2025.
- In **Zed**: Native ACP protocol support (January 2026 ACP Registry launch). Claude Code, Codex CLI, Gemini CLI, and others run inside Zed as first-class agents via the open ACP standard, co-developed with JetBrains. This is Zed's strongest AI differentiator.
- In **VS Code**: Works in the integrated terminal. MCP integration available.
- In **JetBrains**: Works in the integrated terminal. Air IDE supports Claude Agent via ACP.
- In **Cursor**: Works in the integrated terminal, but competes with Cursor's own agent.
- In **Sublime Text**: Can run in a separate terminal alongside Sublime.

The implication: **if you rely heavily on Claude Code, your editor choice matters less for AI capability**. The editor becomes a "viewport" and Claude Code provides the agentic intelligence. This somewhat levels the playing field, especially for Emacs.

### AI Ecosystem Verdict

**Tier 1 — AI-first:**
- **Cursor**: Most mature AI-first editor. Best codebase indexing. Parallel agents. The reference standard.
- **VS Code + Copilot**: Closing fast. Multi-agent orchestration, cloud coding agent, open-sourcing Copilot. Microsoft's distribution advantage is massive.

**Tier 2 — Strong AI, not AI-first:**
- **Zed**: ACP protocol is forward-thinking and now live (ACP Registry launched January 2026). External agent support is the best of any editor. Zeta2 edit predictions are genuinely good. AI features are now prominent in the default UI — some users feel *too* prominent (see Trajectory section).
- **JetBrains**: Deep semantic context is unique. Junie + Air show serious AI commitment. Playing catch-up but leveraging unique IDE intelligence.

**Tier 3 — AI-capable with effort:**
- **Emacs**: 70-80% of Cursor's capability is achievable via gptel + copilot.el + Claude Code, but requires assembly and configuration. No codebase indexing. No inline diff preview. No parallel agents.

**Tier 4 — AI-limited:**
- **Sublime Text**: Basic completion and chat via third-party plugins. No agentic AI. The maintainer of the leading AI plugin has declared it in a "finite state." This is Sublime's biggest competitive weakness.

---

## 4. Costs

### Monthly Cost Comparison (Individual Developer)

| Editor | Editor Cost | AI (basic) | AI (full) | Effective Monthly |
|--------|------------|------------|-----------|-------------------|
| Sublime Text | ~$2.75/mo (amortized) | N/A | N/A | **$2.75** |
| VS Code | Free | Free (limited) | Copilot Pro $10/mo | **$0-$10** |
| Zed | Free | Free (BYOK/external only) | Pro $10/mo (token-based) | **$0-$10+** |
| Emacs | Free | Free (Ollama) | API costs vary | **$0-$20** |
| Cursor | Included | N/A | Pro $20/mo | **$20** |
| JetBrains | ~$15-24/mo (amortized) | AI Free tier | AI Ultimate $20-30/mo | **$15-$54** |

### Three-Year Total Cost of Ownership

| Setup | Year 1 | Year 2 | Year 3 | 3-Year Total |
|-------|--------|--------|--------|--------------|
| **Emacs + Ollama (free AI)** | $0 | $0 | $0 | **$0** |
| **VS Code + Copilot Free** | $0 | $0 | $0 | **$0** |
| **Zed + BYOK** | $0 + API | $0 + API | $0 + API | **$0 + API** |
| **Sublime Text (no AI)** | $99 | $0 | $0 | **$99** |
| **VS Code + Copilot Pro** | $120 | $120 | $120 | **$360** |
| **Zed Pro** | $120 + overages | $120 + overages | $120 + overages | **$360+** (token overages at API price +10%) |
| **Cursor Pro** | $240 | $240 | $240 | **$720** |
| **JetBrains All Products + AI Pro** | $289 | $231 | $173 | **$693** |
| **JetBrains All Products + AI Ultimate** | $529 | $471 | $413 | **$1,413** |
| **Cursor Ultra** | $2,400 | $2,400 | $2,400 | **$7,200** |

### Hidden Cost: Cursor's Credit System

Cursor introduced a credit-based system in June 2025. The $20/mo Pro plan includes $20 in credits. Different models drain credits at different rates (~225 Claude Sonnet requests, ~550 Gemini requests from $20). When credits run out, you fall back to unlimited Auto mode (Cursor's own model) or pay overage at API rates. The June 2025 rollout was rocky — Cursor issued a public apology in July 2025 for unexpected overage charges.

### Hidden Cost: Emacs Time Investment

Emacs is free in dollars but expensive in time:
- **Initial setup from scratch**: 20-80 hours (frameworks like Doom reduce this to 2-5 hours)
- **Ongoing maintenance**: 1-5 hours/month
- **Learning Elisp**: Weeks to months for fluency
- **For an experienced Emacs user adding AI**: 5-15 hours for gptel + copilot.el + Claude Code setup

### Hidden Cost: JetBrains Price Increases

JetBrains raised prices 11-30% in October 2025. The continuity discount (20% year 2, 40% year 3+) softens this, but the trend is upward. JetBrains is bootstrapped and profitable — prices reflect real costs, not VC subsidies.

### Costs Verdict

**Cheapest path to powerful AI editing**: VS Code + Copilot Pro ($10/mo) or Zed Pro ($10/mo). Both offer strong AI at half the base price of Cursor. **Caveat (updated):** Zed Pro's $10/mo includes only $5 of token credits — heavy users will pay overages at API price +10%. The free tier no longer includes any hosted AI prompts (it had 50/mo previously); free users must BYOK or use external agents.

**Best value if you want it all**: JetBrains All Products Pack ($289/yr first year, dropping to $173/yr by year 3). This includes every IDE + AI Pro. For a polyglot developer, this is hard to beat on capability-per-dollar.

**Most expensive**: Cursor at the higher tiers. The Ultra plan ($200/mo) is designed for developers who want maximum AI throughput and is 5-10x the cost of alternatives.

---

## 5. Competitive Trajectory

### The Money Map

| Editor | Funding Model | Valuation/Revenue | Growth Signal |
|--------|--------------|-------------------|---------------|
| **Cursor** | VC-backed ($2.5B+ raised) | $29.3B valuation, ~$1.2B ARR (2025) | Fastest-growing SaaS ever (1,100% YoY) |
| **VS Code** | Microsoft (subsidized) | Part of GitHub ($7.5B acquisition) | 15M+ Copilot users, 75.9% market share |
| **JetBrains** | Bootstrapped (profitable) | ~$252M revenue (2024), ~$593M ARR | Stable, responding to AI threat |
| **Zed** | VC-backed ($42M total) | Early stage, Sequoia-backed | 150K+ active devs, strong growth |
| **Emacs** | Volunteer (FSF) | N/A | Community-driven, no commercial backing |
| **Sublime Text** | Self-funded | N/A (small company) | Niche, sustainable, declining share |

### Where the Betting Money Is Going

**Big bets on Cursor**: $29.3B valuation with NVIDIA, Google, a16z, Accel as investors. Revenue doubling roughly every two months. 50,000+ teams, majority of Fortune 500. The institutional money says "AI-first editor is the future."

**Microsoft doubling down on VS Code + Copilot**: 15M+ Copilot users (4x YoY). Open-sourcing Copilot to counter Cursor's moat. Multi-agent orchestration. The strategy: VS Code as the universal agent platform, Copilot as the monetization layer.

**JetBrains hedging intelligently**: Junie CLI (LLM-agnostic, BYOK), Air IDE (agentic development), free tier expansion, GitHub Copilot compatibility. The message: "we won't lock you into our AI, but our IDE intelligence makes any AI better." Fleet's discontinuation shows they're willing to kill things that aren't working.

**Zed playing the long game — but identity under stress**: Sequoia backing ($42M+ total). ACP protocol co-developed with JetBrains launched January 2026 with a live registry. The bet: performance + open standards will win as AI agents become the primary "users" of editors. **However**, the aggressive AI pivot has triggered community backlash. Users on HN and GitHub report that core editor polish has stalled while AI features dominate development focus. A fork called **Gram** (March 2026) stripped out all AI, chat, collaboration, and telemetry from Zed, citing onerous ToS requirements and philosophical objections to AI integration. Zed overhauled its ToS the same day Gram went public. The tension: Zed's *technical* direction (ACP, Zeta2, open standards) is excellent, but the *product experience* has shifted from "speed-first editor with optional AI" to "AI-first editor that's also fast." Whether this is a feature or a bug depends on which Zed you fell in love with.

### Trajectory Assessment

**Rising**: Cursor (explosive), Zed (steady climb, but community trust fraying over AI focus), VS Code AI capabilities (Microsoft investing hard)

**Stable**: JetBrains (defending position well), VS Code (base editor)

**Declining**: Sublime Text (graceful fade), Emacs (percentage-wise, though ecosystem vitality is high)

### The Meta-Trend

The editor market is bifurcating:
1. **AI-first tools** (Cursor, Copilot in VS Code, Air) — the editor exists to orchestrate AI agents
2. **Traditional editors with AI bolted on** (JetBrains, Emacs, Sublime) — the editor exists for human editing, AI assists

The question isn't "which is better" — it's "where is the balance point?" Today, human developers still write and review most code. But if AI agents write 80%+ of code within 2-3 years (as Cursor's growth suggests developers believe), then AI-first editors have a structural advantage.

---

## 6. Cross-Platform UX

### Platform Support Matrix

| Editor | Linux | macOS | Windows | Config Sync |
|--------|-------|-------|---------|-------------|
| VS Code | ✅ Excellent | ✅ Excellent | ✅ Excellent | ✅ Built-in (MS/GitHub account) |
| JetBrains | ✅ Good (Wayland quirks) | ✅ Best | ✅ Good | ⚠️ Unreliable cross-platform |
| Emacs | ✅ Best | ✅ Good | ⚠️ Adequate (native-comp setup painful) | ✅ Portable config files |
| Cursor | ✅ Works (AppImage) | ✅ Good | ✅ Good | ⚠️ No robust sync |
| Zed | ⚠️ Good (GPU issues) | ✅ Best (original platform) | ⚠️ Newest (Oct 2025) | ⚠️ File-based only |
| Sublime Text | ✅ Good | ✅ Good | ✅ Good | ✅ Portable JSON configs |

### Cross-Platform Gotchas

**VS Code** is the gold standard for cross-platform. Settings Sync works (with a gnome-keyring caveat on Arch Linux). Remote Development (SSH, containers, WSL) is a genuine differentiator — no other lightweight editor matches it. The caveat: remote servers now require glibc 2.28+ (March 2025), which excludes older RHEL 7 / CentOS 7.

**JetBrains** is consistent across platforms thanks to the JVM, but Settings Sync is a known pain point — cross-platform sync can cause incorrect scaling, wrong keymaps, and broken layouts. The recommendation is to disable UI settings sync between different OSes. Wayland support has HiDPI scaling issues. Air IDE is **macOS-only** for now.

**Emacs** configuration is the most portable — your `init.el` works identically on Linux and macOS. Windows is the weak spot: native compilation requires MSYS2 toolchain setup, antivirus slows .eln file generation, and PATH issues are common. For a Linux/Mac developer, this matters less.

**Cursor** distributes as an AppImage on Linux — functional but clunky compared to native packages. No robust settings sync. If you work across machines, you'll need a manual dotfiles solution.

**Zed** shipped Windows support in October 2025 and has been improving it since (SSH remoting, WSL fixes). Linux GPU issues have improved with the migration to wgpu graphics backend, and Wayland screen-sharing now works for collaboration. macOS remains the most polished platform, but the Linux/Windows gap has narrowed significantly by March 2026.

**Sublime Text** is remarkably consistent across platforms — JSON configs are fully portable, and the custom rendering engine ensures identical behavior everywhere.

### For Your Multi-Platform Use Case

If you work across Linux, macOS, and Windows on the same project:

- **Best experience**: VS Code (built-in sync + remote development)
- **Good experience**: Sublime Text (portable configs), JetBrains (consistent but sync issues)
- **Acceptable**: Emacs (great Linux/Mac, rough Windows), Zed (great Mac, improving Linux/Windows)
- **Weakest**: Cursor (no robust sync, AppImage on Linux)

---

## 7. Extensibility

### Extensibility Depth Spectrum

From most to least extensible:

#### 1. Emacs — "The Lisp Machine"
Emacs is in a category of its own. The editor is a small C core with ~80% of functionality written in Emacs Lisp. Every function, every keybinding, every behavior can be inspected (`describe-function`), overridden (`define-advice`), or replaced at runtime without restarting.

**Unique capabilities:**
- Hot-reload any code change (`eval-defun`) — instant feedback loop
- Advise/wrap any function without touching its source code
- Hooks on nearly every action (file save, buffer switch, mode activation)
- No boundary between "core" and "extension" — all Elisp is equal
- EXWM: literally use Emacs as your X11 window manager

**The cost:** Emacs Lisp is niche. Learning it takes weeks/months. But once fluent, you can make Emacs do things no other editor allows.

#### 2. JetBrains — "Deep Native Access"
Plugins have full access to the PSI (Program Structure Interface) — the AST of your code. They can add inspections, intentions, refactorings, tool windows, and custom editors with native Swing components.

**Unique capabilities:**
- Direct AST manipulation (not just text — semantic understanding)
- Native UI components with code completion in text fields
- Deep integration with the debugger, VCS, and build systems

**The cost:** Kotlin/Java required. More complex than VS Code extension development. Smaller plugin author community.

#### 3. VS Code — "The Platform"
Comprehensive TypeScript/JS extension API covering LSP, DAP, webviews, custom editors, notebooks, testing, SCM, terminal, and now Copilot chat participants.

**Unique capabilities:**
- Webview API: render arbitrary HTML/CSS/JS in panels
- Largest extension author community by far
- LSP/DAP standards originated here
- Remote development extension host architecture

**Limitations:** No native code in extensions. Webview workers have URI restrictions. Some internal APIs not exposed.

#### 4. Zed — "Sandboxed by Design"
Extensions are Rust compiled to WASM, running in sandboxed environments.

**Unique capabilities:**
- Security: extensions can't access the filesystem arbitrarily
- Performance: WASM runs fast
- Cross-platform: WASM binaries work everywhere

**Limitations:** No VS Code extension compatibility. Can't modify core editor UI (planned). No arbitrary Node.js execution. The Rust/WASM barrier limits ecosystem growth.

#### 5. Cursor — "Inherited + AI"
Inherits VS Code's extension API plus adds AI-specific hooks (MCP support, one-click MCP server setup).

**Limitations:** Same as VS Code, minus Microsoft-proprietary extensions. AI features can conflict with other AI/autocomplete extensions.

#### 6. Sublime Text — "Capable but Constrained"
Python plugin API. Can manipulate buffers, create panels, add commands, handle events.

**Limitations:** No webview/HTML panel support. Smaller API surface than VS Code. Legacy Python 3.3.6 default (being modernized to 3.13). ~6,000 packages vs VS Code's 50,000+.

### "Make It Your Own" Rating

| Editor | How Easy to Start | How Deep Can You Go | Time to Proficiency |
|--------|------------------|--------------------|--------------------|
| VS Code | Very easy | Moderate (API boundaries) | Hours to days |
| Cursor | Very easy (same as VS Code) | Moderate | Hours to days |
| Sublime Text | Easy | Moderate | Hours to days |
| Zed | Moderate (Rust/WASM) | Growing | Days |
| JetBrains | Moderate | Very deep | Days to weeks |
| Emacs | Hard (initial learning curve) | Unlimited | Weeks to months (but you already know it) |

---

## 8. Debugger Support

### Debugger Feature Matrix

| Feature | JetBrains | VS Code | Cursor | Zed | Emacs (dap-mode/Dape) | Sublime (SublimeDebugger) |
|---------|-----------|---------|--------|-----|----------------------|--------------------------|
| DAP support | ✅ (native) | ✅ (created DAP) | ✅ (inherited) | ✅ (June 2025) | ✅ | ✅ |
| Zero-config for common languages | ✅ | ✅ (improving) | ✅ | ⚠️ (auto-config for some) | ❌ | ❌ |
| Conditional breakpoints | ✅ (with code completion!) | ✅ | ✅ | ✅ | ✅ | ✅ |
| Inline variable display | ✅ (best) | ✅ | ✅ | ⚠️ | ❌ | ❌ |
| Evaluate expression | ✅ (full IDE, object mutation) | ✅ (good) | ✅ | ⚠️ | ⚠️ (basic) | ⚠️ |
| Memory/heap views | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Smart step into | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Drop frame | ✅ | ⚠️ (limited) | ⚠️ | ❌ | ❌ | ❌ |
| Stream/pipeline debugging | ✅ (Java) | ❌ | ❌ | ❌ | ❌ | ❌ |
| Stored procedure debugging | ✅ (PostgreSQL, Oracle, MSSQL) | ❌ | ❌ | ❌ | ❌ | ❌ |
| Hot code reload | ✅ (Java, .NET) | ⚠️ (Node.js) | ⚠️ | ❌ | ❌ | ❌ |
| Thread management | ✅ (excellent) | ✅ (good) | ✅ | ⚠️ | ⚠️ | ⚠️ |
| Remote debugging | ✅ | ✅ (best via Remote Dev) | ✅ | ❌ | ⚠️ (TRAMP) | ❌ |

### Debugger Tier List

1. **JetBrains** — best-in-class, no contest. Conditional breakpoints with code completion, inline variable display, memory views, Smart Step Into, Drop Frame, Stream Debugger, stored procedure debugging. This is a genuine moat.

2. **VS Code / Cursor** — very good. Universal DAP support, zero-config for many languages, excellent remote debugging. Sufficient for most workflows. Cursor inherits this fully.

3. **Zed** — functional and improving. Shipped June 2025, DAP-based, covers major languages. Still maturing.

4. **Emacs** — functional but behind. dap-mode and Dape provide DAP support, but the text-based UI requires more cognitive overhead. No inline variable values. Workable for developers who prefer printf-debugging anyway.

5. **Sublime Text** — basic. SublimeDebugger (single maintainer, DAP-based) works for simple debugging but lacks polish. Most Sublime users debug elsewhere.

### For Your Go + SQL Workflow

Go debugging specifically:
- **JetBrains GoLand**: Delve integration is seamless. Best Go debugging experience.
- **VS Code / Cursor**: Delve via dlv-dap. Very good. Zero-config.
- **Zed**: Delve support via extension. Functional.
- **Emacs**: Delve via dap-mode/Dape. Works but requires more setup.

---

## 9. SQL & Database Workflow

This section is critical for your use case. You rely on Emacs's `sql-connect` for quick, keyboard-driven SQL interaction alongside code.

### Database Tooling Matrix

| Capability | JetBrains DataGrip | Emacs sql-mode | VS Code | Cursor | Zed | Sublime |
|------------|-------------------|----------------|---------|--------|-----|---------|
| Schema-aware SQL completion | ✅ Best-in-class | ❌ | ⚠️ (extension-dependent) | ⚠️ (extension-dependent) | ❌ | ❌ |
| Visual schema browser | ✅ | ❌ | ⚠️ (extensions) | ⚠️ (extensions) | ❌ | ❌ |
| ER diagrams | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Editable result grids | ✅ (including JOINs, new 2025) | ❌ (text output) | ⚠️ (basic) | ⚠️ (basic) | ❌ | ❌ |
| EXPLAIN visualization | ✅ (color-coded) | ❌ | ⚠️ (PostgreSQL ext) | ⚠️ | ❌ | ❌ |
| Stored procedure debugging | ✅ (PG, Oracle, MSSQL) | ❌ | ❌ | ❌ | ❌ | ❌ |
| AI-powered SQL | ✅ (schema context) | ⚠️ (gptel, manual) | ✅ (@pgsql) | ✅ (MCP) | ⚠️ (MCP prototype) | ❌ |
| Send region to REPL | ❌ | ✅ (C-c C-r, the best) | ❌ | ❌ | ❌ | ❌ |
| SQL buffer = full editor | ❌ (dedicated console) | ✅ (any buffer, full keybindings) | ❌ | ❌ | ❌ | ❌ |
| Multiple simultaneous connections | ✅ | ✅ | ⚠️ | ⚠️ | ❌ | ⚠️ |
| Org-mode SQL notebooks | ❌ | ✅ (unique capability) | ❌ | ❌ | ❌ | ❌ |
| TRAMP remote DB access | ❌ | ✅ | ✅ (Remote Dev SSH) | ❌ | ❌ | ❌ |

### Workflow Comparison: Code ↔ SQL Bouncing

**Emacs sql-connect (your current workflow):**
Split frame: code on left, SQL REPL on right. Write a query in any buffer. `C-c C-r` sends the selected region to the REPL. Results appear inline. Modify query, re-send. Full Emacs editing power on SQL (macros, search-replace, kill-ring). Zero context switch. This is genuinely the fastest keyboard-driven SQL workflow in any editor.

**JetBrains DataGrip / Database plugin:**
The most feature-rich SQL environment. Schema-aware completion knows your tables, columns, and functions. Results appear in interactive grids you can sort, filter, and edit. ER diagrams show relationships. EXPLAIN plans are color-coded. The trade-off: it's a GUI-heavy experience. You can't "send a region from your code buffer to a SQL console" as seamlessly as in Emacs. The SQL console is a separate tool window.

**VS Code:**
The PostgreSQL extension (Microsoft, 2025) provides IntelliSense, query execution, and Copilot integration (`@pgsql`). SQLTools covers other databases. The experience is adequate for development queries but not as fluid as Emacs's buffer-based approach. AI can help generate SQL from natural language, which is a different (and sometimes better) workflow.

**Cursor:**
Same as VS Code's extension story, plus MCP-based database access where the AI agent can query databases directly. The MCP approach is interesting — describe what data you want, and the agent writes and runs the SQL. But for iterative query refinement, it's slower than Emacs's direct REPL.

**Zed:**
No built-in database tooling. Community prototypes exist. You'd use the integrated terminal with `psql`/`mysql` CLI clients. Feature requests are open on GitHub.

**Sublime Text:**
SQLTools plugin provides basic query execution. Functional but behind VS Code's offerings, much less Emacs or DataGrip.

### SQL Workflow Verdict

Your Emacs `sql-connect` workflow is genuinely best-in-class for **iterative, keyboard-driven SQL development**. No other editor replicates the fluidity of "write SQL anywhere, send to REPL with a keystroke, see results, iterate."

However, **JetBrains DataGrip** is best-in-class for **rich database interaction** — schema browsing, visual query plans, result editing, stored procedure debugging.

The realistic options if you leave Emacs:
1. **JetBrains** — different workflow (GUI-oriented SQL console) but far more powerful database features. Closest to replacing Emacs + dedicated SQL tool.
2. **VS Code + PostgreSQL extension** — adequate for quick queries. Keep DBeaver/DataGrip open for heavy work.
3. **Any editor + Claude Code** — ask the AI to write and run SQL via MCP. A fundamentally different workflow that might be better for some tasks.
4. **Any editor + terminal SQL client** — `psql` in a split terminal. Functional but loses Emacs's buffer integration.

---

## 10. The External Agent Normalizer

This deserves its own section because it fundamentally changes the calculus.

Editor-native AI is no longer the whole story. If you expect to rely heavily on external agents (Claude Code, Codex CLI, Aider, Junie CLI), the gap between editors narrows dramatically. The editor becomes a viewport, terminal host, diff/review surface, and navigation tool — not the AI orchestrator.

**Who this helps most:** Emacs, Zed, JetBrains. All three have strong non-AI fundamentals (extensibility, speed, debugging/database depth respectively) and can host external agents effectively. Their relative AI weakness compared to Cursor matters less when the agent runs outside the editor.

**Who this helps least:** Cursor. Its biggest advantage *is* its native agent workflow. If you're running Claude Code in a terminal anyway, you're paying $20/mo for an editor whose differentiating feature you're bypassing.

**Practical implication:** If your likely future is "I drive a strong external agent from a terminal or integrated panel," then VS Code, Zed, JetBrains, and even Emacs all become more competitive. If your likely future is "I want the editor itself to be the primary AI orchestration environment," then Cursor and VS Code pull ahead.

---

## 11. The SQL Decision, Reframed

The SQL question isn't really about feature checklists. It's about interaction style.

**The real question:** Do you want to preserve your lightweight, keyboard-driven SQL loop (write query in buffer → send to REPL → see text results → iterate)? Or are you willing to switch to a GUI-oriented database workflow in exchange for richer tooling (schema completion, visual query plans, editable result grids, ER diagrams)?

- **Preserve the lightweight loop:** Emacs remains best. Nothing else replicates `C-c C-r` into a live REPL with full editor power on both sides. The closest modern approximation is running `psql` in a split terminal pane in any editor — functional but loses Emacs's buffer integration.

- **Switch to rich GUI database work:** JetBrains DataGrip is the gold standard. Schema-aware completion, editable JOIN results (new 2025), stored procedure debugging, cloud provider integration. It's a different *kind* of workflow, not a worse one.

- **The middle ground:** VS Code with the Microsoft PostgreSQL extension (2025) plus Copilot's `@pgsql` chat participant. Adequate for development queries with AI assistance. Note: Azure Data Studio was retired February 28, 2026, with Microsoft directing users toward VS Code — so this path is getting more investment.

- **The AI-native approach:** In Cursor or VS Code with MCP, describe what data you want in natural language and let the agent write and run the SQL. Fundamentally different from your current workflow but potentially faster for exploratory queries. Slower for iterative refinement.

---

## 12. Notable Omission: Windsurf

This analysis focused on the six editors you specified. One notable competitor not covered is **Windsurf** (formerly Codeium) — another VS Code fork with its own agentic engine ("Cascade") and proprietary inference model. It competes directly with Cursor at a lower price point ($15/mo vs $20/mo). If Cursor interests you, Windsurf is worth evaluating alongside it. However, note that Windsurf's claims about inference speed and context handling should be verified carefully — third-party benchmarks are scarce.

---

## 13. Final Comparative Matrix

### Summary Ratings

| Dimension | Cursor | VS Code | Zed | JetBrains | Emacs | Sublime |
|-----------|--------|---------|-----|-----------|-------|---------|
| **Performance** | C+ | B | A- (Linux caveats) | C (heavy but justified) | B+ | A+ |
| **General Ecosystem** | A- (VS Code minus MS) | A+ | C+ (growing) | A- | B+ | B- (declining) |
| **AI Ecosystem** | A+ | A | B+ | B+ | B- | D |
| **Cost (value)** | B- | A+ | A+ | B | A+ | A |
| **Trajectory** | A+ (explosive) | A (dominant) | B+ (ascending, identity tension) | B+ (defending) | C+ (niche stable) | C- (fading) |
| **Cross-Platform** | B- | A | B+ (Linux/Windows improving) | B+ | B+ (weak Windows) | A- |
| **Extensibility** | B+ (inherited) | A | B- (small ecosystem) | A- | A+ (unmatched) | B |
| **Debugger** | B+ (inherited) | B+ | B- (new) | A+ (best) | C+ | C- |
| **SQL/Database** | C+ | B | D | A+ (DataGrip) | A (REPL workflow) | C |

### Editor Archetypes

**Cursor** — *The AI Maximalist*
Best for: Developers who want AI-first coding with deep codebase understanding. Power users who leverage parallel agents and Composer for large-scale changes.
Weakest: Memory hungry, no robust config sync, credit system can surprise you, locked out of some MS extensions.

**VS Code** — *The Safe Default*
Best for: Polyglot developers, teams, remote development, anyone who values the massive ecosystem. Copilot is comprehensive and Microsoft is investing aggressively.
Weakest: Electron overhead, not best at anything except ecosystem breadth and remote dev.

**Zed** — *The Speed Demon (Now With AI Ambitions)*
Best for: Developers who prioritize responsiveness, want the best external agent integration (ACP Registry with Claude Code, Codex CLI, Gemini CLI), and value open standards over vendor lock-in. The `disable_ai: true` setting lets you use it as a pure speed editor.
Weakest: Default experience is now AI-forward (agent panel prominent, AI model selector in your face on first launch). Free tier lost hosted AI prompts. Smallest extension ecosystem. No database tooling. Community trust fraying — the Gram fork signals real dissatisfaction with the AI-first direction. Token-based billing can surprise heavy users.

**JetBrains** — *The Engineering Workstation*
Best for: Java/Kotlin, Python, .NET, Go developers who need powerful debugging and database work. Enterprise teams. Developers who value "IDE intelligence."
Weakest: Heavy resources, expensive, slower startup, settings sync unreliable, Air is macOS-only.

**Emacs** — *The Sovereign Hacker*
Best for: Developers who've already invested in Emacs. Org-mode users. People who value the SQL REPL workflow, Magit, TRAMP, and the ability to modify anything. Claude Code integration is legitimate.
Weakest: AI is 6-12 months behind leaders, debugging is basic, long-lines problem, community is small.

**Sublime Text** — *The Lightweight Veteran*
Best for: Quick editing, large file handling, developers who value startup speed and minimal resource usage above all else.
Weakest: AI ecosystem is practically nonexistent. Declining community. 2-3 person team. Not a serious contender if AI capability matters.

---

## Appendix: Recommendation Framework

Rather than prescribe a single answer, here are decision paths based on priorities:

### If AI capability is your #1 priority:
**Cursor** or **VS Code + Copilot Pro**. Cursor has deeper AI today; VS Code is closing the gap with Microsoft's backing. VS Code is half the price.

### If you want AI + speed + open standards:
**Zed**. Still the fastest modern GUI editor. ACP protocol gives the best external agent integration of any editor. BYOK and `disable_ai: true` let you opt out of the AI-first default. Accept the smaller ecosystem and the fact that the default experience is now AI-forward — you'll need to configure your way out of it rather than opting in.

### If debugging and database work are critical:
**JetBrains**. Nothing else comes close for debugging depth and DataGrip's SQL experience. Pair with Claude Code CLI for agent capability.

### If you want to stay in Emacs:
It's viable. gptel + copilot.el + Claude Code gets you 70-80% of Cursor's AI capability. Your SQL workflow is unmatched. Magit is unmatched. You lose: polished AI inline diffs, codebase indexing, parallel agents, and modern debugging UX. The gap is real but not insurmountable, especially with Claude Code leveling the AI playing field.

### If you want to minimize cost and maximize capability:
**VS Code + Copilot Free** (or Pro at $10/mo). Free editor, massive ecosystem, strong AI, best remote development. The "Honda Civic" of editors — reliable, practical, gets the job done.

### The "hedge" approach:
Use **VS Code** (or Zed) as your primary editor for AI-assisted work, keep **Emacs** open for SQL work and Org-mode, and use **Claude Code** as your primary AI agent (it works everywhere). This gives you the best of multiple worlds without fully committing to any single editor.

---

*Sources: All data points are sourced from official documentation, blog posts, GitHub repositories, Stack Overflow 2025 Developer Survey, JetBrains Developer Ecosystem Survey 2025, company press releases, and community forums. Full source URLs are available in the individual editor research documents.*
