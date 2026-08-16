# Release announcement body conventions

Companion to [SKILL.md](SKILL.md). SKILL.md covers eligibility, process, and front matter; this
file covers the *body* of a release announcement (`kubernetes-vX-Y-release`).

Derived from the review of
[kubernetes/website#56990](https://github.com/kubernetes/website/pull/56990) (v1.37 announcement):
128 review threads across 11 reviews, nearly all of them the same dozen rules applied over and
over. Applying this file before requesting review frees reviewers to check technical accuracy
instead of mechanics.

## Tense and framing

Write as if the release **has already happened**. The announcement is read after publication.

- Pre-release behavior goes in the **past** tense. Do: "The route controller previously reconciled
  routes on a fixed interval, by default every 10 seconds." Don't: "reconciles routes on a fixed
  interval."
- Don't carry KEP proposal wording across. A KEP says "Currently, Kubernetes cannot frobnicate";
  the announcement says "As an Alpha feature, Kubernetes can now frobnicate."
- Watch for paragraphs that describe a Kubernetes several releases old. Two threads in #56990
  flagged text that described pre-1.36 and pre-1.32 behavior as if it were still current — this
  happens when a section is drafted from an old KEP or an old blog post.
- Describe the change, not the proposal document. "Describe the change itself, not the proposal
  document" was an explicit review comment.
- Audience is **users, not contributors**. Drop internals that only matter to someone writing the
  code (how a component computes a value internally, refactors, CI jobs).

## Headings

- **Sentence case, always.** Around 20 review suggestions in #56990 were nothing but this fix.
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
- Add an explicit `{#anchor}` on any heading likely to be deep-linked from release notes or
  social posts, especially long ones:
  `### DRA: ResourceClaim support for workloads {#resourceclaim-support-for-workloads}`.

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

Each of these was hand-corrected more than a dozen times in #56990. Write them right the first time.

**KEP attribution**, one per feature section, no line break inside the link:

```markdown
This work was done as part of [KEP #NNNN: Exact KEP title](https://www.kubernetes.dev/resources/keps/NNNN/) led by [SIG Foo](https://www.kubernetes.dev/community/community-groups/sigs/foo/).
```

- Verify the KEP number against `kubernetes/enhancements`. One thread caught a wrong number that
  had survived to review.
- Two owning SIGs: `led by [SIG Node](…) and [SIG Network](…).`
- **Never break a line inside `[...]` or `(...)`.** Hard-wrapped links still render, but they
  break GitHub suggestions and make review painful — this was flagged repeatedly.

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

## Pre-review checklist

Run this before requesting review. It is where most of the 128 threads came from.

1. **Diff against the previous cycle's announcement** — structure, section order, and the wording
   of the recurring paragraphs (intro, download, release team, project velocity, social). Reviewers
   compare against it directly ("using the same style as the 1.36 announcement… check the other
   pieces in the file too").
2. **Reconcile deprecations against the mid-cycle sneak-peek post.** Every deprecation announced
   there must appear here. A reviewer had to ask for this explicitly in #56990.
3. **Don't re-deprecate.** Check the deprecation is actually new in this release —
   `v1.Endpoints` was deprecated back in v1.21 and did not need announcing again.
4. **Cut entries that aren't end-user-visible.** A KEP having a milestone in this release is not
   enough; CI jobs and internal plumbing don't belong in an announcement.
5. **Link the evidence or cut the claim.** "If we're not linking them or listing them, let's not
   pretend that they are _well known_." Same for "significant", "major", "widely used".
6. **Get the owning SIG to check each section's framing.** Style passes cannot catch this. Real
   examples from #56990: DRA network devices were described as merely improved when they were
   basically unusable before; a kube-proxy efficiency gain in *rule management* read as if it were
   packet-routing throughput; a component described as pending a split that had already happened.
7. **Read every section as a user who has not read the KEP.** If a sentence only makes sense to
   someone who has, rewrite it.
