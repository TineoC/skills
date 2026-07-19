---
name: k8s-blog-style-guide
description: Kubernetes blog contribution guide — content eligibility, the docs-style-guide exceptions that apply to blog articles ("we" is OK, future tense is OK), front matter conventions, the release-comms process, and empirical front matter patterns for release-announcement and sneak-peek posts, sourced from kubernetes.io/docs/contribute/blog. Invoke by name when drafting, reviewing, or editing a Kubernetes blog post — main blog, contributor blog, or release-comms articles (sneak peeks, release announcements).
disable-model-invocation: true
---

Sources:
- https://kubernetes.io/docs/contribute/blog/blog-guidelines/
- https://kubernetes.io/docs/contribute/blog/blog-submission/
- https://kubernetes.io/docs/contribute/blog/post-release-comms/

Blog articles are a **distinct content type** from `/docs/` reference and concept
pages. They use an editorial, first-person voice and follow a looser style regime.
Apply this guide instead of the general [k8s-docs-style-guide](../k8s-docs-style-guide/SKILL.md)
for anything under `content/en/blog/`.

## Which style guide applies

1. The [content guide](https://kubernetes.io/docs/contribute/style/content-guide/)
   applies unconditionally — except restrictions the guide marks as
   documentation-only, which don't apply to blogs.
2. The [general style guide](https://kubernetes.io/docs/contribute/style/style-guide/)
   applies as **"should" (not "must")**.
3. Blog-specific exceptions (below) win over the general style guide wherever they conflict.

## Exceptions to the general style guide, for blog articles only

| General docs rule | Blog exception |
|---|---|
| Avoid "we" — docs describe the system, not the authors | OK to use "we" in an article with multiple authors, or where the intro makes clear the author writes on behalf of a group (e.g. a release team) |
| Don't state future intentions ("will be able to") | OK, but "used with care" in official announcements written on behalf of Kubernetes |
| No `{{</* code_sample */>}}` shortcode requirement | Code samples don't need the shortcode; often clearer without it |
| Follow a consistent, neutral voice | OK for authors to write in their own style, as long as the point comes across |
| — | Never use Kubernetes callout shortcodes (`{{</* note/caution/warning */>}}`) — those target docs readers, blog articles aren't documentation |
| [Diagram guide](https://kubernetes.io/docs/contribute/style/diagram-guide/) targets docs | Good to align with it, but no need to caption diagrams as "Figure 1", "Figure 2", etc. |

## Content eligibility

- **Original content only**, in English. No content already published elsewhere (even your own low-traffic blog), no repurposed third-party content.
- Must apply **broadly** to the Kubernetes community — upstream-focused, not vendor-specific configurations. No vendor pitches, no "using Kubernetes with cloud provider X" posts.
- Prefer **timelessness**: content that won't need edits to stay accurate. If the content is really a tutorial or reference, put it in the docs instead of a blog post — reserve the blog post for the problem space / why-you-should-care framing.
- Diagrams/illustrations: prefer vector formats, SVG strongly preferred (over raster and over Mermaid — Mermaid rendering upgrades can silently break unmaintained diagrams). Always set an `alt` attribute.
- Each language is a separate PR; English is upstream for all localizations.
- Rejected outright: vendor pitches, previously-published content, large code dumps with minimal explanation, cloud-provider-specific how-tos, articles that criticize specific people/groups/businesses, articles with technically misleading or unsafe advice.

## Publishing process

Three routes to publication:
1. **Recommended** — pitch via Slack `#sig-docs-blog` (or `#sig-release-comms` for contributor-blog-only pitches); you get paired with an editor and a writing buddy.
2. **Direct PR** — open a placeholder PR (empty commit is fine) against `kubernetes/website`; not recommended (GitHub isn't ideal for prose review) but works.
3. **Post-release comms** — see below.

Article scheduling:
- Don't set a `date` at all initially — **do not** use a placeholder like `date: XXXX-XX-XX` (explicitly called out as wrong in the official guide).
- Do set `draft: true` in front matter. The PR merges as an unpublished draft.
- A small follow-up PR later sets the real `date` and removes `draft: true` to schedule publication.

File placement: `content/en/blog/_posts/YYYY/abbreviated-post-title.md` (or a `YYYY/abbreviated-post-title/index.md` + asset files, if the post has images). Don't put a date in the filename — reviewers set the final filename and date together.

Front matter example (per the official guide):
```yaml
---
layout: blog
title: "Your Title Here"
draft: true # will be changed to date: YYYY-MM-DD before publication
slug: lowercase-text-for-link-goes-here-no-spaces # optional
author: >
  Author-1 (Affiliation),
  Author-2 (Affiliation),
  Author-3 (Affiliation)
---
```

Other conventions:
- Topmost Markdown heading in the body is `##`, not `#` (the `title` front matter field becomes the H1).
- Commit messages should summarize the post (e.g. `blog: foobar announcement`), not be placeholders (`draft post`, `asdf`, `initial commit`).
- Squash commits before merge.

## Post-release comms (Release Comms team / SIG Release)

For articles announcing a specific release's changes (release announcements, sneak peeks, post-release deep-dives):

- Opt in with a **draft placeholder PR against `main`** (not the `dev-{next-minor}` branch — this differs from the regular new-feature placeholder-PR process).
- Comment on the related `kubernetes/enhancements` issue linking the PR, and notify the team in Slack `#release-comms`.
- You may not get an individual writing buddy; a Release Comms team member guides you instead.
- Must stay `draft: true` — the PR can merge any time during the cycle.
- Publish PRs carry a **`do-not-merge/hold`** label and stay held until the release actually happens, even once approved/LGTM'd.

## Front matter conventions observed in recent release-cycle posts (v1.34–v1.36)

Not officially documented, but consistent across the last 3 cycles — verify against the current cycle's actual merged PRs before assuming these still hold:

- **Release announcement** (`kubernetes-vX-Y-release`) — `author:` is a single link to that cycle's release-team page, no individual names in front matter:
  ```yaml
  author: >
    [Kubernetes vX.Y Release Team](https://github.com/kubernetes/sig-release/blob/master/releases/release-X.Y/release-team.md)
  ```
  Named editors instead appear in the body as a bolded line: `**Editors:** Name One, Name Two, ...`.

- **Sneak peek** (`kubernetes-vX-Y-sneak-peek`) — the inverse: `author:` is a plain comma-separated list of the same editors' individual names, no link, no affiliations:
  ```yaml
  author: >
    Name One,
    Name Two,
    Name Three
  ```
  Don't mix the two styles (a names list *and* a release-team link) in one field — no precedent post does both.

- **Slug** always includes the `v`: `kubernetes-v1-36-sneak-peek`, not `kubernetes-1-36-sneak-peek`.
- **Title** pattern: `'Kubernetes vX.Y Sneak Peek'` (capitalized, with `v`, no colon) for sneak peeks; `"Kubernetes vX.Y: <Release Codename>"` for release announcements.
- **`date` placeholder in practice diverges from the official guide above**: v1.35 and v1.36 sneak-peek drafts *did* use `date: YYYY-XX-XX` as a placeholder (contradicting the "don't do this" guidance), while v1.34's draft set the real target date immediately. Both the filename and the `date` field should stay consistent with whichever choice is made — check with the current cycle's Release Comms lead/editor rather than assuming.
