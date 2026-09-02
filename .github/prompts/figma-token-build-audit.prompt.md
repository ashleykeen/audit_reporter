---
name: figma-token-build-audit
description: "Run a complete Figma to token to live build audit and produce a developer-ready HTML and PDF report. Ask only for the Figma screen URL and live site URL."
agent: agent
---

Run the complete `figma-token-build-audit` workflow for this workspace.

## Required first interaction

Ask for exactly these two inputs and no other setup questions:

1. Figma screen URL, including a specific `node-id`.
2. Live site URL for the matching screen.

If the user already supplied either URL in the conversation, reuse it and ask only for the missing URL. Do not ask for report format, viewport, token naming style, or output location; use the defaults in the skill.

## Defaults

- Compare the supplied Figma node with the supplied live route.
- Use the closest available desktop viewport and record its exact dimensions.
- Also check a mobile viewport if the live page supports responsive rendering.
- Use semantic token names in kebab-case, grouped as `layout/`, `spacing/`, `color/`, `font/`, `component/`, `asset/`, and `effect/`.
- Use `✅ PASS`, `❌ FAIL`, and `⚠️ OBSERVATION`.
- Generate a new uniquely named PDF as the primary deliverable and an optional matching HTML source. Derive the filename from the audited page name, for example `content-reports-token-audit-01.pdf` and `content-reports-token-audit-01.html`.
- Never overwrite an existing report. Scan the workspace for matching report names and increment the number automatically, using the next available zero-padded suffix (`-01`, `-02`, `-03`, ...).
- Keep the report readable when printed on A4 paper.

## Completion standard

Do not finish with a proposal. Execute the audit, generate and validate the newly numbered PDF, and report the PDF download link first. Include the matching HTML source link second when it is generated. State the generated report name. If evidence is blocked, finish the available audit and clearly list the blocked checks plus the exact next evidence needed.
