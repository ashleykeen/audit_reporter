# Figma to Token Audit Instructions

This workspace produces developer-facing UI implementation audits. When the user asks for a Figma versus live-site comparison, use the `figma-token-build-audit` skill and the `figma-token-build-audit.prompt.md` workflow.

## Communication contract

- Be direct, calm, and evidence-led.
- Use impactable language: state what is wrong, why it matters, and what implementation decision fixes it.
- Put findings before background explanation.
- Use `PASS`, `FAIL`, and `OBSERVATION` consistently.
- Use `✅` for a measured pass, `❌` for a measured fail, and `⚠️` only when a comparison is genuinely blocked or state-dependent.
- Never call an item a pass because it looks similar. A pass needs a measured or explicitly verified equivalent.
- Never call a blocked item a fail. Explain the missing evidence and the cheapest next check.
- Do not soften a measurable failure with vague wording such as “slightly different” without the actual values.
- For every fail, include: token name, expected Figma value, observed live value, impact, and recommended fix.
- Keep summaries concise, but make the comparison tables complete.
- Do not use insulting, humiliating, or personal language about developers or teams. Critique the implementation and its consequences.

## Required output

Produce:

1. A short scope and evidence note.
2. A status summary with counts.
3. Modern comparison tables with these columns exactly:
   - Token name
   - Figma
   - Live
   - Result
   - Impact / action
4. Coverage for colour, typography, spacing, sizing, layout, borders, radii, shadows, icons/assets, responsive behaviour, and relevant states.
5. A recommended semantic token contract.
6. An overall verdict with impact-ranked bullets.
7. A practical implementation sequence.
8. A newly named, downloadable PDF report in the workspace. Treat the matching HTML as optional supporting source.

## Evidence rules

- Identify the exact Figma file key and node ID from the supplied URL.
- Use Figma design context first, then targeted node inspection for exact geometry and styles.
- Inspect the live route at a matching viewport where possible.
- Record viewport dimensions and environment (`production`, `staging`, or `dev`).
- Distinguish measured values from approximations.
- State when the live URL redirects, requires authentication, has empty data, or differs from the requested environment.
- Exclude unavailable states from pass/fail counts and list them under observations.
- Do not invent token names from hidden code. Use stable semantic names such as `layout/sidebar-width`, `color/brand-primary`, `font/family-body`, `component/input-height`, and `effect/shadow-xs`.

## File handling

- Preserve existing user files unless explicitly asked to replace them.
- Prefer the existing report style and content structure when regenerating a report.
- Never overwrite a prior audit. Derive a page-matching kebab-case slug and increment a zero-padded report suffix automatically.
- Keep the generated PDF and HTML names identical apart from the extension, for example `content-reports-token-audit-02.pdf` and `content-reports-token-audit-02.html`.
- Use ASCII in source files unless a status symbol or user-facing requirement needs Unicode.
- Validate the HTML and confirm the generated PDF exists, has a non-zero size, uses print-safe A4 margins, and is the first link reported to the user.
- Do not commit changes unless explicitly requested.
