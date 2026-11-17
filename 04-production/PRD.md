# Living PRD

> Module 4 · Production Specs. Refactor for readability; extract a living PRD that stays true as the build evolves.

## Problem

_What user problem does this solve? Tie to the validated hypothesis._

Decision-makers need to find the issue worth investigating and know what to do next. The supplied research describes a dashboard with 12 charts on its landing view, six clicks to reach the most-requested metric, and a reported 60% bounce rate. Stakeholder feedback points to competing charts, unclear next steps, and friction finding a key number. The baseline was supplied for the exercise and has not been independently verified.

The hypothesis is that leading with one headline metric, a plain-language takeaway, and one recommended action will help decision-makers start investigating before they leave. A walkthrough confirmed that a reviewer understood the intended next step and could reach the breakdown. A reduction in bounce or an increase in action clicks across representative users remains unvalidated. This pilot measures starting an investigation. Identifying a fix is a separate outcome for a later test.

## Users & jobs

- **Primary user:** Primary user · Marketing, product, or growth decision-maker. Secondary user · Pilot reviewer.
- **Job to be done:** When I open the dashboard, help me understand the most important issue and start investigating it without sorting through charts or exporting data. When I review the experiment, show me the session count, observed rates, and proposed stopping rule so I can decide whether to collect more sessions, revise the prototype, or proceed to a controlled test.

## Scope

- **In:** Start, ranked insights and details, per-insight trends, the trial setup breakdown by signup channel, and the Admin experiment scorecard. Include loading, empty, and failure states for the breakdown and session measurement in the current browser.
- **Out (explicitly):** Live analytics integrations, cross-browser reporting, participant identity, randomized comparison, automated rollout, and tracking whether users identify or implement a fix. The stopping rule is a recommendation displayed in Admin.

## Requirements

| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 1 | Lead with a decision on Start | Must | Show one featured metric, its takeaway, and a prominent recommended action before the next three ranked issues. The sample headline is 42% trial activation. Selecting **Investigate trial setup drop-off** opens the breakdown. |
| 2 | Explain the rankings | Should | All five insights appear in All Insights and have detail and trend links. Each shows an expandable explanation, a **Provisional** label, and the evidence that could change the order. Start also shows the top insight’s explanation. |
| 3 | Show one trend at a time | Should | Selecting an insight displays only its series. Users can inspect values at weeks 1, 2, 4, 6, and 8 by hover or keyboard focus. The trend links back to the matching details. |
| 4 | Make the recommended action useful | Must | Show stage counts and drop-off by signup channel, compare channels, and highlight the transition with the largest loss by count. Label the counts illustrative. The link to Admin and its return link preserve the channel selection. |
| 5 | Handle breakdown data states | Must | Show skeletons for both the funnel and channel table while loading. For no data, show “No trial setup data for this signup channel yet. Try another channel or check back after more trials have started.” On fetch failure, show “We couldn’t load the trial setup breakdown. Try again.” Retry retains the selected channel. |
| 6 | Record pilot outcomes | Must | Count each completed session once in the denominator of both rates. Count at most one recommended-action click per session, specifically the Start CTA. Apply the bounce definition below. |
| 7 | Show the pilot recommendation | Must | | Below 20 completed sessions, show **Collect more sessions** with the count and how many remain. At 20 or more, show **Stop and revise the prototype** if bounce exceeds 50% or the action click rate falls below 25%. Otherwise show **Continue to a controlled test**. Display the original click-rate baseline as unavailable. |

## Data & events

_What gets stored, what gets tracked._

Data or event | Source and behavior |
|---|---|
| Issue metrics and trends | Embedded illustrative data for five issues. The 42% headline is the sample activation rate at week 8. |
| Funnel counts | Embedded illustrative response loaded asynchronously. The all-channel sample has 420 completions from 1,000 trial starts, consistent with the headline. The prototype has no production analytics feed. |
| Original dashboard baseline | Exercise inputs of 60% bounce, six clicks, and 12 charts. Original recommended-action click rate is unavailable. |
| Session start and end | A session starts when Start is visible and ends when the tab is hidden or closed, or when Admin opens. |
| Interaction and bounce | A user click or control change counts as interaction. A completed session under 15 seconds with neither counts as a bounce. Bounce rate is bounced sessions divided by completed sessions. |
| Recommended-action click | The CTA click on Start counts once per session. Action click rate is sessions with that click divided by completed sessions. The click records intent to investigate, independent of whether the breakdown loads or a fix is found. |
| Measured results | The browser stores completed session outcomes locally. The interactions are recorded by the build, but the records can include internal walkthroughs and repeat sessions by the same person.

## Open questions

- Who qualifies as a pilot participant, and how will participant sessions be distinguished from internal walkthroughs?
- Are 20 sessions, a bounce ceiling of 50%, and a click floor of 25% the agreed pilot thresholds? What comparison will establish improvement in action clicks?
- What production sources define trial starts, signup channels, and completion at each setup step?
- What evidence and decision rule should replace provisional rankings when comparing onboarding with checkout and other issues?
- What should count as identifying a fix in a follow-up test?
