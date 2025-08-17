# Eventify — Event Management & RSVP App

**Stack:** React + Tailwind (Vite) • Node/Express • PostgreSQL • JWT • Nodemailer

## Features

- Public events listing (Home)
- Auth: Sign up / Login (JWT)
- Hosts can create/update events, upload cover images, control capacity, public/private
- RSVP with capacity-aware confirmation (auto-waitlist when full)
- "My Events": created & joined
- Attendee list with status badges + check-in toggle
- Email reminders to attendees
- Export attendee list as CSV
- QR code for event check-in (image)
- Basic host analytics

## Quick Start (Local)

### 1) Database
- Start Postgres (e.g. Docker):  
  ```bash
  docker run --name eventify-pg -e POSTGRES_PASSWORD=postgres -p 5432:5432 -d postgres:15
  ```
- Create DB `eventify`:
  ```bash
  docker exec -it eventify-pg psql -U postgres -c "CREATE DATABASE eventify;"
  ```

### 2) Backend API
```bash
cd backend
cp .env.example .env
npm i
npm run initdb   # create tables
npm run dev      # runs on http://localhost:4000
```

### 3) Frontend
```bash
cd ../frontend
cp .env.example .env
npm i
npm run dev      # opens http://localhost:5173
```

> Update `VITE_API_BASE_URL` if your backend runs elsewhere.

## Minimal Workflow

1. Sign up as **Host**.
2. Create an event.
3. Log out or sign up as a **Guest**, open event page, click **RSVP**.
4. Log in as Host -> Event page -> Host Tools: view attendees, send reminders, export CSV, check-in via email and QR.

## API Overview

- `POST /api/auth/signup` `{ name, email, password, role }`
- `POST /api/auth/login` `{ email, password }`
- `GET /api/events/public`
- `GET /api/events/:id`
- `POST /api/events` (auth host) body: `{ title, description, category, location, event_date, capacity, is_public }`
- `PUT /api/events/:id` (host)
- `DELETE /api/events/:id` (host)
- `POST /api/events/:id/image` (multipart) (host)
- `POST /api/events/:id/rsvp` (auth guest/host)
- `GET /api/events/:id/attendees` (host)
- `GET /api/events/:id/attendees/export` (host) -> CSV
- `POST /api/events/:id/remind` (host) -> emails
- `GET /api/events/:id/qrcode` (host) -> PNG
- `POST /api/events/:id/checkin` (host) `{ email }`
- `GET /api/dashboard/me` (auth) -> created & joined
- `GET /api/dashboard/analytics` (auth) -> counts

## Emails
Configure SMTP in `backend/.env`. For testing, use [Ethereal Email](https://ethereal.email/) creds.

## Deployment Hints

- **Backend**: Render (Node service) or Railway. Add env vars: `DATABASE_URL`, `JWT_SECRET`, SMTP vars, `APP_BASE_URL`.
- **DB**: Render PostgreSQL / Railway PostgreSQL.
- **Frontend**: Vercel. Set `VITE_API_BASE_URL` to your deployed backend URL.

## Demo Video Script (Use this as a checklist to record)

1. Landing -> Public events list.
2. Sign Up (Host) -> Create Event (fill all fields).
3. Log out -> Sign Up (Guest) -> open Public event -> RSVP.
4. Log in as Host -> Open Event -> Host Tools (attendees table).
5. Click **Send Reminders** (show success toast/log).
6. Click **Export CSV** and **QR Code** (open QR in new tab).
7. Check-in a guest by email -> attendees table updates.
8. Show **My Events** page and **Analytics** (GET `/api/dashboard/analytics`).

Record with any screen recorder; include a quick intro & outro.

## License
MIT
