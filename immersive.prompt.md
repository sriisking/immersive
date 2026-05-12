---
description: >
  Analyse a desired Copilot improvement: pick the right primitive
  (instruction / prompt / skill / agent / hook / tool), justify the choice,
  and scaffold the files.
---

The user has invoked `/immersive` and the text of this chat turn describes
an improvement they want to make to GitHub Copilot's behavior. Treat that
text as **the improvement to analyse**.

If the user invoked `/immersive` with no additional text, ask them to
describe the improvement and stop.

Additional editor context (may be empty):

- Active file: ${file}
- Selection: ${selection}

Follow the **required workflow** defined in the `immersive` chatmode:

1. Restate the improvement in one sentence. If any of the four diagnostic
   questions can't be answered, ask exactly one clarifying question and stop.
2. Produce the full **Diagnose** block (improvement, who triggers, how often,
   knowledge vs. capability, persona needed, decision-tree walk,
   recommendation, trade-offs).
3. If the recommendation is non-obvious, confirm with the user before
   scaffolding. Otherwise proceed.
4. Scaffold the file(s) under `.github/` using the templates in the
   chatmode's appendix. Use the user's domain vocabulary, not placeholders.
5. Run `get_errors` on the new files, then summarize: files created (as
   workspace links), how to activate, one smoke test, one promotion
   follow-up.

Do **not** write application code in this turn. If the request is actually
about app code rather than Copilot customization, say so and ask the user
to leave `immersive` mode.
