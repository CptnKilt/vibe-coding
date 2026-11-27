# PROMPTS.md: Living Prompt Pack

> Module 3 · Prompt Chaining. Re-architect the build with prompt chains; capture the reusable ones here.

## How to use this pack

_Each prompt is a reusable step. Chain them: the output of one becomes the input to the next._

## Prompt chain: [name your flow]

### Step 1: Expand: add the two missing screens
```
Build the next phase of this app in a strict sequence:
1. Add a screen "Unclaimed Plan Recovery — When a teammate doesn’t claim the task, offer a next move: remind them, reassign it, revise the action, or take ownership yourself. Keep the account unactivated until a teammate claims or completes the task. This gives the no-claim branch a useful outcome instead of ending at the readout.". Match the layout and spacing of the attached Make this a focused, step-by-step page—not another dashboard. Put the customer and unclaimed task in a prominent card, clearly mark the account Not activated, then offer the next actions: remind the teammate, reassign the task, or revise the plan. screenshot.
2. Add a screen "Customer Outcome Tracking — Follow each plan through its due date and show whether the customer’s situation improved: setup steps completed for Northstar, active seats for Harbor, or champion engagement for Juniper. This connects the activation event to customer results and, eventually, 90-day retention—which the current prototype can’t measure.". Match the data density of the attached Use a dashboard-style account view with a timeline. Show the plan, owner, due date, and progress against the relevant customer measure—for example, Harbor’s active seats or Northstar’s completed setup steps. Include 7-, 14-, 30-, and 90-day checkpoints, and clearly label sample data versus real results. screenshot.
3. Navigation: write the logic so Unclaimed Plan Recovery — When a teammate doesn’t claim the task, offer a next move: remind them, reassign it, revise the action, or take ownership yourself. Keep the account unactivated until a teammate claims or completes the task. This gives the no-claim branch a useful outcome instead of ending at the readout. links to Customer Outcome Tracking — Follow each plan through its due date and show whether the customer’s situation improved: setup steps completed for Northstar, active seats for Harbor, or champion engagement for Juniper. This connects the activation event to customer results and, eventually, 90-day retention—which the current prototype can’t measure..

Build these in order so Unclaimed Plan Recovery — When a teammate doesn’t claim the task, offer a next move: remind them, reassign it, revise the action, or take ownership yourself. Keep the account unactivated until a teammate claims or completes the task. This gives the no-claim branch a useful outcome instead of ending at the readout. is the anchor for Customer Outcome Tracking — Follow each plan through its due date and show whether the customer’s situation improved: setup steps completed for Northstar, active seats for Harbor, or champion engagement for Juniper. This connects the activation event to customer results and, eventually, 90-day retention—which the current prototype can’t measure..
```

### Step 2: Behavior: handle loading, empty and error states
```
Apply the following logic constraints to the Customer Outcome Tracking flow:
- Use skeleton screens for the The list of accounts and their plan progress loading state.
- If no data is present, show the empty state: "“No customer outcomes yet. Accounts will appear here once a retention plan is shared and progress is recorded.” Button: Create a retention plan".
- On fetch failure, trigger the error state: "We couldn’t load customer outcomes right now. Please try again.” Button: Retry".

Maintain the same design language throughout and tether all behavior strictly to these rules.
```

### Step 3: Refine: polish one thing on one screen
```
The Readout looks least finished. Its prefilled four-run tally makes the results feel fabricated, even though the page says they’re prototype sessions.

One change: start the tally at zero and add runs only as someone completes the flow. That would make the readout feel more trustworthy and let it test the hypothesis honestly.
```

## Reusable techniques learned

- Break multi-screen work into a clear build sequence.
- Give each screen a specific user problem, purpose, and key content.
- Specify the rules that govern status changes, such as when an account counts as activated.
- Describe navigation with clear triggers and destinations.
- Attach each visual reference to the screen it should guide, and name the qualities to follow.
- Define loading, empty, and error states with exact copy and button behavior.
- Label sample data clearly so users can distinguish it from real results.
- Start prototype metrics at zero and update them only when a user completes the relevant action.

## What broke (and the fix)

_Where a single mega-prompt failed and chaining fixed it._

The assigning a task is what broke for me. There wasn't any tracking of it and any way to mark the task complete. I added a assigned task page. This will be attached to a sign in page that will show only the tasks that are assigned to a user. An admin will be able to see all assigned tasks.
