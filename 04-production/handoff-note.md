# Engineering Handoff Note

> Module 4 · Production Specs. Open the black box, make the build legible to an engineer.

## What this is

_One paragraph an engineer can read in 60 seconds._

The Retention Engine is a React 19 / TanStack Start app combining a fictional early-churn dashboard, a live AI retention-plan builder, a guided week-one activation prototype, and an assigned-tasks page. Its core experiment defines activation strictly: sharing a plan is not enough; a teammate must claim or complete its first action. Most product rules are centralized in pure, tested TypeScript modules, with thin route files and feature-based screens. The important limitation is that almost everything is local and simulated: customers and outcome figures are sample data, prototype runs and task completions live in browser localStorage, the tester clicks the teammate’s response, and no invitations or analytics are sent. Lovable Cloud tables, role rules, and task policies exist but are unused because sign-in was deliberately deferred. The AI builder is the exception: it makes a real server-side model call, but it is separate from the prototype’s templated plans. The hypothesis remains untested, and 90-day churn is not measured.

## Architecture (plain language)

- **Frontend:** Framework: TanStack Start v1, TanStack Router, React 19, TypeScript, Vite, Tailwind CSS v4, and Radix-based shared UI primitives. Rendering: TanStack Start SSR with React Query context. Most screens are client-interactive; browser storage is read after mount rather than during SSR. Routing: src/routes/*.tsx files are thin adapters that own URLs and page metadata. Product screens live outside the router and never import route definitions. Feature boundaries: src/features/   early-churn/      Dashboard, account health panel, health scoring   activation/       Guided activation, recovery, outcomes, readout   assigned-tasks/   Task visibility, completion, viewer state   retention-plan/   AI plan form, schemas, request wrapper, server call  src/routes/         URLs and head metadata only src/components/ui/  Shared design-system components src/integrations/   Generated Lovable Cloud clients and auth middleware Within each feature, screens/ and components/ handle display, data/ holds browser-safe content and rules, and hooks/ owns persistence or request state. Model tests sit beside the data modules they exercise.
- **Backend / data:** Layer	What happens today Browser storage	Prototype runs use retention-engine-prototype-v2; sample-task completions use retain-task-completions-v1. Data is device- and browser-specific, with no server synchronization. The older retention-engine-prototype-v1 session is removed on load so the readout starts honestly from zero. Sample domain data	Forty-two fictional accounts, health inputs, plan templates, people, tasks, and outcome checkpoints are TypeScript data in feature modules. Health scoring and activation calculations are real logic operating on sample inputs. Lovable Cloud database	profiles, user_roles, and tasks tables exist. The migration includes RLS, an admin/user enum, a role-check function, signup/profile triggers, first-admin assignment after email confirmation, admin-only task writes, and a constrained task-completion function. No app screen currently reads or writes these tables. AI Gateway	createRetentionPlan is a POST server function. It validates input with Zod, calls @lovable.dev/ai-gateway from a server-only helper, requests strict JSON Schema output, validates the result again, and maps common failures to usable errors. Model credentials stay server-side. External systems	No CRM, product analytics, onboarding checklist, email, notification, or customer-outcome source is connected. No product analytics events are emitted. The published server target is a Cloudflare Worker-style runtime. Keep Node-only APIs out of server functions and SSR modules; model calls belong in *.server.ts behind a server function.
- **Key flows:** 1. Early-churn dashboard (/)

EarlyChurnDashboard filters and ranks fictional accounts using the shared health model. Selecting an account opens AccountHealthPanel, which shows the score breakdown, signal sources, churn reason, and recent activity. The same shared calculations feed summary and detail views so they cannot drift apart.

Health score weights are onboarding 35%, adoption 35%, engagement 20%, and integrations 10%. Risk bands are Critical below 40, At risk from 40–64, and Healthy from 65 upward. These weights and threshold-evidence figures are illustrative, not validated.

2. Guided activation (/prototype)

Brief → Welcome → WeekOnePath → ShareAPlan → TeammateResponse
                                      │             │
                                      │             ├─ claim/complete → activated
                                      │             └─ no claim → UnclaimedPlanRecovery
                                      │                                      │
                                      │                                      └→ CustomerOutcomeTracking
                                      └→ SpreadToTheTeam → Readout
Welcome requires a customer selected from the dashboard’s sample list or typed manually, plus one of three situations.
Sharing records sharedAt; it does not activate the run.
A clicked teammate claim/completion records ahaAt, the actual activation event. Nothing happens automatically.
Remind, reassign, revise, and buyer self-ownership do not activate. Self-ownership ends the run as not activated.
Skip, no-claim, and abandoned runs count against the activation rate once over. The current in-progress run is excluded.
The target is strictly above 40%; the supplied baseline is 22%. Outcome checkpoints at days 7, 14, 30, and 90 are fixed sample values and labelled accordingly.
3. Shared plan → assigned task → activation (/tasks)

Shared prototype plans are derived into tasks for the named teammate. An admin viewer sees all tasks; a regular viewer sees only their own. Completing a shared-plan task calls completeFirstAction, updates the originating run, and activates it if it was not already activated. Sample-task completions are stored separately in localStorage.

This flow is real local state logic, not multi-user behavior: the “Viewing as” picker lets anyone impersonate any viewer.

4. AI retention-plan generation (dashboard account panel)

The form supplies account signals and activity notes to the server function. The model is instructed to use only supplied facts and return evidence-cited actions. The response is schema-validated before display. This live AI path is not connected to the activation prototype, which uses fixed plan templates.

## What's solid vs. what's duct tape

| Area | State | Notes |
|---|---|---|
| •	One source of truth for important rules. •	Honest activation semantics. •	Feature boundaries and thin routes. •	AI boundary and validation. •	Designed state handling. •	Regression coverage for core rules. •	Basic server protections. | solid | •	Activation, tallying, outcome states, health scoring, and task permissions are pure shared functions rather than duplicated screen logic. •	Sharing never activates; claims require an explicit click; unfinished runs stay out of the tally until over; skipped and abandoned runs count against the rate. •	Screens match PRD names, data is separated from display, and generated routing/integration files remain isolated. •	The model call is server-only, input and output are validated, structured output is requested, and refusal/rate-limit/credit failures have explicit UI errors. •	Outcome tracking has distinct loading, empty, error, and populated states, including Retry and Create-a-plan actions. •	Five test files contain 40 passing tests covering health calculations, activation rules, task rules, AI schemas/prompts, and root routing. •	Server functions have CSRF middleware; generated auth attachment middleware is present for future authenticated functions. Database task and role policies are already expressed in the unused migration. |
| •	LocalStorage is the system of record. •	Identity is simulated. •	Names become identities. •	Tasks have two persistence paths. •	Outcomes and customer situations are scripted. •	Notifications are copy, not delivery. •	Two disconnected plan generators. •	Experiment measurement is incomplete. •	Mobile navigation has a gap. •	End-to-end coverage is not committed. | rough | •	Clearing browser data, switching browsers, or using another device loses visibility into runs and completions. Storage write failures are mostly swallowed, so the UI can imply persistence when none occurred. •	Viewer switching is not authentication or authorization. The database policies do not protect the current page because the page never calls the database. •	Prototype teammates are matched to sample people by full name or first name; unmatched names become synthetic IDs. Duplicate first names and renamed teammates can produce incorrect assignments. •	Prototype-task completion updates a run; sample-task completion updates a separate localStorage map. This is adequate for a demo but not a durable task model. •	figures are reused for every selected customer, and outcome values are fixed arrays rather than observed measurements. •	 “Notified,” reminders, reassignment, revised actions, and invitations only alter local state; no message reaches another person. •	The activation flow uses templates while the dashboard uses AI. A production flow must choose how generated plans are reviewed, shared, and audited. •	 “Week one” is displayed but not enforced with a seven-day deadline. There is no cross-tester aggregation, event stream, cohort analysis, or real 90-day retention measurement. •	The tasks page is reachable by URL on small screens but lacks a small-screen navigation entry. •	Browser checks were performed during development, but there is no automated browser suite. The revise-recovery path specifically remains without a recorded click-through verification. |

## Risks & assumptions for the team

Priority	Risk or assumption	Engineering implication
High	The hypothesis is not validated. The 30% churn, 1.4 active seats, 22% activation baseline, customer quotes, health weights, and threshold evidence were supplied or invented for the prototype.	Do not present the readout as business evidence. Define a real experiment, consistent activation baseline, sample size, and retention cohort before drawing conclusions.
High	There is no security boundary in the live UI. The whole site is open and task viewers can be switched freely.	Add authentication before real customer or employee data. Enforce the same visibility/completion rules through backend RLS and authenticated server calls; never rely on hidden buttons or viewer state.
High	The AI generation function is unauthenticated. CSRF middleware does not provide user identity, quota management, or abuse prevention.	Require auth and authorization before production use; add per-user/workspace limits, monitoring, and cost controls.
High	Simulated teammate responses can inflate activation. The tester currently performs both sides of the interaction.	Replace simulation with authenticated teammate actions and notification delivery before comparing results with the 22% baseline.
Medium	Local data is fragile and unobservable. Runs cannot be aggregated, audited, recovered, or trusted as a complete experiment dataset.	Move runs, tasks, and event timestamps to backend storage; add an explicit event schema and idempotent writes. Keep local drafts only as a cache if useful.
Medium	Storage errors are not consistently surfaced. A failed write can leave in-memory state looking successful until reload.	Return persistence status from hooks, show a durable save failure, and add retry/recovery behavior.
Medium	Claim and completion are treated as equivalent activation. A claim without follow-through may not represent first value.	Product must confirm the activation definition; if they differ, model them as separate milestones and choose which one drives the success metric.
Medium	AI input may contain sensitive customer notes. The live model path sends entered evidence to the configured model provider through AI Gateway.	Establish data-handling and retention requirements before using real customer information; avoid confidential notes in the prototype.
Medium	Backend schema and frontend model are not yet proven together. The migration exists, but signup triggers, role assignment, RLS, and task completion have not been exercised through the app.	Add integration tests against authenticated admin and non-admin users before wiring the UI to the tables.
Low	Time and date behavior is prototype-grade. Dates use the browser’s local timezone and sample tasks contain fixed 2026 dates.	Use server timestamps and an explicit product timezone for real due dates, week-one windows, and cohort reporting.
Low	There is no automated full-stack release gate in the repository. Unit tests do not prove browser persistence, AI availability, email confirmation, or cross-screen behavior.	Add browser E2E tests for the golden paths and failure states, plus authenticated backend integration tests when sign-in lands.
Product assumptions to preserve unless deliberately changed
Activation means a teammate, not the buyer, claims or completes the first assigned action.
Sharing, reminders, reassignment, revision, self-ownership, and invitations never activate by themselves.
No automatic claim or default response is allowed.
A run enters the tally only when over; a newer run makes an older open run abandoned and counted as not activated.
“Above target” means strictly greater than 40%, not equal to 40%.
Sample data must remain visibly labelled as sample; simulated results must not be described as live customer outcomes.
Sign-in was intentionally deferred. The intended future shape is email/password, stored display names, whole-site authentication, admins creating tasks, users seeing their own tasks, and admins seeing all tasks.

## How to run it

```
Prerequisites
Bun and a current browser.
The repository’s checked-in bun.lock should be used for reproducible dependency installation.
No environment setup is needed for the fictional dashboard, activation prototype, or local task interactions.
AI generation requires the Lovable AI Gateway runtime and available workspace AI credits. It is not a standalone offline model integration; running the repository outside that environment requires equivalent Gateway configuration.
Install and start
bun install
bun run dev
Vite prints the local URL. In the managed development environment it is http://localhost:8080.

Useful URLs:

Area	URL
Early-churn dashboard and AI plan builder	/
Activation brief	/prototype
Start a guided run	/prototype/welcome
Customer Outcome Tracking	/prototype/outcomes
Readout	/prototype/results
Assigned tasks	/tasks
Test and check
bunx vitest run   # all automated tests
bun run lint      # ESLint
bun run build     # production build
bun run preview   # serve the production build locally
Verified for this handoff: bunx vitest run passes all 40 tests across 5 files. No new build or browser test was run while producing this document.

Manual smoke test
Open /, filter at-risk accounts, open an account, and inspect its health breakdown.
In that account panel, submit the AI plan form and confirm either a valid evidence-cited plan or a clear error.
Open /prototype/welcome, choose or type a customer, select a situation, and share a plan with a named teammate.
Confirm sharing alone leaves the run not activated; click the teammate claim and confirm activation.
Repeat with “doesn’t claim,” then exercise remind, reassign, revise, and self-ownership separately.
Open /tasks, switch between admin and user viewers, complete a shared-plan task, and confirm its originating run activates.
Check /prototype/outcomes for loading, empty, error, account-list, checkpoint, and Sample labels.
Check /prototype/results: the in-progress run is excluded, ended/abandoned runs are counted, and only a rate strictly above 40% is above target.
Reset local prototype state
In that browser’s developer tools, remove these localStorage keys and reload:

retention-engine-prototype-v2
retain-task-completions-v1
This resets only the current browser. There is no shared server state to clear for these flows.
```
