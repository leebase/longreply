# AGENTS

## Repository summary
- This repository is a content library for long-form replies.
- Content is primarily Markdown and lives in `_pages/`.
- GitHub Pages serves the site using Jekyll with a remote theme.

## Important rule sources
- Cursor rules: none found in `.cursor/rules/` or `.cursorrules`.
- Copilot rules: none found in `.github/copilot-instructions.md`.
- If new rule files are added, mirror them here.

## Build, lint, and test commands
- No build tooling is committed in this repository.
- No lint configuration is present (no ESLint/Prettier/etc.).
- No test framework or test folder exists.

## Single-test execution
- Not applicable; there are no tests or runners.
- If a test framework is added, document single-test commands here.

## Local preview (optional)
- GitHub Pages uses Jekyll with a remote theme in `_config.yml`.
- There is no `Gemfile` or script in this repo to run locally.
- If you add a Jekyll toolchain, add `bundle exec jekyll serve` here.

## File structure conventions
- `_pages/` holds published Markdown pages and collections.
- Root files are standalone Markdown or site config.
- `SottRyan/` holds related content assets.
- Keep new content close to similar material for discoverability.

## Markdown front matter
- Pages in `_pages/` use YAML front matter.
- Keep the opening/closing `---` lines intact.
- Typical fields:
  - `title`: human-readable title.
  - `author`: name or handle.
  - `date`: `YYYY-MM-DD` format.
  - `categories`: array of strings.
  - `layout`: typically `default`.
  - `permalink`: site path, ending with `/`.
- Do not remove fields from existing pages.
- Add new fields only when needed by the theme.

## Markdown style
- Use ATX headers (`#`, `##`, `###`).
- Prefer sentence-case headings.
- Keep line lengths reasonable for readability.
- Use blank lines between paragraphs and sections.
- Use blockquotes for quotations or callouts.
- Use ordered lists for sequences, unordered lists for groupings.

## Writing tone and structure
- Write in clear, direct language.
- Use emphasis sparingly; avoid excessive bold/italic.
- Prefer short paragraphs for skimmability.
- Avoid trailing whitespace.
- Keep titles and section names descriptive.

## Code snippets and examples
- Use fenced code blocks with language tags (e.g., ` ```md `).
- Keep snippets minimal and focused on the point.
- Prefer spaces over tabs in code blocks.
- Avoid placeholder secrets or realistic credentials.
- If showing errors, include short, user-facing messages.
- Avoid long stack traces unless the content is about debugging.

## Imports, types, and naming in examples
- Keep import lists short and grouped by source.
- Use explicit names in examples; avoid single-letter variables.
- Prefer `camelCase` for variables in JS/TS examples.
- Prefer `snake_case` for variables in Python examples.
- Prefer `PascalCase` for types or classes.
- If types are shown, keep them explicit and readable.

## Error handling guidance
- Prefer describing the error cause and resolution steps.
- Use consistent terminology for the same error concept.
- Avoid blaming language; keep the tone constructive.
- Provide remediation steps when relevant.

## Naming conventions
- Use `kebab-case` filenames for new Markdown pages.
- Keep filenames short but descriptive.
- Avoid spaces in filenames.
- Use consistent category names across related posts.

## Links and references
- Prefer relative links within the repo.
- For external links, include the full `https://` URL.
- Avoid bare URLs without link text unless the URL is the content.
- Keep citations and references near the relevant text.

## Images and assets
- Store assets alongside their content or in a dedicated folder.
- Use relative paths for images.
- Include alt text for images.
- Keep file sizes small for web delivery.

## Mermaid and diagrams
- Mermaid files (e.g., `.mmd`) are stored alongside content.
- Keep diagram code readable with section comments.
- Use consistent naming for nodes and labels.
- Avoid HTML in Mermaid unless required for layout.

## Error handling and validation
- There is no runtime error handling in this content repo.
- Validate YAML front matter syntax before committing.
- Check Markdown render in GitHub to spot formatting issues.

## Formatting and tooling
- No formatter is configured; do not introduce one unless requested.
- Preserve existing formatting conventions within a file.
- Avoid adding emojis unless the content calls for them.

## Content updates
- Make minimal, focused changes.
- Preserve author voice and intent.
- Avoid broad rewrites unless asked.
- If adding new pages, include front matter and a clear title.

## Git and workflow notes
- Use small, scoped commits when requested.
- Avoid committing secrets; do not add `.env` files.
- Keep commit messages descriptive if you create them.

## If you add tooling
- Update this file with commands and usage.
- Document how to run a single test or file.
- Add any new lint/format scripts.
- Mention required runtimes (Ruby, Node, etc.).

## Quick checklist for new pages
- Add valid YAML front matter.
- Use `kebab-case` filename.
- Include a top-level `#` heading.
- Check rendering in GitHub.
- Verify links and references.

## Future automation notes
- If adding scripts, keep them in `scripts/`.
- Document any new CLI dependencies.
- Prefer repeatable commands over manual steps.
- Note any required environment variables.
- Update this file after adding automation.

## Scope reminders
- Follow the repository layout described above.
- Keep changes localized to requested files.
- Ask before restructuring directories or adding tooling.
