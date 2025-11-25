# SynergeAnnotator

## Project Overview

Synerge Annotator is designed to be a multimodal data annotation website supporting annotation of PDF documents, images, text, and videos. The website will have two user roles: Administrator and Annotator. Users will log in and select a role accordingly. Administrators are responsible for task publishing, LLM (Large Language Model) setup and configuration, and monitoring task progress and consistency; annotators are responsible for reviewing LLM annotation results and making corrections when necessary. The system must support file upload, task assignment, annotation judgment, consistency calculation, and results export to ensure an efficient and accurate annotation workflow.

Target users include administrative and annotation teams, especially for AI training data preparation. The website should have a user-friendly interface, secure authentication, and robust data processing capability.

## User Roles

- **Administrator**: Manages and monitors tasks, with permissions to upload data, configure/connect to LLMs, publish/assign tasks, monitor consistency, and export results.
- **Annotator**: Executes assigned annotation tasks, reviews LLM-generated results, and submits corrections where appropriate.

## Functional Requirements

### Login Interface

- Users log in via username and password.
- Post-login, users select their role: Administrator or Annotator.
- Role switching supported (with re-login or re-authentication for security).
- “Forgot password” feature: reset via email.
- Registration: Admin can create new annotator accounts.

### Administrator Interface

Focuses on task management and monitoring, with sidebar navigation (Task Publishing, Monitoring, Result Export, etc.).

- **Data Upload**:
  - Supports uploading PDFs, images (JPEG/PNG), text (TXT/CSV), and video (MP4/AVI).
  - Bulk upload and file size limitation (e.g., max 500MB per file).
  - Uploaded files stored on server, with previews: thumbnail for PDFs/images, duration for videos, and summary for text.

- **Task Creation & Management**:
  - Create new tasks with name and description.
  - Custom Prompt: specify the LLM input prompt (e.g., “Identify and classify objects in image”).
  - Select LLM model (e.g., GPT-4, Claude) via dropdown.
  - Enter and securely store the API key for chosen LLM.
  - Assign tasks to annotators, set deadlines.
  - Automatically annotate uploaded files via LLM using the given prompt (LLM is called for each file; auto annotations saved, e.g., as JSON: `{file_id: "annotation"}`).

- **Annotation Monitoring**:
  - Real-time task progress overview: completed/total files.
  - Annotator’s annotation records: completion time, number of corrections.
  - Consistency calculation: compare annotator vs. LLM results using metrics (e.g., Cohen’s Kappa or simple accuracy percentage).
    - Filtering by task, annotator, or data type.
    - Visualization: bar or pie charts for consistency rates.

- **Export Annotated Results**:
  - Export formats: CSV, JSON, or Excel.
  - Content includes file ID, LLM annotation, annotator correction (if any), reason, and consistency score.
  - Filtering export data by task or annotator.

### Annotator Interface

Focuses on annotation execution, with a task list and detail view.

- **Task Overview**:
  - List of assigned tasks: name, deadline, progress (completed/total).
  - Sorting and searching by name/date.

- **Annotation Execution**:
  - Select a task to enter its details/annotation view.
  - Preview associated data (PDF viewer, image preview, video player, text editor).
  - LLM-generated annotation results shown with visual emphasis.
  - Judgement options: “Correct” or “Incorrect”.
    - If correct: confirm annotation.
    - If incorrect: input correct result and provide a mandatory reason.
  - Save and continue: automatically loads the next file; progress automatically saved.
  - Submit task upon completing all assigned items.

## Non-Functional Requirements

- **Performance**: Page load time ≤2 seconds; support at least 50 concurrent users.
- **Security**: Use HTTPS; all data stored encrypted; API keys accessible only to admins; RBAC enforced.
- **Usability**: Responsive UI (desktop/mobile); multi-language support (default English).
- **Scalability**: Modular to support additional LLM models/types in the future; cloud storage for large files (e.g., AWS S3).
- **Technical Stack**:
  - Frontend: React (with Ant Design or similar UI framework).
  - Backend: Node.js + Express.
  - Database: MongoDB or PostgreSQL.
  - File storage: Integrated with cloud storage.
  - LLM API integration (e.g., OpenAI SDK).
