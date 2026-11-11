# PROMPTS.md: Living Prompt Pack

> Module 3 · Prompt Chaining. Re-architect the build with prompt chains; capture the reusable ones here.

## How to use this pack

_Each prompt is a reusable step. Chain them: the output of one becomes the input to the next._

## Prompt chain: Checkout Insight to Action

### Step 1: Expand: add the two missing screens
```
Build the next phase of this app in a strict sequence:
1. Add a screen "Checkout Diagnostics". Match the layout and spacing of the attached Mixpanel screenshot.
2. Add a screen "Action Tracker". Match the data density of the attached Asana screenshot.
3. Navigation: write the logic so Checkout Diagnostics links to Action Tracker.

Build these in order so Checkout Diagnostics is the anchor for Action Tracker.
```

### Step 2: Behavior: handle loading, empty and error states
```
Apply the following logic constraints to the Checkout Diagnostics flow:
- Use skeleton screens for the A checkout funnel with counts and drop-off rates at each step. Clicking a step reveals the browser and payment-method breakdown. loading state.
- If no data is present, show the empty state: "No activity found. Please refresh the dashboard.".
- On fetch failure, trigger the error state: "Connection lost. Please refresh the dashboard.".

Maintain the same design language throughout and tether all behavior strictly to these rules.
```

### Step 3: Refine: polish one thing on one screen
```
The Overview needs a professional Mixpanel polish.
1. Start by listing the 3 biggest gaps in typography and spacing compared to Checkout Diagnostics.
2. Once you've identified those, resize the headers and remove teh dropdown in the upper right corner. Have all the number change from the dropdown in the Gap is Widening section

Don't change anything else in the project or touch the underlying logic.
```

## Reusable techniques learned

- _____
- _____

## What broke (and the fix)

_Where a single mega-prompt failed and chaining fixed it._

_____
