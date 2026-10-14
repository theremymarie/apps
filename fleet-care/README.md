# Fleet Care — interactive fleet management mockup

A responsive prototype of the fleet operations workflow discussed for KI Coachlines, with synthetic sample data only. This is a demonstration, not the existing WordPress installation or a production fleet system.

## Run

Download this folder and open `index.html` in a modern browser. For more consistent browser storage, serve it locally:

```sh
python3 -m http.server 8000
```

Then open `http://localhost:8000`. No build step, dependencies or API credentials are required.

## Included

- Overview dashboard with vehicle and attention counts
- Searchable register and status filter
- Add/edit vehicle, registration, location, odometer and due dates
- Vehicle photo upload (PNG, JPEG, WebP; maximum 2 MB per photo)
- Maintenance entries and recorded changes to status/due-date fields
- Registration, insurance and service reminders within 30 days, including overdue and missing-date items
- Simulated email digest, JSON export and reset
- Responsive layout and labelled controls

Changes persist in this browser's local storage when available. Export before resetting or clearing browser data. There is no backup import screen in this prototype.

## Documentation

- [Staff guide](docs/STAFF_GUIDE.md)
- [Architecture and production plan](docs/ARCHITECTURE.md)

## Boundaries

No live email is sent. No login, shared database, production scheduler, legal deadline calculation or immutable audit log is implemented. Browser storage may be blocked or fill up, particularly with photos. Do not enter private fleet information in a publicly hosted copy. Dates must come from verified registration, insurer and service records; the mockup does not determine statutory deadlines.

All registrations and vehicles in the repository are fictional. Operational readiness is the recorded status, not a safety certification. Due-date calculations use the Adelaide calendar date.

## Verification

JavaScript syntax and date/alert boundary logic were checked. See the production plan for acceptance checks. Browser visual and interaction verification was not available in this execution environment.
