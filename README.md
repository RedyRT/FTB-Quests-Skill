# FTB Quests Skill

An English-language agent skill for creating and refining FTB Quests questbooks, with an emphasis on readable progression and uncluttered layouts.

This is a reusable skill for an AI coding assistant, not a Minecraft mod or a premade questbook. It helps the assistant plan and edit quests without turning the chapter canvas into a visual maze.

## Features

- Clear main progression with separate side branches and meaningful milestones.
- Consistent spacing, aligned lanes, and fewer crossing dependency lines.
- A worked ten-quest layout with exact coordinates and sizing assumptions.
- Presentation-only edits that preserve IDs, tasks, rewards, and dependency semantics.
- Version-aware SNBT/JSON5 guidance and compatibility checks for newer visual features.
- Explicit static, visual, gameplay, and persistence verification boundaries.

## Install

1. Clone or download this repository:

   ```bash
   git clone https://github.com/RedyRT/FTB-Quests-Skill.git
   ```

2. Copy the `ftb-quests/` folder into your agent's configured skills directory.
3. Reload skills or restart the agent if its runtime requires it.

Copy the entire `ftb-quests` directory into the skills directory recognized by your agent. Keep `SKILL.md` and `references/` together. If your runtime supports the cross-agent convention, the destination is `~/.agents/skills/ftb-quests/`; otherwise use that runtime's configured skill directory. Reload skill discovery if required by your agent.

This repository does not install anything globally or modify Minecraft files automatically.

## Use

Ask your agent to use `ftb-quests`, for example:

> Use the ftb-quests skill to reorganize this chapter into a clear progression map. Preserve IDs, tasks, rewards, dependency semantics, and completion settings. Return coordinates and verify the player-facing layout.

For new content:

> Use the ftb-quests skill to design an introductory quest chapter for my modpack. Inspect the installed versions and existing quest exports first. Make the main route obvious and keep side activities visually separate.

Provide the modpack or quest files where available. A screenshot helps assess layout, but cannot establish dependency settings or file compatibility. Without a target export/version, the skill produces a complete design manifest rather than pretending to generate import-ready data.

## Contents

- [SKILL.md](ftb-quests/SKILL.md): discovery metadata, workflow, and required output.
- [Layout reference](ftb-quests/references/layout.md): topology choices, spacing rules, coordinate algorithm, worked ten-quest example, and visual checks.
- [Authoring reference](ftb-quests/references/authoring.md): version-aware editing, task/reward guidance, and verification matrix.
- [Sources](ftb-quests/references/sources.md): the three requested sources, additional visual examples, version evidence, and access limitations.

The skill distinguishes older SNBT workflows from the upstream JSON5 transition and gates newer visual features by version. Layout recommendations do not change gameplay implicitly.

```text
FTB-Quests-Skill/
|-- README.md
`-- ftb-quests/
    |-- SKILL.md
    `-- references/
        |-- layout.md
        |-- authoring.md
        `-- sources.md
```

## Design approach

Choose the progression before placing nodes. Use one reading direction, group related quests, reserve space around forks and joins, and keep side activities outside the main route. Use a grid for independent collections rather than inventing prerequisites to make them look like a tree.

Spacing values are starting points, not engine limits. Verify the actual questbook at the target GUI scale with editing mode off. A clean diagram alone does not prove that files load or gameplay works.

## Sources and affiliation

Based on the [official CurseForge listing](https://www.curseforge.com/minecraft/mc-mods/ftb-quests-forge), [FTBTeam source repository](https://github.com/FTBTeam/FTB-Quests), and [quest-layout community discussion](https://www.reddit.com/r/feedthebeast/comments/1gxe0l2/my_first_attempt_at_making_a_quest_system_with/), with an additional public visual reference. See [source notes](ftb-quests/references/sources.md) for exact evidence and access limitations.

This is an independent skill, not an official FTBTeam project. No third-party questbook data or screenshots are bundled.

## Validation

The package was checked with five fresh-context applications without the skill and five with the full skill. The guided applications consistently included the SNBT/JSON5 and visual-feature version boundaries, explicit spacing assumptions, complete coordinate tables, preserved gameplay semantics, and runtime limitations.

A separate application covered a large node, a one-of-two dependency threshold, a nonoptional side branch, conflicting cosmetic requests, and a cyclic component. It retained the real dependencies and flags, accommodated the large node, and reported the unresolved cycle rather than changing gameplay.

Package checks passed for frontmatter, local Markdown links, code-fence balance, and English-only content. A geometry check of the worked ten-node, ten-edge example found no overlapping circular node footprints, unrelated edge crossings, overlapping edge segments, or edges passing through unrelated nodes. These results assume the sizes and straight connections stated in the example.

No Minecraft instance was available. In-game loading, GUI appearance, unlocks, rewards, and persistence were not tested. The skill explicitly requires those checks when used against an actual modpack.
