# A reproducible method for auditing Claude Code controls

**Purpose:** distinguish a public configuration contract, a declaration in an installed executable,
a traced consumer, and behavior actually observed in a controlled test. This method applies to one
pinned artifact at a time. It is not a catalog of supported internal flags or a recommendation to
enable them.

The [2.1.239 method](claude-code-hidden-settings-2.1.239.md#method) established the progression from
artifact identity through consumers and public comparison to loopback tests. The
[2.1.280 method](claude-code-hidden-settings-2.1.280.md#method-and-counting-boundaries) refined it with
syntax-tree binding resolution, computed readers, schema composition, and explicit counting
boundaries. Their findings remain version-specific; repeat the investigation rather than importing
an old name, parser, offset, count, or behavioral conclusion into a new report.

## 1. Define the claim and its evidence level

Keep public status and implementation evidence as separate dimensions. A documented control can be
ineffective in one configuration; an undocumented name can have an active consumer. An observed
request change does not establish a public support contract.

| Implementation evidence     | What it establishes                                                    | What it does not establish                                     |
| --------------------------- | ---------------------------------------------------------------------- | -------------------------------------------------------------- |
| Literal occurrence          | The bytes contain a name or text                                       | A declaration, a read, reachability, or behavior               |
| Declaration                 | An environment export or settings shape names a field                  | That any consumer uses it                                      |
| Bound read                  | Executable syntax reads the actual environment/settings binding        | That this path runs in the selected session                    |
| Traced effect               | The value reaches a gate, resolver, renderer, request builder, or tool | That all gates are satisfied at runtime                        |
| Isolated observation        | A specified process and fixture produced the measured effect           | Live API acceptance, model compliance, or other untested modes |
| Authorized integration test | A specified real integration worked                                    | Universal availability across accounts, platforms, or releases |

For public status, distinguish the current canonical reference, another current official page,
release history, a public example, the linked schema, and absence from the checked corpus. Preserve
experimental, deprecated, removed, host-owned, and runtime-output labels. Do not collapse these
categories into “supported” or “hidden.”

Use **hidden candidate** only with an explicit definition: a name with the stated level of traced
consumption that is absent from a specified, successfully fetched public-source set. It does not
mean safe, stable, useful, or intentionally user-configurable. A name can be absent even when the
feature is documented through another setting or CLI flag.

## 2. Pin the installed artifact without changing it

Record the resolved executable path, installation channel, reported version if already available,
platform/architecture, byte length, and SHA-256. Inspect package-manager inventory and symlinks
without updating the installation. Under a no-execution assignment, do not run even `--version`;
identify the version from supplied metadata and embedded bytes and label that distinction.

Extract the embedded version, build timestamp, and internal revision with their exact byte spans.
Keep three identities separate:

- **Executable identity:** its SHA-256 and embedded build revision.
- **Public release identity:** the release tag and the commit to which the tag resolves.
- **Schema identity:** the separately maintained schema's source revision and content hash.

A matching version string does not establish that the public repository contains the executable's
source. The [2.1.280 artifact record](claude-code-hidden-settings-2.1.280.md#installed-artifact-and-source-revisions)
states this distinction: “The embedded build revision identifies the executable's build. The public
commit pins the release repository and changelog.”

Use zero-based byte offsets and half-open spans `[start, end)`. A small offline check, supplied with
an explicit artifact path, expected hash, and span, can recover an excerpt without launching it:

```python
# Save as inspect_span.py; pass ARTIFACT SHA256 START END.
import hashlib
from pathlib import Path
import sys

path, expected, start, end = sys.argv[1:]
data = Path(path).read_bytes()
assert hashlib.sha256(data).hexdigest() == expected
start, end = int(start), int(end)
assert 0 <= start <= end <= len(data)
print(data[start:end].decode("utf-8"))
```

Retain the original artifact where permitted, or retain enough identity information for an
independent reader to obtain and verify it. Re-hash before using an old offset against a file that
may have been replaced by an updater.

## 3. Extract byte-accurate source regions

First inspect the artifact's actual source header and storage layout. For builds with NUL-terminated
embedded JavaScript, locate the exact header bytes, then take the bytes up to the following NUL.
Validate the product/source header as well as the bundler marker. Do not assume every future build
uses the same packaging, module names, headers, or encoding.

The [2.1.280 extraction record](claude-code-hidden-settings-2.1.280.md#method-and-counting-boundaries)
describes regions beginning with “`// @bun @bytecode` and Claude Code source header” and notes that
“the extracted code was analyzed without executing it.” Adopt those checks only after confirming
that the new artifact has that layout.

For each region, preserve:

- Start byte, exclusive end byte, source-byte length, and SHA-256.
- Original UTF-8 bytes, including line endings; decode strictly, never with replacement characters.
- Embedded module path when recoverable, with the evidence that associates it with the region.
- A stable offset-based filename when the original path is unresolved.
- Syntax-parser version, options, success/failure, and diagnostics.

Never format the primary extraction. If a readable copy helps, keep it separate and map its spans
back to the original. JavaScript parser positions often count UTF-16 code units; Python string
positions count Unicode code points. Neither is automatically a UTF-8 byte offset. Convert the
source prefix using the parser's indexing convention before adding the module's binary start.
Verify the resulting bytes against the artifact.

Report extraction completeness honestly: marker hits, accepted regions, rejected/truncated regions,
source-byte sum, successfully parsed modules, and failures. A syntax parse is not module execution,
reachability proof, or proof that embedded source and the executable's bytecode are equivalent.

The following offline extractor recognizes the exact two-line header used by the inspected build
family. Confirm that header on each new artifact. It records unmatched bundler markers separately,
validates every accepted region before writing, and requires a new output directory. The manifest
provides the source-byte count and the information needed to reproduce absolute offsets; original
module-path recovery and JavaScript parsing are subsequent steps.

```python
# Save as extract_sources.py; pass ARTIFACT SHA256 NEW_OUTPUT_DIRECTORY.
import hashlib
import json
from pathlib import Path
import re
import sys

artifact, expected, destination = sys.argv[1:]
data = Path(artifact).read_bytes()
assert hashlib.sha256(data).hexdigest() == expected
marker = b"// @bun @bytecode\n"
header = marker + (
    b"// Claude Code is a Beta product per Anthropic's Commercial Terms of Service.\n"
)
records, skipped = [], []
previous_end = -1
for match in re.finditer(re.escape(marker), data):
    start = match.start()
    if not data.startswith(header, start):
        skipped.append(start)
        continue
    end = data.find(b"\0", start)
    assert end >= start, f"Missing NUL at {start}"
    assert start > previous_end, f"Overlapping region at {start}"
    source = data[start:end]
    source.decode("utf-8", errors="strict")
    records.append({
        "path": f"modules/module-{start}.js",
        "start": start, "end": end, "bytes": end - start,
        "sha256": hashlib.sha256(source).hexdigest(),
    })
    previous_end = end
assert records, "No matching source headers"
root = Path(destination)
root.mkdir(parents=True, exist_ok=False)
(root / "modules").mkdir()
for record in records:
    (root / record["path"]).write_bytes(data[record["start"]:record["end"]])
manifest = {
    "artifact_sha256": expected, "marker_hits": len(records) + len(skipped),
    "skipped_header_offsets": skipped, "modules": records,
    "source_bytes": sum(record["bytes"] for record in records),
}
(root / "manifest.json").write_text(json.dumps(manifest, indent=2) + "\n")
```

An extraction fixture should include multiple modules, Unicode before a target expression, a marker
inside an unrelated string, and a missing NUL terminator. Check that mismatched headers are listed
as skipped and that invalid UTF-8, overlapping accepted regions, a wrong artifact hash, and a
missing terminator fail. This example rejects an exact nested header as overlapping; inspect such
an ambiguity before changing the extraction rule. Compare extracted bytes and totals against an
independent extraction of the pinned artifact.

## 4. Resolve the actual environment binding

Start with name searches to locate the generated environment map and parser factory. Then use a
JavaScript syntax tree and lexical bindings to follow each relevant edge:

1. Exported property name to the declared symbol or export getter.
2. Symbol to its constructor and any wrapper, default, coercion, or validator.
3. Export binding to imports, re-exports, namespace access, and local aliases in consumers.
4. Local binding to reads and their enclosing condition or call.
5. Consumer return value to the final prompt, tool description, attachment, request, UI, or action.

Do not treat an identifier with the same spelling in another scope as the same object. Exclude
comments, ordinary strings, declarations alone, object keys without reads, shadowed bindings,
write-only assignments, and deletions from a direct-read inventory. Compound assignments and
updates can read and write; classify those separately instead of dropping them with plain writes.
Record destructuring, optional chaining, aliases, and direct `process.env` reads explicitly. Verify
that `process` itself is not a shadowed local. Inspect the symbol's declaration kinds: a JavaScript
property assignment can produce an analyzer symbol without declaring a local binding. Include a
fixture that assigns `process.env.UNRELATED` and then reads another environment key in the same
module, alongside genuine parameter/local-shadowing fixtures.

Define “direct” before counting. For example, a conservative direct set can include literal property
reads from the proven environment binding and `process.env`, while recording destructured aliases
and computed flows separately. Whatever convention is chosen, retain the evidence categories so
another run can reproduce the same set.

### Computed access needs a second pass

Search for dynamic access such as `env[key]`, a wrapper like `readEnv(name)`, and descriptors holding
an environment name. Trace the possible values of `key` or `name` through constant objects, arrays,
function parameters, and call sites. Resolve dependency injection and closure aliases where they
matter to a claim. Distinguish a statically resolved literal-key access from an unresolved dynamic
key; state which inventory contains each.

The [2.1.280 computed-reader example](claude-code-hidden-settings-2.1.280.md#computed-reminder-readers)
contains `let r=a[e.envVar]` and descriptors that supply the actual names. This is why “no direct
read found” is not evidence that a control is dead. Conversely, a descriptor containing a name is
not enough without the reader and downstream effect.

### Parser shape is only one part of semantics

Trace the parser implementation rather than inferring behavior from a constructor called `bool`,
`triBool`, `int`, or `enum`. Record accepted inputs, trimming/case normalization, unset and invalid
results, bounds, and whether values are latched or re-read.

Then trace the consumer's operation. For a boolean value, `value || fallback` permits the fallback
when `value` is false; `value ?? fallback` preserves false. An explicit-value branch such as
`value !== undefined` can also preserve false, but later availability or policy gates may still
veto the feature. The [2.1.280 parser discussion](claude-code-hidden-settings-2.1.280.md#method-and-counting-boundaries)
summarizes this as “Consumer logic decides precedence.” Neither a constructor name nor an initial
resolver proves a final off switch.

For a derived number, cite both operands and the operation: the constant/default definition,
validation/clamping, units conversion, and the consuming expression. For precedence, cite each
competing source and the operation choosing among them. Do not replace these with a schema's prose
description or a similarly named resolver.

## 5. Reconstruct the effective root settings shape

Locate the settings loader and actual root constructor it invokes. Distinguish `settings.json` from
global preferences, plugin option schemas, tool input schemas, managed policy, and host-specific
configuration. A search for every object property in the bundle cannot identify the root contract.

Follow every contribution to the root shape:

- Direct object properties and imported fragments.
- Object spreads, helper-returned shapes, merges, and extensions.
- Feature registries: selected entries, each shape factory, and its build/runtime gate.
- Conditional spreads, including which inputs make a field present.
- Duplicate keys and merge order, so a later override is not counted twice.
- Aliases, editor annotations, nested-only roots, catchalls, and unknown-key handling.

Report unconditional declarations separately from potentially declared conditional fields. If a
registry or helper remains unresolved, report the known set as incomplete; do not call it the full
schema. Count names as a set, and retain each expression and dependency that supports membership.

Inspect whether the constructor is strict, strips unknown properties, or preserves them. The
[2.1.280 root analysis](claude-code-hidden-settings-2.1.280.md#root-settings-declared-by-the-executable)
quotes `...Ar(e)}).passthrough()` and cautions: “Unknown properties can survive parsing; a
successfully parsed key still needs a functioning consumer.” That conclusion describes the cited
build's constructor, not a guarantee for later releases.

Trace each important key beyond the declaration: loading and merge order, trusted-source filters,
normalization, compatibility rewrites, default resolvers, read sites, and the effect. Separate
schema descriptions and migration/config-snapshot references from active consumers. A stored consent
record is not permission to preseed consent; a shell-command helper is not a harmless scalar.

## 6. Build a versioned public comparison corpus

Use unauthenticated public static requests. Do not log in, refresh credentials, inspect authentication
caches, or fetch private source for this comparison. Save raw response bodies before parsing them.

### Pin what can be pinned

- Fetch release metadata for the exact tag, not just `latest`.
- Resolve the tag reference; peel annotated tags until the commit is known.
- Fetch the public repository archive by that commit and its complete tree. Check API pagination,
  rate-limit errors, and the tree's truncation flag.
- Inventory every archive member. Verify regular-file paths and Git blob identities against the
  tree; handle symlinks and submodules explicitly rather than following them or counting them as
  ordinary text. Reject archive paths that escape the scratch directory.
- Follow the schema URL in the fetched settings guide. Fetch the published bytes and the file's
  source history, then fetch the revision-pinned file. Compare bytes. If they differ, retain both
  and leave the published file's exact source revision unresolved until proven.

A Git blob identity uses SHA-1 over the header and bytes: `SHA1("blob " + decimal_byte_length + NUL +
file_bytes)`. Keep an additional SHA-256 of each downloaded artifact and text file for the audit.
Do not infer a schema revision merely from a repository's latest commit or a matching filename.

### Snapshot rolling documentation

Fetch the canonical settings guide, settings index, and environment table, plus complete related
pages for CLI flags, prompts, memory/context, tools, permissions, sandboxing, agents, teams,
workflows, hooks, plugins, models/providers, caching, telemetry, sessions, remote execution, and SDK
interfaces. Use the official documentation index to discover moved or newly split pages. An old URL
redirecting to a generic landing page is not coverage of the old page's subject.

For each request, save requested/final URLs, UTC fetch time, status, content type, byte length,
SHA-256, and useful response headers. Record failures, redirects, and exclusions. Validate the body:
HTTP 200 can still be HTML, an error page, or a generic redirect target rather than the requested
Markdown. Count requested URLs, accepted pages, duplicate bodies, and rejected responses separately.
Do not imply that a rolling page was the documentation shipped with the release.

Retain the raw line layout used by citations. Every material public claim should have a snapshot
path, source URL, content hash, one-based line range, and short exact excerpt. Large table counts
need the full row inventory and the parser's boundary rules, not just an example row.

### Normalize before comparing

For the settings index:

1. Identify the actual table by its heading and columns, not all backticked words on the page.
2. Preserve each row's exact name, scope, description, and source line.
3. Exclude global-config rows from the settings-file root set.
4. Map nested paths to their root and deduplicate. Include a root represented only by nested rows.
5. Track aliases and `$schema` separately from canonical behavioral names.
6. Keep removed/no-op rows in the indexed set, but exclude them from an explicitly defined
   currently behavioral subset. Deprecation alone does not imply no behavior.

Count environment-table rows separately from names mentioned anywhere in the public corpus. Do not
count headings, examples, wildcard prefixes, or other tables as canonical variable rows. Preserve
runtime-output and obsolete labels instead of calling every row a user input.

For public presence, scan the **full contents** of all accepted docs, all successfully decoded text
files in the pinned repository, and the schema. Store every exact-name occurrence, including
release history and examples. Do not scan only snippets returned by a search engine or only the
canonical table. Keep prefix patterns unexpanded unless the source explicitly defines the concrete
names. State the matching expression and encoding/exclusion rules.

## 7. Publish reproducible sets and arithmetic

Name the sets used in the report. One conservative convention is:

- `E`: typed exported environment names in the chosen namespace.
- `R`: exported names with a binding-resolved direct read, so `R` is a subset of `E`.
- `C`: separately resolved computed/dependency-injected names.
- `U`: direct-read names outside the typed export set.
- `P`: exact names in the checked public-source union.
- `B`: potentially declared root settings names, with conditional members labeled.
- `I`: normalized canonical public settings roots.
- `A`: editor annotations and accepted aliases excluded from canonical root comparisons.

Then show the actual operands for each reported total:

- Exported names outside direct reads: `|E − R| = |E| − |R|`.
- Strict direct-read hidden candidates: `|R − P| = |R| − |R ∩ P|`.
- Additional computed hidden candidates: `|(C − R) − P|`, not all of `C` added again.
- Unindexed declarations: `|B − (I ∪ A)|`; subtract intersections, not whole sets whose overlap is
  unknown.
- Public union: `|docs ∪ repo ∪ schema|`; account for intersections instead of summing counts.

Keep `U` outside calculations defined only over typed exports. Keep unresolved dynamic reads out of
strict totals and visible in coverage limits. Publish sorted machine-readable sets and per-name
provenance so every subtraction can be recomputed independently.

For release comparisons, use both complete inventories under the same rules. A new index row is not
necessarily a new feature; a missing row is not necessarily removed code. Wider public-source
coverage can reduce a hidden-candidate count without any executable change. If only the older
report is available, label a reconstruction from that report and do not invent a precise binary
addition/removal count.

## 8. Test locally only with explicit authorization and isolation

Static analysis and public fetching do not authorize running the executable. If execution is out of
scope, stop at the appropriate evidence level. Authentication repair, credential generation,
consent changes, and live-profile experiments are separate user-run or explicitly authorized work.

When loopback testing is authorized, build a disposable fixture rather than using the normal profile:

- Use an explicit child-environment allowlist, not the parent's environment minus a few known keys.
- Give each case separate HOME, configuration, cache, temporary, and working directories. Avoid
  inheriting shell startup files, proxy variables, cloud-provider credentials, or agent sockets.
- Disable automatic configuration/plugin/hook discovery through the exact build's verified
  controls. Supply empty settings sources and an empty strict MCP configuration where supported.
  Check managed/host sources separately; a temporary HOME alone does not eliminate them.
- Bind the fake API to loopback only, preferably a dynamically allocated port. Supply only a dummy,
  non-secret local protocol key. No real account or billing is needed for request serialization.
- Enforce an OS/container boundary that blocks external network access, real home/config reads,
  credential stores, and credential-helper execution. Restrict access to unrelated localhost
  services and Unix sockets as far as the fixture permits. Do not mistake a base-URL override for
  an egress boundary.
- Validate isolation before Claude starts: the intended loopback request must succeed, an external
  connection must fail for a reason stronger than DNS failure, and a harmless canary in a denied
  real-home path must be unreadable. Never probe by reading actual secrets.
- Use bounded processes, capture exit codes and diagnostics, terminate fixture-owned children on
  failure, and remove only task-owned temporary files.

The [2.1.280 runtime scope](claude-code-hidden-settings-2.1.280.md#runtime-test-scope) records both
successful loopback traffic and a failed outbound probe, while acknowledging that its profile
“permits other filesystem paths and localhost ports.” State a test boundary's limitations rather
than describing it as universal isolation.

### Capture requests with a deterministic protocol fixture

Capture complete serialized Messages requests, including system content, message blocks, tool
schemas/descriptions, selected model, and relevant headers. Return fixed valid responses, including
streaming events when required. Unknown routes, malformed requests, tool mismatches, and unexpected
additional turns should fail the fixture rather than silently receive a generic success response.

For a multi-turn reminder, return a predetermined tool-use response against a harmless fixture file,
verify the CLI's corresponding tool result, then capture the next request. Keep all paths inside the
fixture. Count issued tool calls separately from successful results and actual file reads: caching
or unchanged-file optimization can make those totals differ.

### Pair every absence claim with a positive control

Start with a smoke case that proves capture works. Then use a documented, traced prompt/tool control
to validate the measurement path. For each candidate:

1. Hold model, mode, fixture, input, and other controls constant.
2. Compare unset, explicit off, explicit on, and relevant valid/invalid values as justified by the
   parser. Use fresh processes when values may latch.
3. Require the positive case to exhibit the target content on the same eligible path.
4. Exercise the model defaults and additional gates necessary to distinguish a real off override
   from a false value that merely permits fallback.
5. For settings/environment counterparts, test each input source and their conflict separately;
   an environment-only test does not prove the settings loader or precedence.
6. For conversation-dependent behavior, assert absence before the triggering event and presence
   afterward, with a negative case that reaches the same event.

Inspect structured request fields or exact standalone text lines, not arbitrary substrings of the
whole JSON. A custom marker can also occur in a filename, user prompt, or tool result; those matches
do not prove reminder injection. Keep raw requests and a narrowly normalized comparison view,
listing every field removed as nondeterministic. Request-size changes alone are weak evidence.

Record a case matrix, expected assertions, actual request counts, successful tool results, failed
cases, retries, and smoke tests separately. Derive totals from captures rather than from the planned
matrix. A mode that never reaches the positive-control path is unverified, not evidence that the
control is ineffective. An alternate branch may establish shared serialization but not the
untested mode's complete behavior.

## 9. Package evidence for independent verification

Keep the version report readable and put bulky machine evidence in an audit bundle. A useful layout
contains the artifact manifest, original extracted modules, extraction/parse index, declaration and
consumer records, public snapshots and fetch manifest, normalized name sets, test harness and
fixtures, raw request captures, assertions, and a verification summary. Record tool/parser versions
and exact commands with non-secret inputs.

For every decision-critical claim, provide enough evidence to check both the quotation and the
inference:

- **Binary:** artifact hash, module start, original source line, absolute byte span, exact excerpt,
  and the definitions/imports on which the interpretation depends.
- **Public:** source URL, raw snapshot hash/path, line range, exact excerpt, and current versus
  historical status.
- **Derived count/value:** named input sets or operands, operation, exclusions, and machine output.
- **Runtime:** case ID, exact non-secret environment/settings delta, model/mode, request/turn ID,
  assertion target, positive control, exit status, and isolation checks.

An offline public-snapshot verifier can check hashes and exact line excerpts without re-fetching
rolling URLs. The following schema expects each record to have `path`, `sha256`, `line_start`,
`line_end`, and `excerpt`, with paths relative to the snapshot directory:

```python
# Save as verify_excerpts.py; pass SNAPSHOT_DIRECTORY RECORDS_JSON.
import hashlib
import json
from pathlib import Path
import sys

root = Path(sys.argv[1]).resolve()
records = json.loads(Path(sys.argv[2]).read_text())
for record in records:
    path = (root / record["path"]).resolve()
    assert path.is_relative_to(root)
    raw = path.read_bytes()
    assert hashlib.sha256(raw).hexdigest() == record["sha256"]
    lines = raw.decode("utf-8").splitlines()
    start, end = record["line_start"], record["line_end"]
    assert 1 <= start <= end <= len(lines)
    assert "\n".join(lines[start - 1:end]) == record["excerpt"]
print(f"Verified {len(records)} excerpts")
```

Have a second check recompute table normalization and set arithmetic from the raw snapshots, verify
source spans against the artifact, and replay content assertions from saved captures. A matching
excerpt proves what the source says; it does not by itself validate alias resolution, arithmetic,
reachability, or the behavioral conclusion. Check those separately.

## 10. State the limits before recommending changes

The final report should make these boundaries explicit:

- Unfetched, failed, redirected, or untranslated sources are outside public-absence claims.
- Unparsed modules, unresolved dynamic keys, native code, and untraced hosts limit static coverage.
- Declaration, parse acceptance, compatibility persistence, and live consumption are different.
- Account rollout, compiled defaults, platform, policy, trust scope, and startup/session latching can
  all constrain an otherwise active control.
- Loopback tests establish local serialization or fixture behavior, not real provider acceptance,
  billing, model quality/compliance, UI behavior, or server-side entitlement.
- An old spelling disappearing does not prove a feature disappeared or identify its replacement.
- Auth, consent, remote execution, upload, policy, privacy, and executable-helper controls need
  separate authorization and end-to-end evaluation; do not experiment on a live profile by default.

Prefer the current documented setting when it covers the intended behavior. Report blocked and
unverified paths directly. The result of a rigorous audit is a bounded, reproducible claim about an
exact artifact, with enough evidence to repeat the audit on its successor.
