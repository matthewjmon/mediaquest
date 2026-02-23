# MediaQuest

Full-stack iTunes media search app built with React, Node.js, and Express.

![MediaQuest Screenshot](https://res.cloudinary.com/dyj8e4dsr/image/upload/v1763379402/itunes-app_qprhkn.png)

## Why This Project

MediaQuest demonstrates practical full-stack fundamentals for a junior developer role:

- Building a responsive React UI with reusable components
- Creating and securing backend API routes with Express + JWT
- Integrating a third-party API (Apple iTunes Search API) through a backend proxy
- Managing client state for search results, favourites, and UI theme
- Structuring frontend and backend for local development and production serving

## Tech Stack

- Frontend: React 19, React Router, Vite, Bootstrap, React-Bootstrap
- Backend: Node.js, Express, Axios, JSON Web Token (`jsonwebtoken`), CORS, dotenv
- External API: Apple iTunes Search API

## Features

- Debounced search input (500ms) to reduce request spam
- Media-type filtering (`all`, `movie`, `podcast`, `music`, `audiobook`, etc.)
- Server-issued JWT token (`/api/token`) with 1-hour expiry
- Protected search endpoint (`/api/itunes/search`) requiring `Bearer` token
- Results cards with artwork, artist, and formatted release date
- Add/remove favourites with duplicate protection
- Toast notifications for favourites actions
- Responsive navigation (desktop sidebar + mobile navbar)
- Light/dark mode toggle

## Architecture Overview

- `frontend` handles UI, routing, state, and user interactions.
- `backend` handles token issuance, JWT verification, and iTunes API proxying.
- In development, Vite proxies `/api/*` requests to `http://localhost:5000`.
- In production mode, Express serves `frontend/dist` as static assets.

## API Endpoints

- `GET /api/token`
  - Returns a signed JWT token: `{ "token": "..." }`
- `POST /api/itunes/search`
  - Requires `Authorization: Bearer <token>`
  - Body: `{ "term": "search text", "media": "music" }`
  - Returns an array of iTunes search results (limited to 20)

## Local Setup

### Prerequisites

- Node.js 18+
- npm

### 1) Clone and install dependencies

```bash
git clone https://github.com/matthewjmon/itunes-media-app.git
cd itunes-media-app

cd backend
npm install

cd ../frontend
npm install
```

### 2) Configure environment variables

Create `backend/.env`:

```env
JWT_SECRET=your_secret_key_here
PORT=5000
```

### 3) Run in development (2 terminals)

Terminal 1:

```bash
cd backend
npm start
```

Terminal 2:

```bash
cd frontend
npm run dev
```

Open the app at the Vite URL shown in your terminal (typically `http://localhost:5173`).

## Production Build (Serve Frontend From Backend)

```bash
cd frontend
npm run build

cd ../backend
npm start
```

Then open `http://localhost:5000`.

## Project Structure

```text
itunes-media-app/
  backend/
    routes/
      itunes.js      # JWT-protected iTunes proxy route
      token.js       # JWT token generation route
    server.js        # Express server + static frontend serving
  frontend/
    src/
      components/    # SearchBar, ResultsList, FavouritesList, Navbar
      styles/        # Global, responsive, and component styles
      App.jsx        # Main app logic and state
      main.jsx       # App entry + router/bootstrap setup
    vite.config.js   # Dev proxy config for /api
```

## Known Limitations

- No persistent database yet (favourites reset on refresh)
- No user accounts/authentication flow (JWT is app-level, not user login)
- No automated tests currently

## Improvements I Would Build Next

- Persist favourites with a database (e.g., PostgreSQL + Prisma)
- Add user authentication and per-user saved lists
- Add loading skeletons and stronger error boundaries
- Add unit/integration tests (frontend + backend)
- Add response caching and rate limiting on the API proxy

## About Me

Matthew Monaghan  
Junior Full-Stack Developer

- Email: monaghanmatt18@gmail.com
- GitHub: https://github.com/matthewjmon
- LinkedIn: https://www.linkedin.com/in/matthew-monaghan-b9742b311/
