# CampusFix 🛠️

A streamlined campus maintenance reporting tool for students and staff to quickly log infrastructure issues, enabling efficient triage and resolution by facility administrators.

**Live app:** https://campus-fix.base44.app

## Features

### For reporters (students & staff)
- **ID-based login** — sign in with your registered student/staff ID code (rate-limited per IP to prevent brute-forcing).
- **Quick reporting** — submit an issue with a photo (required), block, room, exact location, and description.
- **Anonymous reporting** — optionally report anonymously; admins see "Anonymous" and never your identity.
- **My tickets** — track the status of every ticket you've reported, and delete your own tickets while they're still marked *New*.
- **Staff-only access** — Faculty Block is visible only to staff accounts.
- **Live status mirroring** — duplicate tickets show the status of the ticket they're linked to.

### For administrators
- **Secure admin login** — dedicated credentials verified server-side with PBKDF2-SHA256 hashing (plain passwords are never stored), with temporary lockout after repeated failures.
- **AI-powered triage** — every new ticket is automatically classified by **trade** (Plumbing, Electrical, Carpentry, Civil/Masonry, Housekeeping, IT/Network, Other) and **urgency** (High / Medium / Low), with the AI's reasoning shown for review.
- **Duplicate detection** — the AI matches new reports against existing tickets at the same location and flags them as *Linked*, *Possible*, or *Parent* (recurring), with one-click confirm/reject/undo controls.
- **Organized queue** — status tabs (All / New / On going / Done) with live counts, plus filters by status, block, trade, and urgency; tickets grouped by trade and sorted by urgency.
- **Ticket management** — update status, correct trade/urgency, add notes, permanently delete tickets (with reporter-link cleanup and parent re-pointing), and bulk-delete demo data.

## How it works

1. A student or staff member signs in with their ID code.
2. They report an issue with a photo and location — the ticket gets a unique reference number.
3. AI triage runs in the background: it classifies the issue and checks for duplicates without blocking submission.
4. Admins log in, review the queue, confirm or dismiss AI suggestions, manage duplicates, and move tickets through *New → In Progress → Resolved*.
5. Reporters see live status updates in *My tickets*.

## Security & privacy

| Concern | How it's handled |
|---|---|
| Reporter identities | Stored in a private `ReporterLinks` entity, invisible to admins — anonymous reports never reveal who filed them |
| Ticket data | Row-level security closes all client-side access; data is reachable only through verified server-side sessions |
| ID login abuse | Per-IP sliding-window rate limiting (10-minute window) |
| Admin credentials | PBKDF2-SHA256 salted hashes, lockout after repeated failures |
| Sessions | Issued server-side, verified on every privileged call |

## Tech stack

- **Frontend:** React + Tailwind CSS (mobile-first, blue/white design) on Vite
- **Backend:** Base44 server-side functions with service-role data access
- **AI:** Built-in LLM integration for classification and duplicate matching
- **Storage:** Private file storage for ticket photos (accessed via signed URLs)

## Project structure

```
src/pages/          Home, Report, MyTickets, Admin, auth pages
src/components/     Shared UI (badges, photo viewer, forms, admin widgets)
base44/entities/    Tickets, IdRegistry, ReporterLinks, Sessions,
                    AdminCredentials, LoginThrottle, Locations
base44/functions/   loginWithId, createTicket, triageTicket, getMyTickets,
                    deleteMyTicket, adminLogin/Logout/GetTickets/
                    UpdateTicket/DeleteTicket/DeleteDemoData
```

## Demo data

The app ships with demo tickets, demo ID codes, and a demo admin account, all clearly labeled **"Demo data"**. Admins can bulk-delete demo tickets at any time; real reports are unaffected.

---

Built with [Base44](https://base44.com).
