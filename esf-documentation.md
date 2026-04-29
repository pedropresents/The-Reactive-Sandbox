# ESF Documentation: The Reactive Sandbox
**Pedro Febles Bula | AI 201 | Professor Tim Lindsey**
**SCAD Atlanta | Spring 2026**

*Per SCAD ESF Protocol. All creative direction and editorial decisions made by Pedro Febles Bula. AI was used as a manufacturing partner against specifications written by hand.*

---

## Records of Resistance

### Record 1: Vague Persona Language in the User Portraits

**What the AI produced (initial framing):**
When I went to generate the two user portraits (Marcus the contractor, Pedro the operator), the default direction Claude wanted to take was a generic UX persona format. Demographics, "values efficiency," "tech-savvy enough but prefers simplicity." Stock language that could describe any small business owner in any industry.

**Why I rejected it:**
A generic persona is useless for designing actual UI. If I cannot trace a specific UI decision back to a specific user truth, the persona is decoration, not design infrastructure. The whole point of building the portraits before the screens was to produce constraints I could design against.

**What I did instead:**
I built a prompt that explicitly required every claim to be traceable to a real behavior, a real physical constraint, or a real decision the user faces. I included a "bad output" filter at the end: any sentence like "values his time" or "tech-savvy enough but prefers simplicity" was to be cut and rewritten as a scene, a number, or a consequence. The prompt also required a Design Implication Table where every user truth had to map to a specific UI decision.

The resulting Marcus portrait has a laminated paper checklist zip-tied to his truck dashboard. His abandonment trigger is the first week his GBP score drops and nobody from Go Local Pin calls him before he notices himself. Both of those details produce real UI decisions: the score must surface a human name attached to every score drop, and the operator panel's highest-value feature is the alert system that flags drops before clients see them. Generic persona language never produces consequences like that.

---

### Record 2: Spinning Up Another Claude Session as a "Researcher"

**What I was tempted to do:**
After building the prompt for the user portraits, I asked Claude whether I should spin up another Claude session in a "researcher" role to improve the prompt before running it on the portraits. My instinct was to add another preparation layer.

**Why this was the wrong move:**
Claude pushed back directly. It pointed out this was my "permanent preparation" pattern: staying upstream when I already had what I needed to execute. The prompt was already doing the hard work. It had a role, a constraint mechanism, a bad output filter, a specific format. Another session before execution would just be another step between me and the actual deliverable. The portraits were the thing. There was no evidence the prompt needed fixing yet, only an assumption it might.

**What I did instead:**
I ran the prompt as written. The portraits came back specific and usable on the first attempt. If they had failed, then iterating with evidence in hand would have been the correct response. Iterating without evidence would have been procrastination dressed as rigor.

This is a meta-resistance: resisting my own tendency to over-prepare against AI workflows that are already well-scoped.

---

### Record 3: Letting the Build Session Touch Existing Code Across Phases

**What the AI would have done by default:**
When I moved from Phase 1 (the roster column) to Phase 2 (the detail panel and controls column), the natural behavior of a build session is to revisit the entire file as it adds new sections. Refactoring already-working code, renaming variables, "improving" CSS that already works. A common failure mode the companion doc explicitly names: "AI produces beautiful CSS before the state management works."

**Why I rejected it:**
Phase 1 was tested, working, and traced to the spec. Re-touching working code is the fastest way to break the architecture I had just verified. The companion doc names this as a primary AI failure mode for this project, and the ESF practices warn against it.

**What I did instead:**
Every phase prompt included explicit constraints written into the build instructions: "Phase 1 is complete and correct. Do not touch the roster, the state object, or any existing CSS. Build Phase 2 starting with the detail panel. Show me the full file after the detail panel is complete before moving to the controls column."

I also enforced a checkpoint protocol: after each major section was built, the AI had to surface the full file for my review before continuing. No silent multi-screen builds. The architecture remained stable across all three build phases because the AI was never given permission to revisit decisions I had already accepted.

---

## AI Direction Log

### Entry 1: Splitting the Design Intent Doc from the Build Brief

**The decision:** I directed Claude to produce two separate documents, not one. The Design Intent Doc explains the why (architecture statement, user portraits, design philosophy, named principles) and is the document my professor reads. The Build Brief is the spec my coding session reads (color tokens, layout dimensions, state object structure, element-by-element instructions).

**Why this mattered:** Mixing them would have made the design intent doc too tactical (full of pixel measurements) and the build brief too philosophical (full of user stories). When Claude initially offered to produce a single combined doc, I rejected that and required the split. Each document now serves one moment cleanly: one for the crit, one for the keyboard.

---

### Entry 2: Two Separate Claude Sessions for Portraits vs. Build

**The decision:** I ran the user portrait generation in one Claude session and the code build in a different session. The portrait session was given role context and qualitative constraints. The build session was given a precise spec and told not to design.

**Why this mattered:** A single continuous session would have carried portrait reasoning into the build session and tempted the AI to second-guess design decisions during code generation. Separating them enforced a clean handoff: portraits produced constraints, the build brief encoded those constraints into rules, the build session executed the rules. Each session had one job.

---

### Entry 3: Phased Build with Mandatory Checkpoints

**The decision:** The build was structured as three sequential phases with required show-and-review checkpoints. Phase 1: roster column only. Phase 2: detail panel + controls column. Phase 3: client mobile view + view toggle. The AI was instructed to stop after each phase and present the full file before proceeding.

**Why this mattered:** Asking AI to build all three panels at once produces the failure mode the companion doc warns about: architectural decisions made without my approval, unnecessary complexity, styling before state management. Phasing forced me to verify the wiring before each new layer was added. After Phase 1, I tested that clicking a roster row updated state and that status pills wrote to the correct client. Only after that worked did Phase 2 begin.

---

### Entry 4: Vanilla HTML/CSS/JS Over React

**The decision:** I directed the build sessions to produce a single `index.html` file with no frameworks, no build tools, no external dependencies beyond `canvas-confetti` from a CDN. The Browser/Detail/Controller architecture was implemented with a centralized JavaScript state object and a single `render()` function called on every state change.

**Why this mattered:** The single-file constraint made the architecture readable in one document. A new reader can open `index.html`, search for the `state` object, and follow every read and write of that state through the code. The shared-state pattern is enforced by a small, auditable surface area. This is also the constraint that lets the project deploy directly to GitHub Pages without a build step. A React + Vite stack would have produced a dist folder and a config layer that obscure the architecture this assignment is teaching.

---

### Entry 5: The Reframe: "The Dashboard Catches Things, It Doesn't Display Them"

**The decision:** During the operator portrait session, the AI surfaced an insight I then pulled forward into the design intent doc as the core philosophy: "The dashboard is not a place you go to check things. It is a system that catches things before they become problems." The highest-value feature is not the roster, not the status flags, not the detail view. It is the mechanism that surfaces a score drop to me before the client notices.

**Why this mattered:** This reframe changed how I evaluated every UI decision afterward. Features that helped me see what I already knew became lower priority. Features that surfaced what I didn't know yet became the product's actual job. The auto-sort behavior in the roster (drops float to top, on-track accounts sink) is a direct expression of this principle. So is the rule that a red number cannot appear in the client view without a human name attached to an action. I directed the AI to surface this principle by name in both the design intent doc and the build brief so it would be cited as a reason in future build decisions, not just an unstated value.

---

## Five Questions

**1. Can I defend this?**
Yes. The Browser/Detail/Controller architecture maps directly to my three columns. The single state object is auditable in the source code. Every UI decision in either user-facing screen traces to either Marcus's "5:47 PM in the truck" scenario or my "Octane Coffee Monday triage" scenario, both documented in the user portraits. I can defend the auto-sort logic, the share-button-only-on-tier-upgrade rule, the two-click budget for operator actions, and every color choice against a written spec.

**2. Is this mine?**
Yes. The agency context (Go Local Pin, the roofing contractor vertical, the GBP scoring methodology) is mine. The user portraits are mine: the laminated checklist on the dashboard, the Atlanta heat as a real design constraint, the Octane Coffee Monday morning triage scenario. The reframe ("the dashboard catches things, it doesn't display them") was surfaced in conversation but pulled forward and named by me as a design principle. The AI manufactured CSS and HTML to specifications I wrote and rejected when they drifted.

**3. Did I verify?**
Yes. Click any client in the roster (Browser) and the detail panel (Detail View) updates with that client's data. Hover over a roster row and the inline status pills appear. Click a status pill (Controller writing to state) and the roster row reflects the change with a flash animation and the client's status updates. Mark a client as Reviewed and the alert flag clears in the roster. The detail panel never holds its own copy of the data; it reads from the state object on every render. The architecture is verified, not faked.

**4. Would I teach this?**
Yes. The pattern is: one state object, three components reading from it, two of them (Browser and Controller) writing back to it. Data flows down via the state object being passed into the render functions. User actions flow up via setState calls. I could walk a classmate through any interaction and trace it: which function fires, what it writes to state, which components re-render, what the user sees. The whole system fits in one file because it is meant to be readable.

**5. Is my documentation honest?**
Yes. The AI Direction Log entries describe decisions I actually made in real sessions, not retrospective justifications. The Records of Resistance describe moments I genuinely pushed back, including the meta-resistance against my own tendency to over-prepare. The portraits credit Claude as a manufacturing partner where it acted as one and credit me as the editorial direction where I made the calls. Nothing in this documentation is invented to satisfy a rubric.

---

*Documentation authored by Pedro Febles Bula per SCAD ESF Protocol.*
*Reactive Sandbox build sessions: April 22 to April 29, 2026.*
