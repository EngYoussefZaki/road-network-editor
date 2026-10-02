# Road Network Editor

A browser-based infrastructure editor for game maps.

Design road, rail, bridge, tunnel and metro networks visually, then export them as
JSON for use in your own game engine. Built for strategy games, wargames, city
builders, logistics simulations and custom map tools.

It runs as a **single HTML file** — no build step, no dependencies, no server.
Open `index.html` in any modern browser and you're editing.

![screenshot](docs/screenshot.png) *(optional: add a screenshot later)*

## Features

- **Drawing tools:** point-chain, freehand, and straight ruler (Shift = 45° snap).
- **Infrastructure types:** local road, highway/arterial, bridge (elevated),
  tunnel (sunken), surface railway, underground metro.
- **Templates:** square cross, roundabout, mini-roundabout, Y/T junctions, layby,
  bottleneck, straight/curved ramps, exit ramp, cloverleaf, diamond, loop,
  outer connection, directional (DDI), semi-directional, lane tapers/parallels/
  optional/lane-drop, bridge exit/merge, rail crossing, flying junction,
  metro station, tunnel portal.
- **Smart placement:** snap to nodes and to road midlines (mid-road snaps split
  the road and preserve its curve), auto-align + auto-rotate templates to the
  road they touch.
- **Topological welding:** drag a node/joint onto another node or road and, on
  release, the geometry genuinely merges (shared node, or a split T-junction) —
  not just overlapping coordinates. Welds are undoable.
- **Three grab modes:** move a whole template, edit a single joint, or move an
  entire connected network (roads + welded templates travel together).
- **Multi-select:** Shift-click to add/remove joints, rubber-band marquee select,
  rigid multi-node move, single-node magnet snap.
- **Rail / metro separation:** surface railway and underground metro render as
  distinct systems, with an automatic portal marker where they meet. Convert any
  segment between surface and underground from the right-click menu.
- **Editing:** move / rotate / scale / duplicate / delete placed templates;
  per-template lanes, lane width, shoulder; right-click context menus.
- **Eraser modes:** point+connected roads, single segment, whole network, area.
- **Persistence:** named local saves in the browser (dropdown + Load/Delete), plus
  JSON import/export. `Reset Canvas` clears the drawing only — saved maps survive.
- **Undo** for drawing, splitting, welding and segment conversion.

## How to run

Just open `index.html` in a browser. To try it online without downloading, see the
GitHub Pages link in the repo's About section (if enabled).

## Controls (quick reference)

| Action | Input |
|---|---|
| Pan the map | Middle-drag, or Shift+left-drag |
| Ruler angle snap | Hold Shift while drawing a straight segment |
| Select / edit tool | Switch to **Select / Edit** |
| Grab mode | ⬚ Whole template · ● Single node · ⛓ Connected (ALT cycles temporarily) |
| Add / remove a joint from selection | Shift-click |
| Rubber-band select | Drag on empty space |
| Weld on release | Drag a node/template onto another node or road, then release (needs *Auto-align & merge*) |
| Template / rail segment menu | Right-click |
| Finish a chain | Right-click · Cancel: Esc |
| Curve a chain segment | Mouse wheel while chaining |
| Zoom | Mouse wheel |

## JSON map format

Exported files are versioned JSON with three stores plus ID counters:

```jsonc
{
  "version": 1,
  "name": "Road Map",
  "nextId": 0,
  "nextGroupId": 0,
  "nodes":  [ { "id", "x", "y", "z", "grp?", "lx?", "ly?", "_ring?" } ],
  "edges":  [ { "id", "from", "to", "type", "surface", "width", "lanes",
                "laneWidth", "shoulder", "curvature?", "cx?", "cy?",
                "grp?", "ring?" } ],
  "groups": [ { "id", "type", "wx", "wy", "size", "rotDeg", "lanes",
                "laneWidth", "shoulder", "anchorId" } ]
}