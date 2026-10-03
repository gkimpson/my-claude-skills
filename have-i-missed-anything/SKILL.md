  Purpose:
  Review the current branch against the Jira ticket in `_docs/jira-ticket.md` and tell me whether the work is actually complete, whether I’ve missed anything, and whether any
  changed endpoints or related code could negatively affect other parts of the system.

  Behavior:
  1. Read `_docs/jira-ticket.md` first and extract the expected scope, requirements, acceptance criteria, assumptions, and any referenced endpoints or business rules.
  2. Inspect the current git branch and determine the correct comparison base.
     - Prefer the merge-base against `development`.
     - If that is not available, use the default branch or upstream branch and clearly state what was used.
  3. Review the full diff on this branch, including:
     - PHP/domain logic
     - controllers/routes/endpoints
     - Twig/Vue/frontend usage of changed endpoints
     - migrations/seeds
     - tests
     - config or environment-sensitive changes
  4. Check for gaps between `_docs/jira-ticket.md` and the implemented changes:
     - missing requirements
     - partially implemented flows
     - unhandled edge cases
     - missing validation, authorization, permissions, or error handling
     - missing tests or outdated tests
     - missing migrations, seed updates, or docs where needed
  5. Focus especially on endpoint risk analysis:
     - identify every modified or newly introduced endpoint, controller action, request contract, response shape, query behavior, auth rule, or side effect
     - determine whether those changes could break or negatively impact other parts of the system
     - search for consumers of those endpoints across the codebase
     - call out backward compatibility risks, naming/contract drift, changed defaults, stricter validation, changed nullability, pagination/filtering changes, status code changes,
  or altered payload structure
  6. Look for broader regression risk:
     - shared services/facades/models touched by the branch
     - reused methods whose behavior changed
     - database/query changes that could affect other screens or jobs
     - frontend/API mismatches
     - hidden coupling not obvious from the ticket
  7. Do not just summarize the diff. Act like a senior reviewer trying to prove whether this is safe to merge.

  Output format:
  - Start with **Findings** only.
  - Order findings by severity: `High`, `Medium`, `Low`.
  - For each finding include:
    - severity
    - concise title
    - why it matters
    - exact file references
    - impacted endpoint(s) or subsystem(s), if relevant
    - recommended fix or follow-up
  - Then include:
    - `Missed Against Ticket`: explicit checklist of ticket requirements that are complete / incomplete / unclear
    - `Endpoint Impact Review`: list of changed endpoints and whether each appears safe, risky, or needs manual verification
    - `Testing Gaps`: specific tests that should exist or be updated
    - `Conclusion`: one of `Looks complete`, `Mostly complete with follow-ups`, or `Not complete`

  Important rules:
  - Be skeptical and specific.
  - Prefer evidence from the codebase over assumptions.
  - If no issues are found, say that explicitly, but still list residual risks and unverified assumptions.
  - Include file paths and line references wherever possible.
  - Do not make code changes; this is a review-only command.
