---
name: lovable-prompt-builder
description: Guide users through a structured interview to define their app idea, then generate a ready-to-paste prompt for Lovable (or similar vibe coding tools like Bolt, Replit Agent, etc.). Use this skill whenever the user wants to build an app with Lovable, create a Lovable prompt, plan a vibe coding project, needs help defining an app before building it, or mentions "Lovable", "Bolt", "vibe coding", "no-code app builder", or similar tools. Also trigger when the user says things like "I want to build an app that...", "help me plan my app", "create a prompt for Lovable", or "I have an app idea." This skill turns fuzzy app ideas into precise, structured prompts that vibe coding tools can execute cleanly on the first try.
---

# Lovable Prompt Builder

A skill that turns app ideas into well-defined, ready-to-paste prompts for Lovable and similar vibe coding tools. Works through an interactive interview that progressively builds up a complete app specification, then packages it into a single prompt optimized for how these tools actually work.

## First step — always

Read the Lovable best practices reference before starting:

```
view /path/to/skill/references/lovable-best-practices.md
```

This contains the principles that guide how the final prompt should be structured. Read it fresh each time.

## The interview process

The goal is to go from a fuzzy idea to a fully specified app through a structured conversation. Ask 2-3 grouped questions per round. Move through the phases below, but stay conversational — skip questions the user has already answered, and adapt based on what they've told you.

The interview has five phases. Each phase builds on the previous one, and you should summarize what you've captured at the end of each phase before moving to the next.

### Phase 1: The big picture

Start here. Understand the core idea before getting into details.

Questions to cover:

- What does the app do in one sentence?
- Who is it for? (target users, their context, their skill level)
- What is the single most important thing a user should be able to do?
- Is this an MVP/prototype or something more polished?

At the end of this phase, reflect back a one-paragraph product summary and confirm it with the user before proceeding. Getting this wrong means everything downstream is wrong.

### Phase 2: Features and scope

Now define what the app actually does. The goal is a concrete, scoped feature list — not a wish list.

Questions to cover:

- What are the core features? (the things without which the app doesn't make sense)
- Are there secondary features you'd like but could live without for v1?
- Does the app need user authentication? (login, signup, roles)
- Does it need to connect to any external APIs or services? If so, which ones and what data do they provide?
- Does it need a database? What data needs to be stored and persisted?

Help the user scope ruthlessly. If the feature list is growing too long, push back and help them identify the MVP core. Lovable works best with focused, well-scoped prompts — trying to build everything at once leads to messy results.

For each feature, make sure you understand it well enough to write an acceptance criterion — a clear statement of what "done" looks like. You don't need to write these out formally, but you should be able to.

### Phase 3: Pages, layout, and navigation

Define the structure of the app — what screens exist and how users move between them.

Questions to cover:

- What pages or screens does the app have?
- What's on each page? (describe the main sections/components)
- How does the user navigate between pages? (sidebar, top nav, tabs, etc.)
- What's the landing/home experience?
- Are there any modals, drawers, or overlay interactions?

Think in terms of components, not monolithic pages. For each page, identify the key building blocks: a header, a list, a form, a card grid, a chart, a table, etc. This maps directly to how Lovable thinks about UI.

Also consider states:

- What does the page look like when it's loading?
- What about when there's no data yet (empty state)?
- What happens on errors?

### Phase 4: Data model and integrations

If the app has a backend, database, or API integrations, define the data shape.

Questions to cover:

- What are the main entities/objects? (e.g., users, projects, tasks, invoices)
- What fields does each entity have?
- How do entities relate to each other? (a user has many projects, a project has many tasks)
- Which data is user-specific vs. shared?
- If using external APIs: what endpoints, what auth method, what does the response look like?
- If using Supabase or similar: any specific requirements for tables, RLS policies, or real-time subscriptions?

You don't need a formal database schema, but you need enough that Lovable can scaffold the right tables and relationships. Describe entities in plain language with their key fields and connections.

### Phase 5: Visual direction

Define how the app should look and feel — without prescribing implementation.

Questions to cover:

- What's the overall vibe? (e.g., minimal and clean, bold and playful, dark and professional)
- Any color preferences or existing brand colors?
- Are there apps or websites that have a similar look to what you want?
- What kind of layout? (dashboard with sidebar, card-based, list-based, etc.)
- Mobile-first, desktop-first, or both equally important?
- Any specific UI preferences? (dark mode, animations, specific component styles)

Translate the user's answers into Lovable's aesthetic vocabulary — buzzwords like "premium," "cinematic," "playful," "glassmorphism," "brutalist" directly influence the output quality.

## Generating the prompt

Once you've completed the interview (or enough of it to have a solid spec), generate the Lovable prompt.

### Prompt structure

Follow this exact structure, which aligns with how Lovable processes information most effectively:

```
## App Overview
[One paragraph: what it is, who it's for, what problem it solves]

## Features
[Bulleted list of features, grouped logically. Each feature is specific
and concrete — not vague. Core features first, then secondary.]

## Pages & Navigation
[For each page: name, purpose, key components/sections, and how users
get there. Include navigation structure.]

### [Page Name]
- Purpose: ...
- Components: ...
- Key interactions: ...

## Data Model
[Entities with their fields and relationships. Plain language,
not SQL. Include which data is user-scoped.]

### [Entity Name]
- Fields: ...
- Relationships: ...

## API Integrations (if applicable)
[For each external service: what it provides, endpoints used,
authentication method, response shape]

## Visual Direction
[Aesthetic description using Lovable-friendly buzzwords. Cover
vibe, colors, typography direction, layout style. Reference
similar apps if the user provided any.]

## User Flow
[The critical path: what happens from first visit to completing
the key action. Describe the journey, not just the screens.]
```

### Writing principles for Lovable prompts

These are critical — they determine whether Lovable produces a clean app or a mess:

- **Describe what, not how.** Say "a dashboard showing monthly spending trends as a line chart" — not "create a React component using recharts with a useEffect hook." Lovable makes better technical decisions when you stay at the product level.
- **Be specific about features, vague about implementation.** "Users can filter tasks by status, assignee, and due date" is perfect. "Use a dropdown component with a multi-select pattern" is too prescriptive.
- **Use real content.** If the app has a headline, write the actual headline. If there are categories, name them. Lovable generates better layouts with real content than with placeholders.
- **Name things clearly.** Component names, page names, entity names — these become internal references in the generated code. Choose clear, consistent names and stick with them.
- **Scope aggressively.** A focused 20-feature prompt produces dramatically better results than a sprawling 50-feature prompt. If the user's scope is too large, the prompt should note which features are core vs. "nice to have for later iterations."
- **Include states.** For each interactive component, mention empty states, loading states, and error states. This prevents Lovable from only building the happy path.
- **Describe the data shape.** Even a rough entity list with fields and relationships gives Lovable enough to scaffold a proper backend. Without this, it guesses — and it guesses wrong.

### Final checks before saving

Before delivering the prompt, verify:

- Every feature is specific enough that you could test whether it works
- Pages are described with their components, not just names
- Data model covers all the entities the features reference
- Visual direction uses descriptive language, not technical specs
- The prompt reads as a coherent product description, not a disconnected checklist
- Scope is realistic for a single Lovable generation (if too big, note where to split into iterations)

## Delivering the output

Save the final prompt as a clean markdown file named `lovable-prompt-[app-name].md`. The file contains only the prompt — ready to paste directly into Lovable.

After presenting the file, briefly mention in chat:

- Which features you'd recommend building first (if the scope is large)
- Any areas where the user might want to iterate after the first generation
- Whether the prompt should be sent all at once or broken into sequential prompts for better results (for complex apps, Lovable performs better with an initial foundation prompt followed by feature-specific additions)

## Iteration

After the user tests the prompt in Lovable, they may come back with:

- Features that didn't generate correctly
- Layout issues
- Missing states or interactions
- Scope changes

For each iteration, help them write a focused follow-up prompt rather than regenerating the whole thing. Lovable works better with incremental refinement than full regeneration. Re-read the best practices reference to ensure follow-up prompts align with Lovable's component-by-component approach.
