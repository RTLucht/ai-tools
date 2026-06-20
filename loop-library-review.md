# Loop Library Review
> Source: https://signals.forwardfuture.ai/loop-library/
> Last checked: 2026-06-20
> Install: `npx skills add Forward-Future/loop-library --skill loop-library -g`

---

## What Is This

Forward Future's Loop Library is a community-shared collection of bounded AI agent workflows. Each loop has:
- A defined goal
- Explicit stopping conditions
- Success criteria / verification steps

The key value: no runaway agents. Every loop terminates when a measurable condition is met.

---

## Engineering (18 loops)

| Loop | What It Does | My Take |
|---|---|---|
| **The docs sweep** | Keeps docs aligned with codebase, opens PR | High value. Run this on fort-monitor and switchmatic — both have doc drift. |
| **The architecture satisfaction loop** | Refactors in small, tested, independently reviewed checkpoints | Pairs well with `/code-review`. Good for incremental refactors without big-bang risk. |
| **The sub-50ms page-load loop** | Optimizes every page to consistently load under 50ms | Narrow scope but useful for front-end perf sprints. Concrete metric = easy stop condition. |
| **The production error sweep** | Finds, fixes, verifies actionable production errors | Strong ops loop. Would work well with fort-monitor's error log pipeline. |
| **The 100% test coverage loop** | Adds meaningful tests until full suite hits 100% | Useful but "meaningful" is doing heavy lifting — watch for coverage gaming. |
| **The logging coverage loop** | Adds tested logs to every important system path | Good for observability gaps. Pairs with fort-monitor alerting work. |
| **The test-suite speed loop** | Speeds up test suite without weakening coverage or isolation | Underrated. Slow tests = tests that don't get run. |
| **The repository cleanup loop** | Recovers valuable work, safely removes stale state | Run before major version bumps. Good pre-release hygiene. |
| **The ticket-to-PR-ready loop** | Turns a ticket/complaint into a reviewer-ready PR | Practical for solo dev workflow. Bridges issue triage to implementation. |
| **The Clodex adversarial-review loop** | Uses Codex to review Claude's PR until blocking findings clear | Adversarial multi-agent review. Matches your Claude+Codex AI team setup exactly. |
| **The fresh-clone loop** | Repeats clean onboarding from README until no hidden setup assumptions remain | Critical for any project you want others to use. Run on reddit-finder and switchmatic. |
| **The autonomy-loop builder-reviewer loop** | Passes code between builder and reviewer until tests prove each fix | Structured version of your existing Claude/Codex delegation pattern. |
| **The five-minute repository maintainer loop** | Keeps repo work moving through dedicated threads without interrupting active agents | Useful for long parallel agent sessions. Complements `claude agents` dashboard. |
| **The recent-feedback sweep** | Turns user corrections into project-wide audit and verified fixes | High signal. Turn accumulated `/rewind` corrections into systemic fixes. |
| **The propagation compliance loop** | After one value changes, finds every place still showing the old value | Excellent for config/constant renames across large codebases. |
| **The Goal Forge loop** | Turns rough coding idea into measurable planning files before Codex starts a long run | Good pre-flight for ultracode sessions — forces goal clarity before token burn. |
| **The cold-load trimmer loop** | Reduces data downloaded before first screen without changing behavior/appearance | Front-end focused. Good for Jarvis dashboard and fort-monitor UI. |
| **The housekeeper loop** | Cleans code one proven, low-risk change at a time | Conservative cleanup loop. Good for legacy code where big refactors are risky. |

---

## Operations (5 loops)

| Loop | What It Does | My Take |
|---|---|---|
| **The stale-safe batch release loop** | Batches valid changes, releases complete artifacts from latest integrated main | Safer than ad-hoc deploys. Relevant for fort-monitor rptsvr releases. |
| **The production data cleanup loop** | Removes disallowed production data, prevents same classification errors returning | Data governance loop. Relevant if Omni-Clerk or Mneme accumulates bad embeddings. |
| **The post-release baseline loop** | Benchmarks each completed release and records a reproducible baseline | Good discipline. Fort-monitor lacks release baselines right now. |
| **The customer AI deployment loop** | Moves one customer AI priority through validation, controlled rollout, monitoring | Enterprise-flavored. Overkill for home lab but good pattern for Fort Brands work. |
| **The Loop Harness verification loop** | Ships scheduled agent work only after independent verification pass | Meta-loop for your scheduled agents — adds a verification gate before cron jobs fire. |

---

## Evaluation (10 loops)

| Loop | What It Does | My Take |
|---|---|---|
| **The quality streak loop** | Fixes product failures until a defined streak of realistic tests passes | Strong. "Streak" requirement prevents flaky-pass acceptance. |
| **The full product evaluation loop** | Tests every major product capability, fixes outcomes below quality bar | Comprehensive. Use before major releases on any user-facing app. |
| **The self-improving champion loop** | Promotes prompt/policy changes only when they win on fresh holdout cases | ML-style champion/challenger. Essential for Mneme's nightly revision daemon. |
| **The devil's-advocate loop** | Challenges a design until every high-impact objection is resolved or accepted | Pre-architecture design review. Run before starting major new projects. |
| **The Revolve versioned-experiment loop** | Improves prompts/code/config through comparable, checkpointed experiments | Systematic prompt iteration. Valuable for refining Omni-Clerk query patterns. |
| **The promise-to-proof loop** | Checks every customer-facing claim is true, fixes riskiest mismatch first | Trust/accuracy loop. Good for LinkedIn post system — don't publish claims you can't back. |
| **The multi-LLM convergence loop** | Two different AI systems review same work until both approve one unchanged version | High-confidence review. Expensive but worth it for security-sensitive changes. |
| **The easy onboarding loop** | Acts as first-time user, fixes one obstacle, retries from clean session | UX loop. Would catch the hidden setup assumptions the fresh-clone loop misses. |
| **The accessibility repair loop** | Finds keyboard/screen-reader/low-vision barriers, fixes most harmful first | A11y compliance. Relevant for any public-facing UI work. |
| **The Axelrod subagent arena loop** | Tests whether AI agents learn to cooperate, retaliate, or forgive in repeated game | Research/experimental. Interesting for Mneme's multi-agent belief reconciliation work. |

---

## Content (3 loops)

| Loop | What It Does | My Take |
|---|---|---|
| **The SEO/GEO visibility loop** | Fixes highest-impact gaps in search and AI answer visibility | Useful for LinkedIn Post System and any public-facing project pages. |
| **The nightly changelog loop** | Keeps changelog current with meaningful changes from previous day | Low-effort, high-value. Wire to your existing GitHub Actions CI on fort-monitor/switchmatic. |
| **The product update podcast loop** | Turns meaningful product updates into a short, source-grounded podcast episode | Interesting for the YouTube horror channel pipeline — adapt for AI-narrated update episodes. |

---

## Design (5 loops)

| Loop | What It Does | My Take |
|---|---|---|
| **The Boeing 747 benchmark** | Builds and improves a Three.js Boeing 747 across nine repeatable views | Creative benchmark loop. More demo/showcase than practical utility. |
| **War Loops: frontend reconstruction** | Reconstructs a real interface, repairs weakest visual and motion mismatches | UI fidelity loop. Useful for pixel-accurate redesign work. |
| **The Infinite Clickbait thumbnail loop** | Iterates thumbnail concepts until one clears quality bar without misleading | High relevance for YouTube horror channel thumbnails. |
| **The UI/UX Score Loop** | Walks through real user task, scores each screen, improves weak spots, retests | Structured UX audit. Good for Jarvis dashboard and fort-monitor web UI. |
| **The pixel-safe CSS trim loop** | Shrinks styling code without changing any tested screen's appearance | CSS optimization. Run after design is locked in. |

---

## Priority Picks for Your Stack

| Loop | Why |
|---|---|
| Clodex adversarial-review | Matches Claude+Codex AI team exactly |
| Goal Forge | Pre-flight for ultracode sessions |
| Loop Harness verification | Safety gate for your cron/scheduled agents |
| Self-improving champion | Mneme nightly revision daemon |
| Nightly changelog | Wire to fort-monitor/switchmatic CI |
| Fresh-clone | reddit-finder, switchmatic onboarding |
| Recent-feedback sweep | Turn accumulated corrections into systemic fixes |

---

## Change Log

| Date | Change |
|---|---|
| 2026-06-20 | Initial doc created. 41 loops catalogued. |
