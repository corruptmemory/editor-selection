# Antigravity Comparative Research (March 28, 2026)

This memo compares **Google Antigravity IDE** against the editors already in this bake-off:
`Cursor`, `VS Code`, `JetBrains`, `Zed`, `Emacs`, and `Sublime Text`.

## Scope and Method

- Focus: agent capabilities, ecosystem fit, pricing posture, risk profile, and SQL/debug workflow fit.
- Evidence standard: official product pages/docs first; external reporting only when official docs are thin.
- Date context: all facts verified against sources accessible on **March 28, 2026**.

## What Is Solidly Confirmed About Antigravity

From Google sources, Antigravity is an **agent-first** development environment where agents can operate across **editor + terminal + browser**.

Confirmed elements:
- It launched alongside Gemini 3 announcements on **November 18, 2025**.
- It is positioned as an agentic platform, not just a chat/completion add-on.
- AI Pro subscribers get higher Antigravity quotas with refresh windows (5-hour cadence is documented in Google support text).
- Antigravity availability is described for desktop OSes (Windows/macOS/Linux) in Google support docs.

## Comparative Matrix (Research-Backed)

| Dimension | Antigravity | Cursor | VS Code + Copilot | JetBrains + AI/Junie | Zed | Emacs | Sublime Text |
|---|---|---|---|---|---|---|---|
| Product posture | Agent-first IDE | AI-maximalist VS Code fork | General editor now with multi-agent stack | Engineering IDE + coding agent | Native performance editor with open AI stack | Extensible editor platform | Lightweight editor |
| Agent autonomy | Strong; centered on autonomous task execution | Strong; local + cloud/background agents | Strong; local agents + CLI/background + coding agent | Strong; Junie can execute tasks with approvals/allowlist | Strong in-editor tools + subagents | Via external tools/packages | Limited by default |
| Browser-control agent surface | **Yes (core differentiator)** | No native browser surface in core docs | No native browser agent surface in core docs | No native browser agent surface | No native browser agent surface | Possible only via custom wiring | No |
| Model/provider openness | Google-first, with third-party models referenced in support docs | Multi-model on paid plans | Multi-model and MCP-centric | JetBrains-managed AI + Junie stack | Hosted or BYOK across many providers; local models supported | Fully user-defined | User-defined via plugins/integrations |
| Extensibility pattern | Emerging; Google ecosystem gravity | VS Code-compatible + Cursor features | Largest extension ecosystem + MCP + custom agents | Mature IDE plugin model | Open source editor + MCP + external agents | Maximal (Lisp) | Mature package model, smaller AI-native ecosystem |
| Remote/background execution | Agent-manager async orientation | Yes (Background/Cloud Agents) | Yes (Copilot coding agent + CLI sessions) | Yes (agent can run commands/tests; GitHub app in EAP) | In-editor agents/subagents; external agents available | External tool dependent | External tool dependent |
| Pricing posture (individual) | Coupled to Google AI plans; quota/priority model | $20 Pro, $60 Pro+, $200 Ultra | Copilot Pro starts at $10 | Usually highest all-in stack cost for full IDE+AI setup | Pro $10 with usage model; BYOK path | Free software; integration effort cost | Paid license model |
| SQL workflow fit vs Emacs `sql-connect` style | Better for autonomous full-loop tasks than human REPL bounce | Good for app coding; SQL loop via extensions | Good via extensions; not Emacs-style by default | Strongest integrated DB tooling in this group | Improving but DB tooling ecosystem smaller | Best direct REPL bounce customization | Basic unless heavily customized |
| Debugger depth | Inherited VS Code-class workflows, agent-heavy focus | VS Code-class | Strong general debugger ecosystem | Best-in-class integrated debugger suite | Improving DAP path | Functional but assembled | Basic |
| Strategic risk | New product + Google discontinuation risk | Fast-moving startup pricing/compute risk | Lowest platform risk | Vendor cost/weight tradeoff | Ecosystem maturity risk | DIY maintenance burden | AI strategic gap risk |

## Where Antigravity Is Actually Better

- **End-to-end autonomous web-app verification loops**: Antigravity’s browser-enabled agent surface is the clearest product-level differentiator vs the rest of this list.
- **Task orchestration mindset**: It is designed around delegating chunks of work to agents first, then supervising artifacts/results.
- **Google ecosystem leverage**: If your stack already lives in Google tooling, the integration path is likely smoother.

## Where It Is Not Better (Yet)

- **Trust and continuity risk**: Compared with VS Code/JetBrains/Emacs, long-horizon workflow confidence is weaker because Antigravity is newer and Google has a known product-churn reputation.
- **Pricing/quota clarity**: The Antigravity value proposition appears quota-governed inside Google AI memberships; this is less straightforward than VS Code Copilot’s explicit request-based pricing page.
- **Human-first SQL ergonomics**: For developers who care about a tight human REPL loop (like Emacs `sql-connect`) or rich DB IDE tooling (JetBrains), Antigravity’s advantage is less clear.

## Practical Ranking for Different Priorities

If your top priority is:

1. **Agentic autonomy including browser actions** -> Antigravity first, Cursor second.
2. **Lowest-risk mainstream platform** -> VS Code first.
3. **Debugger + SQL workstation depth** -> JetBrains first.
4. **Fast local UX + open AI plumbing** -> Zed first.
5. **Maximum control/customization sovereignty** -> Emacs first.

## Decision Guidance for This Repository’s Original Question

For an experienced Emacs developer evaluating migration risk vs AI upside:

1. Use **VS Code** as control baseline.
2. Evaluate **Antigravity** only if browser-enabled autonomous agents are a core requirement (not a novelty).
3. Keep **JetBrains** in the loop if SQL/database and debugger depth are central.
4. Keep **Emacs** available for the workflows where it still wins (tight SQL iteration and deep personal customization).

## Sources

### Antigravity / Google
- Google Gemini 3 launch (includes Antigravity intro): https://blog.google/products/gemini/gemini-3/
- Google One Help (AI Pro benefits with Antigravity details, quotas, platform notes): https://support.google.com/googleone/answer/14534406?hl=en
- Google AI Ultra pricing announcement ($249.99/month in US at launch): https://blog.google/products/google-one/google-ai-ultra/

### Cursor
- Cursor pricing: https://cursor.com/pricing
- Cursor Background Agents docs: https://docs.cursor.com/en/background-agents
- Cursor Cloud Agents announcement: https://cursor.com/blog/cloud-agents

### VS Code / Copilot
- VS Code agents overview: https://code.visualstudio.com/docs/copilot/agents/overview
- Copilot coding agent docs: https://code.visualstudio.com/docs/copilot/copilot-coding-agent
- GitHub Copilot plans: https://github.com/features/copilot/plans

### JetBrains
- Junie in IDE docs: https://www.jetbrains.com/help/go/junie.html
- Junie action allowlist / approvals: https://www.jetbrains.com/help/junie/user-approval.html
- AI Pro inclusion in All Products Pack FAQ: https://sales.jetbrains.com/hc/en-gb/articles/16544922728466-Is-JetBrains-AI-subscription-included-in-All-Products-Pack-or-dotUltimate

### Zed
- Zed AI overview: https://zed.dev/docs/ai/overview
- Zed AI tools: https://zed.dev/docs/ai/tools
- Zed pricing: https://zed.dev/pricing/

### Emacs / Sublime
- GNU Emacs releases overview (30.2 listed): https://www.gnu.org/software/emacs/?lang=en
- Sublime Text sales FAQ (license model): https://www.sublimetext.com/sales_faq
