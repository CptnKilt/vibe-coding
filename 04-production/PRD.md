# Living PRD

> Module 4 · Production Specs. Refactor for readability; extract a living PRD that stays true as the build evolves.

## Problem

_What user problem does this solve? Tie to the validated hypothesis._

New B2B SaaS accounts leave before they see value, and the value they do see stays with one person.

Evidence (from the brief)	Figure	What it says
90-day churn	30%	Nearly a third of new accounts are gone within three months.
Seats active per account	1.4	Adoption rarely spreads past the buyer.
Week-one activation	22%	Fewer than one in four accounts reach the "aha" action in their first week.
Two customer voices describe the two halves of the problem:

"I signed up, poked around for ten minutes, and never figured out what it actually did for my team." — Ops lead, churned day 12. The product does not show its value in the first session.
"Nobody on my team adopted it, so I stopped logging in. It felt like one more tool to babysit." — Eng manager, churned day 47. Value never reaches a second person, so the buyer carries the tool alone.
Hypothesis under test (not yet validated): We believe a guided week-one activation path that surfaces the core "aha" action will cause more new accounts reaching first value and staying past 90 days, for new B2B SaaS accounts. We'll know we're right when week-one activation rises above 40% and 90-day churn drops.

How the problem ties to the hypothesis. The prototype turns the hypothesis into one measurable event. The "aha" action is: share a retention plan and have a teammate claim or complete its first action. This definition was chosen deliberately so that a single event answers both quotes: the buyer sees what the product does for their team in the first session (day-12 quote), and the account only counts once a second person has acted (day-47 quote, 1.4 seats).

What the prototype can and cannot say about it.

It can show whether testers reach the aha action when guided, where they drop out, and what happens when the teammate does not respond.
It cannot measure 90-day churn. Activation and seats invited stand in as leading signals; the link from activation to retention is an assumption.
Its activation rate comes from prototype sessions in which the teammate's response is clicked by the tester. It is not comparable to the 22% baseline until real teammates respond.

## Users & jobs

- **Primary user:** The buyer of a new account in its first week — a customer success lead or manager (the "Ops lead" / "Eng manager" in the quotes) who signed up, is worried about a specific customer, and has not yet got a teammate using the product.
- **Job to be done:** When I have just signed up and one of my customers is at risk, I want to turn that worry into a plan a teammate actually picks up, so that the customer gets help this week and I am not the only one using the tool.

## Scope

- **In:** In scope (built in the prototype)
Area	What exists
Brief	Hypothesis, success line, three metrics and both quotes shown on screen as content.
Welcome	Tester chooses a customer (from the dashboard's sample list, or types a name), then what is going wrong: stalled onboarding, only the buyer using the product, or a champion gone quiet. No run starts without a customer.
Week-one path	One pinned next step, a day marker, and a "skip, explore on my own" exit that is recorded as a failure path.
Share a plan (aha action)	Pick a plan template, name a teammate, share. Success state names the teammate and the first action they now own.
Teammate response	Two clickable outcomes: the teammate claims the task, or does not. Nothing happens automatically.
Unclaimed Plan Recovery	Remind, reassign, revise the action, or take ownership yourself. Account stays Not activated until a teammate claims or completes.
Spread to the team	Invite up to two more teammates with a suggested role and ready-made task.
Customer Outcome Tracking	Account list with plan progress; per-account plan, owner, due date and the customer measure at 7, 14, 30 and 90 days. Skeleton, empty and error states.
Readout	This run's result plus a tally of ended runs against the 40% target and 22% baseline. Starts at zero.
Assigned tasks	Each person sees their own tasks; an admin sees all; an incomplete task can be marked complete. Tasks shared in the prototype appear here, and completing one activates that run.
Early churn dashboard	At-risk accounts ranked by health score, churn drivers, account panel with health breakdown.
AI retention plan builder	In the dashboard's account panel: a model writes a plan from signals and notes the user supplies.
- **Out (explicitly):** Sign-in, sign-up, accounts and roles. Deferred by the product owner. Decisions already taken for later: email and password, a stored name, the whole site behind sign-in, david_mccormack@outlook.com as first admin, admins only may add and assign tasks.
Real teammates. No invitations, reminders or notifications are sent to anyone.
Real customer data. No connection to a CRM, product usage events, onboarding checklist or integration logs.
Measuring 90-day churn or retention.
Shared results. Runs are stored per browser; there is no combined view across testers.
Analytics. No events are sent anywhere.
Admins creating or assigning tasks in the tasks page.
Phone navigation to the tasks page (reachable by address only on small screens).
Billing, plan tiers, permissions beyond user/admin.

## Requirements

| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 1 | A new account is guided to one next step in its first session | Must | After choosing a customer and a situation, the user sees a single pinned next action; no empty workspace is shown first. |
| 2 | The user chooses the customer the plan is for. | Must | A customer can be picked from a list or typed. A run cannot start without one. The chosen name appears on every later screen. |
| 3 | The user can share a retention plan with a named teammate. | Must | Sharing requires a template and a teammate name. The success state names the teammate and the first action they own. Sharing alone does not mark the account activated. |
| 4 | An account is activated only when a teammate claims or completes the first assigned action. | Must | Activation is recorded at the moment of claim or completion. Sharing, reminding, reassigning, revising, or the buyer doing the task never activate. A claimed task can later be completed. |
| 5 | The teammate's response is never assumed. | Must | No timer or default produces a claim. An unanswered task stays Not activated. |
| 6 | When the teammate does not claim, the buyer is offered a next move. | Must | Remind, reassign (to a different name), revise (to different wording) and take ownership are offered. The account is clearly marked Not activated throughout. Taking ownership ends the run as not activated. |
| 7 | Leaving the guided path is recorded. | Must | Choosing "skip" before sharing ends the run as not activated and it counts against the rate. |
| 8 | The readout reports activation honestly. | Must | Tally starts at zero. A run is added only when it is over (finished, skipped, taken on yourself, or abandoned when a newer run starts). The run in progress shows "not counted yet". Rate = activated runs ÷ all counted runs. |
| 9 | The result is judged against the stated line. | Must | "Above target" only when the rate is strictly above 40%; between 22% and 40% is reported as better than baseline but short of target; 22% or below as not better. 90-day churn is shown as not measurable here. |
| 10 | A shared first action appears as a task for the teammate. | Must | The task appears for the assignee and for admins with customer, plan, and due date. Marking it complete counts as the teammate completing it and activates the account. A task the buyer took over is not listed for the teammate. |
| 11 | Users see only their own tasks; admins see all. | Must | A non-admin's list contains only tasks assigned to them. An admin's list contains every task. Only an incomplete task can be marked complete, by its assignee or an admin. |
| 12 | Each plan is followed to its due date against a customer measure. | Should | For each shared plan: plan, owner, due date, and the measure at 7, 14, 30 and 90 days (setup steps, active seats, or champion engagement). Each checkpoint is Met, In progress or Behind. Sample figures are labelled as sample and separated from real results. |
| 13 | Outcome tracking has defined loading, empty and error states. | Should | Loading shows skeleton placeholders. Empty shows "No customer outcomes yet. Accounts will appear here once a retention plan is shared and progress is recorded." with "Create a retention plan". Failure shows "We couldn't load customer outcomes right now. Please try again." with "Retry". |
| 14 | The buyer can bring more teammates in. | Should | Up to two further teammates can be invited with a suggested role and first task; invited seats are counted in the run. |
| 15 | Sign-in and accounts. | Should | Email and password; a name stored per account; the whole site requires sign-in; first admin is the named address; roles are stored separately from user-editable details. |
| 16 | Task rules are enforced by the backend. | Should | The same rules as R11 hold when the page is bypassed. |
| 17 | Real teammates are notified and respond themselves. | Should | Share, remind and reassign each deliver a message to the teammate; the teammate claims or completes from their own account; R4 is recorded from that action. |
| 18 | Admins can add and assign tasks. | Should | Only admins see the control; a new task appears for its assignee. |
| 19 | Results are shared across testers and devices. | Should | Runs are stored centrally; the readout shows all sessions, and can be filtered by date. |
| 20 | Outcome measures come from real sources. | Later | Each checkpoint value is read from its named source (onboarding checklist, usage events, CRM) and labelled Real. |
| 21 | The shared plan is written from the customer's own signals. | Later | The plan in the activation path is generated from that customer's data, with each action citing the evidence it rests on. |
| 22 | 90-day retention is measured per cohort. | Later | Activated and non-activated accounts can be compared on 90-day retention. |
| 23 | Tasks page is reachable on phones. | Later | The menu entry is available at small screen widths. |

## Data & events

_What gets stored, what gets tracked._

Data held today
Record	Fields	Where it lives	Real or mocked
Run (one new-account session)	id, customer, situation, started time, plan shared time, activation time, teammate response (claimed / completed / none), template, teammate name, revised action, recovery moves taken, taken-on-yourself flag, seats invited, skipped flag, ended time	The tester's browser	Real, per device
Sample accounts	name, segment, onboarding, adoption, engagement, integrations, churn driver, recent activity	In the app's code	Mocked
Outcome measure (per situation)	measure, source name, start value, target, due day, values at day 7/14/30/90 for teammate-owned, self-owned and unclaimed plans	In the app's code	Mocked
People	name, role (admin / user)	In the app's code, plus teammates named in runs	Mocked
Tasks	title, detail, customer, assignee, due date, completed date, originating run	Sample list in code; shared-plan tasks derived from runs; completions saved in the browser	Mixed
Profiles, roles, tasks tables	—	Backend	Created, empty, unused
Derived measures
Activation rate = activated runs ÷ all counted runs. Target: strictly above 40%. Baseline: 22%.
Time to aha = activation time − run start.
Seats active = buyer + first teammate (only once they act) + seats invited.
Shared, not activated = runs where a plan was shared and no teammate acted.
Outcome status at a checkpoint: Met when the measure reaches target; In progress before the due day; Behind after it.
Events
No analytics events are emitted today. The prototype records the moments below as changes to the run; these are the events a real build should send.

Event	Fires when	Properties	Recorded in prototype
run_started	Customer and situation chosen	customer, situation	Yes
path_skipped	"Skip, explore on my own" chosen before sharing	—	Yes
plan_shared	Plan shared with a teammate	template, teammate	Yes
first_action_claimed	Teammate claims — activation event	seconds since start	Yes (simulated)
first_action_completed	Teammate completes, in the flow or from the tasks page — activation event if not already activated	seconds since start, where completed	Yes (simulated)
first_action_unclaimed	Teammate does not claim	—	Yes (simulated)
recovery_move_taken	Buyer reminds, reassigns, revises or takes ownership	move, new teammate or new wording	Yes
teammates_invited	Buyer finishes the team step	count (0–2)	Yes
run_ended	Finished, skipped, or taken on yourself	reason	Yes
run_abandoned	A newer run starts while this one is open	—	Inferred, not timestamped
outcome_checkpoint_recorded	Day 7/14/30/90 measure read	measure, value, status	No (sample values only)
task_completed	A task is marked complete on the tasks page	task, by whom	Sample tasks: saved locally; shared-plan tasks: yes
retention_plan_generated	AI plan builder returns a plan	account, score	No event recorded

## Open questions

1. Is the hypothesis validated? Not by this prototype. What evidence counts — how many sessions, with whom, over what period — before the 40% line is called met or missed?
2. Where do the baselines come from? Is 22% measured against the same aha action defined here (teammate claims or completes)? If the old definition was looser, the comparison is unfair to the new path.
3. Does a claim count the same as a completion? Today either activates. A claim with no follow-through may not be "first value".
Is "week one" enforced? The prototype shows a day marker but does not expire a run after seven days.
4. Should abandoned runs lower the rate? Currently yes (confirmed). In a real product, when is an unfinished first week declared abandoned?
5. How long is "unclaimed"? With real teammates, how much time passes before recovery is offered — hours, a day?
6. Taking the task yourself: it helps the customer but leaves the account not activated. Is that the right trade-off, or does it discourage the buyer from helping?
7. How will 90-day churn be measured, and how will activated accounts be compared with non-activated ones fairly?
8. Outcome targets and due days (6 of 8 setup steps by day 14; 8 of 20 seats by day 14; a decision-maker at a review within 10 days) are invented. What are the real ones, and which systems supply them?
9. Which plan does the real path share — the fixed templates, or the AI-written plan from the dashboard? If AI, who reviews it before a teammate is assigned work?
10. How are teammates reached — email, in-app, chat — and can someone without an account claim a task?
11. Who is the first user exactly? The quotes describe an ops lead and an engineering manager; the prototype is written for customer success.
12. Which one is being tested?
13. Sign-in timing: the whole site is to sit behind sign-in. Does that apply to test sessions too, and does it change the first-session experience being tested?
14. Do the three situations cover the real reasons new customers are at risk, and should each customer's own figures replace the fixed sample lines?
15. Health score weights and risk bands are illustrative. Do they need validating before the customer picker ranks by them?
