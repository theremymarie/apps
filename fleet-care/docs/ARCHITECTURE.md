# Architecture and production implementation plan

## Prototype

`index.html` supplies responsive styling, navigation, table and edit dialog. `app.js` manages synthetic records, local storage, photo data URLs, maintenance notes, date comparisons and simulated reminders. There are no external dependencies. Seed dates are relative to the day the demo is first opened or reset.

### Record model

| Field | Meaning |
| --- | --- |
| id | Browser-generated UUID |
| registration | Unique registration string |
| name / location / status | Vehicle identification and operational status |
| odometer | Non-negative kilometres |
| rego / insurance / service | ISO calendar dates or blank when unknown |
| photo | Optional data URL in prototype only |
| history | Date, note and entry type |

## Proposed production system

Retain the existing WordPress fleet workflow if that remains the chosen deployment. This folder is a UI reference, not an installable WordPress plugin. Implement fleet records and maintenance history in dedicated database tables or a reviewed plugin; store photo files in restricted media/object storage, with metadata in the vehicle record.

Add authenticated administrator and staff roles, capability checks on every operation, CSRF protection, server-side validation, duplicate registration detection, upload size/content checks, backups and restore. Store author, timestamp, old/new values and supporting document references for an append-only audit log. Define permissions for editing, status changes, exports and renewals before rollout.

Use explicit user-configured registration and insurance expiry dates from current documents. Do not derive insurance expiry from registration expiry. Service may require both calendar and odometer intervals; only calendar dates are demonstrated here. Obtain authoritative SA compliance requirements separately before implementing statutory rules.

## Reminder algorithm (proposed, not implemented)

Run a daily job at a configured local time in `Australia/Adelaide`; account for daylight saving. A reliable system cron should trigger the WordPress scheduler or a job worker, avoiding dependence on site visits.

1. Read current records and alert policy at execution time.
2. Classify each date as missing, overdue, due today, or due within 30 days.
3. Send daily reminders for due and overdue items until a verified replacement date is logged. Send missing-date items as data-completion requests.
4. Build a digest for authorised recipients; use configured addresses, not guessed staff contacts.
5. Enforce an idempotency key per local date, recipient and digest revision. Record provider message ID and delivery result. Retry transient failures without duplicate mail.
6. Re-read dates before dispatch to avoid sending an obsolete reminder after a staff update. If a new date is still in the alert window, the item remains eligible; a future date outside the window stops reminders until it enters the window again.

Connect FluentSMTP or another authenticated mail provider and test delivery with a controlled recipient. Configure sender identity and DNS authentication in the provider. Monitor missed jobs, failed messages, bounces and retry exhaustion. Do not assume the existing SMTP connection or saved snippets are enabled merely because this mockup exists.

## Acceptance checklist

- Add/edit survives page refresh; duplicate registrations are rejected.
- Blank, overdue, today, 30-day and 31-day dates classify correctly.
- Adelaide date boundary is respected.
- Renewing registration clears only its own alert, without clearing insurance/service.
- Off-road status and repair history remain visible after editing.
- Invalid/oversize photo files are rejected; accepted photos survive saving.
- Text entered as markup displays as text.
- Export contains the current records; reset requires confirmation.
- Production: permission checks, concurrent edits, backup restore, daily scheduler, idempotency, retries, delivery logging and timezone transitions pass end-to-end tests.

## Roadmap

1. Review the mockup with staff and reconcile actual fleet data.
2. Implement authenticated shared storage, document attachments and full audit history.
3. Connect email, recipient settings and reliable daily scheduling.
4. Test with a small verified vehicle set, then import the full fleet.
5. Add odometer-based service rules, reporting and mobile inspection forms if needed.
