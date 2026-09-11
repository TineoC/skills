# Release announcement body conventions

Companion to [SKILL.md](SKILL.md). SKILL.md covers eligibility, process, and front matter; this
file covers the *body* of a release announcement (`kubernetes-vX-Y-release`).

Derived from the review of
the v1.37 announcement:
330+ review threads across the full review cycle, nearly all of them the same dozen rules applied
over and over. Applying this file before requesting review frees reviewers to check technical accuracy
instead of mechanics.

## Tense and framing

Write as if the release **has already happened**. The announcement is read after publication.

- Pre-release behavior goes in the **past** tense. Do: "The route controller previously reconciled
  routes on a fixed interval, by default every 10 seconds." Don't: "reconciles routes on a fixed
  interval."
- Don't carry KEP proposal wording across. A KEP says "Currently, Kubernetes cannot frobnicate";
  the announcement says "As an Alpha feature, Kubernetes can now frobnicate."
- Watch for paragraphs that describe a Kubernetes several releases old. Two threads
  flagged text that described pre-1.36 and pre-1.32 behavior as if it were still current — this
  happens when a section is drafted from an old KEP or an old blog post.
- Describe the change, not the proposal document. "Describe the change itself, not the proposal
  document" was an explicit review comment.
- Audience is **users, not contributors**. Drop internals that only matter to someone writing the
  code (how a component computes a value internally, refactors, CI jobs).

## Headings

- **Sentence case, always.** Around 20 review suggestions were nothing but this fix.
  Release announcements are high-profile, and reviewers hold them to the style guide harder than
  they do ordinary blog posts.
- Phase prefix where the section is about one graduation event:
  `### Stable: Resilient watchcache initialization`, `### Alpha: Pod-level checkpoint and restore`.
- Sub-features under a grouping heading take `####`:
  ```markdown
  ### DRA features graduating to Stable
  #### DRA: ResourceClaim status with possible standardized network interface data
  #### DRA: derived attributes
  ```
- Add an explicit `{#anchor}` on **every** feature heading, not just long ones. Hugo's generated
  anchors churn whenever the heading text is edited, and release notes and social posts deep-link
  these sections: `### DRA: ResourceClaim support for workloads {#resourceclaim-support-for-workloads}`.
  Reviewers attached an anchor to essentially every heading suggestion they made.
- Component names keep their backticks inside headings, and the heading is still sentence case:
  `` ### `kubelet`: static Pods can no longer reference Secrets or ConfigMaps ``, not
  "### Kubelet: Static Pods…". Same for `` ### Deprecating `kube-proxy`'s support for `ipvs` mode ``.
- **No two headings with the same text.** The v1.37 draft used
  "Graduations, deprecations, and removals in v1.37" twice; rename the earlier one after the
  previous cycle's announcement.

## Naming and emphasis

This is the cluster of rules the v1.37 draft got backwards most often.

- **Italics, lowercase, on first mention of a feature or concept**: `_memory QoS_`,
  `_gang scheduling_`, `_native histograms_`, `_extended resource_`, `_node declared features_`,
  `_read your writes_`, `_pod-level resource managers_`.
- **Title Case is only for graduation phases** — Alpha, Beta, Stable, Generally Available. Do
  *not* Title Case a feature name. "Kubernetes v1.37 sees _gang scheduling_ graduate to Beta",
  not "Gang Scheduling graduates to beta".
- Prefer "Generally Available" over "GA" or "stable" in the intro summary sentence:
  "introduces new Generally Available, Beta, and Alpha features."
- **API kind spelling beats KEP-title spelling.** Write `ClusterTrustBundle`, not "Cluster Trust
  Bundle"; "Configurable tolerance for HorizontalPodAutoscalers", even though KEP #4951's title
  spells it with spaces.
- **Attribute changes to Kubernetes, not the SIG.** "Kubernetes has deprecated `kube-dns`", not
  "SIG Network has deprecated `kube-dns`" — it reads with more authority, and SIG credit belongs
  in the `led by` line. (Component-name backticks and Alpha/Beta capitalization rules from
  SKILL.md still apply.)
- U.S. English. It settles fulfill/fulfil-style disputes rather than leaving them to preference.

## Boilerplate

Each of these was hand-corrected more than a dozen times. Write them right the first time.

**KEP attribution**, one per feature section, no line break inside the link:

```markdown
This work was done as part of [KEP #NNNN](https://www.kubernetes.dev/resources/keps/NNNN/) led by [SIG Foo](https://www.kubernetes.dev/community/community-groups/sigs/foo/).
```

- **Link text is `KEP #NNNN` — number only, no KEP title, no `KEP-NNNN` hyphen form.** The v1.37
  draft mixed all three; the review converged on the bare `KEP #NNNN` form used by previous
  announcements, and every remaining title and hyphen was suggested away. Apply it in
  "learn more"/deprecation pointers too (`refer to [KEP #5573](…)`, not `[KEP-5573: Remove cgroup
  v1 support](…)`). This differs from the sneak-peek convention in SKILL.md, which does spell out
  the KEP title.
- Verify the KEP number against `kubernetes/enhancements`. One thread caught a wrong number that
  had survived to review.
- Two owning SIGs: `led by [SIG Node](…) and [SIG Network](…).`
- **Never break a line inside `[...]` or `(...)`.** This is the single most-repeated mechanical
  defect — roughly a dozen threads, several with preview screenshots showing the link
  rendering as literal text `[KEP #2021] (https://…)`. It breaks the rendered page, not just
  GitHub suggestions.
- **Internal links are site-relative**: `/releases/1.37/`, `/blog/2026/07/31/kubernetes-v1-37-sneak-peek/`,
  `/docs/tutorials/` — not `https://kubernetes.io/...`.

**Feature gate state.** Say it explicitly for every feature; reviewers asked for it every time it
was missing.

```markdown
This is an opt-in, off-by-default Alpha feature. To try it out, enable the
`SomeFeatureGate` feature gate.
```

For Beta and Stable, state whether it is on by default, in bold if it is a change:
"behind the `EtcdRangeStream` feature gate (kube-apiserver only, **on** by default)."

**Release page links.** The intro and the download section link the release page:
`[Kubernetes vX.Y](https://kubernetes.io/releases/X.Y/)`. Earlier announcements omitted this only
because `https://kubernetes.io/releases/a.bb/` did not exist yet.

**Social section** at the end:

```markdown
- Follow us on [Bluesky](https://bsky.app/profile/kubernetes.io) for the latest updates
- Follow us on [LinkedIn](https://www.linkedin.com/company/kubernetes/)
- Follow us on [X](https://x.com/kubernetesio)
```

## Mechanics that generate the most review threads

Every item here was corrected many times over. A single self-review pass on these
removes most of the review volume.

- **Wrap source Markdown at ~80 characters** — [style guide: line
  breaks](https://kubernetes.io/docs/contribute/style/style-guide/#line-breaks). Reviewers asked
  for this across the whole document. It does not conflict with the never-split-a-link rule: wrap
  at a word boundary *outside* every `[...](...)`, and let a long link overrun 80 characters
  rather than split it.
- **Backtick every component, tool, and daemon name, everywhere** — this was the single largest
  category of suggestions. The list that actually got flagged: `kubelet`, `kubectl`,
  `kube-apiserver`, `kube-proxy`, `kube-controller-manager`, `kube-dns`, `etcd`, `cAdvisor`,
  `nftables`, `nft`, `iptables`, `ipvs`, `kubeadm`. `etcd` and `cAdvisor` are the two that
  drafters most reliably leave bare.
- **Every fenced code block declares a language** — ```` ```shell ````, ```` ```yaml ````. A bare
  ```` ``` ```` was flagged on sight.
- **U.S. English**, confirmed by SIG Docs leads on this PR: "fulfill", not "fulfil".
- **Proofread pass for the dumb stuff**: doubled words ("support for for cluster
  administrators"), space before a colon ("promoted to Stable :"), stray blank lines mid-paragraph,
  trailing whitespace, `KEP-4960:Container` with no space after the colon.
- **Thousands separator on contributor counts**: "1,709 individuals", not "1709".

## Getting technical accuracy reviewed

Style review and accuracy review are separate jobs, and the second one does not happen on its own.
The workflow that worked:

- Post a review comment on **each feature section's heading** with `/cc` and the handles of that
  KEP's authors plus the owning SIG's leads — for example `/cc @<kep-author> @<sig-lead>` on the DRA
  taints section. One comment per section, so the reply lands in context.
- Pull the handles from the KEP's `authors`/`owning-sig` metadata in `kubernetes/enhancements` and
  the SIG's `sigs.yaml` leads, not from memory.
- Expect this to surface things no style pass can: a feature that graduated to Beta but shipped
  **disabled** by default after a late revert, a "graduation" that partly happened two releases
  ago, an "X has no visibility into Y" framing that overstates the old limitation, missing Node
  condition names, wrong bucket-boundary wording.
- KEP authors answer with `lgtm` or a rewrite of the paragraph. Take the rewrite — they are
  describing their own feature.

## Pre-review checklist

Run this before requesting review. It is where most of the 330+ threads came from.

1. **Diff against the previous cycle's announcement** — structure, section order, and the wording
   of the recurring paragraphs (intro, download, release team, project velocity, social). Reviewers
   compare against it directly ("using the same style as the 1.36 announcement… check the other
   pieces in the file too").
2. **Reconcile deprecations against the mid-cycle sneak-peek post.** Every deprecation announced
   there must appear here. A reviewer had to ask for this explicitly.
3. **Don't re-deprecate.** Check the deprecation is actually new in this release —
   `v1.Endpoints` was deprecated back in v1.21 and did not need announcing again.
4. **Cut entries that aren't end-user-visible.** A KEP having a milestone in this release is not
   enough; CI jobs and internal plumbing don't belong in an announcement.
5. **Link the evidence or cut the claim.** "If we're not linking them or listing them, let's not
   pretend that they are _well known_." Same for "significant", "major", "widely used".
6. **Get the owning SIG to check each section's framing.** Style passes cannot catch this. Real
   examples: DRA network devices were described as merely improved when they were
   basically unusable before; a `kube-proxy` efficiency gain in *rule management* read as if it were
   packet-routing throughput; a component described as pending a split that had already happened.
7. **Read every section as a user who has not read the KEP.** If a sentence only makes sense to
   someone who has, rewrite it.
8. **Check each "graduates to Stable/Beta" claim against the previous announcements.** A KEP can
   carry several feature gates that graduate in different releases — v1.37 credited "resilient
   watch cache initialization" as newly Stable when one of its two gates went Stable back in
   v1.34. Either state precisely which gate graduated now, or pick a different spotlight.
9. **Confirm the default state as of code freeze, not as of the KEP.** Pod-level resource managers
   graduated to Beta *disabled* by default after a late fix; cAdvisor-less stats went Beta
   off-by-default. Say "Beta, off by default" explicitly when that is the case.
10. **Name what is new in *this* release for multi-release work.** For a feature that has been Beta
    for two cycles, say what this cycle added (circuit breaking, extra metrics) rather than
    re-announcing the feature.
11. **Freeze the project-velocity numbers and their links.** The devstats links must use an
    explicit `to=<timestamp>`, never `to=now`, or the numbers in the prose drift away from the
    dashboard. Refresh the contributor/company counts right before merge — they moved during
    review — and phrase them as a maximum at any given time.
12. **Download section: no interactive tutorials.** Katacoda has been gone since 2023; link
    `/docs/tutorials/`, minikube, and kubeadm. The
    [release-blog template](https://github.com/kubernetes/sig-release/blob/master/release-team/role-handbooks/communications/templates/release-blog.md)
    still carries the stale wording twice — fix it there too rather than re-inheriting it next
    cycle.

## Reviewing an announcement: separate cosmetic from technical

When you review someone else's announcement — especially with tooling that posts many inline
suggestions at once — sort every change into one of two piles **before** posting anything. Mixing
them buries the claims that need scrutiny under dozens of whitespace fixes, and the trivial ones
can no longer be bulk-applied because a reviewer has to stop and think at each interleaved
technical claim.

**Cosmetic / formatting** — safe to apply on sight, no domain knowledge required:

- whitespace, trailing spaces on headings, line rewraps, blank lines mid-paragraph
- heading case, heading anchor ids (`{#some-anchor}`)
- backticks on component and tool names (`kubelet`, `kube-proxy`, `nftables`, `etcd`)
- emphasis markers, including a `_pair_` that spans a line break
- broken link syntax — `[SIG CLI] (url)` → `[SIG CLI](url)`, markdown links split across lines
- typos and grammar — `pn-place` → `in-place`, `anddisabling` → `and disabling`, tense fixes
- naming consistency — `cgroup` → `cgroups`, `1.37` → `v1.37`, `Cluster Trust Bundles` →
  `ClusterTrustBundles`
- paragraph moves where the wording is unchanged

**Technical** — anything that asserts or alters a fact: feature-gate names, defaults and stages,
version numbers, behavior descriptions, new sentences or whole sections. These are not review
nits; each one is a claim that has to be proved before it is posted.

A mechanical test that classifies most changes correctly: strip backticks, emphasis markers and
punctuation from both sides, then compare the **word sequences**. Identical sequence → cosmetic.
Different → technical, unless the only difference is one of the trivial substitutions listed above.

Post the cosmetic pile as pure ```suggestion blocks with no prose — they are self-explanatory, and
prose around them only slows the author down. Keep the technical pile out of the PR until each
claim is verified and the right person is tagged (below).

## Technical review: always tag the owner

**Never post a technical suggestion without naming the person who owns that feature.** A style
reviewer rewriting a paragraph about someone else's feature gate is guessing, and guesses that
look authoritative are worse than no comment. Every technical comment names at least one of:

1. the **KEP author(s)** — from `authors:` in the KEP's `kep.yaml` in `kubernetes/enhancements`
2. the **owning SIG's leads** — from `sigs.yaml`, when the KEP authors are unresponsive
3. the **feature author** — whoever wrote the implementation PR, found via the CHANGELOG entry or
   `git log` on the relevant package

Pull the handles from that metadata, never from memory. One comment per feature section, on the
section heading, so replies land in context.

This matters because upstream source and the feature owner can disagree, and you need them on the
thread to resolve it. A reviewer stated a route-controller feature had "moved to beta
now"; `pkg/features/kube_features.go`, the `controller-manager` staging package, `CHANGELOG-1.35.md`
and this site's own feature-gate page all said Alpha since v1.35, default false. The code wins for
what you write, but the owner is the one who confirms whether a promotion landed somewhere the
gate table doesn't yet reflect — so tag them and say which sources you checked.

When you correct a technical claim, cite the evidence in the comment: `file:line` in
`kubernetes/kubernetes`, or the KEP section. Pin the reference to a commit SHA when you link it.
And verify your own replacement text the same way — three suggestions had to be
withdrawn after checking: a feature-gate stage taken from the PR description rather than the gate
table, a "no-op unless the driver opts in" claim contradicted by the scheduler plugin's `Score`
function, and two error-constant names that did not exist in the codebase at all.
