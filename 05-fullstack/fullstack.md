# Full-Stack: Data, Access Rules, Edge Cases, Deploy

> Module 5 · Full-Stack. Add data schemas, access rules, and edge cases; stress-test and deploy.

## Deployed link

_The working, shareable link that survives real users._

_____

## Data schema

| Entity | Key fields | Notes |
|---|---|---|
| Supabase Auth users | id, email | Supports email signup and login. The Auth UUID identifies the invite owner. |
| Invites | id, user_id, sender_email, recipient_email, status, timestamp, email_submitted_at | Tracks the invite separately from email submission. The new submission timestamp is part of the pending database update. |

## Access rules

_Who can see / do what? Where are the auth boundaries?_

Users sign in to Admin and can view or edit only their own invite records. Once email sending is configured, the server verifies that the sender is authorized and owns the invite before submitting it for delivery. The email provider key stays on the server.

## Edge cases hardened

| Case | Before | After |
|---|---|---|
| Empty / first-run state | Empty / first-run | Get Started opened a tracking-only form. / Get Started opens the invite form. Once email sending is enabled, Send invite submits a signup email and saves its tracking record. |
| Bad / malicious input | Bad / malicious input | Database rules restricted invite records by owner. / The email endpoint also checks the signed-in sender, ownership, and a valid invite ID before contacting the provider. |
| Failure / offline | Failure / offline | Retry covered record saves. / 	If delivery submission can’t be confirmed, the form reports it and retries using the same invite ID. The provider deduplicates retries within its supported window. |

## Stress test results

_What you threw at it, and what held / broke._

_____
