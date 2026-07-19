Source: https://kubernetes.io/docs/contribute/style/hugo-shortcodes/

Reference for the Hugo shortcodes used across kubernetes.io. Consult when a doc needs one of these constructs.

## Note, caution, warning

```
{{< note >}}
This is a note.
{{< /note >}}

{{< caution >}}
This is a caution.
{{< /caution >}}

{{< warning >}}
This is a warning.
{{< /warning >}}
```

Don't nest these inside numbered lists or `{{% include %}}` statements — they don't render correctly there.

## Feature state

```
{{< feature-state state="STATE" >}}
{{< feature-state for_k8s_version="VERSION" state="STATE" >}}
{{< feature-state feature_gate_name="GATE_NAME" >}}
```

- `state`: `alpha`, `beta`, `deprecated`, or `stable`.
- `for_k8s_version`: optional, defaults to the page/site version, e.g. `"v1.10"`.
- `feature_gate_name`: pulls state from the feature gate's description file instead of a literal `state`.

```
{{< feature-state state="stable" >}}
{{< feature-state for_k8s_version="v1.10" state="beta" >}}
{{< feature-state feature_gate_name="NodeSwap" >}}
```

## Feature gate description

```
{{< feature-gate-description name="GATE_NAME" >}}
```

Example: `{{< feature-gate-description name="DryRun" >}}`

## Glossary

```
{{< glossary_tooltip text="DISPLAY_TEXT" term_id="TERM_ID" >}}
{{< glossary_definition prepend="PREFIX_TEXT" term_id="TERM_ID" length="LENGTH" >}}
{{< glossary_definition term_id="TERM_ID" length="LENGTH" >}}
```

- `text`: display text for the tooltip.
- `term_id`: glossary term identifier.
- `prepend`: optional text prepended to the definition.
- `length`: `"short"` or `"all"`.

```
{{< glossary_tooltip text="cluster" term_id="cluster" >}}
{{< glossary_definition prepend="A cluster is" term_id="cluster" length="short" >}}
{{< glossary_definition term_id="cluster" length="all" >}}
```

## API reference links

```
{{< api-reference page="API_PAGE" >}}
{{< api-reference page="API_PAGE" anchor="ANCHOR" >}}
{{< api-reference page="API_PAGE" anchor="ANCHOR" text="LINK_TEXT" >}}
```

- `page`: URL suffix of the API reference page, e.g. `"core/pod-v1"`.
- `anchor`: optional section anchor.
- `text`: optional custom link text.

```
{{< api-reference page="core/pod-v1" >}}
{{< api-reference page="core/pod-v1" anchor="PodSpec" >}}
{{< api-reference page="core/pod-v1" anchor="environment-variables" text="Environment Variable" >}}
```

## Table captions

Screen-reader-only caption for a table:

```
{{< table caption="CAPTION_TEXT" >}}
Parameter | Description | Default
:---------|:------------|:-------
`timeout` | The timeout for requests | `30s`
`logLevel` | The log level for log output | `INFO`
{{< /table >}}
```

## Tabs

```
{{< tabs name="UNIQUE_TAB_NAME" >}}
{{< tab name="TAB_NAME" codelang="LANGUAGE" >}}
CONTENT
{{< /tab >}}
{{< tab name="TAB_NAME" include="FILE" />}}
{{% tab name="TAB_NAME" %}}
MARKDOWN_CONTENT
{{% /tab %}}
{{< /tabs >}}
```

- `name` on `tabs`: unique identifier for the whole tab set — must be unique within the page.
- `name` on `tab`: the tab's display label.
- `codelang`: highlighting language for code content (`bash`, `go`, etc.).
- `include`: relative path (or leaf-bundle file) to include as the tab's content; this form is self-closing (`/>}}`), no matching `{{< /tab >}}`.
- Use `{{% %}}` delimiters when a tab's inner content is Markdown; use `{{< >}}` delimiters for code/HTML content.

```
{{< tabs name="tab_with_code" >}}
{{< tab name="Tab 1" codelang="bash" >}}
echo "This is tab 1."
{{< /tab >}}
{{< /tabs >}}

{{< tabs name="tab_with_md" >}}
{{% tab name="Markdown" %}}
This is **some markdown.**
{{% /tab %}}
{{< /tabs >}}

{{< tabs name="tab_with_file" >}}
{{< tab name="Content File #1" include="example1" />}}
{{< /tabs >}}
```

## Version strings

- `{{< param "version" >}}` — current page/site version.
- `{{< latest-version >}}` — latest Kubernetes version.
- `{{< latest-semver >}}` — latest semantic version.
- `{{< version-check >}}` — version validation.
- `{{< latest-release-notes >}}` — link to the latest release notes.

## Code samples

```
{{% code_sample %}}
```

Embeds a source code file inline; requires the file to exist in the page's associated example bundle.

## Third-party content marker

Wraps a list of external tools/content so it's flagged as third-party rather than an official recommendation:

```
{{< third-party-content >}}
{{< third-party-content-list >}}
{{< third-party-content-item "ITEM_NAME" >}}
{{< /third-party-content-list >}}
{{< /third-party-content >}}
```

## Collapsible details

```
{{< details "SUMMARY_TEXT" >}}
CONTENT
{{< /details >}}
```
