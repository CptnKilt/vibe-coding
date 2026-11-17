# Engineering Handoff Note

> Module 4 · Production Specs. Open the black box, make the build legible to an engineer.

## What this is

_One paragraph an engineer can read in 60 seconds._

Insights / Analytics is a static browser prototype that tests whether one headline metric, a plain-language takeaway, and one recommended action help decision-makers start investigating before they leave. **Start → Investigate trial setup drop-off → Experiment decision screen in Admin** is the main pilot path. The dashboard’s issue metrics and funnel counts are illustrative. The app records actual clicks and bounce outcomes in the current browser, where repeat visits and internal walkthroughs can count as sessions. The Admin scorecard recommends collecting more sessions until 20 are complete, then applies proposed bounce and click thresholds. Open `dist/index.html` with the full `dist` folder in place. There’s no install or build step.

## Architecture (plain language)

- **Frontend:** `dist/index.html` loads shared CSS and feature scripts in order. `dist/dashboard.js` reads the URL fragment and renders the corresponding screen. Feature folders under `dist/features` hold Start, Insights, Trends, Trial Setup, and Admin. Classic scripts support direct opening from disk. Their shared names make script order significant.
- **Backend / data:** The app runs entirely in the browser. Insights use embedded sample series. The funnel fetches an embedded JSON response, validates stage counts, and aggregates channels. Admin reads completed session outcomes from browser storage.
- **Key flows:** | Investigation flow | Start links to the funnel, which provides a signup-channel filter, a channel comparison, and the largest drop-off by count. The funnel links to the Admin scorecard with the current channel in the URL. The return link restores that channel. |
| Insight flow | All Insights links to individual details and trends. The Trends selector displays one insight at a time, and its back link returns to that insight’s details. |
| Measurement flow | A session starts when Start is visible and ends when the tab hides or closes, or Admin opens. A bounce is a session under 15 seconds without a click or control change. Only the Start CTA counts toward the recommended-action rate, once per completed session. Both rates use completed sessions as the denominator.

## What's solid vs. what's duct tape

| Area | State | Notes |
|---|---|---|
| The core screens have direct routes. | solid | The funnel presents skeletons during loading and distinct empty and error states. Retry keeps the selected channel. Scripted checks exercised screen routing, funnel loading states, and the scorecard’s decision boundaries. The local build was also reviewed in Codex. |
| The funnel’s apparent fetch reads an embedded fixture. | rough | Admin is a navigation label with no access control. Session results live in browser storage with no participant identity or shared reporting, and saving is best effort. The scorecard displays a recommendation with no automatic enforcement. |

## Risks & assumptions for the team

- **Pilot evidence.** Browser-local counts can mix repeat sessions and walkthroughs. The reported 60% bounce baseline came from the exercise, and the original action-click baseline is unavailable. The pilot measures intent to investigate. A controlled comparison with identified participant sessions is needed to assess improvement.
- **Stopping rule.** The proposed review point is 20 completed sessions. At that point, bounce above 50% or action clicks below 25% produces **Stop and revise the prototype**. Meeting both targets produces **Continue to a controlled test**. Confirm those thresholds with the pilot owner before using the scorecard to make a decision.
- **Decision data.** Rankings are provisional. Production use requires agreed metric definitions, real funnel counts, and evidence about the likely impact of fixing each issue.

## How to run it

```
1. Open `dist/index.html` in a browser. Keep `dashboard.css`, `dashboard.js`, and the `features` folders alongside it.
2. On **Start**, choose **Investigate trial setup drop-off**, select a signup channel, and open **Experiment decision screen in Admin**. Use the return link to verify the channel selection is restored.
3. For an HTTP preview, run `python -m http.server 8765 --bind 127.0.0.1 --directory dist` from the project root, then open `http://127.0.0.1:8765/` in a browser on the same machine. Use the same browser and URL for a consistent set of local pilot results.
```
