# Living PRD

> Module 4 · Production Specs. Refactor for readability; extract a living PRD that stays true as the build evolves.

## Problem

_What user problem does this solve? Tie to the validated hypothesis._

The Dashboard Nobody Reads starts from a reported 60% bounce rate and six clicks to reach the most-requested metric. The problem is the work users must do to find an insight, understand it, and decide what to do next.

## Users & jobs

- **Primary user:** A product lead reviewing checkout performance. Marketing managers, growth analysts, and product marketing managers are also intended users.
- **Job to be done:** When I review checkout performance, help me see what needs attention, inspect the evidence, and record a follow-up with an owner so I can move from observation to action.

## Scope

- **In:** Overview with the headline metric, takeaway, and recommended action. Checkout Diagnostics with a filterable funnel and breakdowns. Action Tracker with the selected finding, action details, and follow-up measurement. Facilitator controls for recording and exporting prototype sessions.
- **Out (explicitly):** Live commerce integrations, shared task storage, notifications, centralized event collection, and automated assignment to an experiment or control group.

## Requirements

| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 1 | Surface the checkout insight on Overview | Must | The analytics view shows the mobile completion rate, explains it in plain language, and presents Review Action Plan as the recommended action. |
| 2 | Open the recommended action | Must | Review Action Plan opens the checkout investigation steps. Close returns the user to Overview. |
| 3 | Keep Overview aligned with the reporting period | Should | Selecting 7, 14, or 30 days in The Gap Is Widening updates the trend and period-dependent mobile metrics. Explore Checkout Diagnostics carries the selected period forward. |
| 4 | Explain where shoppers drop off | Must | Diagnostics shows counts and drop-off rates by funnel step. Selecting a step updates the browser and payment-method breakdowns. The page shows a skeleton while loading and distinct messages for empty data and fetch failures. |
| 5 | Carry evidence into the follow-up | Must | Use finding in Action Tracker carries the step, reporting period, browser, and payment method into the tracker. The tracker displays the finding and supporting counts. View in Checkout Diagnostics restores that selection. |
| 6 | Record and revisit an action | Should | Users can save a title, owner, due date, status, next step, and follow-up completion rate. Reopening the same finding in the same browser restores the saved action. Entering a follow-up rate shows the percentage-point change from baseline. |
| 7 | Record a facilitated session | Must | A facilitator can start and end a session and export its events as JSON. A completed session records duration, bounce status, and whether the recommended action was clicked. |

## Data & events

_What gets stored, what gets tracked._

Mocked data. Overview uses illustrative checkout figures. Diagnostics and Action Tracker fetch a bundled JSON file containing sample funnel counts by reporting period, browser, and payment method. The 60% bounce rate and six-click figure are supplied research inputs. The 14-day mobile totals currently differ between Overview and Diagnostics.

Working behavior. The prototype fetches the sample data, calculates funnel breakdowns, passes the finding between pages, and saves actions in browser storage. Facilitator events are captured from actual prototype interactions and can be exported from that browser.
Event	When it is recorded
session_start	A facilitator starts a participant session.
first_interaction	The participant first interacts with dashboard content.
recommended_action_clicked	The participant selects Review Action Plan during a session.
session_end	The facilitator ends the session or the page attempts to record an exit. Includes duration, bounce status, and whether the recommended action was clicked.


A bounce is a session under 15 seconds with no recorded interaction. Recommended-action click rate is the share of eligible sessions with at least one Review Action Plan click. Opening Diagnostics and saving an action are separate behaviors that the current facilitator event log does not measure.

## Open questions

Open questions
- What dashboard will serve as the comparison, and what reduction in bounce and increase in action clicks would count as success?
- The public page opens with research context, while Start participant session jumps to the analytics view. Which entry point should the experiment measure?
- Should opening the plan remain the primary action signal, or should success require a later step such as saving a follow-up?
- Which source should supply consistent counts for Overview and Diagnostics, and what event definitions should production use for each checkout step?
- Who should be able to see or edit saved actions if the tracker moves beyond a single browser?
