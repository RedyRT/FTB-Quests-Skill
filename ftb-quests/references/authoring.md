# Version-Safe FTB Quests Authoring

## Compatibility profile

Before producing loadable data, establish:

- Minecraft version, loader, exact FTB Quests version, and installed quest-related addons.
- Actual quest root, chapter files, reward tables, localization files, and generated-data pipeline.
- One working export from the same installation, including a task, reward, dependency, and chapter.
- Whether this is a new questbook, gameplay revision, or presentation-only change.

Inspect manifests, installed mod metadata, and existing exports before asking the author. If unavailable, finish a design-only manifest; state that serialization is blocked by the missing target profile, not that layout design is blocked.

### Known boundaries, not universal assumptions

| Evidence | Consequence |
|---|---|
| Upstream 26.1.2.1 changelog: quest and progress data switched from SNBT to JSON5 | Do not assume every version uses `.snbt` or feed SNBT numeric suffixes to JSON5 |
| Same release: translations under `config/ftbquests/quests/lang` split by object type and chapter | Preserve the target's localization layout rather than embedding every title into chapter objects |
| Upstream 26.1.2.3: Bezier connections and visual presets introduced | Gate these features by the installed release; check backports explicitly |
| A nonempty visual preset overrides quest size and shape | Resolve quest/chapter/book preset inheritance before diagnosing ineffective resizing |

Earlier releases commonly use SNBT. Determine the exact schema from the installed version, not a guessed version-number comparison across release branches. Never convert formats by renaming extensions.

## Data editing contract

1. Back up the authoritative quest configuration and, where relevant, world/team progress. Work on a disposable test instance rather than live player data.
2. Find the real source of truth. A common modern root is `config/ftbquests/quests/`; verify it. A server, pack script, or generated-data step may overwrite local edits.
3. For layout-only work, patch coordinates and explicitly intended presentation fields on existing objects. Preserve quest, task, reward, chapter, and link IDs plus references to them. Do not regenerate IDs or rewrite unrelated files.
4. For new content, obtain unique IDs from the installed editor or its confirmed format. Check uniqueness across all quest objects, not merely the new chapter. Do not use diagram labels or titles as IDs.
5. Use a parser/serializer that understands the target format, or the target editor itself. JSON is not a general SNBT or JSON5 parser. Preserve supported value types, unknown fields, and localization conventions.
6. Save through the appropriate editor workflow. For external file edits, close the editor or stop the instance so an in-memory save cannot overwrite them. Use only reload/save commands verified by that installation's help or matching-version docs.
7. Reopen and confirm the change persists. Check logs for parse errors, unresolved objects, and missing item IDs. Do not reset progress to make a failed reload appear successful.

### Selected current-source fields

This table describes the inspected upstream revision linked in [sources.md](sources.md), not a cross-version import schema. Inspect a target export before writing any field.

| Field | Meaning / risk |
|---|---|
| `x`, `y` | Quest center coordinates |
| `size`, `shape` | Presentation; inherited defaults and presets may override them |
| `icon_scale` | Icon scaling independent of button size |
| `dependencies` | Actual referenced prerequisite objects |
| `dependency_requirement` | All/one, started/completed semantics |
| `min_required_dependencies` | Positive values override ordinary dependency-requirement behavior; preserve N-of-M gates |
| `hide_dependency_lines` | Hide incoming dependency lines, not the dependencies themselves |
| `hide_dependent_lines` | Hide outgoing lines to dependents |
| `optional`, `can_repeat` | Gameplay/completion behavior, not decorative labels |
| `hide_until_deps_visible`, `hide_until_deps_complete` | Visibility behavior; not a substitute for an organized layout |
| `progression_mode` | Linear versus flexible task progression; preserve inheritance |
| `preset`, `dep_control_pts` | Version-gated presentation fields; export real editor-generated values rather than guessing their encoding |

ALL versus ANY versus N-of-M is part of the graph contract. A pretty fork does not mean both branches are mandatory. Started is not completed. In flexible mode, tasks can progress before prerequisites finish even though quest completion remains gated. Preserve these distinctions.

Do not transitive-reduce gameplay edges merely because they look redundant: dependency modes, visibility, exclusions, and repeatable quests can make them meaningful. Selective line hiding requires a navigation check. A quest link can provide a reference to an existing quest where supported; it is not a duplicate task/reward quest or an automatic replacement for a prerequisite.

## Quest content

For a new quest, specify purpose, an actionable title, why/what/how in the description, measurable tasks, prerequisites with explicit gate semantics, rewards, and a visual role. Use existing pack terminology and recipe/progression changes.

- Verify item/entity/tag identifiers and task types against the installed registry and editor. Avoid invented mod items, recipes, or completion methods.
- Distinguish having an item, consuming/submitting it, crafting it, visiting a location, and manually acknowledging text. Use supported task behavior that actually measures the goal. Do not substitute a checkbox for an unimplemented objective.
- Use multiple tasks in one quest when they describe one coherent goal; separate milestones when they teach different decisions. Do not require filler crafts solely to fill the layout.
- Keep titles short; put explanation in the quest panel. Respect the user's requested quest language; this skill's English documentation does not force English player-facing quests.
- Rewards should reinforce the next step without bypassing the progression the questbook teaches. Review team versus individual rewards, repeatability, consumption, and any command rewards. Do not introduce executable rewards without a real need and an explicit review.

## Verification matrix

| Layer | Check | Evidence to report |
|---|---|---|
| Static data | Correct parser; unique IDs; references resolve; prerequisites have no unintended cycles; registry IDs valid where available | Actual checks run and unresolved references |
| Layout-only invariants | Same IDs, tasks, rewards, dependencies, gates, flags, localization, and chapter membership | Semantic comparison before/after; presentation changes listed separately |
| Geometry | Footprint overlaps, edge/node collisions, crossings, spacing, bounds | Counts under stated size/shape assumptions; not a runtime claim |
| Player view | Editing off; normal zoom; smaller viewport; longest title; locked/available/completed states | Screenshots or direct observations with GUI scale |
| Gameplay | First quest; fork; ALL/ANY/N-of-M join; optional branch; cross-chapter reference; rewards | Observed unlock and completion outcomes |
| Persistence/team | Save/reload/reopen; a fresh test player/team; shared progress/rewards when relevant | Observed behavior, without modifying production progress |

For an ALL join, one completed prerequisite must not satisfy the join; for ANY, one qualifying prerequisite must suffice; for N-of-M, check just below and at N. Account for started/completed and flexible-mode settings instead of assuming every lock prevents task progress.

If Minecraft is unavailable, check data and geometry where possible and explicitly mark player-view, gameplay, and persistence checks as not run. A rendered diagram is useful design evidence, not proof that a quest file loads in Minecraft.
