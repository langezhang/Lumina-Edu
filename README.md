<div align="center">

# Lumina-Edu

**从课件到理解，让 AI 参与每一步学习。**

An AI-assisted learning workspace for teachers and students.

![React](https://img.shields.io/badge/React-19-149ECA?style=flat-square&logo=react&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-6-646CFF?style=flat-square&logo=vite&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-Backend-3ECF8E?style=flat-square&logo=supabase&logoColor=white)
![Status](https://img.shields.io/badge/Status-Prototype-8B5CF6?style=flat-square)

[在线入口 / Live Site](https://lumina-edu-30.vercel.app)

</div>

---

点击下方语言标题展开或收起内容，无需离开当前页面。

Click a language heading below to expand or collapse its content on this page.

<details open>
<summary><strong>简体中文</strong> · 点击展开 / 收起</summary>

[功能概览](#功能概览) · [本地开发](#本地开发) · [项目结构](#项目结构)

## 项目介绍

Lumina-Edu 是面向教师与学生的 AI 辅助教学原型。它围绕课程和章节组织学习材料，将课件处理、逐页摘要、文档问答、拓展阅读和练习测验连接起来，探索从「上传资料」到「理解与巩固」的学习过程。

教师可以组织课程、上传资料并查看测验情况；学生可以加入课程、阅读材料、向 AI 提问并完成练习。项目还包含 **Lumina Vision**，通过分步骤图形演示帮助理解计算机科学概念。

> 项目处于原型阶段。以下功能已有对应代码，实际使用依赖 Supabase 配置、数据库结构与模型服务权限；不代表所有部署环境都已完成验证。

## 功能概览

### 面向教师

- **课程与章节管理**：创建课程、维护课程信息与章节，使用课程码连接学生。
- **课件处理**：上传课程材料，查看内容提取、阅读材料生成与测验生成的处理进度。
- **AI 辅助备课**：生成拓展阅读、章节测验及指定页的内容摘要。
- **学习反馈**：查看学生测验提交与成绩信息。
- **课程沟通**：发布课程公告，参与课程群聊。

### 面向学生

- **课程学习空间**：加入课程，按章节浏览资料、拓展阅读和测验。
- **课件问答**：结合当前章节材料向 AI 提问。
- **逐页摘要**：按页提炼课件中的概念、定义和重点。
- **练习巩固**：完成测验并提交答案。
- **可视化学习**：在 Lumina Vision 中输入计算机科学主题，查看节点、连线与分步骤说明。

### 技术实现

- **界面**：React 19、TypeScript、Tailwind CSS 4、Lucide 图标与 Motion 动画。
- **服务端**：Express + Vite 开发服务，提供课件处理、问答和摘要接口。
- **数据层**：Supabase Auth、PostgreSQL、Storage 与实时订阅。
- **AI 能力**：Qwen 文档处理/问答与 Gemini 内容生成；部分处理路径包含模型回退逻辑。
- **知识可视化**：Gemini 生成结构化教学步骤，D3 渲染交互图形。

## 学习流程

1. 教师创建课程和章节，并上传学习材料。
2. 系统处理课件，生成拓展阅读与章节测验。
3. 学生进入课程，阅读资料，使用页摘要和 AI 问答辅助理解。
4. 学生提交练习，教师查看学习反馈。

## 本地开发

准备 Node.js 22 LTS、npm、独立的 Supabase 测试项目，以及所需的模型 API 凭据。

### 1. 获取项目

```bash
git clone https://github.com/langezhang/Lumina-Edu.git
cd Lumina-Edu/lumina-edu
npm ci
```

应用位于仓库内的 `lumina-edu/` 子目录，后续命令均在该目录执行。

### 2. 配置环境

在应用目录新建 `.env.local`，填写自己的配置：

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

# 可选：Gemini 课件处理/摘要及 Lumina Vision
GEMINI_API_KEY=your-gemini-api-key
```

Qwen 服务地址与模型权限需要匹配自己的账号。文档问答接口当前使用 Qwen；仅配置 Gemini 并不能覆盖全部功能。Lumina Vision 当前使用代码中指定的 Gemini 模型，需确认账号能够访问。

### 3. 准备数据库与文件存储

参考 [数据库脚本](lumina-edu/supabase-schema.sql) 和 [存储脚本](lumina-edu/supabase-storage.sql) 配置测试项目及 `course-materials` 存储桶。

**现有 SQL 是原型初始化参考，并非完整迁移集。** 初始化脚本包含 `DROP TABLE`，只应在确认可重建的测试环境中使用。当前业务代码还引用了 `extracted_text`、`content_status`、`processing_stage`、`processing_progress`、`processing_error` 等章节字段，需先核对并补齐对应结构，不能仅执行旧脚本就视为初始化完成。

### 4. 启动开发服务

```bash
npm run dev
```

访问 [http://localhost:3000](http://localhost:3000)。通过教师/学生入口体验课程流程；AI 操作需要前述服务配置有效。

## 项目结构

```text
Lumina-Edu/
├── README.md
├── README.en.md                # English documentation
└── lumina-edu/
    ├── src/
    │   ├── App.tsx                 # 应用入口与会话管理
    │   ├── components/
    │   │   ├── TeacherDashboard.tsx # 教师工作台
    │   │   ├── StudentTutor.tsx     # 学生学习空间
    │   │   ├── QuizViewer.tsx       # 测验与提交
    │   │   ├── GroupChat.tsx        # 课程群聊
    │   │   └── lumina-vision/      # AI 知识可视化
    │   └── lib/supabase.ts         # Supabase 客户端
    ├── api/                       # 课件处理、文档问答、页摘要
    ├── server.ts                  # Express 服务与 Vite 集成
    ├── build-server.ts            # 服务端打包
    ├── supabase-schema.sql        # 原型数据库参考
    └── supabase-storage.sql       # 课件存储配置参考
```

## 开发与部署说明

```bash
npm run dev    # 启动完整开发服务
npm run lint   # TypeScript 类型检查
npm run build  # 构建前端与服务端
```

生产模式需要先设置 `NODE_ENV=production`，再运行 `npm start`。`npm run preview` 仅用于预览前端构建，不替代 Express API 服务。

发布到公开环境前，需要处理当前原型的具体限制：SQL 中的访问策略较宽松，课件桶允许公开读取；Vite 配置会将 `GEMINI_API_KEY` 注入前端供 Vision 使用。应收紧数据权限并将 Gemini 调用迁移到服务端，再用于真实课堂数据。`SUPABASE_SERVICE_ROLE_KEY` 始终只应放在服务端环境中。

## 后续方向

- [ ] 补齐数据库迁移与可重复的初始化流程
- [ ] 完善教师、学生和课程之间的数据权限
- [ ] 将可视化模型调用统一迁移到服务端
- [ ] 增加课件处理、问答和测验流程的自动化测试
- [ ] 改进模型配置、失败重试与部署文档

## 交流与反馈

欢迎通过 [Issues](https://github.com/langezhang/Lumina-Edu/issues) 分享使用反馈与功能建议。提交问题时，请提供复现步骤、预期行为和经过脱敏的错误信息。

</details>

<details>
<summary><strong>English</strong> · Click to expand / collapse</summary>

[Features](#features) · [Local Development](#local-development) · [Project Structure](#project-structure)

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

</details>
