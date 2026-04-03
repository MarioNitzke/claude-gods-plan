# Confession — question areas

## Mandatory areas (always ask)

- **Goal & vision:** What exactly should the result be? How will it look from the user's perspective?
  - "What does success look like for this feature? Describe the user flow from start to finish."
  - "Is this replacing something existing or entirely new?"
  - "What is the single most important outcome?"

- **Scope:** What is IN scope, what is OUT of scope? Where is the boundary?
  - "Should this also cover [related concern X] or is that a separate task?"
  - "What is the minimum viable version vs. the full vision?"

- **Users:** Who will use this? What roles/personas?
  - "Which user roles interact with this? (e.g., Admin, Manager, Customer)"
  - "Are there different permission levels or views per role?"

- **Business rules:** What are the conditions, states, transitions?
  - "What happens when [edge case]? Should it fail silently, show an error, or prevent the action?"
  - "Are there state transitions? What triggers them?"
  - "Are there any time-based rules (expiration, cooldowns, scheduling)?"

- **Constraints:** Deadline? Performance requirements? Backward compatibility?
  - "Does this need to be backward-compatible with existing API consumers?"
  - "Any hard performance requirements (response time, concurrent users)?"

## Contextual areas (ask when relevant)

- **Technical context:** Dependencies on existing code? DB migrations?
  - "Are there existing services or handlers we should reuse or extend?"

- **Edge cases:** What if X? What if Y? How to behave on error?
  - "What should happen when the external service is down?"

- **Priority:** Must-have vs nice-to-have?
  - "If we can only ship one part this week, which part?"

- **Inspiration:** Is there a reference implementation elsewhere? How does competition do it?
  - "Is there a similar feature in the codebase we should model this after?"

- **Security:** Any security requirements? Authorization? Input validation?
  - "Who should NOT be able to access this? What authorization checks are needed?"

- **UI/UX:** How should it look? Are there mockups?
  - "Any design requirements or existing UI patterns to follow?"

- **Data:** What data goes in/out? Formats?
  - "What are the input/output formats? Any validation rules on the data?"

## Confession rules
- Ask **2-4 questions at a time** via AskUserQuestion (not one by one — that's too slow)
- Ask SPECIFIC questions, not vague ones
- Suggest options (A/B/C) via AskUserQuestion where appropriate
- After each batch of answers, follow up with the next batch
- **Minimum 8 questions total** — this is an Iron Law
- Confession ends when Claude says "I have enough information for the plan"
- Do not skip an area because you think you already know — confession reveals what you don't know
