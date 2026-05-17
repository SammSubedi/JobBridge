An AI-powered job portal that connects job seekers with employers. It goes beyond a typical job board by using on-device machine learning to parse resumes and intelligently rank candidates against job requirements — no external AI API needed.

---

## Features

**For Job Seekers**
- Register, verify email, and set up a profile
- Browse and search job listings with filters
- Apply to jobs with CV upload (PDF or DOCX)
- Track application status
- Save jobs for later
- View company profiles

**For Employers**
- Post, edit, and manage job listings
- Review incoming applications
- AI-powered candidate ranking with match scores and insights
- View candidate profiles
- Profile view tracking

**General**
- JWT-based authentication with email verification and password reset
- Real-time notification center
- Location autocomplete
- Profile completion tracking
- Responsive UI

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18, Vite, Tailwind CSS |
| Backend | Node.js, Express |
| Database | MongoDB (Mongoose) |
| AI / ML | Xenova Transformers (`all-MiniLM-L6-v2`), pdf-parse, mammoth, natural NLP |
| Auth | JWT, bcryptjs, Nodemailer |

---

## How the AI Ranking Works

1. Uploaded CVs (PDF/DOCX) are parsed into plain text
2. Skills and keywords are extracted using NLP (`natural`, `compromise`)
3. Both the CV and job description are converted into semantic vectors using the `all-MiniLM-L6-v2` model via `@xenova/transformers`
4. Cosine similarity is calculated between the two vectors to produce a match score (0–100)
5. Candidates are tiered as Excellent / Good / Fair / Poor and shown to the employer with insights

The model runs entirely on the server — no OpenAI or external API calls.

---

## Project Structure

```
├── backend/
│   ├── ai-service/         # CV parsing, skill extraction, embeddings, ranking
│   ├── middleware/         # Auth, file upload
│   ├── models/             # Mongoose schemas
│   ├── routes/             # API route handlers
│   ├── services/           # Notification, location services
│   ├── utils/              # Email service, profile completion
│   └── server.js
│
└── frontend/
    └── src/
        ├── components/     # Reusable UI components
        ├── contexts/       # Auth context
        ├── hooks/          # Custom React hooks
        └── pages/          # All page components
```

---

## Getting Started

### Prerequisites

- Node.js v18+
- MongoDB (local or Atlas)

### Backend Setup

```bash
cd backend
npm install
```

Create a `.env` file in the `backend/` folder:

```env
PORT=5000
MONGODB_URI=mongodb://127.0.0.1:27017/jobbridge
JWT_SECRET=your_jwt_secret
JWT_EXPIRE=7d
NODE_ENV=development

EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_app_password

FRONTEND_URL=http://localhost:5173
```

```bash
npm run dev
```

### Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

The app will be available at `http://localhost:5173`.

---

## API Overview

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/register` | Register a new user |
| POST | `/api/auth/login` | Login |
| GET | `/api/jobs` | Get all job listings |
| POST | `/api/jobs` | Post a new job (employer) |
| POST | `/api/applications` | Submit an application |
| GET | `/api/ai-matching/ranking/:jobId` | Get AI-ranked candidates for a job |
| GET | `/api/notifications` | Get user notifications |
| GET | `/api/health` | API health check |

---

## Environment Notes

- The AI model (`all-MiniLM-L6-v2`) is downloaded automatically on first run via `@xenova/transformers`. Expect a short delay on initial startup.
- Email features require a Gmail account with an App Password (not your regular password).
