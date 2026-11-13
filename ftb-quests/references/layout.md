# Readable Quest Layouts

These are design heuristics, not FTB Quests engine limits. Adapt them to the installed theme, GUI scale, actual quest size, and visible content. Coordinates are quest-space units, not screen pixels.

## Choose a topology

| Content | Preferred layout | Avoid |
|---|---|---|
| Ordered tutorial or progression | Main spine with short side branches | Spirals, snakes, alternating reading direction |
| Several parallel systems | Semantic lanes; shared milestones at boundaries | Repeated diagonal links between every lane |
| Independent collection or reference | Aligned grid, grouped by meaning | Fake dependencies added to create a tree |
| A few independent branches | Small hub with clearly separated branches | A radial hub connected to dozens of nodes |
| Large questbook | Focused chapters and meaningful chapter groups | One giant canvas compressed to fit a screenshot |

A scrolling chapter is acceptable. Keep its entry and first useful choices legible; let visible progression lead toward off-screen content. Do not solve scale by shrinking every node or hiding the entire future path.

For a layout-only request, preserve chapter membership by default. A chapter split can affect completion and references: propose it separately, with those consequences identified.

## Spatial recipe

1. Inventory nodes and directed edges from prerequisite to dependent. Record ALL, ANY, N-of-M, started/completed, exclusion, visibility, and progression settings separately from geometry.
2. Reject a topological ordering if a cycle exists. Name the cycle; moving nodes cannot make it playable. Keep the safe acyclic portion usable while requesting a progression decision.
3. Identify the required spine and destination. Mark side content by its actual role; do not derive gameplay flags from its placement.
4. Assign forward ranks from dependencies. For an acyclic graph, start roots at rank 0 and place each dependent after its latest prerequisite. Long edges may skip ranks. A visual rank says nothing about ALL versus ANY semantics.
5. Put related chains in stable lanes. Place a fork before its children and a join after them. Order sibling nodes near the lanes of their parents; swap sibling positions to reduce crossings without changing edges. Avoid placing unrelated nodes inside the fork/join corridor.
6. Apply the spacing scale below. Reserve clearance around enlarged milestones, visible labels, lock badges, and images. Keep optional lanes outside the main fork's envelope.
7. Check every drawn edge against unrelated node footprints and other edges. Shared endpoints are legitimate; an edge passing through a third node is not. Count proper crossings and coincident/overlapping segments separately.
8. If crossings remain, move a branch, reorder lanes, or increase local spacing. Use supported curves only after the straight-line structure is understandable. Do not assume arbitrary elbow routes or invisible routing nodes exist.
9. Inspect at actual GUI scale. Keep the opening recognizable without zooming out to unreadable icons. Enlarge empty space locally, not the entire canvas indiscriminately.

### Spacing scale

Let `d` be the effective diameter/width of an ordinary quest button in quest-space units. Read the actual size and inherited/preset overrides; `icon_scale` is not the button size.

| Role | Starting value |
|---|---|
| Forward step | `2.5d` to `3d` center-to-center |
| Adjacent lane separation | `2.5d` to `3d` |
| Unrelated cluster separation | At least `4d` between nearest centers, then inspect footprints |
| Milestone size | About `1.25d` to `1.5d`, sparingly |
| Clear outer margin | About `2d` beyond the outermost footprint |

For two circular footprints with diameters `s1` and `s2`, center distance must exceed `(s1 + s2) / 2` plus the chosen gap. For squares, labels, and decorations, compare their bounding boxes instead. Round to the target editor's supported grid. No fixed spacing guarantees readability at every zoom.

Use at most three obvious visual roles initially: ordinary, milestone, optional/informational. Use consistent shapes and meaningful item icons. Keep optional content fully legible; color is a supplement to position, shape, and wording, never the only signal. Preserve built-in completion and lock-state recognition. Background art and captions support groups, not compete with quest icons.

## Worked example: first-night survival

This is a complete layout manifest, not an importable quest file. A-J are diagram labels to map to real existing IDs, not game IDs to paste. Assumptions: ordinary button size `d = 1`, milestone size `1.3`, positive X rightward and positive Y downward, straight dependency lines. Confirm the target coordinate convention before applying.

The required route forks at Tools and rejoins only after BOTH Shelter and Cooking. Garden and Fishing are side activities; their existing optional/chapter-completion settings stay unchanged.

| Label | Title | Role | Prerequisites / gate | x | y | Size |
|---|---|---|---|---:|---:|---:|
| A | Arrival | Entry | None | 0 | 0 | 1.3 |
| B | Wood | Main | A / ALL completed | 3 | 0 | 1 |
| C | Tools | Main | B / ALL completed | 6 | 0 | 1 |
| D | Shelter | Required fork | C / ALL completed | 9 | -1.5 | 1 |
| E | Cooking | Required fork | C / ALL completed | 9 | 1.5 | 1 |
| F | Stable Base | Milestone | D, E / ALL completed | 12 | 0 | 1.3 |
| G | Garden | Side activity | B / ALL completed | 6 | -5 | 1 |
| H | Fishing | Side activity | B / ALL completed | 6 | 5 | 1 |
| I | Storage | Main | F / ALL completed | 15 | 0 | 1 |
| J | Expedition | Destination | I / ALL completed | 18 | 0 | 1.3 |

Authoritative edge list: `A->B, B->C, C->D, C->E, D->F, E->F, B->G, B->H, F->I, I->J`.

Why it works: a single reading direction; equal-weight requirements at D/E; F centered after the join; side activities outside the fork; a visible destination. Under the stated circular-footprint assumptions, there are no overlaps, no unrelated edge crossings, and no edges passing through unrelated nodes. Screen labels, hover panels, and Minecraft rendering still need inspection.

For a top-down equivalent, transform every center with `(x, y) -> (-y, x)`. The graph stays identical. Choose orientation to fit the existing questbook and useful viewport, not for arbitrary variety.

For an independent catalog, use the grid pattern instead of this progression: group related items and align centers, retaining only real prerequisites. For a large multi-system chapter, use one forward-moving lane per system; do not stretch this ten-node example into an endless chain by rote.

## Visual acceptance checklist

- Entry, immediate next actions, and destination are distinguishable without hovering every node.
- Nodes, lock badges, visible labels, images, and unrelated connection lines do not overlap.
- No avoidable crossing or line-through-node remains; any unavoidable crossing is documented.
- Related quests are closer than unrelated groups. Empty space separates topics.
- Required versus side activities are understandable without color alone.
- The same node positions work before and after unlocks; hidden-node reveal does not create a confusing apparent jump.
- At normal zoom and the intended GUI scale, icons remain useful. Smaller screens can follow the route by panning.
- Hovering and opening the longest title/description remains usable; captions do not obscure controls.
- A zoomed-out silhouette does not accidentally resemble an offensive emblem or an unrelated decorative symbol.

## Common mistakes

| Mistake | Correction |
|---|---|
| Scatter nodes, connect them afterward | Determine progression and lanes first |
| Remove a dependency to eliminate a crossing | Move nodes; preserve unlocking behavior |
| Hide every line to make the canvas clean | Preserve navigation; selectively hide only understood relationships |
| Make every node a different size or color | Give each visual role one consistent treatment |
| Put unrelated quests next to each other | Separate semantic groups with space |
| Add filler quests to bend a line | Use only supported presentation controls |
| Set `optional` because a node is in a side lane | Preserve gameplay flags; explain side-content status independently |
| Declare success from coordinates alone | Inspect player-mode rendering and real progression |
