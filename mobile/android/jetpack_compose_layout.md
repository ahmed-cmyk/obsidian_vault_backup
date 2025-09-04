
## Grouping

- **Definition**: connecting related elements that share context (e.g., image + caption).
- **Explicit grouping**:
    - Uses **boundaries**: outlines, dividers, shadows, cards.
    - Often signals **interactivity**.
- **Implicit grouping**:
    - Uses **proximity** and **open space**.
    - Example: headline + subhead + thumbnail clustered together.

---
## Margins

- **Definition**: space between the edge of a window and its contents.
- **Adaptive**: margin widths scale with window size class.
- **Wider margins**: used on larger screens to create breathing room.

---
## Spacers

- **Definition**: space between panes in a layout (usually 24dp wide).
- Can contain a **drag handle** for resizing panes.

---
## Padding

- **Definition**: space between UI elements.
- Measured in increments of **4dp**, vertical or horizontal.

---
## Windows & Layout Regions

- A **window** frames the product, divided into:
    - **Navigation region**: contains nav components (drawer, rail, bar).
    - **Body region**: main app content (images, text, lists, cards, buttons, app bars).

### Panes

- **Definition**: layout containers inside the body region.
- A window can have **1–3 panes**:
    - **Fixed**: fixed width.
    - **Flexible**: resizes with available space (every layout needs at least one flexible pane).
- **Adaptation strategies**:
    - Show & hide
    - Levitate (overlay one pane on another)
    - Reflow (reorganize panes when resized)

### Containment

- **Implicit containment**: panes blend into background.
- **Explicit containment**: use color separation (esp. in XR / spatial environments).

### App Bars

- Can appear inside panes (top app bar, bottom app bar).
- Actions shown or hidden based on width.
- Avoid moving elements between panes when layout changes.

### Columns (inside panes)

- Segment and align content within a pane.
- Not used at the window level.

### Drag Handle

- Resizes panes dynamically.
- Placed in **spacers** between panes.
- Accessibility:
    - Labeled as interactive (“Resize layout”)
    - Supports keyboard navigation (Tab → Space/Enter → Arrow keys)

---
## Density

### Information Density

- Refers to **how much content is visible** in a given screen space.
- Achieved through **layout spacing** (margins, spacers, padding), not only component scaling.
- High-density layouts:
    - Useful for **scanning and comparing lots of information** (tables, dashboards, news).
    - Increase list/table/form density so more items fit on screen.
- Low-density layouts:
    - Prioritize **aesthetics, clarity, or navigation ease**.
- Density shouldn’t automatically change with window size class or orientation — let users decide.

### Component Scaling

- Controls **internal spacing of individual components**.
- Scale starts at **0 (default density)**.
    - Negative numbers (e.g., -1, -2) = higher density (reduced padding/height, usually by 4dp increments).
- **Text size stays the same**, only spacing shrinks.
- Don’t apply scaling by default if it reduces tap targets below **48×48 dp**.
- **Good use cases**: tables, data-heavy UIs.
- **Bad use cases**: menus, snackbars, dialogs (reduced usability).

### Targets & Accessibility

- Default interactive target = **48×48 dp (CSS pixels)**.
- Even if icons/visuals are smaller, the **touch target must remain 48×48 dp**.
- Smaller targets (<48×48) = **less accessible**; use caution.
- Provide **density controls** in settings so users can opt in/out easily.
- Example: a density menu (large / medium / small) for table layouts.

### Pixel Density

- **Pixel density** = number of pixels per inch.
    - High-density screens → more pixels in same area → UI elements look smaller.
    - Low-density screens → fewer pixels → UI looks larger.
- **Density-independent pixels (dp)**: scale UI consistently across screen densities.
    - 1 dp = 1 px on a 160 dpi screen.
    - Formula:

```kotlin
dp = (px * 160) / screen density
```

---
## Terms (Quick Reference)

- **Column**: vertical block(s) of content within a pane
- **Drag handle**: resizes panes
- **Fold**: hinge/flexible area in foldables
- **Margin**: space between screen edge & content
- **Multi-window mode**: multiple apps sharing screen
- **Pane**: layout container inside body region
- **Spacer**: 24dp space between panes
- **Window size class**: breakpoint for adapting layouts

---
