# Full-Stack: Data, Access Rules, Edge Cases, Deploy

> Module 5 · Full-Stack. Add data schemas, access rules, and edge cases; stress-test and deploy.

## Deployed link

_The working, shareable link that survives real users._

https://acme-retention.lovable.app

## Stress test results

I tried creating multiple accounts with the same email address. The site would not let the additional user be created.

## Data schema

| Entity |	Key fields | Notes |
|---|---|---|
| profiles |	id, display_name, email, created_at |	One per account, created automatically at sign-up. You read your own; admins read all. Not editable from the app yet. |
| user_roles |	id, user_id, role (admin / user) |	Kept separate from profiles so nobody can give themselves a role. Everyone gets user at sign-up; the first admin is granted on email confirmation. You read only your own roles.
| customers |	id, name, segment, days_since_signup, mrr, churned, churn_driver, onboarding, adoption, engagement, integrations, owner_name, is_sample, created_by |	Mirrors the dashboard's accounts. The four signals are 0–100; the health score is calculated, not stored. Everyone signed in can read; only admins add, edit or delete. Empty today: the dashboard still uses sample accounts from code. |
| activation_runs |	id, created_by, customer_id, customer_name, goal, template, teammate_name, first_action, started_at, shared_at, response, activated_at, invited, skipped, self_owned, ended_at |	One pass through the prototype. activated_at can only be set when a plan was shared and a teammate claimed or completed it. You see your own; admins see all. Empty today: runs are still saved in the browser. |
| recovery_moves |	id, run_id, move (remind / reassign / revise / self), detail, created_at |	What was tried after a teammate didn't claim a task. Access follows the parent run. Cannot be edited once added. |
| outcome_checkpoints |	id, run_id, checkpoint_day (7 / 14 / 30 / 90), measure, value, target, source (recorded / sample / real), recorded_at |	Progress on a plan at each checkpoint. source keeps sample figures apart from real results. Access follows the parent run. |
| retention_plans |	id, created_by, customer_id, customer_name, signals, activity_notes, plan, created_at |	A saved AI-generated plan with the inputs behind it. You see and delete your own; admins see all. Cannot be edited. Empty today: generated plans are shown but not saved. |
| tasks |	id, title, details, due_date, assigned_to, created_by, completed_at, customer_id, run_id, created_at |	You see tasks assigned to you; admins see all. Only admins add, edit or delete; an assignee can only mark their own task complete. Empty today: the tasks page still shows sample tasks. |
| invites |	id, user_id, sender_email, recipient_email, status (pending / accepted / declined / expired), timestamp |	The only table the app reads and writes live; it feeds the four dashboard cards. You see only invites where user_id is you; admins see all. No self-invites, no duplicate recipient per person. An older sender_id field remains but is retired. |

## Access rules

_Who can see / do what? Where are the auth boundaries?_

Entity	Read	Add	Edit	Delete
profiles	Your own; admins all	No one (created at sign-up)	No one	No one
user_roles	Your own only	No one (set at sign-up)	No one	No one
customers	Everyone signed in	Admins	Admins	Admins
activation_runs	Your own (created_by); admins all	Yourself, as yourself	Your own; admins all	Admins
recovery_moves	If you can see the parent run	If you can see the parent run	No one	Admins
outcome_checkpoints	If you can see the parent run	If you can see the parent run	If you can see the parent run	Admins
retention_plans	Your own (created_by); admins all	Yourself, as yourself	No one	Your own; admins all
tasks	Assigned to you (assigned_to); admins all	Admins	Admins; an assignee can only mark their own open task complete	Admins
invites	Your own (user_id); admins all	Yourself, under your own email, status pending	Admins, status only	Your own; admins all
Rules that apply everywhere

Row-level security is on for every table, and nothing can be read or written without signing in.
"Your own" always means the record's owner field equals the signed-in person's ID. The app never trusts an ID sent from the browser.
Admin status is checked in the database against the separate roles table, never from anything a person can edit.
App-level rules

Every page requires sign-in except the sign-in and password-reset pages.
The AI plan builder checks for a signed-in person on the server, not only on the page.
The dashboard invite cards count only the invites the signed-in person is allowed to read.
Two deliberate exceptions to "own records only"

Customers are shared: everyone signed in can read them, by your earlier choice.
Admins can read everything, as you set up for tasks.

## Edge cases hardened

| Case | Before | After |
|---|---|---|
| Empty / first-run state | Edge cases hardened (Before / After) | Empty lists show a short explanation and a Get Started button that leads to the first action. |
| Bad / malicious input | Unreadable saved data was silently ignored, and there were no invite or sign-up forms to validate. | Invalid emails, self-invites, duplicates and weak sign-up details are rejected in the form and again by the database. |
| Failure / offline | Loading showed blanks or "…", and load failures were swallowed or shown as a small text link. | Loading shows skeletons, failures show an inline error with a Retry button, and simultaneous edits follow last-write-wins. |

## Stress test results

_What you threw at it, and what held / broke._

_____
