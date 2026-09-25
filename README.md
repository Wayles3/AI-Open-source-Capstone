# Contribution [7]: [Dashboard header says "1 Applications" instead of "1 Application"]

**Contribution Number:** [7]  
**Student:** [Adewale Abodunde]  
**Issue:** [https://github.com/shanker-codepath/offer-tracker/issues/7]  
**Status:** [Phase I] [Completed]
**Status:** [Phase II] [Completed]
**Status:** [Phase III] [Completed]
**Status:** [Phase IV] [Completed]

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
I found the bug at  line 18 of src/app/page.tsx it always renders "Application" regardless of count.

### Proposed Solution

[High-level description of your fix approach]
I intend to add an inline ternary on the heading: {stats.totalCount} {stats.totalCount === 1 ? "Application" : "Applications"} — so the label switches to plural whenever the count isn't exactly 1

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]
The home page heading currently displays a static string " Applications" (or appends the count directly without handling singular forms), regardless of how many applications are loaded. When there is exactly 1 application in the system, it grammatically fails to render as "1 Application". The objective is to implement conditional pluralization logic so that:
1 application renders as "1 Application"
0 or 2+ applications render as "[Count] Applications"

**Match:** [What similar patterns/solutions exist in the codebase?]
Pattern: Evaluated how strings and counts are rendered across the codebase's React components (src/app/page.tsx).
Solution: The codebase uses inline JSX ternary operators (count === 1 ? "singular" : "plural") for conditional rendering, which fits the existing React style without adding extra dependencies.

**Plan:** [Step-by-step implementation plan]
1. Open src/app/page.tsx and navigate to the heading element at line 18.
2. Locate where stats.totalCount is rendered alongside the static label string.
3. Replace the static string with an inline ternary conditional check: {stats.totalCount} {stats.totalCount === 1 ? "Application" : "Applications"}.
4. Run the local development server to verify the UI output across 0, 1, and 2+ application states.
5. Run existing test suites (npm test / npm run lint) to ensure no regressions or formatting warnings were introduced.


**Implement:** [Link to your branch/commits as you work]
https://github.com/shanker-codepath/offer-tracker/commit/bd2bdaf28d4c2f703bce4f05917e9863503e69e7

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]
Branching & Scope: Created branch "Fix-issue-Dashboard" off main. Kept the fix focused exclusively on src/app/page.tsx line 18 per workflow guidelines.
Code Style: Followed existing JSX patterns and maintained strict TypeScript mode with zero type workarounds (any).
Local Checks: Verified the application builds locally with npm run dev and ran required checks (npm run lint, npm run typecheck, npm test).

**Evaluate:** [How will you verify it works?]
Automated Tests: Ran npm test (Vitest). All 4 test files (applications, validation, applications.route, and StatusBadge) passed cleanly (14/14 tests passed in 2.78s).
Type & Lint Validation: Confirmed npm run lint and npm run typecheck complete with zero warnings or errors.
Manual UI Verification: Tested the home page locally with 0, 1, and 2+ applications in dev.db, verifying the header text correctly displays "1 Application" when count is 1 and "2 Applications" when count is greater than 1.

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Single Application (Singular): Verifies the dashboard header renders "1 Application" when stats.totalCount equals 1.]
- [ ] Test case 2: [Multiple Applications (Plural): Verifies the dashboard header renders "[Count] Applications" when stats.totalCount is greater than 1 (e.g., "2 Applications").]
- [ ] Test case 3: [Zero Applications (Plural/Default): Verifies the dashboard header renders "0 Applications" when stats.totalCount is 0.]

### Integration Tests

- [ Full API & UI Workflow: Create an application via POST /api/applications and delete it via DELETE /api/applications/[id], verifying that stats.totalCount in src/app/page.tsx updates dynamically and switches the header label between singular and plural forms.] Integration scenario 1
- [ Database & Page Render: Query the SQLite database via Prisma with 0, 1, and 2+ seeded application records, ensuring GET / returns a 200 status and correctly renders "0 Applications", "1 Application", or "2 Applications".] Integration scenario 2

### Manual Testing

[What you tested manually and results]

Tested Scenario 1 (Singular Count = 1): Deleted test entries until exactly 1 application remained in dev.db. Loaded http://localhost:3000/ and verified the header correctly displayed "1 Application".
Tested Scenario 2 (Plural Count > 1): Created a new application via /applications/new. Navigated back to the dashboard (POST /api/applications succeeded) and confirmed the header updated to "2 Applications".
Tested Scenario 3 (Zero Count = 0): Removed all applications from the database via DELETE /api/applications/[id]. Verified the dashboard header defaulted to "0 Applications".

## Implementation Notes

### Week [1] Progress

[What you built this week, challenges faced, decisions made]
Setup & Environment Fix: Cloned the offer-tracker repo and configured local development (Next.js 16.3.4, Prisma, SQLite). Resolved missing environment configuration by creating .env with DATABASE_URL="file:./dev.db", successfully executing prisma migrate deploy and database seeding (tsx prisma/seed.ts).
Issue Identification & Fix: Located the pluralization bug in src/app/page.tsx line 18 where the header hardcoded "Applications" regardless of item count. Replaced static text with an inline ternary operator: {stats.totalCount === 1 ? "Application" : "Applications"}.
Testing & Git Workflow: Executed Vitest test suite (14/14 tests passing) and performed manual browser verification across 0, 1, and 2+ application states. Resolved terminal bash quoting syntax issues, created local branch Fix-issue-Dashboard, and pushed changes to remote upstream.

### Week [2] Progress
PR & Documentation: Finalized UMPIRE framework documentation, filled out self-review checklists following the project's CONTRIBUTING.md guidelines, and opened a Pull Request referencing the target issue.
[Continue documenting as you work]

### Code Changes

- **Files modified:** [src/app/page.tsx]
- **Key commits:** [[Links to important commits](https://github.com/shanker-codepath/offer-tracker/commit/bd2bdaf28d4c2f703bce4f05917e9863503e69e7)]
- **Approach decisions:** [Used a simple inline JSX ternary operator ({stats.totalCount === 1 ? "Application" : "Applications"}) directly in src/app/page.tsx rather than adding a heavy third-party pluralization library or custom helper utility. This keeps the codebase minimal, respects strict TypeScript types without introducing unnecessary abstractions, and aligns with the existing React patterns in the app.]

---

## Pull Request

**PR Link:** [[GitHub PR URL when submitted](https://github.com/shanker-codepath/offer-tracker/pull/25)]

**PR Description:** [This change updates the dashboard header in src/app/page.tsx to conditionally pluralize the word "Application" based on stats.totalCount.
It is needed because the header hardcoded "Applications" (or static "Application" text), causing grammatical errors when displaying a single item (e.g., rendering "1 Applications" instead of "1 Application"). Adding an inline ternary operator ensures the text accurately switches between singular and plural forms.



**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review]

---

## Learnings & Reflections

### Technical Skills Gained

[Next.js & React Conditional Rendering: Applied inline ternary logic in server components for dynamic UI pluralization.
Database State Management: Managed Prisma/SQLite migrations and seed scripts to test edge cases (0, 1, 2+ records).
Git CLI String Escaping: Handled shell escaping issues when committing strings with nested double quotes.]

### Challenges Overcome

[Git Commit Bash Error: Fixed pathspec errors caused by nested double quotes by wrapping commit messages in single quotes (git commit -m '...').
Missing Upstream Branch: Resolved git push failures by linking tracking with git push -u origin Fix-issue-Dashboard.]

### What I'd Do Differently Next Time

[Automate UI Tests: Write a component test using React Testing Library rather than relying solely on manual UI checks.
Extract Utility Helper: Create a reusable pluralize() utility if the app grows, rather than using inline ternaries across multiple pages.]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
