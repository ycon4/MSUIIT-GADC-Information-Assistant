# GIA: Gender and Development Center Information Assistant

GIA is a web-based, AI-powered information assistant for the **Gender and Development Center (GADC)** of **Mindanao State University - Iligan Institute of Technology (MSU-IIT)**. It lets GADC staff query sex-disaggregated data (SDD) on students, employees, and events using plain language, and pairs the chat assistant with dashboards, event management tools, and a public analytics portal.

Developed as a capstone project by Andrei G. Raagas.

## Features

### AI Chat Assistant
- Ask questions in natural language, such as enrollment by college, sex breakdowns, vulnerability indicators (PWD, solo parent, first-generation learner, indigenous), and event attendance
- Answers arrive as structured text, markdown tables, and on-the-fly charts
- Prompt-augmented design: each query is parsed into a structured intent, matched against live Firestore data, and answered under a dual-layer prompt (a permanent system prompt plus response rules that keep answers faithful to the data)
- Powered by the Groq API (`qwen/qwen3-32b`)

### Data Distribution
- Visual dashboards for student enrollment and employee information
- Excel upload for enrollment and employment data with validation
- Report generation (PDF and Word) with MSU-IIT header and footer

### Event Management
- Create, edit, submit, and withdraw events (draft and review workflow)
- QR-based attendance with session QR management
- Public event listing and public registration form
- Event poster generator, printable attendance sheets, and report exporter
- Event analytics with sex-disaggregated attendance

### Public Portal
- Read-only analytics for the public, without requiring a login

### Access Control
- Firebase Authentication with three roles: **User**, **Secretariat**, and **Admin**
- Role-based permissions for chat, data viewing, data import and deletion, event handling, and user management
- Admin-only user creation and role assignment

## System Overview

The pipeline and response rules are documented in the diagrams included in this repository:

- `GIA_pipeline_flowchart.svg`: end-to-end query pipeline
- `GIA_response_rules.svg`: response rules applied to model output

## Tech Stack

- **Frontend:** React 19, Vite, Tailwind CSS, React Router, Recharts, Chart.js, Lucide
- **AI:** Groq API (Qwen3 32B), with the system prompt stored in `backend/prompt.txt`
- **Backend:** Node.js and Express (local), Vercel serverless functions (deployment)
- **Database and Auth:** Firebase (Cloud Firestore and Authentication)
- **Documents and Data:** docx, jsPDF, html2canvas, SheetJS (xlsx), PapaParse, qrcode

## Project Structure

```
.
├── api/                  Vercel serverless functions (chat, parse-intent)
├── backend/              Local Express server and system prompt (prompt.txt)
├── firebase/             Firebase config, auth, and Firestore services
├── public/               Logos, report header and footer images
├── src/
│   ├── components/       Chat, events, excel upload, and visuals components
│   ├── contexts/         Role and permission context
│   ├── pages/            Chat, Distribution, Events, Public Portal, Users, About
│   ├── services/         AI service (intent parsing and data context)
│   └── utils/            Demo data seeding
├── firestore.rules       Firestore security rules
├── vercel.json           Vercel configuration
└── package.json
```

## Getting Started

### Prerequisites

- Node.js 18 or higher
- A Firebase project with Authentication (Email/Password) and Cloud Firestore enabled
- A Groq API key from [console.groq.com/keys](https://console.groq.com/keys)

### Installation

```bash
git clone https://github.com/ycon4/MSUIIT-GADC-Information-Assistant.git
cd MSUIIT-GADC-Information-Assistant

# Frontend dependencies
npm install

# Backend dependencies
cd backend
npm install
cd ..
```

### Environment Variables

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key_here
PORT=3001
```

### Firebase Setup

1. Replace the values in `firebase/config.js` with your own Firebase project settings.
2. Deploy the security rules in `firestore.rules` to your project.
3. Create your first user in Firebase Authentication, then add a matching document in the `users` collection with a `role` field (`ADMIN`, `SECRETARIAT`, or `USER`).

The app reads and writes these Firestore collections: `student_enrollment`, `employee_information`, `events`, `attendance`, and `users`.

### Running Locally

Run the backend and the frontend in separate terminals.

```bash
# Terminal 1: backend (http://localhost:3001)
cd backend
npm start

# Terminal 2: frontend (http://localhost:5173)
npm run dev
```

Check that the backend is up: `http://localhost:3001/api/health`

### Build for Production

```bash
npm run build
```

The output is written to `dist/`.

### Deployment

The project is set up for **Vercel**. The `api/` folder is deployed as serverless functions, and `vercel.json` rewrites all other routes to the single-page app. Add `GROQ_API_KEY` as an environment variable in your Vercel project settings.

## Additional Documentation

- `VERIFICATION_CHECKLIST.md`: steps for verifying the Firebase read-quota fixes (data caching and pagination)
- `MSUIIT_CHART_THEME.md`: the MSU-IIT maroon and gold chart theme

## Data Privacy

GIA works with student and employee records. Limit access to authorized GADC personnel, keep API keys out of version control, and review `firestore.rules` before deploying to a real environment.

## Authors


Andrei G. Raagas
Marhamah Ali
Sittie Hanifa D. Yusoph
BS Information Technology, MSU-IIT
