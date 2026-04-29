# Design Intent Document
## The Reactive Sandbox
### AI 201 | Pedro Febles Bula | SCAD Atlanta

---

## What This Is

The Reactive Sandbox is a dual-mode web application serving two distinct users from a single static HTML file. The operator interface is desktop-primary: a three-panel dashboard for monitoring and controlling multiple client accounts. The client interface is mobile-primary: a single-screen GBP Health Score display designed for one-handed use, outdoor legibility, and a 20-second interaction window. No backend or authentication is required; all state is managed client-side.

The app serves Go Local Pin, a local SEO agency that helps Atlanta roofing contractors dominate Google Maps search results. It has two users: the operator (Pedro, the agency owner) and the client (a roofing contractor who wants to know if his Google presence is working).

---

## Core Design Philosophy

The dashboard is not a place you go to check things. It is a system that catches things before they become problems.

The highest-value feature in the entire operator panel is not the roster, not the status flags, not the detail view. It is the mechanism that tells Pedro on a Tuesday that Marcus's score dropped on Monday before Marcus opens the app on Thursday. Every design decision in the operator panel flows from this.

The client panel has a parallel philosophy: the app must deliver a verdict, not a report. Marcus does not open Go Local Pin to learn. He opens it to confirm. The UI's job is to confirm in under three seconds and get out of his way.

---

## User Portraits

---

### Portrait 01: Marcus Reyes
**Go Local Pin client (roofing contractor, Gwinnett County)**

Marcus Reyes is 44, born in Marietta, been running his own roofing crew out of Gwinnett County for eleven years. His company, Reyes Premier Roofing, has five employees; three of them are cousins. He does not use the word "entrepreneur." He calls himself a roofer. He checks his Google listing the same way he checks the weather. Not to analyze it, but to confirm conditions are favorable before he starts his day. He has never opened a spreadsheet voluntarily. He has a laminated paper checklist zip-tied to his dashboard.

**Device + Physical Environment**

iPhone 14 in an OtterBox Defender case. Screen scratched across the bottom quarter from being set face-down on shingle grit. Display brightness at max. Auto-brightness dimmed the screen when he needed it most, so he turned it off permanently. Right hand only. Phone resting on the steering wheel or cupped in his palm while standing beside the truck. Direct Atlanta summer sun, April through October. Screen glare is a weekly reality, not an edge case. One AirPod in the right ear all day. He is never giving the phone his full attention.

**The Heaviest Use Moment**

Thursday, 5:47 PM. Gwinnett County job site parking lot. The crew loaded out twenty minutes ago. Marcus is still in the cab with the AC running. Been on a re-roof since 6:45 AM. His wife texted about dinner. He opens Go Local Pin from his home screen. Not because he planned to, but because he thought about it while watching his crew pull out.

He has one question: did any calls come in today? He won't say it that way. He will look at the number. If it's green, the answer is yes. If it's not green, he wants to know why before he drives home, because once he's home, he's off. If the app shows him a loading skeleton, a number he doesn't understand, or anything requiring a tap before he sees the verdict, he closes it. He doesn't retry. He texts Pedro.

**What He Actually Cares About**

Calls. Specifically calls from roofing jobs. He wants to tell his wife "we had a good week" and have something to point to. He would mention the score to another contractor the way he mentions his truck's MPG. Proof that something is working, not data. He cares that his name shows up when someone in Gwinnett searches for a roofer. He does not care what position he ranks. He cares that the phone rang.

**What He Does Not Care About**

He will never look at the Visibility sub-score breakdown. He will not read a tooltip explaining what Momentum means. He does not care about impressions, clicks, or search queries. These are marketing department words, and Marcus does not have a marketing department. The number that will make him stop reading is anything that requires him to remember what it meant last week.

**Adoption Trigger**

He keeps the app on his home screen the day his score goes up and he shows it to his wife. She says "that's good, right?" He says yes, and he means it.

**Abandonment Trigger**

He stops opening it the first week the score drops and nobody from Go Local Pin calls him to explain why before he notices it himself.

**Design Implication Table**

| User Truth | UI Decision |
|---|---|
| Screen at max brightness in direct Atlanta sun; he will not move to the shade. | Score number: minimum 80px, near-black on pure white. No gray-on-gray text anywhere in the primary view. Background must be #ffffff. Not off-white, not a surface color. |
| Right thumb, one-handed, phone cupped in palm or on the steering wheel. | Score is vertically centered in the bottom 55% of the screen. No navigation, no back buttons, nothing that requires a reach. The only optional tap (drill-down) lives in the thumb zone, not the top of the screen. |
| He calls it "my Google listing." He has never read a local SEO article. | All copy reads "Google Presence Score" or "your Google listing"; never GBP, CTR, impressions, or NAP. Sub-factor labels must be followed by plain-language one-liners, not metric names. |
| He checks infrequently and returns to the app cold each time. | Full meaning communicated in 3 seconds of load, every visit: color, number, one sentence. No onboarding state, no "what's new" modal, no UI that requires memory of last week's session. |
| He stops trusting the product the moment the score drops without a proactive explanation. | Score decreases must surface a contextual note automatically: "Your score dropped 4 points. Pedro reviewed this Monday. Check-in scheduled Friday." The app cannot show a red number without a human name attached to an action. |
| The adoption flywheel is one visible win shown to one other person. | A share action appears only when the score hits a new tier high. It generates a screenshot (score, green color, Go Local Pin attribution) sized for an iMessage. No share UI exists at other moments; it would feel like a request to do marketing, which he won't do. |
| He will not go looking for insights; they have to surface. | Drill-down is a single large tap below the score circle. Not a "Details" link, not a chevron in a nav bar. If he misses it entirely, the primary experience is still complete. The drill-down is dessert, not dinner. |

---

### Portrait 02: Pedro Febles Bula
**Go Local Pin operator (agency owner, sole operator)**

Pedro is 19, a SCAD freshman running a local SEO agency out of Atlanta with no employees, no office, and a client roster he is actively building. He is the product, the account manager, the data pipeline, and the support line simultaneously. He is not managing accounts from a stable base of operations. He is building the plane while flying it. Every friction point in the operator panel costs him disproportionately more than it would cost a five-person agency, because there is no one to hand the friction to.

**Device + Physical Environment**

15-inch MacBook Pro, two hands, trackpad. He works at Octane Coffee on the Beltline or from his SCAD dorm. At Octane: corner table, ambient noise, AirPods Pro in noise-cancellation mode, one cold brew, laptop on the table with nothing else on it. Coffee shop sessions are for decisions: 90 clean minutes before the first class. Desk sessions are for building. The operator panel should be optimized for the coffee shop triage session. That is when the dashboard earns its value.

**The Heaviest Use Moments**

There are three distinct moments with different cognitive states and different demands on the UI.

*Monday morning triage (heaviest, most time-pressured)*

7:45 AM. Octane Coffee. 90 minutes before the first class. Pedro has not touched client data since Friday. He opens the dashboard with one question running across all six accounts simultaneously: who needs something from me before Friday? He is not reviewing performance. He is triaging. If the roster does not let him complete that triage in under 90 seconds, he will do it from memory instead. Memory is where client accounts fall through the cracks.

*Mid-week status check (lighter, interruptible)*

Wednesday afternoon, between classes. Five minutes, not ninety. He is confirming that Monday's actions are reflected in the data and checking whether any new flags have appeared. This session is interruptible. He does not expect to make decisions here, only to confirm the week is on track.

*Client onboarding (slowest, highest stakes)*

Pedro is sitting across from a new contractor walking them through the dashboard for the first time. This is the moment where density in the operator panel becomes a liability. If Marcus sees six panels of data he doesn't understand, he is not impressed, he is skeptical. The operator panel needs a client view mode: the ability to navigate directly to a clean single-client score view without exposing the full operator triage layer during onboarding.

**What He Actually Cares About**

Speed of decision, not completeness of data. He needs to know which accounts require action and what that action is. A dashboard that shows him everything equally is harder to use than one that surfaces the two accounts that are off-track and deprioritizes the four that are fine. He also cares about Go Local Pin looking credible. The single-client view is shown to clients during onboarding and check-in calls. If the UI looks like a spreadsheet dressed up as an app, it undermines the pitch.

**What He Does Not Care About**

He does not need the operator panel to generate reports. BrightLocal handles reporting. He does not need twelve-month trend charts. He is deciding what to do this week, not presenting to a board. He does not need tooltips explaining the scoring methodology. He built the scoring system. Any UI element that exists to explain the data to him is wasted space. That layer belongs in the client-facing view.

**Adoption Trigger**

He commits to the dashboard as his primary operational tool the first Monday morning it surfaces a client flag he had forgotten about, and he catches it before the client notices.

**Abandonment Trigger**

He stops using it as a triage tool and reverts to a manual spreadsheet the first time he spends more than five minutes in the dashboard trying to answer a question the roster should have answered in five seconds.

**Design Implication Table**

| User Truth | UI Decision |
|---|---|
| Monday triage must complete in under 90 seconds across six accounts. | The roster is the primary view on load. Rows are sorted automatically by urgency: score drop this week floats to top, then unanswered reviews, then "last updated" staleness. Green accounts sink. Pedro never manually reorders. |
| He is triaging, not studying; he needs to identify which accounts need action without opening the detail panel. | Each roster row shows: client name, current score, week-over-week delta with direction arrow, one alert flag in plain language ("1 unanswered review"), and last-updated timestamp. Nothing else lives in the row. |
| Status changes need to happen from the roster, not inside the detail panel. | "Mark as reviewed," "needs attention," and "on track" are inline actions accessible directly from the roster row. Opening the detail panel to change a flag is one unnecessary navigation per client (six unnecessary navigations per triage session). |
| Marcus's abandonment trigger is a score drop with no human accountability. Pedro is that accountability. | When the operator marks a score drop as "reviewed," the client-facing view automatically updates to show "Pedro reviewed this [day]. Check-in scheduled [day]." Reviewing an alert in the operator view is what prevents the client from hitting their abandonment trigger. |
| The stack is GoHighLevel, BrightLocal, Local Falcon. The dashboard is not a replacement for any of them. | The operator panel links out to source tools rather than replicating them. "View in Local Falcon" and "Open in BrightLocal" are link actions, not embedded panels. The dashboard's job is decision-making, not data storage. |
| Client onboarding requires showing the product without exposing the full operator triage layer. | A "client view" toggle exists in the operator panel. It switches to the single-score mobile-style view for the currently selected client. It is not a separate URL; it is a view state the operator controls. |
| He is a solo operator; friction multiplies across every account it touches. | Any action requiring more than two clicks to complete is too expensive. The two-click budget is not a design preference. It is a solo-operator survival constraint. |

---

## Named Design Principles

These principles are derived directly from the portraits. They are named so they can be cited when making UI decisions.

**The Verdict Principle**
The client UI must deliver a verdict in under three seconds. Color, number, one sentence. No scrolling, no tapping, no memory of last week required. If a design element does not serve the verdict, it does not belong above the fold.

**The Alert System Principle**
The dashboard's highest-value function is catching problems before users notice them, not displaying data they already know. Any feature that only shows Pedro what he already knows when he opens it is failing at the product's core job.

**The Two-Click Budget**
Any operator action requiring more than two clicks is too expensive for a solo operator managing six accounts simultaneously. This is a hard constraint, not a preference.

**Dessert, Not Dinner**
Drill-down detail is optional enrichment for users who want it. The primary experience must be complete without it. If a user misses the drill-down entirely, they should still leave informed.

**Human Name on Every Red Number**
A score decrease shown without a human name attached to an action breaks client trust immediately. The UI cannot surface a negative state without also surfacing what the agency is doing about it.

---

## Data Architecture + Backend Rationale

All data in this prototype is statically defined in a centralized JSON object that simulates a real GBP data pipeline. In production, this object would be populated by the Google Business Profile API via a scheduled ingestion layer. The prototype demonstrates the complete UI logic and data relationship without requiring live API credentials or a backend service.

The operator-mediated data model (agency pulls data, client receives score) is architecturally correct for a managed-service product. The client never needs a live API connection. They need an accurate score refreshed by the operator on a weekly cadence. The operator is the backend. That is not a limitation of the prototype; it is the correct service design for a managed agency.

**What a production backend would look like:**
A scheduled job (cron or serverless function) calls the GBP API for each client account and writes results to a database. A lightweight API serves the dashboard. An authentication layer ensures each contractor only sees their own score. GoHighLevel and BrightLocal together can approximate much of this without custom infrastructure.

---

## Feedback + Juice Planning

Three moments in the app where feedback serves the user, ranked by meaning, not visual impact.

**1. Score tier change: the celebration moment**

When a contractor's score crosses a tier threshold for the first time (Fair → Good, Good → Excellent), this is the moment the service feels worth it. A confetti burst over the score circle via `canvas-confetti` (CDN, one script tag). A haptic pulse via the Vibration API on mobile. A line that reads "You hit Good for the first time. 🏆" The share action (a screenshot sized for iMessage) appears only at this moment. This is the show-it-to-your-wife moment. The UI has to recognize it.

**2. Week-over-week score increase: usage reinforcement**

Every time Marcus opens the app and his score is higher than last week, a subtle upward animation plays on the score arc: the needle sweeps to the new position rather than snapping. A small "+4 this week" badge appears next to the number. Not confetti. Acknowledgment. It keeps him checking because checking feels good. Technically: CSS `stroke-dashoffset` transition on an SVG circle, triggered by a class swap in the centralized state update function.

**3. Operator status change: workflow confirmation**

When the operator changes a client from "Needs Attention" to "On Track," the row color transitions, a subtle checkmark flashes, and a single clean tick sound fires. Functional feedback, not celebration. It confirms the action registered so Pedro does not second-guess himself across six accounts. Technically: a CSS keyframe animation on the row background-color, fired at the state update function.

All three are achievable without frameworks inside a single `index.html`. Every state change flows through one centralized function. Feedback calls are added there.

---

## Adoption vs. Usage

**The adoption case**

Marcus does not adopt dashboards. He adopted Go Local Pin because Pedro told him it would get him more calls from Google. The app's job is not to impress Marcus. It is to maintain his trust in the service. He opens it the first time because Pedro showed it to him during onboarding. He keeps it on his home screen because the score went up after the first month and he showed it to his wife. The adoption flywheel is one visible win shown to one other person. If the app requires a tutorial to understand, adoption stalls.

**The usage case**

He checks the score. If it's green, he closes it and feels good. If it's yellow or red, he might tap through to see why, but only if that path is obvious and fast. He will never go looking for insights. Insights have to surface. The full usage loop for the client is: one score, one status line, one optional drill-down. Design for the 20-second interaction, not the 5-minute review session.

---

*Design Intent Document | The Reactive Sandbox | Pedro Febles Bula | Go Local Pin / Febles Digital | SCAD Atlanta Class of 2029*
