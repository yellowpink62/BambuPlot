# Changelog — BambuPlot

## v1.1.0

### SVG import
- Svg files are now accepted in the Image tab (`accept="image/*,.svg"`).
- Added an **SVG style** picker with four tracing modes:
  - **Vector outline** — traces the actual paths as single, clean pen strokes (best for logos and line art).
  - **Contour fill** — concentric, engraving-style rings that follow each shape.
  - **Shaded** — clean outline + light pencil-style shading of the black-filled regions.
  - **Filled (hatch)** — hatches the shapes like a photo.
- Added a **Shading spacing** slider for the shaded/contour modes (lower = denser fill).
- Duplicate contours in SVGs (e.g. a filled shape plus a stroked copy) are detected and dropped so they don't double-plot.
- SVGs with no traceable vector paths fall back to a normal raster image automatically.
- The preview canvas now draws SVG vector outlines instead of a rasterised thumbnail.

### G-code toolpath preview (new on-bed simulator)
- Added a **Preview** button that animates the generated toolpath directly on the bed canvas.
- The preview parses the actual G-code text, so mirrors, pen offsets and arc moves are shown exactly as the printer will run them.
- Includes **Play/Pause**, **Step**, **Reset**, and **Close**, plus a **speed** slider and a scrub bar, with a live status read-out (current line, X/Y position, % done).
- G0 travel is drawn as dashed lines, G1 as ink, G2/G3 arcs highlighted.

### G2/G3 arc moves
- New **"Use G2/G3 arc moves"** setting replaces curved pen paths with true circular-arc moves for smaller, smoother G-code.
- Arc fitting validates radius, sweep (≤180°) and corner safety, and automatically falls back to G1 whenever an arc can't be described safely.

### Pen offset correction
- The pen-tip offset is now **subtracted** from the machine coordinates (previously added), matching the physical reach compensation described in the UI.
- The bed canvas now draws the exact **reachable area** (dashed rectangle) when an offset is set, so it's clear where plots get cut.

### Settings persistence
- Pen settings, printer choice and custom bed dimensions are now saved to `localStorage` and restored on reload.
- Added the "Use G2/G3 arc moves" flag to the saved profile (save/load).

### Other UI & UX improvements
- New **"✕ Clear all"** button in the header clears the bed and generated G-code in one click.
- Reorganised the G-code action bar into icon buttons (**Preview**, **Copy**, **Download**).
- New **Copy** button copies the G-code to the clipboard.
- Downloaded files now use a descriptive filename built from the visible item labels (e.g. `bambuplot_A1_JOEP.gcode`) instead of just the printer id.
- Cleared/un-generated states now clear the "Clear all" action and the on-screen G-code output; the preview bar is dismissed on clear, delete, add, and generate.
- Wider tools panel and a stacked stage layout to accommodate the new toolpath preview and stats display.
- Transparent edges: "top edge = back" hint added to the stage, simplified stats bar.

### Bug fixes
- The spacing/line-height and feed settings that were previously re-computed as fixed steps are now persisted correctly.
- Handwriting "home pause" UI state is now kept in sync when a profile is loaded.
- Dwell-only steps etc. are skipped during preview parsing.
