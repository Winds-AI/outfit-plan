# Agent Workflow Guide

## 1. Plan Structure
The project plan is split into three separate files to maintain clarity and separation of concerns:

1.  **`Logic_Plan.md`**: Core business logic, functional requirements, and system architecture.
2.  **`Database_Plan.md`**: Database schema, storage buckets, and data relationships.
3.  **`UI_Plan.md`**: UI/UX specifications, design system, and frontend details.

## 2. Cross-Referencing
When a part of one file depends on or relates to another, use the following reference syntax:
`[[Ref: Filename.md#Section-ID]]`

**Examples:**
- In `Logic_Plan.md`: "Store user photo. [[Ref: Database_Plan.md#user_photos]]"
- In `UI_Plan.md`: "Display wardrobe grid. [[Ref: Logic_Plan.md#FR-3.4]]"

## 3. Workflow Rules

### Reading Context
- When starting a task, **identify which domain** (Logic, DB, UI) is primary.
- **Read the primary plan file** completely.
- **Check referenced sections** in other files to ensure consistency.

### Updating Plans
- **Single Source of Truth:** If you change a requirement in `Logic_Plan.md`, you **MUST** check if it impacts `Database_Plan.md` (schema changes) or `UI_Plan.md` (interface changes).
- **Update All:** Update all affected files in the same turn or task to keep them in sync.

### Implementation
- **Planning Phase:** Do not code until the relevant plan files are updated and approved.
- **Verification:** Ensure that what is implemented matches the specific details in the plan files.

## 4. General Guidelines
- Only render 50 lines at a time from plan files if they get too large, to avoid context overload.
- Generate infographics and images ( if you can ) about flow of app and architecture where necessary to understand properly.
- **Ask questions openly** without limits. Take time to think about the future and plan properly—there's no hurry. Check available tools and MCP servers; use them when helpful.