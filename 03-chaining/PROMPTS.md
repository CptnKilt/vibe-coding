# PROMPTS.md: Living Prompt Pack

> Module 3 · Prompt Chaining. Re-architect the build with prompt chains; capture the reusable ones here.

## How to use this pack

_Each prompt is a reusable step. Chain them: the output of one becomes the input to the next._

## Prompt chain: Checkout Insight to Action

### Step 1: Expand: add the two missing screens
```
Build the next phase of this app in a strict sequence:
1. Add a screen "Trial setup drop-off breakdown". Match the layout and spacing of the attached funnel drill-down screenshot.
2. Add a screen "Experiment decision screen in Admin". Match the data density of the attached experiment scorecard screenshot.
3. Navigation: write the logic so Trial setup drop-off breakdown links to Experiment decision screen in Admin.

Build these in order so Trial setup drop-off breakdown is the anchor for Experiment decision screen in Admin.
```

### Step 2: Behavior: handle loading, empty and error states
```
Apply the following logic constraints to the Trial setup drop-off breakdown flow:
- Use skeleton screens for the It shows a list of funnel stages and a comparison table by signup channel. loading state.
- If no data is present, show the empty state: "No trial setup data for this signup channel yet. Try another channel or check back after more trials have started.".
- On fetch failure, trigger the error state: "We couldn’t load the trial setup breakdown. Try again.".

Maintain the same design language throughout and tether all behavior strictly to these rules.
```

### Step 3: Refine: polish one thing on one screen
```
The Experiment decision screen in Admin looks least finished. Before the pilot reaches 20 sessions, the progress ring and two metric cards mostly repeat how little data there is.

I’d replace the ring with one prominent status line: “Collect more sessions · 8 of 20 complete · 12 to go.” That gives the reviewer the decision and the next step at a glance.

Don't change anything else in the project or touch the underlying logic.
```

## Reusable techniques learned

- _____
- _____

## What broke (and the fix)

_Where a single mega-prompt failed and chaining fixed it._

_____
