---
name: k8s-blog-style-guide
description: Use when drafting, reviewing, or editing a Kubernetes blog post — main blog, contributor blog, or release-comms articles (sneak peeks, release announcements, post-release feature blogs). Covers eligibility, blog-specific style exceptions, front matter and publishing process, release-announcement body conventions (release-announcement.md), and the full catalogue of what the SIG Docs blog editor (lmktfy) flags across ~2,900 review comments on 349 kubernetes/website blog PRs (reviewer-feedback.md) — "we", headings, emphasis, naming, tense, links, example manifests, AI-slop tells.
disable-model-invocation: true
---

Sources:
- https://kubernetes.io/docs/contribute/blog/blog-guidelines/
- https://kubernetes.io/docs/contribute/blog/blog-submission/
- https://kubernetes.io/docs/contribute/blog/post-release-comms/

Companion files:
- [reviewer-feedback.md](reviewer-feedback.md) — **read first when drafting or reviewing any blog
  PR.** Every rule the blog editor repeatedly flags, grouped by frequency, with the conventions
  that changed during 2025–2026 marked.
- [release-announcement.md](release-announcement.md) — body conventions for `kubernetes-vX-Y-release`.

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
| Don't state future intentions ("will be able to") | OK, but "used with care" in official announcements written on behalf of Kubernetes — see the hedging rules below for anything not yet locked in |
| No `{{</* code_sample */>}}` shortcode requirement | Code samples don't need the shortcode; often clearer without it |
| Follow a consistent, neutral voice | OK for authors to write in their own style, as long as the point comes across |
| — | Never use Kubernetes callout shortcodes (`{{</* note/caution/warning */>}}`) — those target docs readers, blog articles aren't documentation |
| [Diagram guide](https://kubernetes.io/docs/contribute/style/diagram-guide/) targets docs | Good to align with it, but no need to caption diagrams as "Figure 1", "Figure 2", etc. |

## General style guide rules that still apply — and are easy to get backwards

These are *not* blog exceptions. They come from the general style guide, so they
apply to blogs as "should". They are listed here because they contradict habits
carried in from other projects, and because a reviewer who "corrects" them is
introducing an error rather than fixing one. Check the anchor before flagging.

- **[Start case for enhancement graduation phases](https://kubernetes.io/docs/contribute/style/style-guide/#use-start-case-for-enhancement-graduation-phases)**
  — Alpha, Beta, GA are capitalized. Do: "Dynamic Resource Allocation (DRA) is
  Beta." Don't: "…is beta." This holds for feature gates too: "the Alpha
  `FooBar` feature gate". Do not "fix" a capitalized Alpha/Beta to lowercase.
- **[Code style for command tool and component names](https://kubernetes.io/docs/contribute/style/style-guide/#use-code-style-for-kubernetes-command-tool-and-component-names)**
  — component names take backticks. Do: "The `kubelet` preserves node
  stability." Don't: "The kubelet preserves node stability." Same for
  `kubectl`, `kube-apiserver`, `kubeadm`. A post that mixes `` `kubelet` ``
  and bare `kubelet` should be normalized *toward* backticks, not away.
- **[Starting a sentence with a component name](https://kubernetes.io/docs/contribute/style/style-guide/#starting-a-sentence-with-a-component-tool-or-component-name)**
  — lead with an article or descriptor: "The `kubeadm` tool bootstraps…", not
  "`kubeadm` tool bootstraps…".
- **[API kinds are written verbatim, no backticks](https://kubernetes.io/docs/contribute/style/style-guide/#use-code-style-for-inline-code-commands)**
  — StatefulSet, ConfigMap, PersistentVolume are plain text (this is what lets
  you write "the CustomResourceDefinition's `.spec.group` field"). Field names
  and paths *do* take backticks. The API kind spelling wins even when a KEP
  title spells it with spaces: "Configurable tolerance for
  HorizontalPodAutoscalers", `ClusterTrustBundle` not "Cluster Trust Bundle".
- **[Italics for new terms](https://kubernetes.io/docs/contribute/style/style-guide/#use-italics-to-define-or-introduce-new-terms)**
  — a feature or concept on first mention is _italic and lowercase_
  (`_gang scheduling_`, `_memory QoS_`, `_native histograms_`). Title Case is
  for graduation phases *only*; Title-Casing a feature name is the most common
  way to get the previous rule backwards.
- **Sentence-case headings** — `## Configuring a probe`, not `## Configuring A
  Probe`. Blogs get the general style guide as "should", but release
  announcements are high-profile and reviewers hold them to it hard.
- **[Wrap source lines at ~80 characters](https://kubernetes.io/docs/contribute/style/style-guide/#line-breaks)**
  — reviewers ask for it on long-form posts, and it keeps diffs and GitHub
  suggestions reviewable.
- **Never break a line inside `[...]` or `(...)`** — a hard-wrapped link renders
  as literal text (`[KEP #2021] (https://…)`) and breaks the published page, not
  just GitHub suggestions. Let the link overrun 80 characters instead.

Before writing a style comment on any of the above, open the anchor and read the
Do/Don't table. Guessing from general English or from other projects' house
style produces confident, wrong review feedback.

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

File placement: `content/en/blog/_posts/YYYY/abbreviated-post-title.md` (or a `YYYY/abbreviated-post-title/index.md` + asset files, if the post has images). Don't put a date in the filename (convention changed in early 2026; PRs that rename files to add a date are rejected). Lowercase filenames.

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

For the *body* of a release announcement — tense, headings, KEP/SIG boilerplate,
feature-gate call-outs, and the pre-review checklist — see
[release-announcement.md](release-announcement.md).

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
- **"Want to know more?" footer — which past releases to list**: no documented rule, and it has drifted cycle to cycle (v1.36 listed back through 1.30, v1.35 listed 1.34–1.30, v1.34 listed none). The recurring reviewer preference is to list only releases still inside the support window (not EOL) — check the [supported releases list](https://kubernetes.io/releases/patch-releases/#detailed-release-history-for-active-branches) at write time rather than copying the previous cycle's list verbatim.

## Reviewing a blog PR

Two rules that decide how review feedback lands, detailed in
[release-announcement.md](release-announcement.md):

- **Sort every change into cosmetic or technical before posting.** Cosmetic (whitespace, backticks,
  heading case, broken link syntax, typos, rewraps) goes up as pure ```suggestion blocks with no
  prose. Technical (feature-gate names, defaults, stages, versions, behavior claims) stays back
  until it is verified. Mixing the two buries the claims that need scrutiny under dozens of
  whitespace fixes.
- **Every technical comment tags the owner** — the KEP author from `kep.yaml`, the owning SIG's
  leads from `sigs.yaml`, or the implementation PR's author. Pull handles from that metadata, not
  from memory, and cite the source (`file:line` in `kubernetes/kubernetes`, or the KEP section)
  for any claim you assert or correct — including your own replacement wording.

## Sneak-peek & release-comms writing lessons (empirical, from the v1.37 sneak-peek review — [kubernetes/website#56573](https://github.com/kubernetes/website/pull/56573))

Not in the official guide, but consistent, repeated reviewer feedback across a full review cycle. Verify these still hold against the current cycle's reviewers before assuming — Release Comms conventions evolve release to release.

**Hedge anything not locked in, especially deprecation claims:**
- Don't write "X is deprecated" (present tense) unless it's *actually* deprecated already. If a deprecation is only planned or likely, say so explicitly: "not yet deprecated, but likely to be deprecated in a future release" — never let a reader come away thinking a decision has been made when it hasn't.
- Before asserting *any* future deprecation for a long-stable API or feature, check with the team that actually owns it. A beta API that's been stable for years may have no realistic deprecation plan at all — in that case, cut the deprecation mention entirely rather than speculate ("I would remove any mention of deprecation, it is unrealistic at this point in time" — a project maintainer, on `metrics.k8s.io` v1beta1).
- Even for **featured enhancements** (not deprecations), write as if nothing is confirmed until GA: prefer "expected to graduate to Beta" / "we expect the API to reach GA" over a bare present-tense claim. Do this *even though* the post carries a top-of-article disclaimer that details may change — assume a large fraction of readers skip that disclaimer and take the body text at face value.
- Planned **deprecations** can be stated with more confidence than planned **features** — a deprecation timeline that's already KEP-approved is a commitment; a feature graduating to Beta/GA is not, until it actually does.
- A KEP/feature can silently drop out of the release milestone *after* a sneak-peek PR describing it was opened (e.g. missing code freeze). Re-check every featured KEP against the actual `kubernetes/enhancements` milestone immediately before merge, and cut any section for a KEP that's no longer in-milestone.

**Don't expose KEP mechanics to end users:**
- Never put "KEP #NNNN:" (or any KEP number) in a section heading or in the highlighted description text — typical readers don't know or care what a KEP is, and a KEP number reads as more official/permanent than the actual level of certainty.
- KEP numbers/links belong only in the "learn more" pointer at the end of a section (e.g. "To learn more about this enhancement, refer to [KEP-1432: Volume Health Monitor](...)").
- A feature-removal item should never be framed as if the removal itself went through a highlighted enhancement process — that reads as celebrating a removal.

**Section placement — don't misattribute a change to the current release:**
- A bug fix that happens to close off previously-unsupported behavior (e.g. removing an incidental capability that was never officially supported) is not a "deprecation" or "removal" for that release — don't list it under "Deprecations and removals for Kubernetes vX.Y". Frame it separately as a fix.
- Anything describing a **future** release's change (not the release the post is about) needs its own clearly-labeled section — never let it sit under "Deprecations and removals for Kubernetes vX.Y", since readers will (reasonably) assume everything under that heading is relevant to the current release. Put the word "future" directly in that section's heading (e.g. "Future removal of cgroup v1 support").

**Terminology & consistency:**
- Wrap Kubernetes component, tool, and command names in code style consistently (`kubelet`, `kube-proxy`, `ipvs`, `iptables`) per the [general style guide's code-style rule](https://kubernetes.io/docs/contribute/style/style-guide/#use-code-style-for-kubernetes-command-tool-and-component-names) — this applies to blog posts too, it isn't a docs-only rule.
- Cite the Kubernetes **version** a feature/flag was introduced in, not a calendar year ("introduced in v1.8", not "introduced in 2017") — and verify the exact version against the feature's KEP/history rather than approximating from memory (this thread churned through v1.8/v1.9/2017 across multiple rounds of review before landing on the KEP-verified version).
- Pick one casing convention for stability stages (Alpha/Beta/Stable vs. alpha/beta/stable) and apply it everywhere in the same article — don't mix cases across sections.
- For a term with more than one plausible form (e.g. "cgroup v2" vs "cgroups v2"), match whatever form the authoritative KEP uses, and stay consistent with that choice throughout the article.

**Trim, don't pad:**
- If a feature's blog paragraph just repeats detail already covered elsewhere in the same section (or better covered in the feature's own dedicated deep-dive blog post), cut or refactor it — a sneak peek should summarize and point outward, not reproduce implementation detail.
- Split long sentences that cram multiple ideas behind a colon into separate sentences — reviewers flag these consistently as hard to read.
- Small formatting nits reviewers actually catch: missing space after a colon in link text (`KEP-4960:Container` → `KEP-4960: Container`); a multi-item dated timeline reads better as a list than as one run-on sentence.

**Community/contributor CTAs:**
- Don't point new contributors to the contributor-comms meeting — it's already overloaded. Point to **New Contributor Orientation** (run by SIG Contributor Experience / ContribEx) instead.
- For an "share your experience" CTA, link to the [CNCF End User Story / case-studies page](https://www.cncf.io/case-studies/) rather than inventing a different venue.

**Fact-check the small stuff:** verify official handles/links (e.g. the project's actual Bluesky handle) before publishing — don't assume a link someone typed from memory is correct.
