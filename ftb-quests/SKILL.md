---
name: ftb-quests
description: Use when creating, editing, reviewing, or arranging FTB Quests questbooks, chapters, tasks, rewards, dependencies, or SNBT/JSON5 quest files, especially when progression is confusing, nodes overlap, dependency lines cross, or chapter layouts look cluttered.
---

# FTB Quests

Build a readable progression map, not a decorative web. Players should recognize the starting point, next useful action, and destination without tracing unrelated lines.

## Required references

- For every layout task, read [layout.md](references/layout.md): topology, spacing, coordinate recipe, worked example, and visual acceptance checks.
- Before editing quest data, read [authoring.md](references/authoring.md): compatibility, dependencies, safe serialization, tasks, rewards, and runtime checks.
- For provenance or uncertain features, read [sources.md](references/sources.md). Local exports and matching-version source outrank current documentation and screenshots.

## Workflow

1. **Inspect the target.** Find Minecraft, loader, FTB Quests version, installed addons, quest files, and representative screenshots. Record the existing schema, coordinate scale, node sizes, progression settings, and localization convention. Obtain missing facts from project files first.
2. **Set the boundary.** With no target version/export, deliver a complete version-neutral layout manifest and list missing compatibility evidence. Do not present speculative SNBT/JSON5 as import-ready. SNBT is not universal: the 26.1.2.1 line switched to JSON5; curves and visual presets arrived in 26.1.2.3. Verify backports separately.
3. **Map gameplay before geometry.** Inventory stable IDs, tasks, rewards, prerequisite semantics, optional/repeatable flags, visibility, and cross-chapter references. For layout-only work preserve all of them. Detect cycles and missing references before arranging nodes; report ambiguous progression instead of silently repairing it.
4. **Compose.** Default to a left-to-right main spine with semantic side lanes. Preserve an established top-down convention. Use aligned grids for independent collections, not artificial prerequisite chains. Produce coordinates using the reference recipe; move nodes to resolve crossings before adding decoration.
5. **Apply safely.** Back up data, reuse the installed editor's serialization, and preserve existing object IDs. Add only supported presentation fields. Changing an optional flag, hiding quests, deleting dependencies, or adding fake routing quests is not a cosmetic edit.
6. **Verify.** Check graph invariants and geometric clearance; then inspect the actual questbook with editing mode off, at normal zoom and a smaller viewport. Test locked, available, completed, and reward states. Report static, visual, and gameplay verification separately; no runtime means in-game behavior remains unverified.

## Required output

Return these sections, scaled to the task:

1. **Compatibility:** observed version/schema and unresolved facts.
2. **Layout:** chapter purpose, reading direction, entry, destination, lanes, spacing assumptions, and one row per quest: `existing ID | title | role | prerequisites and gate | x | y | size`.
3. **Changes:** actual files/edits, or a clearly labeled design-only manifest when target data is absent; distinguish cosmetic changes from proposed gameplay changes.
4. **Verification:** checks performed, observed results, and remaining runtime checks.

## Red flags

An attractive screenshot is not proof of compatible data. Hidden lines do not remove dependencies. Matching positions do not prove node clearance. A side branch is not necessarily marked optional in FTB Quests. A new UI addon is not required to make the base questbook readable.
