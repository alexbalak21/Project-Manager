# Project Manager  
A minimal, developer‑centric app to manage personal projects, ideas, notes, and todos — fully integrated with GitHub.

## Project overview  
A lightweight dashboard to track all personal projects, sync metadata from GitHub, manage tasks, store notes, and visualize progress.

---

## Core features

### 1. Project management  
Each project contains metadata, links, tasks, notes, and optional participants.

#### Project data model

- **id:** `INT (PK)` — Unique project identifier  
- **name:** `VARCHAR` — Project name  
- **image:** `VARCHAR` or `BLOB` — Thumbnail or logo  
- **description:** `TEXT` — Auto‑fetched from GitHub README or manually edited  
- **start_date:** `DATE` — When the project started  
- **due_date (optional):** `DATE` — Deadline or target milestone  
- **github_link:** `VARCHAR` — Repository URL  
- **live_demo_link (optional):** `VARCHAR` — Deployed version  
- **tech_stack:** `JSON` or `VARCHAR` — Array/list of technologies  
- **advancement:** `INT` or `ENUM` — Percentage or status (e.g. `not_started`, `in_progress`, `done`)  
- **created_at:** `TIMESTAMP`  
- **updated_at:** `TIMESTAMP`  

#### Relationships

- **Project → Notes:** one‑to‑many  
- **Project → Todos/Tasks:** one‑to‑many  
- **Project → Participants (optional):** one‑to‑many (via join table)  
- **Project ↔ Skills (optional):** many‑to‑many  

---

### 2. Task / todo system  
Tasks are grouped by category and can optionally sync with GitHub issues or PRs.

#### Task data model

- **id:** `INT (PK)` — Task ID  
- **project_id:** `INT (FK)` — Linked project  
- **title:** `VARCHAR` — Short description  
- **description:** `TEXT` — Optional details  
- **category:** `ENUM('todo', 'improvement', 'security', 'frontend', 'backend')`  
- **status:** `ENUM('todo', 'in_progress', 'completed')`  
- **github_issue_id (optional):** `INT` — Link to GitHub issue  
- **created_at:** `TIMESTAMP`  
- **updated_at:** `TIMESTAMP`  

#### GitHub integration (tasks)

- Fetch GitHub issues → auto‑create tasks  
- Mark tasks completed → optionally auto‑close GitHub issue  
- Track commits referencing tasks (e.g. `#task-123` or `#issue-123`)  
- Compute project advancement based on completed tasks vs total tasks  

---

### 3. Notes system  
Notes can be plain text, rich text, or voice‑to‑text. Notes can be converted into todos.

#### Notes data model

- **id:** `INT (PK)` — Note ID  
- **project_id:** `INT (FK)` — Linked project  
- **content:** `TEXT` — Rich text content (Markdown or HTML)  
- **audio_url (optional):** `VARCHAR` — Stored audio file path/URL  
- **transcript (optional):** `TEXT` — Speech‑to‑text result  
- **can_convert_to_todo:** `BOOLEAN` — Flag to allow quick conversion to task  
- **created_at:** `TIMESTAMP`  
- **updated_at:** `TIMESTAMP`  

#### Notes features

- Rich text editor (Markdown or WYSIWYG)  
- Microphone icon → record audio → convert to text on the fly  
- One‑click: “Convert note to task”  
- Optional tagging (e.g. `idea`, `bug`, `refactor`, `research`)  

---

### 4. Skills system (optional)  
Useful for tagging projects by technologies or tracking your learning.

#### Skill data model

- **id:** `INT (PK)` — Skill ID  
- **name:** `VARCHAR` — Skill name (e.g. `React`, `Spring Boot`, `Docker`)  
- **image:** `BLOB` or `VARCHAR` — Icon or logo  
- **description (optional):** `TEXT` — What the skill represents  

#### Skill relationships

- **Project ↔ Skill:** many‑to‑many via `project_skills` table:  
  - `project_id (FK)`  
  - `skill_id (FK)`  

Optional: auto‑detect skills from GitHub repo languages API and suggest skills to attach.

---

### 5. Participants (optional)  
If you want to track collaborators on a project.

#### User data model

- **id:** `INT (PK)` — User ID  
- **username:** `VARCHAR` — Display name  
- **avatar:** `VARCHAR` — Avatar URL  
- **github_profile:** `VARCHAR` — GitHub profile link  

#### Project participants join table

- **project_id:** `INT (FK)`  
- **user_id:** `INT (FK)`  
- **role:** `ENUM('owner', 'contributor', 'reviewer')`  

---

## Optional enhancements

### GitHub sync enhancements

- Auto‑fetch README description → populate project description  
- Auto‑fetch repo languages → populate `tech_stack`  
- Auto‑fetch open issues → create tasks  
- Auto‑fetch PRs → show progress and activity  
- Commit activity graph per project  

### Dashboard features

- Global search (projects, tasks, notes)  
- Filter by tech stack, status, category  
- Progress visualization (per project and global)  
- Calendar view for due dates  
- Kanban board for tasks (`todo`, `in_progress`, `done`)  
- Dark/light theme support  

### Automation ideas

- Daily cron job to sync GitHub data  
- Auto‑archive inactive projects (no commits for X days)  
- Auto‑generate changelog from commits  
- Optional AI‑generated project summaries  

---

## API structure (minimal & clean)

### REST endpoints (example)

- `GET /projects` — List projects  
- `POST /projects` — Create project  
- `GET /projects/:id` — Get project details  
- `PUT /projects/:id` — Update project  
- `DELETE /projects/:id` — Delete project  

- `GET /projects/:id/tasks` — List tasks for a project  
- `POST /projects/:id/tasks` — Create task  
- `PUT /tasks/:id` — Update task  
- `DELETE /tasks/:id` — Delete task  

- `GET /projects/:id/notes` — List notes  
- `POST /projects/:id/notes` — Create note  
- `PUT /notes/:id` — Update note  
- `DELETE /notes/:id` — Delete note  

- `GET /skills` — List skills  
- `POST /skills` — Create skill  
- `POST /projects/:id/skills/:skillId` — Attach skill to project  

- `GET /projects/:id/participants` — List participants  
- `POST /projects/:id/participants` — Add participant  

- `POST /github/sync/:projectId` — Trigger GitHub sync for a project  

---

## Tech stack suggestions

### Backend

- Node.js + Express/Fastify **or** Spring Boot  
- PostgreSQL or MySQL  
- ORM: Prisma (Node) or JPA/Hibernate (Java)  
- GitHub API (REST or GraphQL)  

### Frontend

- React + Vite  
- Minimal CSS or Tailwind  
- Rich text editor: TipTap, Quill, or Markdown editor  
- Web Speech API or external STT service for voice notes  

---
