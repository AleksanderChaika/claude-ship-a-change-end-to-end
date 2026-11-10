# NOTES

## Plan
Plan mode first, read-only. The approved plan was:
1. Add `updateUser(id, { name, email })` to `db/store.js`. It returns `undefined` for an unknown id, like `getUserById`.
2. Add `PUT /:id` to `routes/users.js`. Validate, call the store, and return 404 or 200.
3. Write this file.

I did not edit the plan before approving it. Exploration found that `CLAUDE.md` is not in the repo, so I took the conventions from the existing routes and store.

## Decisions
- **All data access goes through `db/store.js`.** The route never touches the array.
- **Validation runs before the lookup.** A malformed body returns 400 whatever the id. `name` and `email` must be non-empty strings after trimming, and `email` must contain `@`. This is deliberately light, not a full email regex.
- **Stricter than POST.** POST only checks truthiness. I left POST unchanged to keep the change scoped.
- **Unknown or non-numeric id returns 404** with `{ error: "User not found" }`, the same as GET.
- **PUT is a full replacement.** Both fields are required, so there are no partial updates.
- Values are trimmed before they are stored.

## Model
Claude Sonnet 5.5. This is a small change with a clear spec and tests, so a fast mid-size model was enough. A larger model would not have added value.

## Commits
1. `feat(store): add updateUser helper`: the data layer on its own.
2. `feat(users): add PUT /users/:id with validation and 404`: the route that uses it.
3. `docs: add NOTES.md`: the write-up.

Each commit is one logical change, and the store helper lands before the code that depends on it.

## Review
Run `/code-review` on the branch before opening the PR and record the findings here. Not yet done.
