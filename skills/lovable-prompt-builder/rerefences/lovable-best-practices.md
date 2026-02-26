# Lovable Prompting Best Practices

Distilled from Lovable's official documentation and community guides. Read this before generating any Lovable prompt.

## Core Principles

### Describe what, not how
Lovable works best when you describe what the app should do — not how to code it. Keep prompts high-level and functional. Avoid implementation details like specific React patterns or file structures. Let Lovable make the technical decisions.

### Plan before you prompt
Before writing anything, answer four questions:
- What is this product or feature?
- Who is it for?
- Why will they use it?
- What is the one key action the user should take?

Vague ideas produce vague outputs. Clear thinking leads to clear results.

### Build by component, not by page
Prompting for an entire page at once produces noisy, hard-to-refine results. Build section by section — a hero, a feature grid, a pricing table. Each component has one clear purpose. This gives you control and makes iteration easier.

However, for the **initial prompt** (the first prompt in a new project), describe the full app holistically. The initial prompt sets the foundation — pages, navigation, data model, overall structure. Subsequent prompts should then refine component by component.

### Use real content, not placeholders
Lovable responds poorly to "lorem ipsum" or "Feature 1 / Feature 2." Use real copy that reflects your message. Real content creates the right layout constraints and reveals design issues early.

### Think in states
Always consider what happens when data is empty, loading, or fails. Shape UI as if the backend is already there — this future-proofs your layout even during the design phase.

---

## Effective Prompt Structure

The structure that works best with Lovable follows this pattern:

**Purpose** → **Features** → **User Flow** → **Data Model** → **UI Style** → **Extras**

### Purpose
One clear sentence about what the app does and who it's for.

### Features
A scoped list of what the app can do. Each feature should be concrete and specific enough that you'd know whether it was built correctly.

### User Flow
The journey from entry to key action. What does the user see first? What builds trust? Where does the action lead? Map transitions, not just screens.

### Data Model
What entities exist and how they relate. If there's a backend (Supabase, external API), describe the data shape. Lovable uses this to scaffold tables, relationships, and queries.

### UI Style
Use aesthetic buzzwords that Lovable understands. Terms like "minimal," "bold," "premium," "cinematic," "playful," "developer-focused" directly influence typography, spacing, shadows, and color. Be specific about:
- Overall vibe/aesthetic direction
- Color palette or mood
- Typography preferences
- Layout patterns (cards, grids, kanban, dashboard)

### Extras
Authentication, responsive design, animations, dark mode, deployment considerations.

---

## What Makes a Good Initial Prompt

A strong initial prompt for Lovable includes:
- A clear product description with target audience
- A complete feature list (scoped to MVP)
- Page/screen breakdown with navigation structure
- Data model showing entities and relationships
- API integration details (if connecting to external services)
- Visual direction using descriptive language

A strong initial prompt does NOT include:
- Implementation specifics (file names, React patterns, specific libraries)
- Overly technical architecture decisions
- Multiple competing design directions
- Placeholder content

---

## Iteration Tips

- Make one meaningful change at a time
- Use Plan mode and ask Lovable to ask clarifying questions
- When debugging, paste error messages directly
- Refactor periodically to keep code clean
- Name components clearly — labels become internal references and routes
- Avoid renaming things after initial creation (causes cascading changes)

---

## Backend Integration Patterns

When connecting to Supabase or external APIs:
- Describe the data schema with entity names, fields, and relationships
- Specify authentication requirements (login, signup, roles)
- Define which data is user-specific vs. shared
- Describe API endpoints, request/response shapes, and auth headers
- Consider edge cases: what happens when the API is down, data is missing, or auth expires