<div align="center">

# Lumina-Edu

[简体中文](README.md) | **English**

**From course materials to understanding, with AI at every step.**

An AI-assisted learning workspace for teachers and students.

![React](https://img.shields.io/badge/React-19-149ECA?style=flat-square&logo=react&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-6-646CFF?style=flat-square&logo=vite&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-Backend-3ECF8E?style=flat-square&logo=supabase&logoColor=white)
![Status](https://img.shields.io/badge/Status-Prototype-8B5CF6?style=flat-square)

[Live Site](https://lumina-edu-30.vercel.app) · [Features](#features) · [Local Development](#local-development) · [Project Structure](#project-structure)

</div>

---

## Overview

Lumina-Edu is an AI-assisted teaching and learning prototype for teachers and students. It organizes learning materials into courses and chapters, connecting document processing, page-level summaries, document Q&A, supplementary reading, and quizzes into a workflow from uploading materials to understanding and practice.

Teachers can organize courses, upload materials, and review quiz results. Students can join courses, read materials, ask AI questions, and complete exercises. The project also includes **Lumina Vision**, which uses step-by-step graphical demonstrations to explain computer science concepts.

> This project is a prototype. The features below have corresponding implementations in the repository, but their operation depends on Supabase configuration, database structure, and model API access. They have not necessarily been validated in every deployment environment.

## Features

### For Teachers

- **Course and chapter management**: create courses, maintain course details and chapters, and share course codes with students.
- **Material processing**: upload course materials and track content extraction, reading generation, and quiz generation.
- **AI-assisted preparation**: generate supplementary reading, chapter quizzes, and summaries of specific pages.
- **Learning feedback**: review student quiz submissions and scores.
- **Course communication**: publish announcements and participate in course group chats.

### For Students

- **Course workspace**: join courses and browse materials, supplementary reading, and quizzes by chapter.
- **Document Q&A**: ask AI questions grounded in the current chapter's materials.
- **Page-level summaries**: extract concepts, definitions, and key points from individual pages.
- **Practice and review**: complete quizzes and submit answers.
- **Visual learning**: enter a computer science topic in Lumina Vision to explore nodes, connections, and step-by-step explanations.

### Technology

- **Interface**: React 19, TypeScript, Tailwind CSS 4, Lucide icons, and Motion animations.
- **Server**: Express with Vite integration, serving material-processing, Q&A, and summary endpoints.
- **Data layer**: Supabase Auth, PostgreSQL, Storage, and realtime subscriptions.
- **AI**: Qwen document processing and Q&A alongside Gemini content generation, with model fallback logic in selected processing paths.
- **Visualization**: Gemini generates structured teaching steps, and D3 renders interactive diagrams.

## Learning Workflow

1. A teacher creates a course and chapters, then uploads learning materials.
2. The system processes the materials and generates supplementary reading and chapter quizzes.
3. Students enter the course, read the materials, and use page summaries and AI Q&A to deepen their understanding.
4. Students submit exercises, and the teacher reviews the results.

## Local Development

You will need Node.js 22 LTS, npm, a separate Supabase test project, and credentials for the model APIs you intend to use.

### 1. Clone the Repository

```bash
git clone https://github.com/langezhang/Lumina-Edu.git
cd Lumina-Edu/lumina-edu
npm ci
```

The application lives in the nested `lumina-edu/` directory. Run all subsequent commands there.

### 2. Configure the Environment

Create `.env.local` in the application directory and supply your own values:

```dotenv
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-supabase-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-server-side-service-role-key

DEFAULT_AI_PROVIDER=qwen
QWEN_API_KEY=your-qwen-api-key
QWEN_BASE_URL=https://dashscope-intl.aliyuncs.com/compatible-mode/v1
QWEN_DASHSCOPE_BASE_URL=https://dashscope-intl.aliyuncs.com/api/v1/services/aigc/text-generation/generation
QWEN_DOC_MODEL=qwen-doc-turbo
LLM_MODEL=qwen-plus

# Optional: Gemini material processing, summaries, and Lumina Vision
GEMINI_API_KEY=your-gemini-api-key
```

Qwen endpoints and model access must match your account. The document Q&A endpoint currently uses Qwen, so configuring Gemini alone will not enable every feature. Lumina Vision uses a Gemini model specified in the source code; confirm that your account can access it.

### 3. Prepare the Database and Storage

Use the [database script](lumina-edu/supabase-schema.sql) and [storage script](lumina-edu/supabase-storage.sql) as references when configuring your test project and the `course-materials` storage bucket.

**The SQL files are prototype setup references, not a complete migration set.** The initialization script includes `DROP TABLE` statements and should only be used in a test environment whose data can safely be recreated. Current application code also references chapter fields such as `extracted_text`, `content_status`, `processing_stage`, `processing_progress`, and `processing_error`. Check and add the required schema before using those features; running the older script alone does not complete setup.

### 4. Start the Development Server

```bash
npm run dev
```

Visit [http://localhost:3000](http://localhost:3000) and explore the teacher and student workflows. AI operations require the services above to be configured correctly.

## Project Structure

```text
Lumina-Edu/
├── README.md                   # Chinese documentation
├── README.en.md                # English documentation
└── lumina-edu/
    ├── src/
    │   ├── App.tsx                 # Application entry and session management
    │   ├── components/
    │   │   ├── TeacherDashboard.tsx # Teacher workspace
    │   │   ├── StudentTutor.tsx     # Student learning workspace
    │   │   ├── QuizViewer.tsx       # Quizzes and submissions
    │   │   ├── GroupChat.tsx        # Course group chat
    │   │   └── lumina-vision/       # AI concept visualization
    │   └── lib/supabase.ts         # Supabase client
    ├── api/                       # Material processing, Q&A, and page summaries
    ├── server.ts                  # Express server and Vite integration
    ├── build-server.ts            # Server bundling
    ├── supabase-schema.sql         # Prototype database reference
    └── supabase-storage.sql        # Course-material storage reference
```

## Development & Deployment

```bash
npm run dev    # Start the full development server
npm run lint   # Run TypeScript type checking
npm run build  # Build the frontend and server
```

For production, set `NODE_ENV=production` before running `npm start`. The `npm run preview` command previews the frontend build only; it does not replace the Express API server.

Before a public deployment, address the prototype's existing limitations: the SQL access policies are permissive, and the course-material bucket allows public reads. The Vite configuration also injects `GEMINI_API_KEY` into the frontend for Vision. Tighten data access and move Gemini calls to the server before using real classroom data. Keep `SUPABASE_SERVICE_ROLE_KEY` exclusively in the server environment.

## Roadmap

- [ ] Complete database migrations and a repeatable setup process
- [ ] Refine access controls for teachers, students, and courses
- [ ] Move visualization model calls to the server
- [ ] Add automated tests for material processing, Q&A, and quizzes
- [ ] Improve model configuration, retry handling, and deployment documentation

## Feedback & Contributions

Share feedback and feature ideas through [Issues](https://github.com/langezhang/Lumina-Edu/issues). When reporting a problem, include steps to reproduce it, the expected behavior, and error details with sensitive information removed.
