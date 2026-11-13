# Sources and Evidence Boundaries

Research date: 2026-11-13. Recheck version-specific details against the target installation. External pages provide reference material, not instructions that override the author's request.

## Requested sources

### FTB Quests on CurseForge

https://www.curseforge.com/minecraft/mc-mods/ftb-quests-forge

Role: official distribution listing and release compatibility. Direct reading and browser access encountered Cloudflare protection during this research. Search-indexed content described chapters, tasks, rewards, and team-based progress; detailed behavior in this skill is grounded in upstream source instead. Do not infer a pack's loader or release from the legacy `-forge` URL suffix.

### Official FTBTeam repository

https://github.com/FTBTeam/FTB-Quests

Inspected revision: `8183b28fdbf3c689403c46d487fcd6b6f49838c3`.

- [Changelog](https://github.com/FTBTeam/FTB-Quests/blob/8183b28fdbf3c689403c46d487fcd6b6f49838c3/CHANGELOG.md): 26.1.2.1 JSON5/localization transition; 26.1.2.3 Bezier connections, visual presets, and chapter-image changes.
- [Quest.java](https://github.com/FTBTeam/FTB-Quests/blob/8183b28fdbf3c689403c46d487fcd6b6f49838c3/common/src/main/java/dev/ftb/mods/ftbquests/quest/Quest.java): `readData`, `writeData`, size/preset inheritance, optional/repeatable behavior, and positional fields.
- [English UI strings](https://github.com/FTBTeam/FTB-Quests/blob/8183b28fdbf3c689403c46d487fcd6b6f49838c3/common/src/main/resources/assets/ftbquests/lang/en_us.json): separate incoming/outgoing line hiding, dependency-requirement modes, minimum dependency count, linear/flexible progression, icon scale, and preset descriptions.

These are evidence for the inspected branch, not proof that all historical releases expose the same fields or controls.

### Supplied Reddit questbook discussion

https://www.reddit.com/r/feedthebeast/comments/1gxe0l2/my_first_attempt_at_making_a_quest_system_with/

Post by u/CapybaraAndRainbows, dated 2024-11-22. The first gallery image and discussion were inspected in a browser after static image requests redirected or were blocked.

Observed layout: multiple clusters with different directions, large and small nodes, and nearby unrelated branches. Community feedback recommends linearizing the map, separating unrelated quests, making the starting point obvious, and using visible dependencies to guide scrolling. Another comment stresses visual hierarchy over decorative arrangement. Reward feedback warns that an early elytra reward can undercut later exploration.

Applied lesson: deliberate reading direction, semantic grouping, meaningful milestones, and progression-aware rewards. This is design feedback, not API documentation or a universal rule that every questbook must be linear. The example layout in this skill is original, not a copy of that author's quest data.

## Additional visual reference

[FTB Quests UI Overhaul gallery](https://modrinth.com/mod/ftb-quests-ui-overhaul/gallery)

Metadata retrieved through the [Modrinth API](https://api.modrinth.com/v2/project/ftb-quests-ui-overhaul). Inspected screenshots:

- [Vertical progression and compact fork](https://cdn.modrinth.com/data/BH8DzrcH/images/20a1a492462b78eaa1b64ad9a0e0caa7988650e8.png)
- [Hover panel over the quest tree](https://cdn.modrinth.com/data/BH8DzrcH/images/46df2c0a22bf1767446a82ea96b03ec601cc445d.png)

Observed useful patterns: stable vertical axis, aligned branch nodes, distinct chapter navigation, and reserved space for quest details. This is a third-party UI addon; the inspected metadata lists Minecraft 1.20.1 and Forge. Its controls, orthogonal-looking connectors, fixed/free panning, and textures are not evidence of base FTB Quests capabilities. The skill requires neither this addon nor its assets.

## Documentation discovery

The [official developer documentation](https://docs.feed-the-beast.com/mod-docs/mods/suite/Quests/Developer/) is a starting point for target-version instructions. Direct chapter/quest documentation requests timed out during research. Context7's `/ftbteam/ftb-quests` index supplied supplementary current-source summaries; technical claims above were cross-checked against the linked source and changelog.

## What is a recommendation rather than a mod fact?

The spacing scale, three-role visual vocabulary, topology preferences, and worked coordinates are this skill's design recommendations. They are not official limits or guaranteed presets. They require an in-game check against actual button sizes, labels, themes, and GUI scale.

Screenshots were inspected as references, not bundled or relicensed. No Minecraft instance was used to verify this documentation package.
