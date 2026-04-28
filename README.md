# Course Companion FTE

Course Companion FTE is a full-stack learning management system for the AI Agent Development course. It combines a deterministic course platform with optional AI-powered learning features for premium users.

## Overview

The project is split into two parts:

- `frontend/` - React + TypeScript + Vite user interface
- `backend/` - FastAPI + SQLModel API and data layer

The app supports course browsing, lesson reading, quizzes, progress tracking, notes, bookmarks, search, premium AI tutoring, adaptive study paths, and admin analytics.

## Core Features

- Course catalog with modules and chapters
- Lesson viewer with previous/next navigation
- Quiz grading and progress tracking
- Search across chapter content
- Personal notes per chapter
- Bookmarks for lessons and other content
- User auth with JWT login/register
- Premium AI tutor chat
- AI-graded assessments
- Adaptive study path generation
- Admin analytics dashboard

## User Roles

- Free: basic course access, quizzes, progress, notes, bookmarks, search
- Premium: access to AI tutor and premium chapter content
- Pro: premium access plus broader AI features
- Team: organization-style access for teams
- Admin: analytics and system summary access

## Tech Stack

| Layer | Tech |
|---|---|
| Frontend | React 18, TypeScript, Vite, Tailwind CSS |
| Backend | FastAPI, SQLModel, SQLAlchemy, Alembic |
| Database | SQLite for local dev, PostgreSQL for production |
| AI | OpenAI-compatible Groq client |
| Storage | Local filesystem or Cloudflare R2 |

## Repository Structure

```text
.
|-- backend/
|   |-- alembic/
|   |-- local_content/
|   |-- src/
|   |   |-- api/
|   |   |   |-- admin/
|   |   |   |-- premium/
|   |   |-- cli/
|   |   |-- core/
|   |   |-- models/
|   |   `-- services/
|   |-- requirements.txt
|   `-- .env.example
|-- frontend/
|   |-- src/
|   |   |-- components/
|   |   |-- pages/
|   |   `-- lib/
|   |-- package.json
|   `-- .env.local
|-- ARCHITECTURE.md
`-- README.md
```

## Frontend Pages

- `/` - Landing page
- `/login` - Login form
- `/register` - Registration form
- `/dashboard` - Course dashboard
- `/lesson/:slug` - Lesson viewer
- `/tutor` - AI tutor chat
- `/upgrade` - Plan upgrade page
- `/admin` - Admin panel

## Backend API

All API routes are mounted under `/api/v1`.

### Auth

- `POST /api/v1/auth/register`
- `POST /api/v1/auth/login`

### Courses and content

- `GET /api/v1/courses/`
- `GET /api/v1/courses/{slug}/structure`
- `GET /api/v1/courses/{slug}/dashboard`
- `GET /api/v1/chapters/{slug}/content`

### Quizzes and progress

- `POST /api/v1/quizzes/{chapter_id}/grade`
- `POST /api/v1/progress/`

### Search

- `GET /api/v1/search?q=keyword`

### User tools

- `GET /api/v1/users/me`
- `POST /api/v1/users/upgrade`
- `GET /api/v1/notes/{chapter_slug}`
- `POST /api/v1/notes/`
- `PUT /api/v1/notes/{chapter_slug}`
- `GET /api/v1/bookmarks/`
- `POST /api/v1/bookmarks/`
- `DELETE /api/v1/bookmarks/{slug}`

### Premium AI

- `POST /api/v1/premium/grade`
- `GET /api/v1/premium/adaptive-path`
- `POST /api/v1/premium/chat`

### Admin

- `GET /api/v1/admin/summary`

## Environment Variables

### Backend

Copy `backend/.env.example` to `backend/.env`.

Required or commonly used variables:

| Variable | Purpose |
|---|---|
| `DATABASE_URL` | SQLite or PostgreSQL connection string |
| `STORAGE_TYPE` | `local`, `r2`, or `db` |
| `LOCAL_STORAGE_PATH` | Path to local lesson content |
| `SECRET_KEY` | JWT signing key |
| `ALGORITHM` | JWT algorithm |
| `ACCESS_TOKEN_EXPIRE_MINUTES` | Token lifetime |
| `API_V1_STR` | API prefix, usually `/api/v1` |
| `GROQ_API_KEY` | AI tutor API key |
| `GROQ_MODEL` | Groq model name |
| `GROQ_BASE_URL` | Groq OpenAI-compatible base URL |
| `R2_ENDPOINT_URL` | Cloudflare R2 endpoint |
| `R2_ACCESS_KEY_ID` | Cloudflare R2 access key |
| `R2_SECRET_ACCESS_KEY` | Cloudflare R2 secret key |
| `R2_BUCKET_NAME` | Cloudflare R2 bucket name |

`GEMINI_API_KEY` still exists in config for compatibility, but the current LLM service uses the `GROQ_*` variables.

### Frontend

Set `frontend/.env.local`:

```bash
VITE_API_BASE_URL=http://127.0.0.1:8000
```

If omitted, the frontend defaults to `http://127.0.0.1:8000`.

## Local Setup

### Prerequisites

- Python 3.11+
- Node.js 18+

### Backend

```bash
cd backend
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
copy .env.example .env
alembic upgrade head
uvicorn src.main:app --reload --host 127.0.0.1 --port 8000
```

### Frontend

```bash
cd frontend
npm install
npm run dev -- --host 127.0.0.1 --port 5173
```

### Run Both

Use two terminals:

1. Start the backend on `http://127.0.0.1:8000`
2. Start the frontend on `http://localhost:5173`

## Default Local Flow

1. Open the landing page in the browser.
2. Register a new account or log in.
3. Browse the course dashboard.
4. Open a lesson and read its markdown content.
5. Complete a quiz and track progress.
6. Add notes or bookmarks.
7. If you have premium access configured, use the AI tutor and adaptive study path.

## Development Notes

- The backend automatically initializes the database on startup.
- Lessons are loaded from local content first, with support for alternate storage backends.
- Premium AI features are gated by user tier and rate limiting.
- Search is deterministic and does not require an LLM.

## Security Notes

- Do not commit real secrets to git.
- Keep credentials in local `.env` files only.
- Rotate any key that was ever committed before the history cleanup.

## License

MIT
