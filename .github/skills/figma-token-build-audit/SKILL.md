---
name: figma-token-build-audit
description: "Use when comparing a Figma screen to a live site, auditing design tokens, checking UI implementation fidelity, producing pass/fail token tables, or generating a developer-ready HTML/PDF audit report."
---

# Figma to Token to Build Audit

## Goal

Create a repeatable, evidence-based comparison:

`Figma screen -> extracted design tokens -> live rendered UI -> pass/fail findings -> developer report -> PDF`

The workflow is complete only when the report files are generated and validated.

## Inputs

Ask only for:

- A Figma screen URL with a specific `node-id`.
- A live site URL for the matching screen.

Reuse URLs already present in the conversation. If authentication blocks the live route, ask the user to authenticate in the shared browser. Do not request passwords, tokens, or secrets.

## Phase 1: Figma evidence

1. Parse the Figma file key and node ID. Convert a URL node ID such as `3393-10804` to `3393:10804`.
2. Load the design-to-code guidance before calling Figma design context.
3. Call Figma `get_design_context` on the supplied node. Treat returned code as reference only.
4. Inspect the target node and visible children for:
   - frame and region geometry
   - fills and opacity
   - strokes and border weights
   - corner radii
   - shadows and focus effects
   - auto-layout direction, padding, gaps, and alignment
   - typography family, size, weight, line height, letter spacing, and casing
   - icon and image assets, outer boxes, and leaf geometry
5. Record the Figma frame size and the visible state. If the node is a populated state, note that explicitly.

## Phase 2: Live evidence

1. Open the live URL in the shared browser.
2. Confirm the actual route after redirects and record the environment.
3. If authentication is required, instruct the user to complete normal browser login. Never request credentials in chat.
4. Inspect the rendered DOM and computed styles at the closest matching desktop viewport:
   - bounding rectangles
   - computed colour and background values
   - font family, size, weight, line height, and letter spacing
   - padding, margin, gap, width, height, border, radius, and shadow
   - visible icons and image dimensions
5. Check a mobile viewport when possible. Record responsive changes and overflow.
6. Exercise relevant states only when safe and available: active/inactive tabs, empty/populated data, focus, hover, disabled, validation, open/closed controls.
7. Separate unavailable states from failures. Empty data is not evidence that a populated row design fails.

## Phase 3: Token mapping

Map findings to stable semantic token names. Prefer these namespaces:

- `layout/*`: shell dimensions, content insets, regions, breakpoints
- `spacing/*`: padding, margin, gap, offsets
- `color/*`: page, surface, text, border, brand, status, focus
- `font/*`: family, size, weight, line height, letter spacing
- `component/*`: control dimensions, radii, badge and utility geometry
- `asset/*`: icon/image source, outer box, leaf geometry
- `effect/*`: shadow, focus ring, opacity, overlay

Do not create a new token for every occurrence. Reuse a semantic token when the role is the same. If the same role has different live values, record that as a token consistency failure.

## Phase 4: Verdict rules

- `✅ PASS`: values match exactly or are an explicitly verified equivalent within a documented tolerance.
- `❌ FAIL`: a measurable mismatch, missing shared contract, inconsistent semantic role, wrong asset, or unverified required behaviour.
- `⚠️ OBSERVATION`: comparison blocked by authentication, unavailable data/state, different environment, unsupported viewport, or insufficient Figma evidence.

Every finding must include:

`Token name | Figma | Live | Result | Impact / action`

Use impact language:

- High: changes shell geometry, brand identity, accessibility, or many downstream alignments.
- Medium: changes component consistency, scanning, or interaction polish.
- Low: isolated cosmetic or minor dimensional drift.

## Phase 5: Report structure

Generate a modern printable report with:

1. Title and source metadata.
2. Scope note and evidence limitations.
3. Summary cards for pass, fail, and observation counts.
4. Executive summary.
5. Separate native-looking tables for:
   - layout and spacing
   - header and breadcrumbs
   - typography
   - colour
   - tabs and controls
   - sidebar/navigation
   - assets and icons
   - responsive behaviour
   - relevant states
6. Recommended token contract.
7. Impact-ranked overall verdict.
8. Recommended implementation sequence.
9. Footer with environment, viewport, date, and remaining verification gaps.

Use four or five table columns depending on report density. The minimum comparison columns are always `Token name`, `Figma`, `Live`, and `Result`; add `Impact / action` for developer handoff reports.

## Phase 6: Artifact generation

- Derive a report slug from the audited page/screen name, normalized to lowercase kebab-case, and append `-token-audit`.
- Scan the workspace for existing files matching `<slug>-NN.pdf` and `<slug>-NN.html`. Choose the next available zero-padded number, starting at `01`.
- Create a new pair such as `<slug>-01.pdf` and `<slug>-01.html`; never overwrite an existing report.
- Use the generated report name consistently in the document title, PDF metadata where supported, HTML filename, PDF filename, and final links.
- Use the existing report style as a visual reference when present, but copy it into the newly numbered HTML output rather than overwriting the prior report.
- Use print-safe A4 margins and left/right gutters. Avoid clipped tables and orphaned headings.
- Export the newly numbered PDF with a local headless browser when available. The PDF is the primary user-facing deliverable.
- Validate that the HTML has the expected headings and tables, and that the PDF exists, is non-empty, printable on A4, and has a readable page count.
- Finish with the PDF link first, the HTML source link second, the generated report name, and a short list of the highest-impact fixes.

## Communication style

Lead with findings. Explain consequences, then fixes. Use direct, professional language. Never shame the team. A strong finding says:

`❌ FAIL — layout/sidebar-width: Figma 250px, live 296px. High impact: this changes the content origin and shifts every downstream alignment. Fix: bind both shell implementations to one layout/sidebar-width token.`

Avoid:

- “It feels a bit off.”
- “The developers failed.”
- “Probably wrong.”
- Pass/fail claims without values.
- Long caveats before the finding.
