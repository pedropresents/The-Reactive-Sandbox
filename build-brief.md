# Build Brief
## The Reactive Sandbox
### AI 201 | Pedro Febles Bula | SCAD Atlanta

---

## How to Use This Document

This is the document you open when you are coding. Every screen, every element, every visual decision is pre-made here. If something is not in this document, it does not get built without a decision first. Do not design during the build session; execute from this brief.

---

## Visual Design Direction

### Color System (Google Maps 2025)

| Token | Hex | Usage |
|---|---|---|
| `--score-green` | `#1A73E8` | Score in good/excellent tier, positive delta arrows, "On Track" status |
| `--score-yellow` | `#F9AB00` | Score in fair tier, neutral delta, "Watch" status |
| `--score-red` | `#D93025` | Score in poor tier, negative delta, "Needs Attention" status |
| `--surface-white` | `#FFFFFF` | All primary backgrounds (client and operator) |
| `--text-primary` | `#202124` | All primary text, score numbers |
| `--text-secondary` | `#5F6368` | Labels, timestamps, secondary info |
| `--border` | `#DADCE0` | Dividers, card outlines, row separators |
| `--accent-blue` | `#1A73E8` | CTAs, links, active states |
| `--surface-hover` | `#F1F3F4` | Roster row hover state |

No off-white backgrounds. No gray-on-gray text pairings. Every text element must pass contrast against `#FFFFFF` at arm's length in sunlight.

### Typography

| Role | Font | Weight | Size |
|---|---|---|---|
| Score number (client) | Google Sans or system-ui | 700 | 80px minimum |
| Screen headers | Google Sans or system-ui | 600 | 18px |
| Body / labels | Google Sans or system-ui | 400 | 14px |
| Alert flags | Google Sans or system-ui | 500 | 13px |
| Timestamps | Google Sans or system-ui | 400 | 12px, `--text-secondary` |

If Google Sans is unavailable, fall back to `system-ui, -apple-system, sans-serif`. Do not use a serif font anywhere in the UI.

### Component Rules

- **Border radius:** 8px on cards and roster rows; 50% on score arc container; 4px on status pills
- **Shadows:** single shadow only (`0 1px 3px rgba(0,0,0,0.12)`), used on cards and detail panel only
- **Spacing unit:** 8px base. All padding and margins are multiples of 8.
- **Transitions:** 200ms ease for color/opacity changes; 400ms ease-out for score arc animation
- **No gradients.** No decorative background patterns. No drop shadows on text.

### Score Tier Definitions

| Tier | Range | Color Token | Label |
|---|---|---|---|
| Poor | 0 to 39 | `--score-red` | Needs Work |
| Fair | 40 to 59 | `--score-yellow` | Building |
| Good | 60 to 79 | `--score-green` | On Track |
| Excellent | 80 to 100 | `--score-green` | Dominating |

---

## Screen Inventory

Three screens total. All live in one `index.html`. Screen state is controlled by the centralized state object.

| ID | Name | Primary User | Viewport |
|---|---|---|---|
| S1 | Operator Dashboard | Pedro (operator) | Desktop, 1280px+ |
| S2 | Client Score View | Marcus (contractor) | Mobile, 390px (iPhone 14) |
| S3 | Client View Toggle | Pedro showing Marcus during onboarding | Desktop renders mobile sim |

S3 is not a separate screen. It is a view state within S1 that renders S2 in a centered mobile frame on the desktop viewport.

---

## S1: Operator Dashboard

### Layout

Three columns. Fixed left column (roster), flexible center column (detail panel), fixed right column (operator controls). No top navigation bar.

```
| ROSTER (320px fixed) | DETAIL PANEL (flex) | CONTROLS (280px fixed) |
```

Full viewport height. No scroll on the outer container. Individual columns scroll internally if content overflows.

### Column 1: Client Roster

**Purpose:** Monday morning triage. Scan all accounts in under 90 seconds. Identify who needs action without opening the detail panel.

**Header row (sticky):**
- "Clients" label: 16px, weight 600, `--text-primary`
- Sort label "Sorted by urgency": 12px, `--text-secondary`
- No sort controls. Sorting is automatic and not user-adjustable.

**Roster row (each client):**

Height: 72px. Padding: 12px 16px. Border-bottom: 1px `--border`.

Elements in order top-to-bottom, left-to-right:
1. Client name: 14px, weight 500, `--text-primary`
2. Score number with tier color: 20px, weight 700, colored by tier token
3. Delta indicator: direction arrow + number (e.g. "↑4"), 12px, colored by direction
4. Alert flag: plain language, one flag maximum per row (e.g. "1 unanswered review"), 12px, weight 500, `--score-yellow` if present
5. Last updated timestamp: 12px, `--text-secondary`

If no alert flag: the alert flag space is empty. Do not show "No alerts." Empty is fine.

**Row states:**
- Default: `--surface-white` background
- Hover: `--surface-hover` background, 200ms transition
- Selected (detail panel open): left border 3px solid `--accent-blue`, `--surface-hover` background
- Status "Needs Attention": no background change. The alert flag communicates this.

**Inline status actions (visible on row hover only):**
Three pills appear on the right edge of the row on hover:
- "On Track": clicking sets status, fires row confirmation animation
- "Watch": clicking sets status
- "Needs Attention": clicking sets status

Pills: 4px border-radius, 12px font, 4px 8px padding. Color matches status tier token. These are the only clickable elements in the roster column.

**Auto-sort order:**
1. Score dropped this week (largest drop first)
2. Unanswered reviews present
3. Last updated more than 5 days ago
4. All other accounts (sorted by score ascending, lowest scores first)

---

### Column 2: Detail Panel

**Purpose:** Full account data for a selected client. Opened by clicking a roster row. Not required for triage; this is the drill-down.

**Empty state (no client selected):**
Centered in panel: "Select a client to review", 14px, `--text-secondary`. Nothing else.

**Loaded state:**

Header (sticky):
- Client name: 18px, weight 600
- Current score: 32px, weight 700, tier color
- Week-over-week delta: 14px, direction arrow + number
- "View in Local Falcon ↗" link: 13px, `--accent-blue`
- "Open in BrightLocal ↗" link: 13px, `--accent-blue`

Score breakdown section:
Five factor cards in a 2-column grid (last card full-width if odd number). Each card:
- Factor name in plain language (not abbreviation)
- Factor score: number out of 20 (each factor is worth 20 points)
- Mini progress bar: colored by tier token
- One-sentence plain language description of what this score means

Factor names and plain language labels:
| System Name | Plain Language Label |
|---|---|
| Visibility | "Shows up in searches" |
| Momentum | "Getting more attention over time" |
| Engagement | "People are clicking and calling" |
| Reputation | "Reviews and ratings" |
| Activity | "Profile is kept up to date" |

Activity feed section (below score breakdown):
- Header "Recent activity": 14px, weight 500
- List of timestamped actions: "Post published," "Review responded to," "Photo added," etc.
- Maximum 5 items shown. No pagination; this is a summary, not a log.

Operator note field:
- Plain text input with "Add a note for this client..." placeholder
- No submit button. Note saves on blur.
- Saved notes display above the input field with timestamp

---

### Column 3: Operator Controls

**Purpose:** Account-level actions and client view access. Not for data review; for doing things.

**Client View button (top of column, always visible):**
- Label: "Show Client View"
- Full width of column
- `--accent-blue` background, white text, 8px border-radius
- On click: activates S3 (client view toggle state)
- This button is always visible regardless of whether a client is selected. If no client is selected, clicking it shows a prompt: "Select a client first."

**Selected client actions (appear when a client is selected):**

- "Mark as Reviewed": clears the alert flag on the roster row, updates client view with "Pedro reviewed this [day]"
- "Schedule Check-in": opens a simple date input, saves a check-in date that appears in the client view
- "Publish Post": placeholder button (no real function in prototype, label reads "Connect GBP to enable")
- "Request Review": placeholder button (same caveat)

Placeholder buttons are shown at 50% opacity with a tooltip on hover explaining they require live GBP connection. They are shown, not hidden, because they communicate the product's full capability even in prototype state.

**Danger zone (bottom of column):**
- "Remove Client": 12px, `--score-red` text, no background. Clicking fires a confirmation state ("Are you sure? This cannot be undone.") with two options: "Cancel" and "Remove." No actual removal logic needed in prototype; the confirmation state is the interaction.

---

## S2: Client Score View

### Layout

Single column. Mobile viewport (390px). No navigation bar. No header. The score is the page.

All interactive elements in the bottom 55% of the screen. Nothing important above the vertical midpoint.

### Elements in order (top to bottom):

**1. Go Local Pin wordmark**
Top center. 14px, weight 600, `--text-secondary`. Small. Present for brand credibility, not prominence. Margin-top: 48px.

**2. Score arc**
SVG circle arc. Centered horizontally. Arc sweeps from 7 o'clock to 5 o'clock (220 degree sweep). Stroke color matches tier token. Background arc stroke: `--border`. Arc animates from 0 to current value on load (400ms ease-out via `stroke-dashoffset` transition).

Score number inside arc: 80px minimum, weight 700, `--text-primary`. Do not color the number; the arc carries the color signal. The number is always near-black.

Tier label below score number inside arc: 14px, weight 500, tier color. ("On Track", "Needs Work", etc.)

**3. Status sentence**
One sentence. 16px, weight 400, `--text-primary`. Centered. Margin-top: 24px from arc bottom.

Format rules:
- If score increased: "You got [X] new reviews this week. Keep it up."
- If score stable: "Your profile is holding steady. Stay consistent."
- If score decreased + operator has reviewed: "Your score dipped slightly. Pedro reviewed this [day]. Check-in [day]."
- If score decreased + operator has NOT reviewed: "Your score dipped slightly. Pedro will be in touch soon."

No other text on this screen. One sentence only.

**4. Delta indicator**
Small. Centered below status sentence. Direction arrow + number + "this week" label. 14px, weight 500, tier color for the direction. Margin-top: 8px.

Example: "↑ 4 this week" in `--score-green`. "↓ 3 this week" in `--score-red`.

**5. Drill-down tap target**
Large tap target (minimum 48px height, full width minus 32px margin). Centered. Located in the bottom 40% of the screen.

Label: "See what's driving your score →"
Style: `--border` outline, 8px border-radius, 14px, weight 500, `--text-primary`. Not a filled button; a ghost button. It is dessert, not dinner.

On tap: expands an accordion below it showing the five factor scores as horizontal bar rows. Each row: factor plain language label, score out of 20, colored bar. No numbers beyond the bar; the bar length communicates the value.

**6. Share button (conditional, tier upgrade only)**
Only renders when the state object contains `tierUpgrade: true` for this client. Not visible at any other time.

Label: "Share your score"
Style: `--score-green` background, white text, 8px border-radius, full width minus 32px margin.

On tap: triggers `canvas-confetti` burst, then opens native share sheet via Web Share API (`navigator.share`) with a pre-built message: "My Google presence score just hit [tier] with Go Local Pin. 📍" If Web Share API unavailable, copies message to clipboard and shows "Copied to clipboard" toast.

---

## S3: Client View Toggle

### What it is

A view state within S1. When the operator clicks "Show Client View" in the controls column, the center panel and right column are replaced by a centered mobile frame (390px wide, 844px tall, with a device chrome outline) rendering S2 for the currently selected client.

The roster column remains visible on the left so Pedro can switch between clients during an onboarding demo without exiting the toggle state.

### Toggle bar (replaces the detail panel header):
- "Client View: [Client Name]" label, 14px, weight 500
- "← Back to Operator View" link, 14px, `--accent-blue`

### Device frame:
- Rounded rectangle outline, 12px border-radius, 2px `--border` stroke
- No fake device buttons or notch. Keep it clean.
- Drop shadow: `0 4px 16px rgba(0,0,0,0.12)` to lift it off the background
- Background outside frame: `--surface-hover`

---

## State Object Structure

All three screens read from and write to one centralized state object. No screen manages its own state.

```javascript
const state = {
  selectedClientId: null,
  clientViewActive: false,
  clients: [
    {
      id: "client_001",
      name: "Reyes Premier Roofing",
      score: 74,
      scorePrevious: 70,
      tier: "good",
      tierUpgrade: false,
      status: "on_track",
      alertFlag: null,
      lastUpdated: "2025-04-18",
      operatorNote: "",
      checkinDate: null,
      operatorReviewedDate: "2025-04-21",
      factors: {
        visibility: 16,
        momentum: 14,
        engagement: 15,
        reputation: 18,
        activity: 11
      },
      activityFeed: [
        { date: "2025-04-20", action: "Review responded to" },
        { date: "2025-04-18", action: "Post published" }
      ]
    }
    // repeat structure for each client
  ]
}
```

Every UI interaction writes to this object first. The UI re-renders from the object. No element updates its own visual state directly.

---

## Feedback + Juice Implementation

| Moment | Trigger | Implementation |
|---|---|---|
| Score tier upgrade | `tierUpgrade: true` on client load in S2 | `canvas-confetti` from CDN on mount; Vibration API `navigator.vibrate(200)`; share button renders |
| Score increase (no tier change) | `score > scorePrevious` | Arc animates from previous score position, not from 0; "+N this week" badge fades in 300ms after arc completes |
| Operator status change | Status pill click in S1 roster | Row background flashes `--score-green` at 30% opacity for 300ms then returns to default; checkmark icon fades in on the row for 600ms then fades out |

All three are wired into the state update function. When state changes, the update function checks for these conditions and fires the corresponding feedback call before re-rendering.

---

## What Is Not Being Built

These features are acknowledged as real product requirements but are explicitly out of scope for this prototype:

- Live GBP API connection
- Authentication / per-client login
- Push notifications for score drops
- PDF report generation (BrightLocal handles this)
- Rank map visualization (Local Falcon handles this)
- Mobile operator view

Placeholder UI exists for out-of-scope features (the grayed-out action buttons in S1 Column 3). They communicate capability without requiring implementation.

---

*Build Brief | The Reactive Sandbox | Pedro Febles Bula | Go Local Pin / Febles Digital | SCAD Atlanta Class of 2029*
