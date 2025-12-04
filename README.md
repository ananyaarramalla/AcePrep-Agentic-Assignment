# 🤖 AcePrep – AI Interview Practice Partner

![Status](https://img.shields.io/badge/Status-Completed-success)
![Tech](https://img.shields.io/badge/Tech-Next.js%20%7C%20Gemini%202.0%20%7C%20VAPI-blueviolet)

> **Assignment Submission for Eightfold.ai** > *Problem Statement 2: Interview Practice Partner*

**AcePrep** is a fully agentic, voice-first mock interview platform. It moves beyond static text generation by offering a real-time, conversational voice agent that adapts its tone to the user’s chosen interview style and maintains a structured, natural interview flow, generating detailed feedback upon completion.

---

## 🔗 Quick Links
- **Live Demo:** [https://ace-prep-umber.vercel.app/](https://ace-prep-umber.vercel.app/)

---

## 🧠 Design Decisions & Architecture

### 1. Why VAPI.ai for Voice Orchestration?
* **Reasoning:** Building a raw WebSocket layer for voice introduces significant latency and state management complexity.
* **Decision:** I chose VAPI as the orchestration layer to minimize "Turn-Taking" latency. This ensures the AI doesn't interrupt the user while thinking, which is critical for the "Conversational Quality" criteria.

### 2. Gemini 2.0 Flash for Intelligence
* **Reasoning:** Voice agents require sub-second inference speeds.
* **Decision:** Gemini 2.0 Flash was selected over GPT-4o for its superior speed-to-cost ratio and its ability to adhere strictly to JSON schemas for the interview generation phase.

### 3. Serverless Next.js + Firebase
* **Reasoning:** The assignment requires a public demo.
* **Decision:** A serverless architecture ensures the application scales automatically during the demo phase without server maintenance. Firebase Firestore provides real-time data syncing, allowing the dashboard to update immediately once an interview is generated.

---

## 🤖 Agentic Behavior

The agent includes the following actual behaviours as required by the assignment:
- Tone adaptation → fast, friendly, or detailed
- Structured question progression (increasing difficulty)
- Tech-stack-aware question generation
- Role & level contextualization
- JSON-controlled generation with no hallucinations
- Backend structured AI feedback after interview

---

## 🚀 Features

### ✅ Dynamic Interview Generation
The voice agent acts as a recruiter first. It collects:
- **Role** (e.g., Data Analyst, Frontend Dev)
- **Tech Stack** (e.g., React, SQL, Python)
- **Experience Level**
- **Interview Style** (Fast vs. Friendly)

### ✅ Real-Time Voice Interview
- **Latency Optimization:** optimized for natural pauses.
- **Context Awareness:** The agent remembers previous answers (context window) to ask relevant follow-up questions.

### ✅ Automated Feedback Loop
Once the call ends, a background process triggers:
1.  Transcript analysis.
2.  Scoring based on **Technical Accuracy**, **Communication**, and **Problem Solving**.
3.  Generation of a "Strengths & Weaknesses" report.

---

## 🖼️ Screenshots

Sign In Page
<img width="958" height="487" alt="image" src="https://github.com/user-attachments/assets/bbb6dda8-5fc7-4a50-ab3c-76abdd5ba2c4" />

Sign Up Page
<img width="956" height="490" alt="image" src="https://github.com/user-attachments/assets/d63ef832-309c-4642-b9f6-076d3ac4d76a" />

Home Page
<img width="944" height="392" alt="image" src="https://github.com/user-attachments/assets/d124ab4b-82c2-421e-8385-293e81cdbe2e" />
<img width="939" height="485" alt="image" src="https://github.com/user-attachments/assets/f77b6092-4d0d-4d6e-beeb-b7f715c13bac" />

Interview Page
<img width="944" height="496" alt="image" src="https://github.com/user-attachments/assets/69e9597d-7b12-4071-a2ea-a3dd2493dce9" />

Feedback Page
<img width="945" height="494" alt="image" src="https://github.com/user-attachments/assets/7d46d31b-04ed-4435-8b80-b22de962b85c" />

---

## 🏗 Project Structure

```bash
.
├── app/
│   ├── api/
│   │   ├── generate/
│   │   │   └── route.tsx          # AI generation logic (Gemini)
│   │   └── feedback/
│   │       └── route.tsx          # Feedback generation logic
│   ├── (root)/
│   │   └── dashboard/
│   │       └── page.tsx           # Dashboard (Your vs Public interviews)
│   ├── interview/
│   │   └── [id]/
│   │       └── page.tsx           # Interview Room (Agent & Vapi)
│   ├── globals.css                # Global styles
│   ├── layout.tsx                 # Root layout
│   └── page.tsx                   # Landing page
├── components/
│   ├── ui/                        # Shadcn UI components (Button, Card, etc.)
│   ├── Agent.tsx                  # Vapi Voice Client
│   ├── DisplayTechIcons.tsx       # Tech stack icon renderer
│   └── InterviewCard.tsx          # Dashboard card component
├── constants/
│   └── index.ts                   # Static data & Prompts
├── firebase/
│   └── admin.ts                   # Firebase Admin configuration
├── lib/
│   ├── actions/
│   │   ├── auth.action.ts         # Authentication logic
│   │   └── general.action.ts      # Firestore fetch/save logic
│   ├── utils.ts                   # Helpers (Icon mapping, cn)
│   └── vapi.sdk.ts                # Vapi SDK init
└── types/
    └── index.d.ts                 # TypeScript interfaces (Interview, User)
```

## 🛠️ Tech Stack

* **Frontend:** Next.js 14 (App Router), TailwindCSS, ShadCN UI
* **AI Logic:** Google Gemini 2.0 Flash
* **Voice Pipeline:** VAPI (Voice Agent Protocol Interface)
* **Database:** Firebase Firestore
* **Auth:** Custom Firebase Authentication (Session Cookies)
* **Deployment:** Vercel

---

## 🔌 Setup Instructions

### 1. Clone the repository
```bash
git clone https://github.com/ananyaarramalla/AcePrep-Agentic-Assignment.git
cd AcePrep-Agentic-Assignment
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Environment Variables
Create a .env.local file in the root directory and add the following keys:
```env
NEXT_PUBLIC_VAPI_PUBLIC_KEY=your_vapi_key
NEXT_PUBLIC_VAPI_WORKFLOW_ID=your_workflow_id
FIREBASE_PROJECT_ID=your_firebase_id
FIREBASE_CLIENT_EMAIL=your_email
FIREBASE_PRIVATE_KEY=your_private_key
GOOGLE_API_KEY=your_gemini_key
```

### 4. Run Locally
```bash
npm run dev
```
Visit http://localhost:3000 to view the application.

## 🧪 Testing The Flow

- 1.  **Sign In:** Access the dashboard via the login page.
- 2.  **Generate:** Click "Start New Interview". Speak to the voice agent to set your preferences (Role, Tech Stack, etc.).
- 3.  **Verify:** Check the Firestore database to see the JSON object created with your specific questions.
- 4.  **Interview:** Enter the interview room and complete the mock session with the AI agent.
- 5.  **Feedback:** Wait ~10 seconds after ending the call to see the generated feedback card with scores and improvements.

---

**Author:** Ananya Arramalla

*Submitted for Eightfold.ai AI Agent Assignment*
