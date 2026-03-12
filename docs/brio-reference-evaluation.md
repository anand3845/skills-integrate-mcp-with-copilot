# Brio Reference Evaluation

Issue: #8
Reviewed repository: https://github.com/ekoka/brio
Date: 2026-03-12

## Decision

Decision: Adopt concepts only (do not fork and rehabilitate `ekoka/brio` as a base).

Rationale:
- The domain model is useful and maps well to our school activities use case.
- The implementation is incomplete and has multiple reliability/security concerns.
- Reimplementing the concepts in this repository's existing stack is lower risk than repairing an old prototype.

## Reusable Domain Concepts

The following concepts should inform our implementation:
- `Tenant` (optional for future multi-school support)
- `Activity` and `Event` entities
- `Schedule` and `ScheduleException` for recurring schedules and one-off changes
- `Kid` (student profile), `Registration`, and `Payment`
- `User` and `Role` authorization model

## Entity Mapping To Current Project

Current project is simple and in-memory. This mapping shows target entities if we evolve beyond the current model.

| Brio Concept | Current Project Equivalent | Recommended Next Target |
| --- | --- | --- |
| Tenant | None | Optional: `school` or `organization` scope |
| Activity/Event | `activities` dictionary entries | `Activity` model with category, schedule metadata |
| Schedule | text schedule string | structured schedule fields (date/time/recurrent rules) |
| ScheduleException | None | explicit cancellations/reschedules |
| Kid | student email string | `Student` model with profile fields |
| Registration | participants list per activity | join model with status/history |
| Payment | None | optional billing extension |
| User/Role | None | teacher/admin role model for protected actions |

## Technical Debt Checklist (If We Ever Revisit Forking)

If we ever decide to fork `brio`, these are mandatory before production use:
- Remove startup side effects from import paths (`create_all()` and server start on import).
- Externalize secrets and database credentials from source code.
- Fix model inconsistencies and imports (several modules appear incomplete/incorrect).
- Add schema migrations and reproducible environment setup.
- Add automated tests for auth, tenant isolation, and scheduling behavior.
- Complete unfinished route handlers and replace stubs.
- Harden auth/token lifecycle and audit authorization checks.

## Immediate Next Steps For This Repository

1. Keep current in-memory app stable for exercise goals.
2. If product scope grows, move activities data to `activities.json` first.
3. Add role-based teacher/admin controls before enabling destructive actions.
4. Add filtering/sorting/search capabilities and responsive UI improvements.

## Status

Issue #8 acceptance criteria now addressed:
- Decision documented: yes.
- Architecture/entity mapping drafted: yes.
- Technical debt checklist documented: yes.
