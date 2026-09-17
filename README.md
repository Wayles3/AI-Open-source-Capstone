# Contribution [1]: [Dashboard header says "1 Applications" instead of "1 Application"
 ]

**Contribution Number:** [1]  
**Student:** [Adewale Abodunde]  
**Issue:** [https://github.com/shanker-codepath/offer-tracker/issues/7]  
**Status:** [Phase I] [Completed]
**Status:** [Phase II] [Completed]


---

## Why I Chose This Issue

[It is a well-scoped UI bug with immediate user impact. Fixing incorrect pluralization improves visual polish, ensures compliance with basic UI/UX formatting standards, and presents a low-risk, high-value entry point for contributing to the codebase.]

---

## Understanding the Issue

### Problem Description

[The dashboard header lacks conditional pluralization logic. It uses a static label string rather than dynamically switching between singular ("1 Application") and plural ("X Applications") based on the count of retrieved records.]

### Expected Behavior

[When the application count equals 1, the header text should dynamically render as "1 Application". When the count is 0 or greater than 1, it should render as "[Count] Applications".]

### Current Behavior

[The header statically displays " Applications" (or appends the count directly to the plural string regardless of quantity), resulting in grammatically incorrect output like "1 Applications".]

### Affected Components

[src/app/page.tsx]

---

## Reproduction Process

### Environment Setup

[Notes on setting up your local development environment - challenges you faced, how you solved them]
Setup Process: Forked the repository on GitHub and cloned it locally to VS Code. Installed node dependencies using the project package manager.

Challenges Faced: The application failed to run because it was missing required local database configurations. The .env file was correctly listed in .gitignore to prevent committing sensitive environment variables, so cloning the repository left out the local database connection string.

How I Solved It: Inspected the project requirements, created a local .env file at the root directory, and defined the required connection string (DATABASE_URL="file:./dev.db"). Initialized the local SQLite instance to enable the app to run locally.

### Steps to Reproduce

1. [Step 1]Open the home page (/) on the local development server.

2. [Step 2]Ensure there is exactly 1 application in the database (or mock data) for the home page to load.

3. [Observed result]Observe the rendered heading text on the home page displaying " Applications" (or "1 Applications") instead of the correct singular form "1 Application"

### Reproduction Evidence

- **Commit showing reproduction:** [https://github.com/Wayles3/offer-tracker/commit/4135ac74a0dc46f16fac30da7a38f6dcf59be807]
- **Screenshots/logs:** [If applicable]
- **My findings:** [During reproduction, I verified that when the home page fetches and loads exactly 1 application, the heading component hardcodes or statically appends the string " Applications". The component directly renders the raw plural label without checking if count === 1. This confirms the issue is driven by missing conditional pluralization logic in the home page header rendering code.]

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
