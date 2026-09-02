---
name: archive-post
description: Use when the user gives you a LinkedIn post, X/Twitter post, or blog post (pasted text, or "here's a post for X/LinkedIn/blog") to save into this social-media-archive repo. Formats the content to match this repo's existing per-platform style, saves it to the right folder (linkedin/, X/, or blog/) with correct date-prefixed naming, and commits.
---

# Archive a social media post

The user will paste raw post content and tell you (or make it obvious from
context) which platform it's for: **LinkedIn**, **X/Twitter**, or **Blog**
(Medium-style long-form). Your job: reformat it to match this repo's house
style for that platform, save it in the right place with the right filename,
and commit. Do not push — the user pushes manually.

Repo root: `/home/rahat/social-media-archive` (contains `linkedin/`, `X/`,
`blog/`, `README.md`). Always use paths relative to this root, not the cwd.

## 1. Figure out platform, date, and topic

- **Platform**: ask only if genuinely ambiguous (e.g. content given with no
  label and no clue). Otherwise infer from what the user says.
- **Date**: default to today's date unless the user gives one. Format
  `YYYY-MM-DD`.
- **Topic slug**: a short `Title_Case` or `PascalCase` phrase capturing the
  subject, matching the existing naming in that folder (e.g.
  `Different_Namespaces`, `ManagedIdentity`, `Cloudflare_tunnels`). Don't
  overthink exact casing — just stay consistent with sibling files.
- **Filename**: `YYYY-MM-DD-Topic.md` in the matching folder. Before writing,
  check the folder for an existing file with that date — if one exists,
  confirm with the user whether this is a same-day second post (pick a
  distinguishing suffix) or a mistake.

## 2. Reformat — don't rewrite

Preserve the user's actual content, voice, and technical facts. You are
reformatting/cleaning up structure and spacing to match house style, not
rewriting their ideas, embellishing, or inventing details (metrics, dates,
tool names) that weren't given.

### LinkedIn (`linkedin/`)

Plain prose, no title, no headers, no frontmatter, no hashtags.

- Short paragraphs (2-5 sentences), first-person, conversational-technical
  tone — reads like a war story or a lesson learned, not a tutorial.
- Separate paragraphs with a blank line; many existing posts use a full
  blank line of spacing (i.e. an extra empty line) between paragraphs for
  breathing room on LinkedIn's renderer — fine to use either single or
  double blank lines, but be consistent within the post.
- Often ends with a one-line question inviting engagement (not mandatory,
  but it's the dominant pattern — keep it if the source content has one, add
  one only if it fits naturally).
- Reference example: `linkedin/2026-07-25-Different_Namespaces.md`.

### X (`X/`)

Numbered tweet-thread format, no title/header block, no metadata lines, no
trailing hashtags (older posts in this folder have a `# Title` + metadata
header and hashtags — that's a retired style; match the *current* convention
below, not those old files).

- Format: `1/ ...`, blank line, `2/ ...`, blank line, etc. A lone `·` on its
  own line can be used as a soft divider between two tightly-linked tweets.
- Each numbered entry should be short and punchy (aim under ~280 chars),
  trimmed down from the fuller LinkedIn-style version of the same idea —
  split one paragraph of dense content into 2-3 thread entries if needed.
- Thread should still land the same lesson/point as the source content, just
  compressed and staccato.
- Reference example: `X/2026-07-25-Different_Namespaces.md`.

### Blog (`blog/`)

Full long-form article, matches Medium style.

- `#` H1 title at the top — punchy, often a claim or a question.
- `##` section headers breaking up the article.
- Fenced code blocks with language tags where relevant.
- `---` horizontal rules between major sections.
- Optional diagram placeholders where a visual would help:
  `> **[INSERT DIAGRAM: short description]**`
- Closing footer, verbatim template (append after the last `---`):

  ```
  *This is part of an ongoing series documenting my path from Environmental Science grad and data analyst to DevOps engineer.*

  💼 LinkedIn: [Rahat Ahsan](https://linkedin.com/in/rahatahsan)
  🐦 X: [@RahatAhsan20](https://x.com/RahatAhsan20)
  🐙 GitHub: [AhsanRahat12](https://github.com/AhsanRahat12)
  ```
- Reference example: `blog/2026-07-26-Cloudflare_tunnels.md`.

## 3. Write the file

Use the Write tool to create the file at
`/home/rahat/social-media-archive/<folder>/<YYYY-MM-DD>-<Topic>.md`.

## 4. Commit (do not push)

From `/home/rahat/social-media-archive`:

```
git add <folder>/<filename>
git commit -m "Added the <topic, plain words> <linkedin post|X thread|blog>"
```

Match the existing commit message style (see `git log`, e.g. "Added the
Managed Identities content", "Added the cloudflare tunnels blog") — short,
lowercase-ish, describes what was added, not how.

Do **not** add a `Co-Authored-By: Claude` trailer, `Claude-Session:` line, or
any other AI-attribution footer to the commit message — the user does not
want Claude listed as a co-author on commits in this repo. Just the plain
message above, nothing appended.

Stage and commit only the new post file — never `git add -A`/`git add .`.

Do **not** run `git push`. The user pushes manually — stop after the commit.

## 5. Confirm

Tell the user the file path it was saved to and that it's committed. Keep it
to one or two lines — no need to re-paste the whole formatted post back
unless they ask.
