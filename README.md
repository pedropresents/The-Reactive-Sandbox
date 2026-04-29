# The Reactive Sandbox
**Pedro Febles Bula | AI 201 | Professor Tim Lindsey**
**SCAD Atlanta | Spring 2026**

---

🔗 **[Live Demo →](https://pedropresents.github.io/The-Reactive-Sandbox/)**

📱 **[Client View Preview →](https://pedropresents.github.io/The-Reactive-Sandbox/?view=client)**

📂 **[GitHub Repository →](https://github.com/pedropresents/The-Reactive-Sandbox)**

---

## What This Is

The Reactive Sandbox is a dual-mode performance dashboard for **Go Local Pin**, a real local SEO agency I run that helps Atlanta roofing contractors dominate Google Maps search results. It serves two users from one shared state object: the agency operator (me) and the contractor client.

The operator interface is desktop-primary: a three-column dashboard for triaging multiple client accounts in under 90 seconds on a Monday morning. The client interface is mobile-primary: a single-screen Google Presence Score designed for one-handed use, outdoor legibility, and a 20-second interaction window.

The whole system lives in one `index.html` deployed to GitHub Pages.

---

## The Architecture

Browser → Detail → Controller. Three components reading from and writing to one centralized state object.

| Panel | Role | Implementation |
|---|---|---|
| Browser | Client Roster | Auto-sorted by urgency; clicking selects a client |
| Detail View | Client Performance Detail | Reads from selected client; never holds its own copy |
| Controller | Operator Controls | Status changes, check-ins, client view toggle |

Every UI interaction writes to the central `state` object. A single `render()` function reads from state and updates all three columns. No component manages its own copy of the data.

---

## Core Design Philosophy

> The dashboard is not a place you go to check things. It is a system that catches things before they become problems.

The highest-value feature is not the roster, not the status flags, not the detail view. It is the mechanism that surfaces a score drop to me on a Tuesday before the client opens the app on Thursday.

The client interface has a parallel philosophy: deliver a verdict, not a report. Marcus does not open Go Local Pin to learn. He opens it to confirm.

---

## Design Intent Summary

**Color system:** Google Maps 2025 tokens. High contrast on pure white. Tier color carries the verdict so the client never has to read to know if his profile is working.

**Typography:** Google Sans with system-ui fallback. Score number 80px minimum. Body 14px. No serifs anywhere.

**Layout:** Operator at desktop 1280px+. Client at mobile 390px. All client-facing primary interactions live in the bottom 55% of the screen because Marcus uses his phone one-handed in his truck.

**Named principles:** The Verdict Principle. The Alert System Principle. The Two-Click Budget. Dessert, Not Dinner. Human Name on Every Red Number.

Full design intent at [`docs/design-intent.md`](docs/design-intent.md). Full build brief at [`docs/build-brief.md`](docs/build-brief.md).

---

## AI Direction Log

**Splitting design intent from build brief.** I produced two separate documents, not one combined doc. The design intent explains why; the build brief specifies what. Mixing them would have made the strategic doc too tactical and the spec doc too philosophical. Each now serves one moment cleanly.

**Two separate Claude sessions for portraits and build.** Portraits were generated in one session with qualitative constraints. The build was executed in a different session with a precise spec and the explicit instruction not to design. This prevented the build session from second-guessing decisions that were already made.

**Phased build with mandatory checkpoints.** The build was structured as three sequential phases: roster column first, then detail panel and controls, then the client mobile view and toggle. Each phase required show-and-review before continuing. Asking AI to build all three at once produces the failure mode the companion doc warns about: architectural decisions made without my approval.

**Vanilla single-file output.** I directed the build sessions to produce one `index.html` with no frameworks, no build tools, no CDN dependencies beyond `canvas-confetti`. The single-file constraint makes the architecture readable in one document. Anyone can search for the `state` object and follow every read and write through the code.

**Naming the alert-system reframe as a principle.** The "dashboard catches things, doesn't display them" insight surfaced during the operator portrait session. I pulled it forward and named it as a stated design principle so future build decisions would cite it as a reason, not just an unstated value.

---

## Records of Resistance

**Rejected vague persona language.** When generating the user portraits, the AI's default direction was generic UX persona format ("values efficiency," "tech-savvy enough but prefers simplicity"). I rejected this and built a prompt requiring every claim to be traceable to a real behavior, a real physical constraint, or a real decision the user faces. The resulting Marcus portrait has a laminated paper checklist zip-tied to his truck dashboard. That detail produces an actual UI decision (no jargon, no abbreviations); a generic persona never could.

**Rejected my own urge to over-prepare.** After writing the portrait prompt, I asked Claude whether I should spin up another session to "improve the prompt" before running it. Claude pushed back, naming this as my "permanent preparation" pattern: staying upstream when I already had what I needed to execute. I ran the prompt as written. The portraits came back specific and usable on the first attempt. This is a meta-resistance against my own AI workflow.

**Rejected refactoring across phases.** AI's natural behavior in a build session is to revisit and "improve" already-working code as it adds new sections. The companion doc explicitly names this as a primary failure mode for this project. I enforced a constraint in every phase prompt: "Phase X is complete and correct. Do not touch [previous components], the state object, or any existing CSS." The architecture stayed stable across all three build phases because the AI was never given permission to revisit accepted decisions.

Full ESF documentation at [`docs/esf-documentation.md`](docs/esf-documentation.md).

---

## Five Questions

**1. Can I defend this?** Yes. Every UI decision traces to either Marcus's "5:47 PM in the truck" scenario or my "Octane Coffee Monday triage" scenario, both documented in the user portraits.

**2. Is this mine?** Yes. The agency context, the user portraits, the design philosophy, and the named principles are mine. The AI manufactured CSS and HTML against specs I wrote.

**3. Did I verify?** Yes. Click any client in the roster and the detail panel updates with that client's data. Click a status pill and the roster row reflects the change. The detail panel never holds its own copy of the data; it reads from state on every render.

**4. Would I teach this?** Yes. One state object, three components reading from it, two of them writing back. Data flows down through the state object. User actions flow up through setState calls.

**5. Is my documentation honest?** Yes. The AI Direction Log entries describe decisions I actually made. The Records of Resistance describe moments I genuinely pushed back. Nothing is invented to satisfy the rubric.

---

## File Structure

```
The-Reactive-Sandbox/
├── README.md                ← you are here
├── index.html               ← the entire app
└── docs/
    ├── design-intent.md     ← professor-facing strategy doc
    ├── build-brief.md       ← element-by-element spec
    └── esf-documentation.md ← full ESF deliverables
```

---

## Working with the Code

The whole app is one file. To run locally, open `index.html` in a browser. To deploy, push to GitHub and enable Pages on the main branch.

To preview the client mobile view in isolation, append `?view=client` to the URL. This was a build-time addition for testing the mobile view without needing to navigate through the operator panel. Useful for screenshots and for showing the client view standalone during demos.

---

*Built April 22 to April 29, 2026 for AI 201 at SCAD Atlanta.*
*Per SCAD ESF Protocol. All creative direction by Pedro Febles Bula.*
