# /immersive

A slash prompt that turns a vague "I wish Copilot would…" into a concrete,
scaffolded customization. It interviews you about a desired improvement,
picks the right Copilot primitive (instruction · prompt · skill · agent ·
hook · tool), justifies the choice, and writes the files under `.github/`.

## How it works

Type `/immersive <describe the improvement>` in Copilot Chat. With no
trailing text it asks you to describe the improvement and stops. Otherwise
it follows a fixed workflow:

1. Restate the improvement in one sentence.
2. Produce a **Diagnose** block — who triggers it, how often, knowledge vs.
   capability, persona needed, decision-tree walk, recommendation,
   trade-offs.
3. Confirm if the recommendation is non-obvious.
4. Scaffold the file(s) under `.github/`.
5. Run `get_errors` on the new files and summarize: workspace links, how to
   activate, one smoke test, one promotion follow-up.

The full prompt body lives in [immersive.prompt.md](immersive.prompt.md);
the decision rules live in the companion `immersive` chatmode.

## Examples 

### Example 1 — Gang of Four design patterns

> "Copilot keeps suggesting design patterns but never names them, and
> picks the wrong one for our codebase."

`/immersive` diagnosed this as **domain knowledge + always-on rules**:
- 23 GoF patterns + a decision tree → a **skill** that loads on demand:
  [.github/skills/gof-design-patterns/SKILL.md](../skills/gof-design-patterns/SKILL.md)
- Naming convention + trade-offs are mandatory → an **instructions** file
  that's always in context:
  [.github/instructions/design-patterns.instructions.md](../instructions/design-patterns.instructions.md)

Why two primitives: the catalog is too big to keep loaded all the time
(skill), but the *rules* about how to talk about patterns are short and
must apply everywhere (instructions).

### Example 2 — format-on-edit

> "Every time Copilot generates code, the formatter doesn't run and PRs
> end up full of whitespace diffs."

`/immersive` diagnosed this as a **post-event capability** — nothing the
model itself should decide, just something that must happen after every
edit. Recommendation: a **hook**.
- [.github/hooks/format-on-edit.json](../hooks/format-on-edit.json) wires
  the post-edit event to a polyglot dispatcher.
- [.github/hooks/scripts/format-on-edit.ps1](../hooks/scripts/format-on-edit.ps1)
  switches by file extension to `prettier` / `black` / `gofmt` / `rustfmt` /
  `dotnet format`.

Why a hook (not a tool): the agent shouldn't have to *decide* to format —
the runtime fires it. Non-blocking, output to log, no return value to the
model. That's the **Hook ≠ Tool** distinction `/immersive` enforces.

## When to reach for `/immersive`

- You catch yourself re-typing the same context every chat → probably a
  **prompt** or **instructions** file.
- A teammate asks "which pattern fits?" and you have a real answer → a
  **skill**.
- Something needs to happen after every Copilot edit → a **hook**.
- The model needs a deterministic capability it doesn't have → an **MCP
  tool**.
- A workflow has a fixed persona and tool allowlist → an **agent**.

`/immersive` walks you through picking among these instead of guessing.
