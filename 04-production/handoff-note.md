# Engineering Handoff Note

> Module 4 · Production Specs. Open the black box, make the build legible to an engineer.

## What this is

_One paragraph an engineer can read in 60 seconds._

Signal M3 is a static prototype for The Dashboard Nobody Reads. It tests whether giving decision makers one headline metric, a plain-language takeaway, and a recommended action helps them act sooner. Overview highlights mobile checkout completion. Checkout Diagnostics lets users examine drop-off by funnel step, browser, and payment method. Action Tracker carries that finding forward so users can record an owner, next step, and follow-up rate. The refactored workspace source lives under src/features/. Checkout data is illustrative, while filtering, navigation, local action saves, and facilitator event export work in the browser. The reported 60% bounce rate is a research baseline. The prototype has yet to establish an improvement against a control. Serve src/ over localhost to run the refactor. The public site serves the existing dist/ build.

## Architecture (plain language)

- **Frontend:** Static HTML, CSS, and native JavaScript modules. src/index.html routes to Overview. The three feature folders contain named screens and their interaction code.
- **Backend / data:** A static server serves the files. Overview calculates metrics from its own sample series in overview-data.js. Diagnostics and Tracker share a JSON fixture and funnel calculations under src/shared/checkout/. Actions and facilitator events use browser storage.
- **Key flows:** Start a session on Overview, observe interactions, end the session, and export JSON. Events capture session start, first interaction, recommended-action clicks, and session end with duration and bounce status.

## What's solid vs. what's duct tape

| Area | State | Notes |
|---|---|---|
| Feature folders and screen names match the PRD. Overview calculations and action persistence have dedicated module. Diagnostics and Tracker use the same funnel module and can recreate a finding from the URL. Diagnostics has loading, empty, and error states. Actions can be saved and reopened in the same browser. Facilitators can export recorded session events. | solid | _____ |
| Overview still handles facilitator event collection and chart rendering in the same screen script. Overview uses a separate sample series. Its 14-day totals differ from the Diagnostics fixture. Actions and events are browser-local. The tracker is an individual follow-up form. Session capture is limited to Overview, with best-effort recording on page exit. | rough | _____ |

## Risks & assumptions for the team

- Measurement. The working bounce definition is a session under 15 seconds with no recorded interaction. A Review Action Plan click measures interest. It does not measure task completion, Diagnostics usage, or a saved follow-up. A comparison dashboard and consistent starting point are needed to test the hypothesis.
- Data consistency. Production needs a common source for counts, periods, and funnel definitions. A saved finding is keyed by its filters and recomputed when reopened, so replacing the fixture could change the evidence displayed beside an existing action.
- Persistence. Saved actions and events are tied to the browser and origin. Different ports, private browsing, or clearing site data can make prior records unavailable.
- Deployment. The published site serves dist/. The refactor is in src/, so publishing the refactor requires a deliberate update to the deployment.

## How to run it

```
Open a terminal at the Signal-M3 project root. With Python 3 installed, run:

python -m http.server 8000 --bind 127.0.0.1 --directory src

On Windows, use py -3 in place of python if that is how Python is installed. Open http://localhost:8000/ in a current browser. The local server gives the JavaScript modules and JSON request an HTTP origin. No npm install, build step, or API key is required.

To smoke-test the flow, change the Overview period, open Diagnostics, choose a funnel step, and send it to Action Tracker. Save an action and reload the tracker to check that the fields return. To check instrumentation, start a participant session on Overview, click Review Action Plan, end the session, and download the event JSON.
```
