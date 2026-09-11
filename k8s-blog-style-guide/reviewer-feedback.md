# What the blog editor actually flags

Companion to [SKILL.md](SKILL.md). Derived from every review comment by the SIG Docs blog editor on `kubernetes/website` PRs labelled
`area/blog`: 349 PRs, ~2,900 comments, 2025-01 through 2026-09. Grouped by how often each
rule came up. Apply this before requesting review; the editor is under capacity and most of
these are mechanical.

Rules marked **(changed)** contradict older guidance still found in merged articles or on
the contribute pages. The newer form wins.

## 1. Process and front matter — the first thing checked on every PR

| Rule | Detail |
|---|---|
| `draft: true` on first merge | Every new article merges as a draft, no exceptions ("I promise you it's the right change"). A small follow-up PR removes `draft: true` and sets the date. The editor will apply this with maintainer access rather than block. |
| Target `main` | Blog PRs always target `main`, even for a future release. `/hold` until fixed. |
| Path **(changed, ~2026-01)** | `content/en/blog/_posts/YYYY/slug.md` (or `YYYY/slug/index.md` when there are images). No date in the filename any more; renames that add a date get rejected. Lowercase only. |
| `date:` | Set a real timestamp with timezone (`2026-03-20T10:00:00-08:00`), never bare midnight — midnight is risky and breaks localization sort order. Two articles must not share the same timestamp. |
| Post-release comms `date` | Leave whatever placeholder is there plus `draft: true`; Release Comms sets the real date. Don't hyperlink the release announcement URL until the release date is confirmed. |
| `slug:` | Post-release feature blogs use `kubernetes-v1-NN-<feature>`. Never change a slug or publication date after publication (feed readers re-notify thousands of people). Titles can change post-publication but avoid it. |
| `author:` | Free text, comma-separated, optional Markdown links. When at least one author has an affiliation, others get `(independent)` lowercase. A lone author may omit affiliation. Move `(Google)` outside the link text. |
| `canonicalUrl:` | Only for mirrors of an article published elsewhere (contributor blog, etcd blog). Not for originals. Localizations are translations, not mirrors — no `canonicalUrl`. If the real URL isn't known yet, leave a `# NOTE: fix canonicalUrl before publication` comment, not a wrong URL. |
| Mirrors | Open the `kubernetes/contributor-site` PR first; hold the website PR until it merges; publish minutes apart. Add `_This article originally [appeared](url) on the X blog._` Mirrors need ≥14 days notice; late ones become a "recap", not a mirror. |
| No `tags:` | The theme supports them; the blog doesn't use them without a discussion. |
| PR hygiene | Squash to 1 commit (or accept `tide/merge-method-squash`). PR title: no `docs:` prefix, no issue reference. Placeholder PRs get `/retitle [WIP] …` so reviewable PRs are findable; drop "placeholder" from the description when ready. Don't bundle unrelated changes (`package-lock.json`, other articles, an update to an older post that should be a separate held PR). |
| Old articles | Policy: no edits to articles over ~1 year old except _evergreen_ ones. Small fixes get closed, not merged. Post-publication edits need a closing italic line `_This article was revised in <Month YYYY> to …_`; significant technical corrections get a `pageinfo` shortcode right after front matter that is honest about how big the inaccuracies were. Never revise a release announcement after publication unless it's plainly wrong. |
| Drafting | Blog team recommends HackMD or Google Docs for the first draft and buddy review, then Git. Nudge `@kubernetes/sig-docs-blog-owners`, not individuals. |
| AI use | Must be disclosed in the PR description. Text that "reads like an LLM wrote it" gets called out and is grounds for closure ("AI slop"). Tells: bold-lead bullet lists instead of subheadings, `---` horizontal rules, block-quote callouts, "In a world where…", "This isn't X, it's Y" closers. |
| Eligibility | No vendor or product boosting (send it to the CNCF blog); no "guest" articles — contributing makes you a contributor; no plagiarism; no repeat of already-published topics. CLA must be signed. |

## 2. "We" — the single most-repeated comment

- "We" is only OK when it unambiguously means the listed authors. One author → "I".
- "We (SIG Auth)" / "the Kubernetes project" / "the contributors working on this" when the
  group is not the authors — spell it out once and stay consistent within the article.
- "Our" reads as "my employer's" ("our" could read as "Red Hat's, given your affiliation").
- Avoid "let's" (contraction of "let us").
- Address the reader as "you". Anchor: style guide `#avoid-using-we`, blog guide
  `#article-content`.

## 3. Headings

- Sentence case for every heading. The `title` in front matter stays Title Case (the editor
  makes exceptions for sentence-case titles on announcement series to match each other).
- Don't start the article with a heading — the title is the H1; "## Introduction" gets
  deleted. First body heading is `##`.
- Use real headings, not a bold paragraph — bold-as-heading is the #1 LLM tell.
- No level-4 heading under a level-2; keep the hierarchy contiguous.
- "This should be a heading" — a bold line introducing a list or section becomes `###`.
- Title describes what the reader can now do, not the mechanism ("Kubernetes v1.34 Lets You
  Load Environment Variables From The Filesystem"); avoid clickbait titles; don't let a
  title read as an imperative ("Restart all containers"); don't imply
  `kubectl get SupplementalGroups` by Title-Casing a feature into an API-kind shape.

## 4. Emphasis and Markdown

| Do | Don't |
|---|---|
| `_italics_`, lowercase, on first mention of a feature or concept (`_gang scheduling_`, `_pod-level resources_`) | Title Case feature names; bold for emphasis on terms |
| Bold sparingly — one word like **not**, **all**, **must** | Bold whole phrases; bold-lead list items |
| Logical API verbs: **list**, **watch**, **get**, **impersonate** — bold lowercase | `LIST`, `` `list` ``, "LIST+WATCH" (no such HTTP verb) |
| HTTP verbs: `GET`, `DELETE` — uppercase in backticks | mixing the two |
| Description lists for term/definition series (`term` newline `: definition`) | bulleted **Term**: definition |
| Block quotes only for actual quotations; move attribution outside the quote | block quotes as callouts or for emphasis |
| Plain Markdown image with real alt text; `figure` shortcode only for diagrams | `figure` for photos/logos; alt text that is really a caption; HTML `<img>` |
| Alt text as an audio description ("Diagram showing monolithic cache on the left…", "(slide) Initial adopters: Akamai (logo), …") | `alt="image"`, alt with underscores |
| Hugo `youtube` shortcode | raw `<iframe>` |
| Inline/blocked math (`$$…$$`) and real symbols `≈ × ÷ →` | `~`, `x`, ASCII formulas, ASCII-art diagrams |
| Wrap Markdown source at 80–100 chars; localization teams want this | one line per paragraph; hard-wrapping inside a `[link](url)` |
| Site-relative links `/docs/...` | `https://kubernetes.io/docs/...` |
| Descriptive link text | "here", a long phrase as anchor text |
| Explicit `{#anchor}` on headings people will deep-link | relying on Hugo-generated anchors |
| 3-space list indentation, spaces not tabs, `1.` auto-numbering | manual numbering |
| `caution` callout when needed | `warning` for something that only merits caution; docs-style callouts at all in most blogs ("we avoid using docs-style callouts in blog articles" — though `note` slips through) |
| CSS for bold | `<b>` |

- No `{{</* code_sample */>}}` and no `kubectl apply -f https://k8s.io/examples/...`: the blog
  is frozen, the examples directory is not, and it adds manifests to maintain/security-check.
  Duplicate the YAML inline.
- No `glossary_tooltip` shortcode in blogs.
- Language tags: `console` (not `bash`) for output/prompt blocks; `yaml` over `json` for
  highlighting (the YAML highlighter is more forgiving); don't tag non-YAML as `yaml`.
  Multi-document YAML starts with `---` on its own line.
- Trailing `=` in base64 examples: drop it so approvers needn't check maintenance implications.
- No `---` horizontal rules in the body.

## 5. Naming conventions the editor corrects on sight

| Thing | Write | Not |
|---|---|---|
| API kinds | PersistentVolumeClaim, ClusterTrustBundle, ServiceAccount, XListenerSet — plain text, no spaces, no backticks (backticks make pluralization hard) | `PersistentVolumeClaim`, Cluster Trust Bundle, Service Account |
| Fields | `.spec.nodeName`, `.status.allocatedResources`, `.metadata.generation` — camelCase as serialized, leading dot, backticks | `Pod.Spec.NodeName`, `PodStatus`, Go names, `nil` (write `null`) |
| Components | `kubelet`, `kube-proxy`, `kube-apiserver`, `kubectl` in backticks; "the `kubeadm` tool" at sentence start | bare kubelet |
| Prose terms | API server, watch cache, cgroup v2, control plane — spaces | apiserver, watchcache |
| Groups | SIG Node, SIG API Machinery, WG Batch — no hyphen; link to `kubernetes.dev/community/community-groups/sigs/<name>/` | SIG-Node, sig-node |
| Graduation phases **(changed, 2026)** | Alpha, Beta, Stable, Generally Available — Title Case (style guide `#use-start-case-for-enhancement-graduation-phases`); don't "fix" back to lowercase | alpha/beta (older articles used this) |
| Features | English name in italics; feature-gate string once, in the "how to enable" sentence | `SupplementalGroupsPolicy` as the feature's name throughout |
| "feature gate" | feature gate | feature flag |
| Controllers | node lifecycle controller, route controller — plain English, no quotes/backticks (proper nouns like Karpenter excepted) | "`taint-manager`" |
| "controller" | controller — VPA, HPA, right-sizers are controllers | operator (nebulous; only when it really is one) |
| "manifest" | manifest (a file describing an object) | "YAML", "config" |
| "good practices" | good practices | best practices (subjective, argued every time) |
| Abbreviations | Full name: _workload aware scheduling_, _node declared features_, _Pod security admission_; DRA is the sanctioned exception (not also an English word) | WAS, NDF, PSA, RSM |
| "infrastructure resource" | when meaning CPU/memory/devices, to disambiguate from HTTP resources | bare "resource" |
| "object" vs "resource" | an object has `.metadata` and `.spec`; `deployments` is a resource (collection); "API type/kind" for the type | using "resource" for a single object |
| Versions | "introduced in v1.8", "Kubernetes v1.33:" in titles, v-prefixed slugs | "introduced in 2017"; `1.33` without `v` in titles |
| Units | 4 KiB, 8 GiB, 512 MiB (dimensionless inside backticks: `10Ti` not `` `10TiB` ``) | 4KB in prose |
| Case | Kubernetes, Linux, Service, Pod, Node (API sense); etcd, kind, containerd lowercase | kubernetes, linux, `Etcd` |
| Architecture | AMD64 / ARM64 | x86 |
| Users | "normal user" for non-ServiceAccount identities | "human user" |
| Ordering third parties | alphabetical (CNCF policy) or genuinely random | your own project first |
| Regions | Americas / EMEA / APAC | US / EU |

## 6. Time framing — the technical-accuracy killer

- **Post-release feature blogs are not the announcement.** They publish days to weeks after
  the release announcement. "I am excited to announce…" becomes "I'm pleased to be writing
  about…"; "Today, Kubernetes…" is wrong because _n_ is unknown.
- Write as if the release has happened: past tense for pre-release behaviour, present for
  what the shipped version does. "Currently", "today", "now", "will be", "expects to",
  "is proposed" are each flagged: "By 'currently' do you mean 'now that v1.34 has been
  released'?"
- Don't carry KEP wording across ("This KEP adds…", "Currently, Kubernetes cannot…").
  Describe what Kubernetes can do, not the proposal document. Users don't care about KEPs.
- Graduation vocabulary: something "graduates" only from Alpha→Beta→Stable. A new Alpha is
  "introduced", never "graduated to alpha". A semantic change that ships stable immediately
  is not a graduation. A GA of something already on by default is not a "massive improvement"
  — say so plainly. Beta does not automatically mean enabled by default.
- Don't describe a feature that was already GA in the previous release as new (check the
  docs: "According to the DRA page, v1.35 already includes this").
- Stale articles (published months after drafting): switch to past tense, add "since".
- **Promises.** Only commit to a future outcome if SIG Architecture or Steering would
  document it as a commitment. Otherwise "we intend", "the project hopes", "expected to".
  Sneak peeks: hedge every feature equally ("expected to graduate") even with a top-of-page
  disclaimer — assume half of readers skip it. Planned deprecations can be stated more
  confidently. Never let "X is deprecated" (present) describe something not yet deprecated.
- A feature-gate removal that locks in a bug fix is not a deprecation or removal — don't list
  it under "Deprecations and removals". Future-release changes get a heading with "Future"
  in it.
- Something isn't "deprecated by SIG Network"; **Kubernetes** deprecated it. Endpoints was
  deprecated in v1.21 — don't deprecate it again.

## 7. Links

| Prefer | Over |
|---|---|
| Concept docs → task docs → tutorial, in that order | KEPs, GitHub issues, PRs, source code, old blog announcements |
| `https://www.kubernetes.dev/resources/keps/NNNN/` **(changed, 2026)** for KEP links in "learn more" lists; `https://kep.k8s.io/NNNN` still accepted (it will redirect there) | `github.com/kubernetes/enhancements/blob/master/keps/...` |
| `https://www.kubernetes.dev/community/community-groups/sigs/<sig>/` (`#meetings` for meetings) | `github.com/kubernetes/community/tree/master/sig-x` |
| A tag or commit SHA | a `master`/`main` branch URL ("Cool URIs don't change") |
| The project website (`jobset.sigs.k8s.io`, `node-readiness-controller.sigs.k8s.io`, `gateway-api.sigs.k8s.io`) | its GitHub repo |
| `https://kubernetes.io/releases/1.37/` alongside the download page | GitHub-only |
| Nothing | Slack channel URLs and Google Docs (login walls for the public); the changelog of an unreleased version; KEP issue as a place for users to report experiences ("we avoid inviting the public to comment on KEP issues") |

- Link targets must match what the link text promises ("a desk labelled Reception that books
  wedding receptions").
- If the docs for a feature don't exist, that's a release problem — "we have a policy of only
  releasing user-facing features if they are documented". Rally the docs work; don't link
  to the KEP instead.
- Check every link in the Netlify preview; broken links block merge.
- The existing docs page for a moving URL is fine to link; note to revisit after release.

## 8. Example manifests and commands

- Domains: `example`, `example.com`, `cniplugin.example.net`, `registry.example`,
  `myregistry.example`. Never `.io`, `.tld`, `eg` (Egypt), or a real domain the project
  doesn't control.
- Add `kubernetes.io/description` annotation to most example manifests ("so that people
  realize they can"), and `app.kubernetes.io/name` labels. Never `:latest` images. Set
  `spec.os` on Pods in security-flavoured examples.
- Prefer one app container plus Kubernetes-native sidecars over three app containers.
- `kubectl apply --server-side`; `kubectl get pods --watch` (long flags); full `http://`
  URLs in `curl` examples with `-sS`; explain `jq` if used; mention the DNS step readers will
  trip over.
- Never teach `curl -k` / `--insecure` quietly. If unavoidable, precede with a loud
  `# THIS IS NOT SECURE. ONLY DO THIS IN A TEST CONTEXT.` and mention the long flag.
- No `hostPath` examples. No `events.k8s.io` legacy core Events API. No `rbd`.
- Don't assume RBAC is the only authorization mode ("not all clusters use RBAC for authz");
  say "if you use RBAC" or "your RBAC configuration or other authorization rules".
- A feature flag is not a security control; `nodes/proxy` grants full kubelet access — say
  so front and centre.
- Don't imply readers know Go / `go install`; JSON examples over Go structs for API bodies.
- Don't promote undocumented APIs or CRDs; register new labels/taints/annotations in
  `/docs/reference/labels-annotations-taints/` before using them in an article.

## 9. Content judgement calls

- Audience is users, not contributors. Describe the benefit ("what you can now do"), not
  the change; drop internals (validation-gen mechanics, how a component computes a value).
- Explain jargon on first use (north/south vs east/west, NUMA node ≠ Node, uncore cache).
- Say when something is Linux-only (cgroups, swap, SELinux, PSI, user namespaces) and what
  happens on Windows nodes — otherwise readers assume it applies everywhere.
- Say when something is library-specific (client-go, cloud-provider) — CCMs and controllers
  can be written in any language.
- Don't imply an exhaustive list ("featured", "some of").
- Don't frame long-stable things as "legacy" (implies don't use it) — "plain impersonation".
- Don't take sides in holy wars (YAML vs KYAML): "we are giving people the option"; over-
  communicate that new formats are optional.
- Opinions are welcome; comparison tables of ecosystem tools and vendor mentions in examples
  are not.
- Accessibility: shapes as well as colours in diagrams (red/green is the commonest
  impairment); SVG preferred, text converted to curves; raster ≤1920px; transparent logo
  backgrounds.
- Credit: KEP contributors are thanked at the end of post-release articles; link to a
  contributors page rather than committing a list file; "sponsored by" → "led by".
- A CTA for new contributors points to New Contributor Orientation, not the comms meeting.
- Bug reports go to `kubernetes/kubernetes` issues, not KEP threads or DM.

## 10. How the editor reviews (so you can predict the round trip)

- First pass is front matter + branch + path; a `/hold` with an explicit unhold condition
  ("OK to unhold once this targets `main` and has `draft: true`"). Anyone in the org can
  unhold once the condition holds.
- Feedback is marked `(nit)` when optional, "MUST FIX" when blocking. "We can fix that
  post-merge" means merge as draft now, fix in the scheduling PR.
- Suggested rewordings are illustrative: "the specific wording isn't what's important…
  work backwards from the suggested text to figure out what wasn't right about the
  original." Unanswered suggestions get re-pinged; ignoring feedback outright gets the PR
  closed.
- Technical claims get "Is this correct?" / "Did you mean…?" — answer them; the editor
  asks the owning SIG for a technical review when unsure.
- Post-release comms deadlines are enforced; exceptions come from Release Comms, and the
  blog team will help ask. Empty placeholder PRs are closed once the release ships.
- Blog approvers may apply small edits with maintainer access and self-approve when the
  article is still a draft (a separate PR is always needed to publish).
- Writing buddies are paired across concurrent PRs; both authors review each other's PR
  against the guidelines and review hints.
