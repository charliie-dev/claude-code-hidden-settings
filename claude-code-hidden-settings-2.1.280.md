# Claude Code hidden settings analysis — v2.1.280

**Snapshot:** 2026-09-23 local date. **Target:** the installed Homebrew `claude-code@latest`
executable, Claude Code **2.1.280**. Public references were fetched on 2026-09-22 UTC. This report
follows the artifact-pinning, runtime-read tracing, public-source comparison, and loopback
differential method in the [2.1.239 report](claude-code-hidden-settings-2.1.239.md#method).

## Findings

- `CLAUDE_CODE_BISON_CAIRN=0` now removes `# Delivering work`, including on the tested Opus 5 prompt
  bundle. Its parser is now `triBool`; both the capability resolver and final prompt assembly honor
  explicit `false`. The earlier report's force-on-only conclusion applies to its 2.1.239 snapshot.
- `CLAUDE_CODE_LARCH_CISTERN` and `CLAUDE_CODE_AMBER_ASTROLABE` retain force-on semantics. Their `0`
  values leave the corresponding sections present in the tested Opus 5 and Fable 5 requests.
- `CLAUDE_CODE_COZY_TEAPOT=strict|relaxed` selects the wording of the Bash-first reminder when
  `CLAUDE_CODE_THRIFTY_SONIC` enables it.
- `CLAUDE_CODE_TOASTY_THIMBLE` remains effective through a **computed property read**.
  `CLAUDE_CODE_GENTLE_PARASOL` shares that reader and adds a second reminder. Both emitted custom
  text after synthetic tool-result turns. A direct-read-only scan would miss these controls.
- The current settings index covers **165 canonical root names**, of which **163** retain documented
  behavior. The environment table has **366 named rows**. `modelSettings`, `feedbackDrafts`,
  `autoContinueAtUsageLimit`, and `remote` have moved into the index since the previous report.
- The exact executable exports **544 typed `CLAUDE_CODE_*` names**. **496** have a direct read
  through the imported environment object or `process.env`; **291** of those are absent from the
  checked public-source union. These are candidates for investigation, including internal protocol
  and test fields.

The behavioral findings below distinguish **verified request serialization** from **static source
tracing**. Model compliance, account rollout, and interactive UI behavior need their own tests.

## Installed artifact and source revisions

| Item                        | Observed value                                                                                                                                                               |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Homebrew inventory          | `brew list --cask --versions claude-code@latest` → `claude-code@latest 2.1.280`                                                                                              |
| Executable version          | `claude --version` → `2.1.280 (Claude Code)`                                                                                                                                 |
| Resolved executable         | `/opt/homebrew/Caskroom/claude-code@latest/2.1.280/claude`                                                                                                                   |
| Size                        | `217,254,576` bytes                                                                                                                                                          |
| SHA-256                     | `387a5c5dcdbb815085edf0baf79591f9d8894efe922bceaf3d75b1b08055229d`                                                                                                           |
| Embedded build time         | `2026-09-21T20:40:17Z`                                                                                                                                                       |
| Embedded build revision     | `80abbfe7d7232280011ff01a21ae3338f4c6e372`                                                                                                                                   |
| Public release              | [`v2.1.280`](https://github.com/anthropics/claude-code/releases/tag/v2.1.280), published `2026-09-22T16:38:14Z`                                                              |
| Public repository commit    | [`56f36532530f88b572854538d685fcf781141e8c`](https://github.com/anthropics/claude-code/commit/56f36532530f88b572854538d685fcf781141e8c)                                      |
| Docs-linked schema revision | SchemaStore [`d2cbdcde9855c1bf9ea99c336163cc6c93753e39`](https://github.com/SchemaStore/schemastore/commit/d2cbdcde9855c1bf9ea99c336163cc6c93753e39), `2026-08-03T20:54:13Z` |

The embedded build revision identifies the executable's build. The public commit pins the release
repository and changelog. The SchemaStore revision pins the separately maintained schema.

The executable's build metadata includes this exact excerpt at byte `169,930,319`:

```text
VERSION:"2.1.280",FEEDBACK_CHANNEL:"https://github.com/anthropics/claude-code/issues",BUILD_TIME:"2026-09-21T20:40:17Z",GIT_SHA:"80abbfe7d7232280011ff01a21ae3338f4c6e372"
```

### Evidence conventions

Binary offsets are **zero-based bytes in the pinned executable**. Source paths such as
`chunk-8p6r0vhj.js:11` are relative to the embedded `/$bunfs/root/` directory; line numbers refer to
the unformatted embedded source. Short excerpts and byte offsets make the claims recoverable from
this exact installed artifact.

Public-document line numbers refer to the fetched Markdown snapshots. Those URLs serve rolling
documentation, so the hashes below identify the content inspected. The version-pinned changelog
supplies release-specific introduction/removal claims.

| Public snapshot                                                                  | SHA-256                                                            |
| -------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| [`settings-reference.md`](https://code.claude.com/docs/en/settings-reference.md) | `062f275b2544f297854e22784333c067a8db5161fc62293ead80a81bdd2f57ea` |
| [`env-vars.md`](https://code.claude.com/docs/en/env-vars.md)                     | `a6ffb00019004668fdce55e33af7e26be4559a467ed1da57e3ff48c760bd3287` |
| [`settings.md`](https://code.claude.com/docs/en/settings.md)                     | `0c5162aa9850ad928c0232e4fd0930f832578602e9cfd52f6fea39ad6d5271b3` |

## Method and counting boundaries

1. Confirm the Homebrew cask, resolve the executable, and record its version, size, hash, and
   embedded build metadata.
2. Extract the NUL-terminated source regions that start with the executable's `// @bun @bytecode`
   and Claude Code source header. This produced **1,991 modules** totaling **37,884,508 source
   bytes**. All 1,991 parsed successfully as JavaScript syntax trees; the extracted code was
   analyzed without executing it.
3. Resolve the environment export map, constructor declarations, and actual imported object. In this
   build, `chunk-0hm7n25m.js` exports the parsed environment as `a`; importing modules refer to that
   binding. Exclude comments, strings, shadowed local identifiers, and write-only
   assignments/deletions from direct read counts. Module names follow matching import/export
   bindings; the indexed byte slices provide the primary artifact identity. Source-level
   reachability and source/bytecode equivalence remain separate from syntax-tree counts.
4. Follow useful controls through their consumers to the resulting prompt, tool, or setting
   behavior. Check computed access separately. In particular, `a[e.envVar]` consumes the two
   reminder names supplied by constant descriptors.
5. Compare exact names against the current official references, related official pages, the pinned
   public repository and changelog, and the docs-linked schema.
6. Capture the serialized Messages API requests from isolated CLI processes against a loopback fake
   API. Pair absence tests with positive controls. Use synthetic tool-result turns for reminders
   that require conversation progress.

The generated parser separates `bool` and `triBool`:

- `bool` returns the true-string test result, including `false` for an unset value.
- `triBool` returns `true`, `false`, or `undefined`, preserving an explicit off value.
- True strings are `1`, `true`, `yes`, `on`; false strings are `0`, `false`, `no`, `off`, matched
  after trimming and lowercasing.
- Consumer logic decides precedence. `env || fallback` permits a fallback to enable a feature;
  `env ?? fallback` preserves explicit `false`.

Evidence: `chunk-cz4cxnyf.js:11`, parser region beginning at byte `170,422,183`, contains
`if(De(n))return!0;if(Co(n))return!1;return`; the definitions of `De` and `Co` in
`chunk-bt7s3dsf.js:11`, bytes `169,961,242` and `169,961,384`, contain the exact arrays
`["1","true","yes","on"]` and `["0","false","no","off"]`.

### Environment inventory arithmetic

| Set or operation                                                | Count |
| --------------------------------------------------------------- | ----: |
| Typed exported `CLAUDE_CODE_*` names                            |   544 |
| Exported names with a traced direct property read               |   496 |
| Exported names outside that direct-read set: `544 − 496`        |    48 |
| All checked public-source `CLAUDE_CODE_*` names                 |   231 |
| Intersection: exported direct-read names also in public sources |   205 |
| Strict hidden-candidate set: `496 − 205`                        |   291 |
| Hidden computed-reader controls verified separately             |     2 |

The two computed-reader controls are `CLAUDE_CODE_TOASTY_THIMBLE` and `CLAUDE_CODE_GENTLE_PARASOL`.
They are absent from the checked public union and are included among the 48 names outside the
direct-read set. The public control `CLAUDE_CODE_DISABLE_ARTIFACT` also has a computed consumer: the
module beginning at byte `176,760,385`, line 11, combines
`envDisableVar:"CLAUDE_CODE_DISABLE_ARTIFACT"`, `N(e.envDisableVar)`, and `a[e]`. It contributes no
new public-absence candidate. Other dependency-injected or computed readers remain outside the
strict count.

Three additional direct-read names lack a corresponding typed export:
`CLAUDE_CODE_COORDINATOR_EXTRA_TOOLS`, `CLAUDE_CODE_COORDINATOR_MODE`, and
`CLAUDE_CODE_ENABLE_TELEMETRY`. They are tracked separately and excluded from the 544/496/291
calculation.

**Hidden** here means an exact control name was absent from the checked public sources and its
runtime consumption was traced. Public documentation can describe an equivalent feature under
another setting or CLI flag. The candidate total includes credentials, host identity, remote-session
plumbing, telemetry, tests, and other controls that require additional investigation before use.

The older report contains a total of 234 candidates and selected examples. The repo contains that
report; the earlier executable was outside this run's analysis. An exact release-to-release addition
count requires both complete candidate sets under the same extraction and public-source rules.

## Prompt controls verified against the installed executable

The following table reports request content captured from 2.1.280. The quoted excerpts are the
positive-control strings or code expressions that establish the behavior. Each control's detailed
source evidence follows the table.

| Control                                | Values tested                                         | Observed result                                                                                                                                                                  |
| -------------------------------------- | ----------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `CLAUDE_CODE_BISON_CAIRN`              | `0`, `1`, Opus 5                                      | `# Delivering work` absent with `0`, present with `1`.                                                                                                                           |
| `CLAUDE_CODE_LARCH_CISTERN`            | `0`, `1`, Opus 5                                      | `# Corrections` present with both values; the model bundle supplies the fallback.                                                                                                |
| `CLAUDE_CODE_AMBER_ASTROLABE`          | `0`, `1`, Fable 5                                     | `You are operating autonomously.` present with both values.                                                                                                                      |
| `CLAUDE_CODE_THRIFTY_SONIC`            | `0`, `1`, Sonnet 4.5                                  | Bash-first reminder absent with `0`, present with `1` in the bypass-permissions test branch.                                                                                     |
| `CLAUDE_CODE_COZY_TEAPOT`              | `strict`, `relaxed`, with `THRIFTY_SONIC=1`           | `strict`: “Do your work through the Bash tool wherever it can accomplish the job”; `relaxed`: “You can do much of your work through the Bash tool when it is the simpler route”. |
| `CLAUDE_CODE_ACT_DONT_REDERIVE`        | `0`, `1`, Sonnet 4.5                                  | “When you have enough information to act, act.” absent with `0`, present with `1`.                                                                                               |
| `CLAUDE_CODE_INTRO_FRAME`              | `0`, `1`, Sonnet 4.5                                  | “You are an agent working with the user toward their goals” appears only with `1`.                                                                                               |
| `CLAUDE_CODE_TOTAL_TOKENS_REMINDER`    | `off`, `fixed`, Sonnet 4.5                            | `off` removes `<total_tokens>`; `fixed` emits `<total_tokens>5000000 tokens left</total_tokens>`.                                                                                |
| `CLAUDE_CODE_THISTLE_GREBE`            | `default`, `no_nudges`, `counter_steer`, Sonnet 4.5   | `no_nudges` removes the positive delegation paragraph; `counter_steer` also adds “Delegate only when the payoff clearly exceeds that overhead.”                                  |
| `CLAUDE_CODE_TOASTY_THIMBLE`           | `0`, `1`, custom text, Fable 5                        | Custom text appears as a standalone model-visible reminder after each tested tool-result batch. Boolean-like strings suppress the custom reminder.                               |
| `CLAUDE_CODE_GENTLE_PARASOL`           | `0`, custom text, Fable 5                             | Custom text appears as a separate secondary reminder after tool results; `0` suppresses it.                                                                                      |
| `CLAUDE_CODE_SILENT_TURN_REMINDER`     | `0`, `1`, with threshold `1` and custom text, Fable 5 | Custom silent-turn reminder absent with `0`, present with `1` after the synthetic silent tool turns.                                                                             |
| `CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS` | `0`, `1`, Sonnet 4.5                                  | The Bash tool description's Git/PR instructions are present with `0` and removed with `1`; this documented control validates the capture method.                                 |

### `BISON_CAIRN`: explicit off now wins

Source: `chunk-0hm7n25m.js:11`, byte `170,456,217`:

```text
Wr=D.triBool()
```

The export map associates `CLAUDE_CODE_BISON_CAIRN` with `Wr`. In `chunk-8p6r0vhj.js:11`, the
capability call passes that value directly:

```text
function Gso(e){return CV("bison_cairn",ze(e),e,a.CLAUDE_CODE_BISON_CAIRN)}
```

`CV` returns the explicit fourth argument first: `if(l!==void 0)return l`. The final prompt assembly
in `chunk-dt8bvbsd.js:1171`, around byte `178,761,054`, also checks the environment value before the
model fallback:

```text
a.CLAUDE_CODE_BISON_CAIRN??(Fue(h)||Gso(s))?wVn:null
```

`wVn` begins with `# Delivering work`. The Opus 5 loopback pair confirms that the false value
survives through serialization.

### Force-on controls retain their fallback

Source: `chunk-8p6r0vhj.js:11`, reads at bytes `173,588,622` and `173,588,786`:

```text
CV("amber_astrolabe",ze(e),e,a.CLAUDE_CODE_AMBER_ASTROLABE||void 0)
CV("larch_cistern",ze(e),e,a.CLAUDE_CODE_LARCH_CISTERN||void 0)
```

The typed declarations are `Kr=D.bool()` and `vr=D.bool()`. The `||void 0` operation drops false
values before `CV` decides from the model/client data. The captured Fable 5 and Opus 5 requests
retain their corresponding sections at `0`.

### Bash-first selection and its new wording variant

Source: `chunk-8p6r0vhj.js:11`, read at byte `173,587,921`:

```text
if(a.CLAUDE_CODE_THRIFTY_SONIC!==void 0)return a.CLAUDE_CODE_THRIFTY_SONIC
```

`THRIFTY_SONIC` uses `triBool`. `COZY_TEAPOT` uses `nE=D.enum(["strict","relaxed"])` at byte
`170,455,092`, and the same module reads it at byte `173,588,182`:

```text
return a.CLAUDE_CODE_COZY_TEAPOT??u().bashFirstSteerVariant()
```

The default resolver checks client data, a model-bundle default, and a remote flag, then falls back
to `"strict"`. The two captured reminders confirm the selected wording when Bash-first is active.
These tests exercise the serialized `bypassPermissions` branch. Live authenticated Auto Mode
behavior remains unverified.

### Computed reminder readers

Source: `chunk-9yybzjm7.js:33`, reader at byte `184,555,307`, parser at `184,555,098`, descriptors
at `184,555,956` and `184,556,133`, tool-result predicate at `184,558,431`.

The typed map still exports `TOASTY_THIMBLE` and `GENTLE_PARASOL`. Their consumer uses a descriptor
to select the property name:

```text
function lh(e,n){let r=a[e.envVar];if(r!==void 0)return ah(r);
```

The two descriptors supply `envVar:"CLAUDE_CODE_TOASTY_THIMBLE"` and
`envVar:"CLAUDE_CODE_GENTLE_PARASOL"`. The parser's decisive expression is:

```text
return Co(n)||De(n)?null:nl(n,"env",void 0)
```

The consumer requires a supported mid-conversation-system-message model and an eligible user message
containing `tool_result`. It inserts `batching_reminder` and `secondary_reminder` attachments, then
serializes them as model-visible text. Custom text is latched by conversation and model.

In the Fable 5 tests, each custom marker was absent as a standalone text line in request one and
present in requests two and three, after reading a synthetic file. Exact-line assertions exclude the
fixture directory names, which also contain the marker strings.

These names, together with the three `SILENT_TURN_REMINDER*` names, are excluded from project/local
`env` blocks. The filter in `chunk-tpcgcc17.js:11`, byte `174,053,630`, contains the exact
diagnostic “project-scoped settings can't set this key” and removes matching properties. Use only an
intentionally selected trusted configuration scope for these controls.

### Remaining verified control paths

| Control                        | Embedded source and byte offset         | Short exact source evidence                                                                            |
| ------------------------------ | --------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Act on established information | `chunk-dt8bvbsd.js:1162`, `178,756,293` | `a.CLAUDE_CODE_ACT_DONT_REDERIVE,n=e??x("tengu_cedar_lantern",!0)`                                     |
| Intro frame                    | `chunk-dt8bvbsd.js:1131`, `178,740,200` | `a.CLAUDE_CODE_INTRO_FRAME,n=e??x("tengu_ochre_wren",!1)`                                              |
| Token reminder mode            | `chunk-dt8bvbsd.js:1070`, `178,720,585` | `a.CLAUDE_CODE_TOTAL_TOKENS_REMINDER;if(AJ(e))return e;let n=Ye().totalTokensReminder`                 |
| Token constants                | `chunk-dt8bvbsd.js:1070`, `178,719,083` | `x2n=5000000,ymt=15000000`                                                                             |
| Delegation guidance            | `chunk-m200zvyg.js:11`, `172,589,666`   | `si(a.CLAUDE_CODE_THISTLE_GREBE);if(n)return{steer:n,source:"env"}`                                    |
| Silent-turn enablement         | `chunk-dt8bvbsd.js:3359`, `180,393,279` | `CV("silent_turn_reminder",n,e,a.CLAUDE_CODE_SILENT_TURN_REMINDER)`                                    |
| Silent-turn custom text        | `chunk-dt8bvbsd.js:3359`, `180,393,065` | `a.CLAUDE_CODE_SILENT_TURN_REMINDER_TEXT;if(e!==void 0)return e`                                       |
| Silent-turn threshold          | `chunk-dt8bvbsd.js:3359`, `180,393,429` | `a.CLAUDE_CODE_SILENT_TURN_REMINDER_TURNS;if(e!==void 0)return e`                                      |
| Git instruction inclusion      | `chunk-dt8bvbsd.js:501`, `177,932,566`  | `a.CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS;if(e!==void 0)return!e;return Ye().includeGitInstructions??!0` |

The `si` validator accepts exactly `default`, `no_nudges`, and `counter_steer`. Token mode
validation accepts `off`, `infinite`, `fixed`, `countdown`, and `padded-countdown`. Token budget and
after-user-turn controls also have direct settings readers; their respective environment values take
precedence. The default budget is 15,000,000 and the fixed display is 5,000,000. These numbers are
reminder text parameters and should be interpreted within that feature's semantics.

### Other static differences worth retaining

- **Cross-session messaging gate:** `CLAUDE_CODE_HARBOR_KITE` is now a string override checked
  before its remote default. `Ts()` in the module starting at byte `174,093,297`, line 11, reads at
  `174,094,120`: `let e=a.CLAUDE_CODE_HARBOR_KITE;if(e!==void 0)return De(e)`. This local gate
  accepts an explicit off value. Additional platform, policy, transport, and session predicates
  still govern actual messaging; they were not exercised here.
- **Prompt-bundle selection:** `CLAUDE_CODE_BREEZY_HORIZON` feeds `modelForPrompt`.
  `chunk-8p6r0vhj.js:11`, byte `173,590,478`, begins
  `let n=a.CLAUDE_CODE_BREEZY_HORIZON;if(Co(n))return e` and validates a supplied model identifier
  with `ie(s)`. This is a static finding about prompt selection; request routing under this override
  was not tested.
- **Writing guidance:** `CLAUDE_CODE_WILLOW_TERN` has a force-on branch,
  `if(a.CLAUDE_CODE_WILLOW_TERN)return!0`, at byte `173,589,086` in `chunk-8p6r0vhj.js:11`. The
  downstream `sVn` function selects the writing-guidance text through `Vso(e)`. Its false value
  permits the model/client-data fallback. This path was traced statically.
- **Historical spellings:** `CLAUDE_CODE_GAULT_KESTREL` and `CLAUDE_CODE_WALNUT_SPIRE` occur zero
  times in the 1,991 extracted source modules. Their old configuration names therefore have no
  traced control path in this build. Feature removal is a separate question. The current
  plugin-evaluation help explicitly contains `generally available; no enablement setting` in the
  module starting at byte `177,240,054`, line 11.

## Public settings and environment changes

### Current inventory

The [settings index](https://code.claude.com/docs/en/settings-reference.md), snapshot lines
**592–822**, has 231 rows. Its normalization is:

```text
231 rows − 7 global-config rows − 60 dotted-key rows + 1 nested-only root = 165 roots
165 roots − keybindingFlavor − taskOutputMaxChars = 163 currently behavioral roots
```

The nested-only root is `remote`, represented by `remote.defaultEnvironmentId`. The environment
Variables table, snapshot lines **136–501**, has 366 rows:
`339 + 28 additions − 1 removed row = 366`. This counts exported context and explicitly obsolete
entries as well as user inputs.

The 20 additions to the root index since the previous report are:

```text
autoContinueAtUsageLimit, bashEditDiffEnabled, bashOutputMaxChars,
desktopSessionCleanupPeriodDays, disableDesktopLocalSessions, feedbackDrafts,
gatewayInternalNetworks, managedMcpServers, managedSourcesBehavior, maxEffortLevel,
modelPicker, modelPricing, modelSettings, promptCacheTtl, remote,
subagentPromptCacheTtl, syncClaudeAiPlugins, taskOutputMaxChars, timeFormat, timeZone
```

These are documentation-index additions. Four already had local-binary entries in the previous
report: `autoContinueAtUsageLimit`, `feedbackDrafts`, `modelSettings`, and `remote`.
`taskOutputMaxChars` is now indexed as removed.

### Practical public controls and qualifications

| Topic                       | Current documented behavior                                                                                                                                                                 | Exact public-source excerpt                                                                                                                                                        |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Per-model effort            | `modelSettings` records per-model effort and accepts per-model `maxEffortLevel`.                                                                                                            | `settings-reference.md:1171–1187`: “for each model you use.”; “The field requires Claude Code v2.1.267 or later.”                                                                  |
| Bash diffs                  | `bashEditDiffEnabled=true` from a trusted source enables change recording in every permission mode. The environment counterpart takes precedence.                                           | `settings-reference.md:2959–2970`: “Set the key to `true` to record them in every permission mode”; “A `true` counts only from your user settings, JSON passed with `--settings`”. |
| Bash output                 | `bashOutputMaxChars` defaults to 30,000, clamps to 4,000–128,000, and supersedes `BASH_MAX_OUTPUT_LENGTH` when set.                                                                         | `settings-reference.md:2713–2727`: “clamps the value into the range `4000` to `128000`”; “up to 30,000 characters”.                                                                |
| Prompt caching              | `promptCacheTtl` and `subagentPromptCacheTtl` provide `5m`/`1h` controls for the main conversation and separate agent/helper requests.                                                      | `settings-reference.md:1221–1238, 1261–1280`: “This key applies to your interactive, `-p`, and Agent SDK turns”; “outside the main conversation”.                                  |
| Subagent models             | `CLAUDE_CODE_SUBAGENT_MODEL` supplies a default below explicit per-agent choices. `CLAUDE_CODE_SUBAGENT_MODEL_FORCE=1` restores forcing, subject to the documented fork/inherit exceptions. | `sub-agents.md:351–412`: “Before v2.1.251, `CLAUDE_CODE_SUBAGENT_MODEL` came first in this order”; the same section documents `CLAUDE_CODE_SUBAGENT_MODEL_FORCE`.                  |
| Project instruction files   | AGENTS.md loading is configured under `pluginConfigs["agents-md@builtin"].options.instructionFiles`.                                                                                        | `memory.md:333–399`: `claude-md-or-agents-md`, `claude-md-and-agents-md`, `claude-md`, `managed-only`. This is a nested plugin option and is rollout-dependent.                    |
| Removed output control      | `taskOutputMaxChars` and `TASK_MAX_OUTPUT_LENGTH` have no effect after TaskOutput's removal in v2.1.277.                                                                                    | `settings-reference.md:2877–2883`: “Removed in v2.1.277”; pinned `CHANGELOG.md:195`: removal of `TaskOutput`.                                                                      |
| Removed keyboard preference | `keybindingFlavor` is retained in the index as a no-op.                                                                                                                                     | `settings-reference.md:3156–3166`: “Deprecated since v2.1.261 and has no effect”.                                                                                                  |

Source URLs: [settings reference](https://code.claude.com/docs/en/settings-reference.md),
[subagents](https://code.claude.com/docs/en/sub-agents.md),
[memory](https://code.claude.com/docs/en/memory.md), and
[pinned changelog](https://github.com/anthropics/claude-code/blob/56f36532530f88b572854538d685fcf781141e8c/CHANGELOG.md).

The tools reference still calls TaskOutput “Deprecated in favor of `Read`”
(`tools-reference.md:57`). The version-pinned changelog and dedicated settings/env entries agree on
removal. The synthetic requests also expose `Read` and omit TaskOutput on the three tested model
configurations. Cached documentation excerpts still describing active `taskOutputMaxChars` should be
interpreted in that context.

### Control specifically announced in 2.1.280

`CLAUDE_CODE_MAX_MCP_DESCRIPTION_LENGTH` is public in
[pinned `CHANGELOG.md:3–7`](https://github.com/anthropics/claude-code/blob/56f36532530f88b572854538d685fcf781141e8c/CHANGELOG.md#L3-L7):
“change the 2,048-character cap on MCP tool descriptions and server instructions”. It is absent from
the fetched environment table, so its current evidence category is release history plus shipped
implementation.

The artifact declares `rS=D.int({min:1,digitsOnly:!0})` at byte `170,479,458`.
`chunk-dt8bvbsd.js:1514`, byte `178,849,392`, contains:

```text
function yz(){return a.CLAUDE_CODE_MAX_MCP_DESCRIPTION_LENGTH??kVe}
```

The same module defines `kVe=2048` at byte `177,623,803`, line 179. Both MCP runtime implementations
import this resolver; their truncation functions return the original string when `e.length<=s` and
otherwise truncate to `s` with a marker. Those functions begin near bytes `202,733,327` and
`202,962,574`. Runtime MCP-server integration was not exercised.

### Environment-table differences

The 28 added table names are:

```text
BETA_TRACING_ENDPOINT
CLAUDE_CODE_AUTO_BACKGROUND_WORKER_CHECKIN_SECONDS
CLAUDE_CODE_AUTO_MODE_SERVER
CLAUDE_CODE_BASH_EDIT_DIFF
CLAUDE_CODE_BG_TASKS_REPORT_RUNNING
CLAUDE_CODE_DISABLE_BEDROCK_CONTENT_TYPE_DEFAULT
CLAUDE_CODE_DISABLE_CFC_PROMPT
CLAUDE_CODE_DISABLE_WINDOWS_SHELL_LAUNCHER
CLAUDE_CODE_GATEWAY_HINT_HEADERS
CLAUDE_CODE_GATEWAY_MODEL_DISCOVERY_TIMEOUT_MS
CLAUDE_CODE_MCP_STARTUP_WAIT_MS
CLAUDE_CODE_NONBLOCKING_STDOUT
CLAUDE_CODE_PROMPT_CACHE_TTL
CLAUDE_CODE_RESTRICTED
CLAUDE_CODE_SEND_FEEDBACK
CLAUDE_CODE_STARTUP_FAILURE_RESULTS
CLAUDE_CODE_SUBAGENT_MODEL_FORCE
CLAUDE_CODE_SUBAGENT_PROMPT_CACHE_TTL
CLAUDE_CODE_TOOL_MEMORY_CGROUP_EXCLUDE
CLAUDE_CODE_WEBFETCH_DEADLINE_MS
CLAUDE_CODE_WORKFLOW_MAX_CONCURRENT_AGENTS
CLAUDE_JOB_DIR
CLAUDE_STREAM_FIRST_BYTE_TIMEOUT_MS
ENABLE_BETA_TRACING_DETAILED
OTEL_LOG_MANAGED_SETTINGS
OTEL_METRICS_INCLUDE_REPOSITORY
VERTEX_REGION_CLAUDE_5_5_OPUS
VERTEX_REGION_CLAUDE_FABLE_5_1
```

The removed table row is `CLAUDE_CODE_ENABLE_APPEND_SUBAGENT_PROMPT`. The current CLI reference
still documents `--append-subagent-system-prompt` and its file variant (`cli-reference.md:66–67`),
and the binary retains a direct environment read. The table change alone establishes a documentation
difference. Some added rows, including the beta-tracing names and `CLAUDE_JOB_DIR`, were already
covered on specialized pages in the earlier report.

### Published JSON schema reconciliation

The guide warns: “The schema can lag behind the newest CLI releases”
([`settings.md:554`](https://code.claude.com/docs/en/settings.md)). The linked SchemaStore artifact
remains at the same August 3 revision as the earlier report, with **142 root properties** and
`additionalProperties: true`.

Its intersection with the 165-name current index is **132**: `165 − 132 = 33` indexed names absent
from the schema, and `142 − 132 = 10` schema names absent from the index. The latter comprise
`$schema`, six global-config names, `requireCoworkFullVmSandbox`, `skippedMarketplaces`, and
`skippedPlugins`.

A concrete mismatch is `managedMcpServers`: the current reference describes an “object keyed by
server name” (`settings-reference.md:4782–4801`); the older schema still declares `"type": "array"`
(`claude-code-settings.json:3882–3888`). Use the current reference and shipped consumer when
resolving a mismatch like this.

## Root settings declared by the executable

The root constructor is `yot`, with the default factory `kS=p(()=>yot(mot()))`. Its tail is
`...Ar(e)}).passthrough()` in `chunk-cd3sgqch.js:14`, bytes `170,901,877–170,901,901`. Thus the
following counts describe **explicit schema declarations**. Unknown properties can survive parsing;
a successfully parsed key still needs a functioning consumer.

| Schema component or set operation                             | Count |
| ------------------------------------------------------------- | ----: |
| Direct, unconditional root properties                         |   177 |
| Properties added by the five enabled feature-registry entries |     7 |
| Unconditional declarations: `177 + 7`                         |   184 |
| Conditional `xaaIdp` declaration                              |     1 |
| Total potentially declared names                              |   185 |
| Intersection with the 165-name public index                   |   160 |
| Editor annotation and marketplace aliases                     |     3 |
| Unindexed declarations: `185 − 160 − 3`                       |    22 |

The seven registry additions are `skipAutoPermissionPrompt`, `useAutoModeDuringPlan`, `autoMode`,
`disableDeepLinkRegistration`, `voiceEnabled`, `defaultView`, and `axScreenReader`. The registry
selects `Su=["autoMode","deepLink","voice","briefView","screenReader"]`, then merges `zt[s].shape()`
for entries whose `buildGate()` passes; all five are compiled true. Evidence:
`chunk-cd3sgqch.js:14`, bytes `170,816,202–170,818,767`.

`xaaIdp` is declared through `...a.CLAUDE_CODE_ENABLE_XAA&&{xaaIdp:...}` at byte `170,844,736`. With
that gate unset, **21** unindexed fields are unconditional. The other five public-index roots are
absent from this constructor's explicit shape: `browserExternalPageTools`,
`disableBrowserExternalNavigation`, `disableDesktopLocalSessions`, `disableMobileSimulatorTools`,
and `sshHostAllowlist`. Their host/platform consumers were not audited here.

### All 22 unindexed declarations

“Unindexed” refers to the canonical settings index. The status column describes what this source
analysis established. UI, cloud, authentication, and consent flows in this table were not exercised.

All declaration byte offsets below refer to `chunk-cd3sgqch.js:14`. Short quoted fragments are exact
excerpts from the corresponding declaration. Consumer qualifications are detailed immediately below.

| Key                                | Shape / declared purpose                                                  | Evidence and current qualification                                                                                                                                                |
| ---------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `autoDreamEnabled`                 | Boolean; background memory consolidation.                                 | `170,894,367`: “Enable background memory consolidation (auto-dream).” A setting reader exists behind availability and auto-memory gates.                                          |
| `precomputeCompactionEnabled`      | Boolean; precompute a compaction summary.                                 | `170,898,292`: “Only applies when auto-compact is on.” The consumer additionally requires its rollout and applicable session gates.                                               |
| `totalTokensReminder`              | `off`, `infinite`, `fixed`, `countdown`, `padded-countdown`.              | `170,892,371`: `V(["off","infinite","fixed","countdown","padded-countdown"])`. Shared prompt serialization was tested through the environment counterpart.                        |
| `totalTokensReminderBudget`        | Positive integer; padded-countdown budget.                                | `170,893,081`: “Defaults to 15000000.” The direct consumer gives a valid environment override precedence.                                                                         |
| `totalTokensReminderAfterUserTurn` | Boolean; re-emit/re-anchor on user turns.                                 | `170,893,351`: “Defaults to on.” Its direct reader checks environment, settings, then remote values.                                                                              |
| `modelProposedGoals`               | `auto`, `alwaysAsk`, `disabled`.                                          | `170,887,971`: “Controls the ProposeGoal tool”; trusted-source readers and approval/tool-availability consumers were traced.                                                      |
| `showMessageTimestamps`            | Boolean.                                                                  | `170,899,314`: “Stamp each message with its arrival time”. Its renderer also checks a rollout.                                                                                    |
| `defaultView`                      | `chat`, `transcript`.                                                     | `170,818,225`: “Default transcript view”. The chat startup path checks brief-mode entitlement.                                                                                    |
| `prependPlugins`                   | Array of plugin IDs.                                                      | `170,869,647`: “whose hooks run first, outermost”. Ordering and trusted-scope behavior are schema-described; complete runtime ordering was not tested.                            |
| `appendPlugins`                    | Array of plugin IDs.                                                      | `170,870,601`: “whose hooks run last among plugins, innermost”. Same verification limit as `prependPlugins`.                                                                      |
| `remoteControl`                    | Object with `shareHostProfile: off/basic/full`.                           | `170,899,822`: “Remote Control”. The collector is rollout-gated; its `full` branch includes configured MCP-server names.                                                          |
| `remoteTools`                      | Object with `allowUnattendedServing`.                                     | `170,895,119`: “How this computer serves tool calls to cloud sessions”. False in managed/user settings constrains unattended serving; consent remains separately stored.          |
| `skipWorkflowUsageWarning`         | Boolean consent record.                                                   | `170,894,848`: “Whether the user has accepted the multi-agent workflow usage warning.” Reader and acceptance persistence were traced.                                             |
| `daemonColdStart`                  | `transient`, `ask`.                                                       | `170,900,759`: “When no background service is running”. A direct resolver and missing-daemon branch exist.                                                                        |
| `proxyAuthHelper`                  | Shell-command string.                                                     | `170,842,232`: “outputs a Proxy-Authorization header value”. An additional enable gate applies; this executes a credential-bearing helper.                                        |
| `policyHelpers`                    | Per-platform helper/static-policy map.                                    | `170,843,381`: “Per-OS variant of policyHelper”. Platform selection and admin-source checks were traced; the complete helper lifecycle remains unverified.                        |
| `xaaIdp`                           | Identity-provider object.                                                 | `170,844,736`: `CLAUDE_CODE_ENABLE_XAA&&{xaaIdp:`. Conditional schema membership; authentication flow untested.                                                                   |
| `breakReminder`                    | Object: `enabled`, `intervalMinutes`, `breakThresholdMinutes`, `message`. | `170,845,478`: “Show a friendly nudge after sustained continuous use”. A partial active-time ledger reader was found; a working reminder UI remains unverified.                   |
| `quietHours`                       | Object: `enabled`, `start`, `end`.                                        | `170,846,240`: “Opt-in quiet hours.” Only the schema declaration was found for the exact key; runtime reminder behavior remains unverified.                                       |
| `doneMeansMerged`                  | Boolean; schema-described completion policy.                              | `170,892,161`: “Claude keeps working until the PR is ready for you to merge”. Only schema and config-category references were found for the exact key.                            |
| `todoFeatureEnabled`               | Boolean; schema-described task panel.                                     | `170,899,513`: “Enable the todo / task tracking panel”. References found were compatibility/preference and config-snapshot code.                                                  |
| `autoUploadSessions`               | Boolean; schema-described session mirroring.                              | `170,901,548`: “Mirror local sessions to claude.ai as view-only”. Compatibility and config-snapshot references were found; upload activation through this key remains unverified. |

All 21 previously listed unindexed keys remain declared, with XAA conditional. Four are now publicly
indexed; the five additional unindexed declarations are `prependPlugins`, `appendPlugins`,
`remoteControl`, `remoteTools`, and `defaultView`: `21 − 4 + 5 = 22`. This compares inventories,
without assigning an introduction version to those five fields.

### Gates that materially affect interpretation

**Precomputed compaction:** `chunk-dt8bvbsd.js:1816`, bytes `179,209,203–179,209,344`, contains:

```text
function jN(){if(!am())return!1;if(!wU())return!1;if(!x("tengu_sepia_moth",!1))return!1;return yo("precomputeCompactionEnabled",TYe()).value}
```

`am()` checks auto-compaction. `wU()` passes local sessions and checks the relevant remote-session
state when `CLAUDE_CODE_REMOTE` is set. `TYe()` returns false. Consequently, setting the key true
still requires the rollout and downstream compaction thresholds.

**Auto-dream:** the module beginning at byte `183,772,572`, line 11, has these functions at bytes
`183,776,012` and `183,776,080`:

```text
function blt(){let e=tn();return e?.enabled===!0||e?.available===!0}
function aMe(){if(!blt())return!1;let e=Ye().autoDreamEnabled;if(e!==void 0)return e;return tn()?.enabled===!0}
```

An explicit value is consulted after availability. The runner separately checks auto-memory
eligibility. A true setting therefore establishes intent within an available feature.

**Retained declarations with limited consumer evidence:** the break ledger uses
`(r.breakThresholdMinutes??xMt)*60000`, with `xMt=10`, at byte `192,635,795`. Its
`ensureFlushTimer(){if(this.flushTimer)return}` implementation is at `192,636,756`, in the module
beginning at `192,633,067`, line 11. This establishes that threshold arithmetic while leaving the
actual reminder UI unverified. `quietHours` had no non-schema exact-name consumer in the scanned
modules. `doneMeansMerged` additionally appears in an `Internal:[...]` config-category list at byte
`204,557,994`, module beginning at `204,541,927`, line 12. The earlier completion/reminder
descriptions remain unverified for 2.1.280.

**Remote consent and host details:** the `remoteTools` reader at byte `189,163,758` (module
beginning at `189,159,634`, line 12) tests `n?.remoteTools?.allowUnattendedServing===!1` in
managed/user sources. The host-profile collector at byte `192,340,195` (module beginning at
`192,311,777`, line 42) exits when the rollout is `"off"`, then applies the most restrictive
configured level and passes `includeMcpServers:s==="full"` to collection. These controls affect
consent and machine information; their declaration alone is insufficient grounds to enable them in a
live profile.

## Runtime test scope

- **28 parameterized cases:** 21 initial-request cases and 7 multi-turn cases.
- **42 captured Messages requests:** `21 + 7 × 3`.
- **14 synthetic Read calls:** two fixed fixture reads per multi-turn case.
- **38 content assertions passed**, including positive and negative controls.
- A separate initial smoke case confirmed the fake API connection before the suite.

The fake server listened on `127.0.0.1`, returned fixed responses, and supplied a dummy non-secret
API-key value solely for the local protocol fixture. Child processes received an explicit
environment allowlist, separate temporary HOME/config/cache/working directories, empty settings
sources, and an empty strict MCP configuration. The macOS sandbox denied public network access and
access to `/Users/charles`; it also denied the `security` executable and keychain service lookups.
The outbound HTTPS probe failed while loopback requests succeeded. This profile permits other
filesystem paths and localhost ports; its validation covers the fixed fixture scenarios. The second
Read in each multi-turn case used the unchanged-file optimization, so the count represents tool
calls.

The suite kept `CLAUDE_CODE_SIMPLE_SYSTEM_PROMPT=0` so the tested sections were on the full-prompt
path. Feature-flag fetching was disabled; compiled model defaults still participated in the tests.
It used model identifiers `claude-sonnet-4-5`, `claude-opus-5[1m]`, and `claude-fable-5[1m]` for
reproducible prompt-bundle coverage. These synthetic requests establish local serialization,
including model-dependent fallbacks. Model quality, real billing, provider acceptance, interactive
UI behavior, and account-specific remote rollout remain unverified.

## Source coverage and reproducibility

The public comparison scanned **56 complete official Markdown pages** plus all **1,422 UTF-8 text
files** in the version-pinned public repository archive. The archive's 1,423 regular files were
checked against the pinned tree; the remaining file is a binary image. The comparison also includes
the docs-linked SchemaStore artifact. Current official pages contribute 211 `CLAUDE_CODE_*` names,
pinned repository text contributes 128, and their union is 229. The schema adds two names, yielding
the 231-name comparison baseline. A public name in this union can be a historical mention, an
example, or an internal protocol field.

The selected pages cover settings, environment variables, CLI flags, memory, context, tools,
agents/teams/workflows, plugins, hooks, permissions, sandboxing, models/providers, prompt caching,
telemetry, sessions, remote/self-hosted execution, and Agent SDK prompt/subagent/task interfaces.
Unfetched documentation and untraced dynamic property flows remain outside the public-absence claim.

To inspect an exact quoted binary span without launching Claude Code:

```python
from hashlib import sha256
from pathlib import Path

binary = Path("/opt/homebrew/Caskroom/claude-code@latest/2.1.280/claude")
data = binary.read_bytes()
assert sha256(data).hexdigest() == (
    "387a5c5dcdbb815085edf0baf79591f9d8894efe922bceaf3d75b1b08055229d"
)
print(data[179209203:179209344].decode("utf-8"))
```

This example prints the precomputed-compaction gate quoted above. Use the pinned byte spans for
other excerpts. For a fresh version, locate its own source headers, resolve its own imports/exports,
regenerate the public-name comparison, and repeat the positive/negative loopback tests.

Independent verification reproduced the strict environment counts and all 38 request-content
assertions. A separate check matched all 185 complete root-field expressions and 423 supporting
definition/consumer excerpts to the executable's bytes, and independently recomputed the public
settings and environment tables.

## Practical conclusions

- Re-check each internal control on the exact installed build. `BISON_CAIRN` and the computed
  reminder readers demonstrate why parser shape and consumer flow both matter.
- Use the current documented settings contract when it covers the intended behavior.
  `modelSettings`, separate cache lifetimes, and Bash-diff/output controls now have public
  references.
- Treat `precomputeCompactionEnabled` and `autoDreamEnabled` as gated features. A true setting still
  needs availability and the applicable session conditions.
- Keep schema-only and compatibility-only entries explicitly unverified. In this snapshot,
  `quietHours`, `doneMeansMerged`, the task-panel interpretation of `todoFeatureEnabled`, and upload
  activation through `autoUploadSessions` lack a demonstrated active consumer.
- Select privacy, remote execution, consent, and executable-helper changes only with explicit intent
  and separate end-to-end verification.
