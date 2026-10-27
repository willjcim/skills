---
name: commit-message-format
description: Write or reformat Git commit messages with a concise Conventional Commits subject and a strict file-by-file body of meaningful changes.
---

# Commit message format

Use this skill when asked to write, review, or reformat a Git commit message. Return only the finished commit message, without Markdown fences or commentary, unless a question must be resolved first.

Writing a message does not authorize staging, committing, amending, or pushing changes.

## Gather evidence

- For a new commit, inspect `git status --short`, `git diff --cached --stat`, and `git diff --cached`. Base the message only on the staged changes unless the user explicitly selects another scope.
- For an existing commit or specified range, inspect that exact commit or range instead.
- Read relevant surrounding code when a diff alone does not establish what changed. Never invent intent, behavior, or verification results.
- If nothing is staged and no other scope was specified, ask which changes the message should describe. Do not stage files automatically.
- When reformatting a supplied message without access to its changes, preserve the supplied facts; do not invent file paths or missing details. Ask if the required file mapping is unavailable.

## Opening line

Follow [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/):

```text
<type>[optional scope][optional !]: <short description>
```

- Use `feat` for a new feature and `fix` for a bug fix. Use another suitable type when appropriate, such as `docs`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `style`, or `revert`.
- Use a lowercase type. Include a short scope only when it adds useful context.
- Keep the description short, concrete, and human-readable. Prefer a brief action phrase with no trailing period. Aim for at most 72 characters for the entire opening line; this is a style target, not a requirement of Conventional Commits.
- Summarize the main outcome rather than listing files or implementation details.
- For breaking changes, put `!` immediately before `:` and describe the breaking change in the opening line. Do not add a breaking-change footer.

## Body: strict file-by-file format

Separate the opening line from the body with exactly one blank line. The body must contain only file sections in this form:

```text
path/to/file_1:
    -> change 1
    -> change 2

path/to/file_2:
    -> change 1
    -> change 2
    -> change 3
```

Rules:

- Use exact repo-relative file paths, without a leading slash, backticks, or a `./` prefix.
- End each file heading with `:`.
- Put each change on its own line, indented with exactly four spaces followed by `-> `.
- Separate file sections with exactly one blank line. Do not put blank lines between changes within a file section.
- Include every changed file in the selected scope exactly once. Keep file sections in a consistent order, preferably the diff's order.
- For a renamed file, use its destination path and mention the old path in a change line. For a deleted file, use its former path and state what was removed.
- Include generated files and lockfiles, but summarize their meaningful update rather than listing mechanical churn.
- Do not include prose paragraphs, section titles, test summaries, footers, trailers, signatures, or any other material outside the file sections.

## Change descriptions

- Write one very short, human-readable line per meaningful change within each file.
- Meaningful changes include adding or deleting a function, changing an algorithm, adding a comment, changing configuration, updating documentation, or adding a dependency.
- Describe the actual change, not every changed line or diff hunk. Combine mechanical edits serving one change; keep distinct meaningful changes separate.
- Prefer direct phrases such as `adds token-owner notification workflow`, `removes unused parser`, or `handles empty responses`.
- Name a function, flag, dependency, or identifier only when it clarifies the change. Avoid long implementation narratives, unnecessary IDs, repeated context, and vague phrases such as `updates code`.
- Keep each description on one line. Shorten the wording instead of wrapping it.
- Do not claim tests passed or behavior was verified unless supported by evidence; verification reports do not belong in this body format.

## Example

```text
feat: add token-owner notifications

workflows/datadog/monitors/notify-token-owners.ts:
    -> adds token-owner notification workflow

workflows/datadog/monitors/index.ts:
    -> registers token-owner monitor workflow

flags.ts:
    -> adds backofficeNotifyEnabled flag
    -> adds workflow-safe flag reader

package.json:
    -> adds flags and @vercel/global-config dependencies

pnpm-lock.yaml:
    -> regenerates lockfile for new dependencies
```

## Final check

Before returning the message, confirm that the subject conforms to Conventional Commits, every selected file is covered, each line describes one meaningful change, and the body contains only the prescribed file sections. Ask a concise question instead of guessing when scope or required facts remain ambiguous.
