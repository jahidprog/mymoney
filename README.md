# MyMoney v3

An offline-ready personal finance PWA backed by Google Sheets + Google Apps Script.

## Included

- `index.html` — redesigned mobile-first UI
- `sw.js` — service worker with network-first caching
- `manifest.webmanifest` — installable PWA metadata
- `icon-192.png`, `icon-512.png` — app icons
- `Code.gs` — Google Apps Script backend
- `README.md` — setup and architecture notes

## Features

- Dashboard with cash position, income, spending, savings and outstanding amounts
- Monthly navigation
- Income, expense, family, lend, borrow and business records
- Loans with partial repayments
- Business bills with due dates, reminders and monthly/yearly repeats
- Google Calendar reminders through Apps Script
- Offline queue: new transactions are stored locally until the connection returns
- Search and transaction filtering
- Spending insights by category and payment method
- Monthly budget tracking
- CSV export and JSON backup
- Dark/light theme
- PWA install support
- Responsive mobile-first interface
- Local cache so the app remains useful when Google Sheets is unavailable

## Setup

1. Create/open the Google Sheet you want to use.
2. Open **Extensions → Apps Script**.
3. Replace the script with `Code.gs`.
4. Set `SECRET` to a long random value.
5. Run `authorize()` once from the Apps Script editor and approve Calendar/Sheets permissions.
6. Deploy as **Web app**:
   - Execute as: **Me**
   - Who has access: **Anyone**
7. Copy the `/exec` URL.
8. Host the files in this folder on HTTPS (GitHub Pages, Netlify, Vercel, etc.).
9. Open MyMoney → Settings → Google Sheets connection.
10. Paste the `/exec` URL and the same secret key.
11. Save and test.

## Important security note

The secret is entered into the browser, so it is visible to the app user. It should be treated as a shared gate, **not as real authentication**. For a production financial product with multiple users or highly sensitive data, move to authenticated user accounts and a proper backend/database.

## Data model

The `Transactions` sheet is automatically created/updated with:

`ID, Date, Type, Category, Amount, Party, Note, Status, Due, EventId, Method, Project, Repeat, Remind, Settled, Log`

Existing rows from the previous version remain compatible with the backend's normalization logic.

## Offline behavior

When offline:

- Existing cached transactions remain visible.
- New transactions enter the local queue.
- The UI marks queued entries as pending.
- When the browser becomes online again, the queue is uploaded automatically.

Edits to an already-synced record require connectivity. This keeps the sync model simple and avoids conflicting offline edits.

## Recommended next production upgrades

For a real multi-user product, consider:

- Firebase/Supabase/PostgreSQL instead of Google Sheets
- Proper user authentication
- Per-user data isolation
- Server-side authorization
- Audit history / soft delete
- Idempotency keys
- Conflict resolution
- Automated backups
- Automated tests
- Error monitoring
- CI/CD
