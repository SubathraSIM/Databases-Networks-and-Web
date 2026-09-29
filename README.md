# CM2040 Databases, Networks and the Web - University of London

Coursework for **CM2040 Databases, Networks and the Web** (BSc Computer Science, University of London). The project is **EventEase**, a full-stack event booking web app built with a three-tier architecture: EJS front end, Node.js/Express server, and SQLite database.

| Assessment | Project | Key topics |
|---|---|---|
| Mid Term | EventEase: event booking app | Express routing, SQL, ER design, sessions and authentication, three-tier architecture |

**Tech:** Node.js · Express · SQLite · EJS · bcrypt · express-session

---

## EventEase - Event Booking Web Application

The app has two user roles:
- **Organisers** create, edit, publish, and delete events, manage site settings, and view bookings
- **Attendees** browse published events and book tickets

### Architecture
- **Presentation tier:** EJS templates (home, login, register, organiser dashboard, edit event, settings, bookings, attendee home, event page)
- **Application tier:** Express routes split into `organiser.js` and `attendee.js`
- **Data tier:** SQLite database (`database.db`) queried with SQL

### Database design (ER)
- `Organisers` → `Events` (one-to-many), with passwords stored as **hashes**
- `Organisers` → `Site_Settings` (one-to-one)
- `Events` → `Tickets` (one-to-many), where tickets track remaining quantity
- `Events` → `Bookings` (one-to-many), where bookings store the attendee name and ticket counts

### Extensions I built

**1. Login and authentication**
- Organiser registration and login using **bcrypt** (salt rounds = 10) and **express-session**
- `check_login` middleware protects every organiser route
- Duplicate username check on registration, input sanitisation, and clear error messages

**2. Real-time remaining tickets**
- A `quantity_available` field on tickets, shown to both organisers and attendees
- Bookings are checked against availability, and the count goes down on every valid booking to prevent overbooking

**3. View bookings page**
- A new organiser page listing all bookings for their events, with event title, attendee, and ticket counts
- Uses a **SQL JOIN** across `Bookings` and `Events`, filtered by the logged-in organiser's ID so organisers only see their own data

---

## How to run

```bash
cd midterm
npm install
npm run build-db     # creates database.db from the SQL schema (if provided)
npm run start
```
Then open `http://localhost:3000` in your browser.
