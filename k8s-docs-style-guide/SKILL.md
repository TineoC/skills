---
name: k8s-docs-style-guide
description: Kubernetes documentation style guide — formatting, terminology, voice conventions, and Hugo shortcode reference from kubernetes.io/docs/contribute/style. Invoke by name when writing or editing Kubernetes/K8s-adjacent docs (READMEs, API references, tutorials, CRD docs) to match upstream style.
disable-model-invocation: true
---

Source: https://kubernetes.io/docs/contribute/style/style-guide/

Apply these rules when writing or editing Kubernetes-flavored documentation. Language is U.S. English.

## Casing and typography

| Do | Don't |
|---|---|
| The HorizontalPodAutoscaler resource is responsible for ... | The Horizontal pod autoscaler is responsible for ... |
| A PodList object is a list of pods. | A Pod List object is a list of pods. |
| Every ConfigMap object is part of a namespace. | Every configMap object is part of a namespace. |
| For managing confidential data, consider using the Secret API. | For managing confidential data, consider using the secret API. |

- API objects: UpperCamelCase (`PersistentVolume`, `HorizontalPodAutoscaler`).
- Placeholders: angle brackets, explained — `kubectl describe pod <pod-name> -n <namespace>`.
- UI elements: **bold** — Click **Fork**.
- New/defined terms: _italics_ on first introduction — a _cluster_ is a set of nodes. Feature and concept names go here too, italic and lowercase (`_gang scheduling_`, `_memory QoS_`) — never Title Case them.
- Filenames, directories, paths: code style — `envars.yaml`, `/docs/tutorials`.
- Quotation punctuation: period/comma outside the quotes — recorded with an associated "stage".
- Graduation phases (Alpha/Beta/Stable/Deprecated): Start Case, capitalized — "DRA is Beta," not "beta." These are the *only* thing that gets Start Case mid-sentence.
- API kind spelling wins over an upstream title that uses spaces — write HorizontalPodAutoscalers and ClusterTrustBundle even if the KEP or doc being cited says "Horizontal Pod Autoscalers" / "Cluster Trust Bundle".

## Code style vs. plain text

| Do | Don't |
|---|---|
| The `kubectl run` command creates a Pod. | The "kubectl run" command creates a Pod. |
| The kubelet on each node acquires a Lease… | The kubelet on each node acquires a `Lease`… |
| A PersistentVolume represents durable storage… | A `PersistentVolume` represents durable storage… |
| The CustomResourceDefinition's `.spec.group` field… | The `CustomResourceDefinition.spec.group` field… |
| Set the value of the `replicas` field. | Set the value of the "replicas" field. |
| Run the process as a DaemonSet in the `kube-system` namespace. | Run the process as a DaemonSet in the kube-system namespace. |
| The `kubelet` preserves node stability. | The kubelet preserves node stability. |
| The `kubectl` handles locating and authenticating to the API server. | The kubectl handles locating and authenticating to the apiserver. |
| Set the value to `"true"`. Set `replicas` to `3`. | Set the value to true. Set replicas to 3. |

Rules of thumb:
- Code style: commands, flags, field names, namespaces, tool/component names (`kubectl`, `kube-apiserver`), literal string/int field values.
- Plain text (no code style): general-concept nouns not referring to a literal API field or command — "the kubelet acquires a lease" (concept) vs. "the `Lease` object" (the API type).
- Never start a sentence with a bare tool/component name — prefix it: "The `kubeadm` tool bootstraps..." not "`kubeadm` tool bootstraps...".
- Prefer a general descriptor over a specific component name when the concept, not the component, is what matters: "The control plane component ..." over "The kube-apiserver ...".

## API terminology

Use precisely, don't conflate:
- **Resource** — a URL endpoint in the API, e.g. `/api/v1/namespaces/{namespace}/pods/{name}`.
- **Object** — an entity in the system, created from a resource.
- **Field** — a component of an object or resource definition.

Resource type names are lowercase and code-formatted: `pods`, `services`, `namespaces`.

## Code snippets

- No command prompt in snippets: `kubectl describe pod nginx`, not `$ kubectl describe pod nginx`.
- Show commands and their output as separate blocks, not interleaved.
- Test examples against the current and previous Kubernetes minor version; update or remove examples that reference deprecated APIs.
- Use MathJax for equations: `$$ ... $$` block, `$ ... $` inline.

## Voice and tense

| Do | Don't |
|---|---|
| This command creates a pod. | This command will create a pod. |
| The kubelet does not evict pods. | The kubelet will not evict pods. |
| The controller manages the deployment. | The deployment is managed by the controller. |
| Kubernetes schedules pods on nodes. | Pods are scheduled on nodes by Kubernetes. |
| You can create a Deployment by using `kubectl apply`. | One can create a Deployment by using `kubectl apply`. |
| For example, ... / That is, ... | e.g., ... / i.e., ... |

- Present tense, active voice, second person ("you").
- Simple, direct sentences over complex ones; common words over jargon.
- Spell out Latin abbreviations in English.

## Patterns to avoid

| Do | Don't |
|---|---|
| The Kubernetes API supports... | We support the Kubernetes API... |
| Version 1.4 removes... | In version 1.4, we removed... |
| Kubernetes can schedule work across clusters. | Kubernetes will be able to schedule work across clusters. |
| This page describes how ExecProbe works in Kubernetes v1.13 and earlier. | This page describes how ExecProbe works in Kubernetes. |

- Never say "we" — Kubernetes docs describe the system, not the authors.
- Don't state intentions about the future ("will be able to") — document what exists now.
- Don't write claims that go stale silently — pin version-dependent behavior to a version.
- Avoid "just", "simply", "easily", "obviously" and other words that assume the reader's understanding — they add nothing and can shame a struggling reader. Define technical terms on first use instead.

## Markdown conventions

- Blank line between paragraphs; two trailing spaces for a hard line break within one.
- Sentence-style capitalization for headings (`## Configuring a probe`, not `## Configuring A Probe`).
- One idea per paragraph; keep paragraphs short.
- Descriptive link text, never "click here"; relative paths for internal links (`/docs/concepts/overview/`), absolute URLs for external ones.
- Never break a line inside `[text](url)` — it renders fine but breaks GitHub suggestions and makes review harder. Let the line run long.
- `-` for unordered lists, `1.` for sequential/ordered steps.
- Pipe tables with a header separator row (`|---|---|`).
- Don't nest Hugo shortcodes (`{{< note >}}`, `{{< caution >}}`, `{{< warning >}}`) inside numbered lists or `{{% include %}}` statements — they don't render correctly there; use plain Markdown instead.
- Full shortcode syntax (feature-state, glossary, tabs, api-reference, table captions, version strings, code samples, details): see [hugo-shortcodes.md](hugo-shortcodes.md).
