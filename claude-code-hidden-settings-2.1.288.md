# Claude Code hidden settings analysis — v2.1.288

**Snapshot:** 2026-10-04 local date. **Target:** the installed Homebrew
`claude-code@latest` executable, Claude Code **2.1.288**. Public sources were fetched on
2026-10-03 UTC. The reusable procedure now lives in
[Claude Code reverse-analysis method](claude-code-reverse-method.md).
The [2.1.280 report](claude-code-hidden-settings-2.1.280.md) supplies the previous snapshot.

## Findings

- The typed environment has **588 names**. **537** meet the binding-resolved direct-read
  criterion; **320** of those are absent from the checked public corpus. These are investigation
  candidates with varied purposes and risk.
- The binary declares **190 potential root settings**, including **20 outside the current
  172-root public index**. `appendPlugins` and `prependPlugins` are now publicly indexed.
- `precomputeCompactionEnabled` now has a **true fallback** after its prerequisite gates.
  The old false-fallback conclusion belongs to the 2.1.280 snapshot. This is a static finding;
  live compaction was untested.
- `CLAUDE_CODE_BISON_CAIRN=0` still removes `# Delivering work` on the tested Opus 5
  prompt bundle. `LARCH_CISTERN=0` and `AMBER_ASTROLABE=0` retain their model-dependent
  fallback sections.
- `TOASTY_THIMBLE` and `GENTLE_PARASOL` still inject custom reminders through computed
  environment reads after eligible tool results. Their boolean-like values suppress custom text.
- `totalTokensReminder` works through JSON settings as well as the environment control.
  A valid `CLAUDE_CODE_TOTAL_TOKENS_REMINDER=off` overrides JSON `"fixed"`.
- The final isolated loopback suite covers **32 cases, 46 Messages requests, and 59 passing
  content assertions**, including **14 successful synthetic Read tool results**.

These runtime findings establish local request serialization. Live provider acceptance, model
compliance, account-specific rollouts, and interactive UI behavior remain outside this test scope.

## Installed artifact

| Item                        | Observed value                                                                                                                                       |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Homebrew inventory          | `brew list --cask --versions claude-code@latest` → `claude-code@latest 2.1.288`                                                                      |
| Executable version          | `claude --version` → `2.1.288 (Claude Code)`                                                                                                         |
| Resolved executable         | `/opt/homebrew/Caskroom/claude-code@latest/2.1.288/claude`                                                                                           |
| Size                        | `229,255,312` bytes                                                                                                                                  |
| SHA-256                     | `bbe93063f7a0879a1021b2891e5c9354e5b3b98433e32efe6750f7710afed750`                                                                                   |
| Embedded build time         | `2026-10-02T16:42:03Z`                                                                                                                               |
| Embedded build revision     | `17fe1eb736e5b1433d6ca86a1db334cec8520450`                                                                                                           |
| Public release              | [`v2.1.288`](https://github.com/anthropics/claude-code/releases/tag/v2.1.288), published `2026-10-02T20:19:57Z`                                      |
| Public repository commit    | [`1c229fcd1e1e4e452e29a8f116b45fe4cfe2c528`](https://github.com/anthropics/claude-code/commit/1c229fcd1e1e4e452e29a8f116b45fe4cfe2c528)              |
| Docs-linked schema revision | SchemaStore [`d2cbdcde9855c1bf9ea99c336163cc6c93753e39`](https://github.com/SchemaStore/schemastore/commit/d2cbdcde9855c1bf9ea99c336163cc6c93753e39) |
| Extracted source            | `2,183` NUL-terminated modules, `41,567,397` bytes                                                                                                   |

The build metadata at byte `178,864,829` contains:

```text
VERSION:"2.1.288",FEEDBACK_CHANNEL:"https://github.com/anthropics/claude-code/issues",BUILD_TIME:"2026-10-02T16:42:03Z",GIT_SHA:"17fe1eb736e5b1433d6ca86a1db334cec8520450"
```

### Evidence conventions

All binary offsets are zero-based bytes in this exact executable. Extracted paths such as
`modules/module-179193283.js:11` identify the source region beginning at byte `179193283`;
line numbers refer to its unformatted source. The module-start number and a cited excerpt's
exact offset are separate coordinates. The extraction procedure in the method document recreates
these paths without relying on unstable chunk names.

Public-document lines refer to the fetched, unformatted Markdown snapshots. Rolling documentation
is pinned by content hash, while release history uses an immutable public repository commit.
The executable build revision and the public repository revision identify separate artifacts.

## Environment inventory and counting boundaries

All **2,183** extracted source modules parsed successfully with TypeScript **6.0.3** in JavaScript
syntax mode. Source was analyzed without executing extracted modules. The analyzer uses lexical
symbols to distinguish imported environment bindings from shadowed local identifiers, and excludes
comments, strings, write-only assignments, and deletions. Literal-key reads, read/write compound
operations, and direct destructuring from the proven environment object are recorded; unresolved
computed keys are handled separately.

The generated environment export map is in `modules/module-179193283.js:11`:

| Exact binary byte | Short exact excerpt                                       | Meaning                                                   |
| ----------------- | --------------------------------------------------------- | --------------------------------------------------------- |
| `179,258,927`     | `var _x={...B,...d,...m,...b,...F,...H,...u,...G}`        | Eight declaration maps compose the typed environment.     |
| `179,259,310`     | `var a=Ntt(_x,p)`                                         | The constructed object is exported as `a`.                |
| `179,259,097`     | `let C=process.env[r];if(C!==n)e=s.parse(C),n=C;return e` | Lazy getters read and parse the actual environment value. |

Consumers import that object from `/$bunfs/root/chunk-w397p0p5.js`. The combined imported-export
signature resolves uniquely to this source module. The parser import
`/$bunfs/root/chunk-mx3ntmmr.js` resolves to `modules/module-179191874.js`, whose line 11 contains
`bool:()=>s,triBool:()=>l` at byte `179,192,928` and
`if(Le(n))return!0;if(_s(n))return!1;return` at byte `179,192,773`.
Parser shape and downstream gates both determine whether an explicit off value survives.
The actual true/false recognizers are in `modules/module-178927304.js:11`, bytes
`178,927,919` and `178,928,061`: they lowercase and trim before checking
`["1","true","yes","on"]` and `["0","false","no","off"]`, respectively.
`M.bool()` applies only the true-string test; `M.triBool()` retains a separate undefined state.

| Set or operation                                            | Count |
| ----------------------------------------------------------- | ----: |
| Typed exported `CLAUDE_CODE_*` names                        |   588 |
| Typed exported names with a binding-resolved direct read    |   537 |
| Typed exports outside the direct-read set: `588 − 537`      |    51 |
| Exact names in the checked public-source union              |   249 |
| Typed direct-read names also in that union                  |   217 |
| Strict public-absence candidate set: `537 − 217`            |   320 |
| Verified hidden computed-reader controls tracked separately |     2 |

The two computed controls are `CLAUDE_CODE_TOASTY_THIMBLE` and `CLAUDE_CODE_GENTLE_PARASOL`.
They are included in the 51 names outside the direct-read set. Additional dynamic consumers remain
outside the strict count; this is a defined scan boundary, and it makes no dead-code claim about
those 51 names.

Three additional direct-read names have no typed export and are excluded from the above arithmetic:
`CLAUDE_CODE_COORDINATOR_EXTRA_TOOLS`, `CLAUDE_CODE_COORDINATOR_MODE`, and
`CLAUDE_CODE_ENABLE_TELEMETRY`.

**Hidden candidate** means a traced name is absent from the checked public corpus. The set includes
host/session protocol fields, telemetry, credentials, tests, and internal plumbing. Each useful
control still needs a complete consumer trace. The previous report's **291** and this report's
**320** use different binary snapshots and differently sized public corpora, so their difference
provides no exact count of newly introduced hidden controls.

## Prompt controls verified against the executable

The test matrix uses CLI model identifiers `claude-sonnet-4-5`, `claude-opus-5[1m]`, and
`claude-fable-5[1m]`. The serialized request normalizes the latter two to their base model IDs.
All cases retain the full prompt with `CLAUDE_CODE_SIMPLE_SYSTEM_PROMPT=0`.

| Control                                | Cases                                                 | Observed request content                                                                                                                           |
| -------------------------------------- | ----------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `CLAUDE_CODE_BISON_CAIRN`              | `0`, `1`; Opus 5                                      | `# Delivering work` absent at `0`, present at `1`.                                                                                                 |
| `CLAUDE_CODE_LARCH_CISTERN`            | `0`, `1`; Opus 5                                      | `# Corrections` present at both values.                                                                                                            |
| `CLAUDE_CODE_AMBER_ASTROLABE`          | `0`, `1`; Fable 5                                     | `You are operating autonomously.` present at both values.                                                                                          |
| `CLAUDE_CODE_THRIFTY_SONIC`            | `0`, `1`; Sonnet 4.5                                  | Strict Bash-first reminder absent at `0`, present at `1`, in the bypass-permissions branch.                                                        |
| `CLAUDE_CODE_COZY_TEAPOT`              | `strict`, `relaxed`, with `THRIFTY_SONIC=1`           | Selects the corresponding wording; each case excludes the other variant.                                                                           |
| `CLAUDE_CODE_ACT_DONT_REDERIVE`        | `0`, `1`; Sonnet 4.5                                  | `When you have enough information to act, act.` appears only at `1`.                                                                               |
| `CLAUDE_CODE_INTRO_FRAME`              | `0`, `1`; Sonnet 4.5                                  | `You are an agent working with the user toward their goals` appears only at `1`.                                                                   |
| `CLAUDE_CODE_TOTAL_TOKENS_REMINDER`    | `off`, `fixed`; Sonnet 4.5                            | `off` removes `<total_tokens>`; `fixed` emits `<total_tokens>5000000 tokens left</total_tokens>`.                                                  |
| JSON `totalTokensReminder`             | `"off"`, `"fixed"`; Sonnet 4.5                        | Reproduces the environment pair. Environment `off` overrides JSON `"fixed"`.                                                                       |
| `CLAUDE_CODE_THISTLE_GREBE`            | `default`, `no_nudges`, `counter_steer`; Sonnet 4.5   | `no_nudges` removes the broad-exploration delegation instruction; `counter_steer` additionally emits the delegation-cost warning.                  |
| `CLAUDE_CODE_TOASTY_THIMBLE`           | `0`, `1`, custom text; Fable 5                        | Custom text appears after tool results. Neither boolean literal appears as a standalone reminder.                                                  |
| `CLAUDE_CODE_GENTLE_PARASOL`           | `0`, custom text; Fable 5                             | Custom secondary text appears after tool results; `0` produces no literal reminder.                                                                |
| `CLAUDE_CODE_SILENT_TURN_REMINDER`     | `0`, `1`, with threshold `1` and custom text; Fable 5 | The custom reminder appears after the synthetic silent tool turns only at `1`.                                                                     |
| `CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS` | `0`, `1`; Sonnet 4.5                                  | The Bash description's `# Committing changes with git` section is present at `0` and absent at `1`. This public control checks the capture method. |

The strict and relaxed Bash markers are, respectively:

```text
Do your work through the Bash tool wherever it can accomplish the job
You can do much of your work through the Bash tool when it is the simpler route
```

The delegation assertions use these exact markers:

```text
For broad codebase exploration or research that'll take more than 3 queries
Delegate only when the payoff clearly exceeds that overhead.
```

Custom reminders use distinct markers `AUDIT_PRIMARY_REMINDER`, `AUDIT_SECONDARY_REMINDER`, and
`AUDIT_SILENT_REMINDER`. Assertions require exact text lines, with absence on request one and
presence on requests two and three. Disabled cases and boolean-literal checks are separate
assertions. The fixture path and prompt contain none of those markers.

### Source paths behind the runtime results

The typed declarations below are all on `modules/module-179193283.js:11`; the export getters
and constructors were matched by their symbols:

| Control            | Export getter byte and exact excerpt                | Constructor byte and exact excerpt               |
| ------------------ | --------------------------------------------------- | ------------------------------------------------ |
| Bison              | `179,214,903`: `CLAUDE_CODE_BISON_CAIRN:()=>Qr`     | `179,226,461`: `Qr=M.triBool()`                  |
| Larch              | `179,220,365`: `CLAUDE_CODE_LARCH_CISTERN:()=>zr`   | `179,226,476`: `zr=M.bool()`                     |
| Amber              | `179,213,435`: `CLAUDE_CODE_AMBER_ASTROLABE:()=>Jr` | `179,226,449`: `Jr=M.bool()`                     |
| Bash-first         | `179,222,437`: `CLAUDE_CODE_THRIFTY_SONIC:()=>sE`   | `179,225,141`: `sE=M.triBool()`                  |
| Bash wording       | `179,215,468`: `CLAUDE_CODE_COZY_TEAPOT:()=>nE`     | `179,225,171`: `nE=M.enum(["strict","relaxed"])` |
| Primary reminder   | `179,222,470`: `CLAUDE_CODE_TOASTY_THIMBLE:()=>EE`  | `179,225,119`: `EE=M.str()`                      |
| Secondary reminder | `179,219,570`: `CLAUDE_CODE_GENTLE_PARASOL:()=>rE`  | `179,225,130`: `rE=M.str()`                      |

Bison's final assembly preserves explicit false:
`modules/module-186881176.js:1222`, byte `188,353,115`, contains
`a.CLAUDE_CODE_BISON_CAIRN??(Xbe(h)||jzo(s))?lmo:null`.
The Opus pair verifies the resulting section. The Larch and Amber capability calls
discard false through `||void 0`:

| Source                           | Byte          | Exact excerpt                             |
| -------------------------------- | ------------- | ----------------------------------------- |
| `modules/module-182904495.js:11` | `182,911,865` | `a.CLAUDE_CODE_LARCH_CISTERN\|\|void 0`   |
| `modules/module-182904495.js:11` | `182,911,701` | `a.CLAUDE_CODE_AMBER_ASTROLABE\|\|void 0` |

Their compiled model fallbacks remain visible at `0` in the tested requests. The Bash-first resolver
at `modules/module-182904495.js:11`, byte `182,911,013`, checks
`a.CLAUDE_CODE_THRIFTY_SONIC!==void 0` before fallback selection. The wording resolver at byte
`182,911,278` reads `a.CLAUDE_CODE_COZY_TEAPOT??Ae().bashFirstSteerVariant()`.

The other direct consumer expressions are:

| Control                        | Source                             | Byte          | Short exact excerpt                                                                                        |
| ------------------------------ | ---------------------------------- | ------------- | ---------------------------------------------------------------------------------------------------------- |
| Act on established information | `modules/module-186881176.js:1213` | `188,347,207` | `a.CLAUDE_CODE_ACT_DONT_REDERIVE,n=e??eo("tengu_cedar_lantern",!0)`                                        |
| Intro frame                    | `modules/module-186881176.js:1182` | `188,331,148` | `a.CLAUDE_CODE_INTRO_FRAME,n=e??eo("tengu_ochre_wren",!1)`                                                 |
| Token mode and precedence      | `modules/module-186881176.js:1119` | `188,311,345` | `a.CLAUDE_CODE_TOTAL_TOKENS_REMINDER;if(Aie(e))return e;let n=st().totalTokensReminder;if(Aie(n))return n` |
| Delegation                     | `modules/module-181421676.js:14`   | `181,537,034` | `a.CLAUDE_CODE_THISTLE_GREBE);if(n)return{steer:n,source:"env"}`                                           |
| Silent-turn enablement         | `modules/module-186881176.js:3295` | `190,295,472` | `a.CLAUDE_CODE_SILENT_TURN_REMINDER`                                                                       |
| Git instructions               | `modules/module-186881176.js:516`  | `187,580,537` | `a.CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS;if(e!==void 0)return!e;return st().includeGitInstructions??!0`     |

The token and Git consumers' imported settings module
`/$bunfs/root/chunk-gqegtvbg.js` resolves uniquely by its export signature to
`modules/module-181046967.js`. The token tests exercise both settings loading and the valid
environment-first branch. The displayed token numbers belong to reminder text; the actual model
context window is governed separately.

### Computed reminder readers

`modules/module-194994587.js:34` contains the computed reader at byte `195,026,668`:

```text
let n=a[e.envVar];if(n!==void 0)return Lu(n)
```

Its descriptor values are `envVar:"CLAUDE_CODE_TOASTY_THIMBLE"` at byte `195,027,300` and
`envVar:"CLAUDE_CODE_GENTLE_PARASOL"` at byte `195,027,477`. The parser at byte `195,026,472`
contains `return _s(o)||Le(o)?null:os(o,"env",void 0)`. This explicitly separates boolean-like
text from a custom reminder. The multi-turn captures establish the effect after successful tool
results; the initial request alone cannot exercise that path.

### Trusted scope for reminder overrides

The two computed reminders, the three `SILENT_TURN_REMINDER*` names, and
`CLAUDE_CODE_MODEL_CAPABILITIES` are filtered from project/local `env` blocks.
`modules/module-182643872.js:11` includes the names in `RQn`: the primary and secondary names occur
at bytes `182,649,709` and `182,649,738`; model capabilities at `182,649,310`.
The filtering function starts at byte `182,650,511` with
`function P(e,n,r,E){if(!e||!hqt(n))return e` and removes matching keys with `delete o[s]`.

The scope operand is `zc=new Set(["projectSettings","localSettings"])` at
`modules/module-179448471.js:12`, byte `179,495,269`, consumed by `hqt`.
Its allow-through exceptions cover telemetry/exporter opt-outs and additional disable-only controls
in `Hc`, `Kc`, and `mJr`
(bytes `179,497,048`, `179,497,134`, and `179,511,786`); these reminder names match none of them.
The diagnostic explicitly says “project-scoped settings can't set this key”. The loopback tests
supply overrides in the launched process environment; project/local acceptance was outside the
runtime matrix.

### Additional static controls and qualifications

These are selected current traces. Their inclusion supplies no introduction-version claim.

**Hidden capability override:** `CLAUDE_CODE_MODEL_CAPABILITIES` is a string control absent from
the checked public union. `modules/module-180043993.js:11`, bytes `180,066,484–180,066,870`,
defines `iJr`, which parses semicolon-separated clauses and comma-separated capabilities:

```text
n=a.CLAUDE_CODE_MODEL_CAPABILITIES
for(let s of n.split(";"))
let c=l.trim(),v=!c.startsWith("-");t.set(v?c:c.slice(1),v)
```

A clause may use a `model=...` selector; a selector ending in `*` matches a prefix.
The helper at byte `180,066,870` normalizes the input model with
`return e.replace(/\[1m\]/gi,"")`. Later entries overwrite earlier entries in the map.
At byte `180,066,227`, `function uy(e,t,n){return Eet(t,e)??lqt(e,t,n)}` consults this map
before model data. Control-specific explicit values still win in `R2`:
`if(r!==void 0)return r` at `modules/module-182904495.js:11`, byte `182,908,482`.
This changes local capability decisions; actual backend acceptance and other feature gates remain
separate and untested.

**Public concurrency name with a narrow demonstrated consumer:**
`CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS` resolves through
`function _zn(){return a.CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS??nuo}` at
`modules/module-186881176.js:1054`, byte `188,204,850`, with `nuo=20` immediately before it.
The traced enforcement is per-plugin `$.agent.spawn`. `Bho` at
`modules/module-186881176.js:1639`, byte `188,552,367`, uses
`let{plugin:h}=g,b=r.get(h)??0,w=n();if(b>=w)throw` and increments/decrements the count keyed by
plugin. A global limit across every Agent-tool path remains unverified.

**Public spawn-depth resolver:** `CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH` is declared with
`M.int({min:1,digitsOnly:!0})` at `modules/module-179193283.js:11`, byte `179,250,188`.
`ew` at `modules/module-186587895.js:11`, byte `186,588,639`, checks
`a.CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH;if(o!==void 0)return o` before a cached remote fallback;
`n=3` at byte `186,588,607` supplies the default. This establishes the resolver; complete
spawn-path enforcement was outside this audit.

**Public cache selection:** `RWt` at `modules/module-186881176.js:1909`, byte `188,864,485`,
returns forced `5m` first through `if(a.FORCE_PROMPT_CACHING_5M)`, then chooses the main/subagent
environment TTL, the corresponding settings TTL, eligible agent frontmatter, and enable-1h flags.
The environment-first return is `if(g!==void 0)return{ttl:g,reason:"env"}`; the settings return is
`if(b!==void 0)return{ttl:b,reason:"setting"}`. This static selector leaves provider acceptance,
cache billing, and downstream provider restrictions untested.

## Root settings declared by the executable

### Lazy field factories and counting boundaries

The schema implementation now builds from lazy field factories. `Ta` returns the root factory map
in `modules/module-179448471.js:16`, starting at byte `179,659,264`. The default factory is
`St=f(()=>new Sn(Ta(Get()),dm))` at byte `179,728,937`.
The class `Sn`, at `modules/module-179448471.js:14–16`, bytes `179,632,627–179,634,184`, includes:

```text
whole(){return this.wholeSchema??=u(this.shapeOf(this.keys)).passthrough(),this.wholeSchema}
forDocument(e,n="passthrough")
```

Ordinary parsing therefore preserves unknown fields. A separate stripping path exists:
`function QJo(e){return St().forDocument(e,"strip")}` at
`modules/module-179448471.js:16`, byte `179,729,035`.
An accepted value still needs a functioning consumer.

The registry selection is `Lg=["autoMode","deepLink","voice","briefView","screenReader"]`
at `modules/module-179448471.js:14`, byte `179,629,876`. Each selected entry has
`buildGate:()=>!0`. `Get()` filters by those gates and `qi()` merges their shapes at bytes
`179,632,311–179,632,441`. The final root spread at byte `179,728,826` is
`...Yr(qi(e),(i)=>()=>i)`: it wraps values as factories while retaining the property names.
Its helper resolves to `Fi` in `modules/module-178957070.js:11`, bytes
`178,980,681–178,980,776`, where `iX(o,i,t(r,i,s))` retains the original key `i`.

| Component or operation                                    | Count |
| --------------------------------------------------------- | ----: |
| Direct unconditional root properties                      |   182 |
| Seven properties from five compiled-true registry entries |     7 |
| Unconditional union: `182 + 7`                            |   189 |
| Conditional `xaaIdp` property                             |     1 |
| Potential explicit names: `189 + 1`                       |   190 |
| Intersection with the 172-root public index               |   167 |
| Editor annotation and marketplace aliases                 |     3 |
| Unindexed declarations: `190 − 167 − 3`                   |    20 |
| Unindexed declarations with the XAA gate unset            |    19 |

The registry adds `skipAutoPermissionPrompt`, `useAutoModeDuringPlan`, `autoMode`,
`disableDeepLinkRegistration`, `voiceEnabled`, `defaultView`, and `axScreenReader`.
Eight other root spreads are compiled `...!1` and contribute no keys. The XAA spread at
`modules/module-179448471.js:16`, byte `179,662,446`, begins
`...a.CLAUDE_CODE_ENABLE_XAA&&{xaaIdp:`.

The three exclusions are `$schema`, `additionalMarketplaces`, and `allowedMarketplaces`.
The alias declarations say “Alias for extraKnownMarketplaces” and “Alias for strictKnownMarketplaces”
at `modules/module-179448471.js:16`, bytes `179,694,135` and `179,695,231`.
The five public-index roots absent from this explicit shape are `browserExternalPageTools`,
`disableBrowserExternalNavigation`, `disableDesktopLocalSessions`, `disableMobileSimulatorTools`,
and `sshHostAllowlist`; their host/platform implementations were outside this audit.

All 20 unindexed names below also appeared in the previous report's 22-name list.
`prependPlugins` and `appendPlugins` have moved into the current public index:
`22 − 2 = 20`. The total declared-name change, `185 → 190`, compares reported inventories;
it supplies no introduction version for an individual key.

### All 20 unindexed declarations

“Unindexed” refers to the canonical settings index. **15** entries have a statically traced
active/gated consumer chain; **5** retain limited or inert evidence. Runtime verification applies
only to the separately listed loopback cases. Every declaration offset below is in
`modules/module-179448471.js:16`, except `defaultView`, whose registry declaration is on line 14.
The source column gives a short consumer excerpt within its cited byte span.

| Key                                | Declared shape / declaration byte                                          | Consumer evidence and qualification                                                                                                                                                                                                                                 |
| ---------------------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `autoDreamEnabled`                 | Boolean; `179,721,171`                                                     | `modules/module-194935956.js:11`, bytes `194,937,856–194,937,966`: `if(!sRt())return!1;let e=st().autoDreamEnabled`. Availability precedes the setting; the runner has additional auto-memory and session gates.                                                    |
| `precomputeCompactionEnabled`      | Boolean; `179,725,165`                                                     | `modules/module-186881176.js:1905`, bytes `188,821,232–188,821,370`: `return Yo("precomputeCompactionEnabled",!0).value`. The fallback is now true after prerequisite gates.                                                                                        |
| `totalTokensReminder`              | `off`, `infinite`, `fixed`, `countdown`, `padded-countdown`; `179,719,149` | `modules/module-186881176.js:1119`, bytes `188,311,324–188,311,619`: `let n=st().totalTokensReminder;if(Aie(n))return n`. Environment-first precedence; JSON `off`/`fixed` and the conflict case were tested.                                                       |
| `totalTokensReminderBudget`        | Positive integer; `179,719,863`                                            | `modules/module-186881176.js:1119`, bytes `188,311,716–188,312,040`: `let n=st().totalTokensReminderBudget;if(Number.isFinite(n)&&n>0)return n`. A valid environment budget wins; fallback is 15,000,000.                                                           |
| `totalTokensReminderAfterUserTurn` | Boolean; `179,720,137`                                                     | `modules/module-186881176.js:1119`, bytes `188,312,151–188,312,413`: `let n=st().totalTokensReminderAfterUserTurn;if(n!==void 0)return n`. Defined environment value wins; the setting and subsequent remote/default values are latched per session.                |
| `modelProposedGoals`               | `auto`, `alwaysAsk`, `disabled`; `179,714,680`                             | `modules/module-181046967.js:24`, bytes `181,154,666–181,154,824`: `if(await EOe("modelProposedGoals",e)!=="absent")return"alwaysAsk"`. Policy/flag/user values control the tool; raw present or unreadable configuration can conservatively fall back to approval. |
| `showMessageTimestamps`            | Boolean; `179,726,215`                                                     | `modules/module-207446103.js:24`, bytes `207,612,798–207,612,835`: `ut=V(UI),wt=CP(BI),vt=ut&&wt`. The renderer combines application-state preference and rollout. Its public VS Code spelling has a distinct namespace, discussed below.                           |
| `defaultView`                      | `chat`, `transcript`; `179,631,899`                                        | `modules/module-196118310.js:49`, bytes `196,248,558–196,248,701`: `if(g())Uve(!0)`. Chat startup additionally requires interactive-session eligibility and brief-view entitlement.                                                                                 |
| `remoteControl`                    | Object with `shareHostProfile: off/basic/full`; `179,726,743`              | `modules/module-202610261.js:11`, bytes `202,612,823–202,613,099`: `return t6e.find((s)=>d.includes(s))??S`. Rollout, restrictive-source selection, and error gates constrain host-profile collection.                                                              |
| `remoteTools`                      | Object with `allowUnattendedServing`; `179,721,943`                        | `modules/module-200501646.js:12`, bytes `200,506,131–200,506,307`: `n?.remoteTools?.allowUnattendedServing===!1`. Explicit false in policy/user sources constrains unattended serving. Consent is separately stored and checked.                                    |
| `skipWorkflowUsageWarning`         | Boolean consent record; `179,721,664`                                      | `modules/module-181046967.js:24`, bytes `181,153,877–181,154,088`: `ge("userSettings")?.skipWorkflowUsageWarning`. The reader also checks local/flag/policy sources; workflow eligibility and acceptance persistence are separate.                                  |
| `daemonColdStart`                  | `transient`, `ask`; `179,727,688`                                          | `modules/module-186799378.js:11`, bytes `186,800,978–186,801,151`: `let n=hO()?.settings.daemonColdStart;if(n!==void 0)return n;return i()`. Valid environment value wins, then cached settings, then rollout.                                                      |
| `proxyAuthHelper`                  | Shell-command string; `179,659,914`                                        | `modules/module-180130702.js:15`, bytes `180,161,617–180,161,722`: `if(!a.CLAUDE_CODE_ENABLE_PROXY_AUTH_HELPER)return`. Separate enablement, trust, and noninteractive branches apply; output becomes a proxy authorization header.                                 |
| `policyHelpers`                    | Per-platform helper/static-policy map; `179,661,087`                       | `modules/module-181046967.js:16`, bytes `181,105,581–181,110,098`: `else if(n===null\|\|!xn.has(n))`. Admin-source checks and separate remote-execution/payload authorization constrain this path.                                                                  |
| `xaaIdp`                           | `issuer`, `clientId`, optional `callbackPort`; `179,662,476`               | `modules/module-181421676.js:51`, bytes `181,857,573–181,857,606`: `function nY(){return st().xaaIdp}`. Both schema membership and the authentication flow check XAA enablement.                                                                                    |
| `breakReminder`                    | Object with enablement, intervals, and message; `179,663,200`              | **Limited:** `modules/module-203021056.js:11`, bytes `203,022,943–203,025,076`: `this.cachedBreakThresholdMs=(r.breakThresholdMinutes??m)*60000`. A ledger reader exists; its timer does no scheduling. A firing reminder remains unverified.                       |
| `quietHours`                       | Object with `enabled`, `start`, `end`; `179,663,966`                       | **Inert retained reader:** `modules/module-216818348.js:11`, bytes `216,826,839–216,826,956`: `Wellbeing settings are not available in this build`. The exported call is a stub; the retained draft function contains the exact-name read.                          |
| `doneMeansMerged`                  | Boolean; `179,718,935`                                                     | **Limited:** `modules/module-216660407.js:12`, bytes `216,675,004–216,676,078`, includes `"doneMeansMerged"` in the Internal config category. Schema/category evidence establishes no completion-policy enforcement.                                                |
| `todoFeatureEnabled`               | Boolean; `179,726,422`                                                     | **Limited:** `modules/module-200533325.js:11`, bytes `200,538,105–200,539,167`: `todoFeatureEnabled:s.todoFeatureEnabled??K(n,"todoFeatureEnabled")`. Compatibility and snapshot readers establish no task-panel activation.                                        |
| `autoUploadSessions`               | Boolean; `179,728,485`                                                     | **Limited:** `modules/module-200533325.js:11`, bytes `200,538,105–200,539,167`: `autoUploadSessions:s.autoUploadSessions??vbt("autoUploadSessions",n.autoUploadSessions)`. Compatibility/snapshot persistence establishes no upload activation through this key.    |

### Precompute's changed fallback

The full current gate, `modules/module-186881176.js:1905`, bytes
`188,821,232–188,821,370`, is:

```text
function QU(){if(!Df())return!1;if(!sG())return!1;if(!T("tengu_sepia_moth",!1))return!1;return Yo("precomputeCompactionEnabled",!0).value}
```

The previous report records a false default. This build passes true to the preference resolver
after auto-compaction, remote-session, and rollout checks. The resolver `Yo` in
`modules/module-182829487.js:11`, bytes `182,830,601–182,830,986`, searches enabled sources and
legacy compatibility before `return{value:i,source:"default"}`. Thus `!0` is the fallback argument.

The actual local `Jwt` prerequisite in `modules/module-186881176.js:914`, bytes
`187,935,740–187,935,827`, is
`function Jwt(){return Boolean(Le(process.env.DISABLE_COMPACT)||a.DISABLE_AUTO_COMPACT)}`.
The runner `YHr` checks `if(!QU())return!1` and then applies history, failure, SDK, and hook conditions
at `modules/module-186881176.js:1905`, bytes `188,826,867–188,831,992`.
Setting true expresses a preference within those gates. Live compaction was outside the tests.

### Provenance, privacy, and limited readers

The merged-settings getter is `function st(){return D_().settings||{}}`
(`modules/module-181046967.js:16`, bytes `181,143,100–181,143,139`). `D_` obtains the merged result;
`Qsr` composes plugin base and enabled sanitized settings tiers at
`modules/module-179448471.js:20`, bytes `179,879,966–179,881,576`.
Controls that require trusted sources use separate source-specific readers.

For `remoteControl.shareHostProfile`, the ordering operand is
`t6e=["off","basic","full"]` at `modules/module-179448471.js:16`, byte `179,642,612`.
The collector exits on rollout `off` before consulting settings:
`let e=H();if(e==="off")return` at `modules/module-202610261.js:11`, bytes
`202,613,809–202,614,041`. Full collection uses `g.mcpServers.slice(0,B)` with `B=16`; its MCP
enumerator says `if(r==="project")continue` at bytes `202,616,850–202,617,214`.
Actual collection/transmission was untested.

For `remoteTools`, setting true leaves consent to the separate host/terms-bound record.
The record check includes `j6(r.data.hostname)===j6(e.hostname())&&r.data.terms===ztn`
at `modules/module-200501646.js:11`, bytes `200,503,761–200,504,296`.
The final predicate also refuses absent or stale consent at line 12, bytes `200,506,451–200,506,636`.
This audit made no consent changes.

The proxy-helper executor's trust condition is
`if(Je(n)&&!Ce()&&!n.proxyAuthHelperConfig.trustAccepted())` at
`modules/module-180130702.js:15`, bytes `180,161,901–180,162,925`.
`Ce()` means noninteractive: `return!n().host.launchOptions.isInteractive()` at
`modules/module-178957070.js:11`, bytes `179,046,595–179,046,655`.
The warmup has a separate trust check. These branches make blanket trust claims unsafe;
no credential helper or authentication flow was executed.

The `quietHours` classification was checked beyond an exact-name search. The retained draft builder
reads `let wn=i("quietHours",{enabled:!1}).value` at
`modules/module-216818348.js:11`, bytes `216,826,071–216,826,829`, while the exported `call`
returns the disabled-build message. Its draft function identifier occurs only at its declaration
in the module's syntax tree. This is stronger evidence about that specific path than a missing
search hit; other dynamic consumers remain outside a global dead-code claim.

Public status also needs namespace and history checks. `proxyAuthHelper` appears in the current
policy-precedence table at `settings-reference.md:6002`, under “Read from the highest-priority source
only,” while remaining outside the canonical index. `showMessageTimestamps` occurs in
[VS Code extension settings](https://code.claude.com/docs/en/vs-code.md), snapshot line 538;
lines 515–524 distinguish that namespace from `~/.claude/settings.json`.
`todoFeatureEnabled` also has a historical mention in the pinned `CHANGELOG.md:7444`.
These public spelling occurrences carry narrower evidence than a current canonical root contract.

## Public contract and documentation changes

### Snapshot identity and coverage

| Public snapshot                                                                                                                 | SHA-256                                                            |
| ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| [`settings-reference.md`](https://code.claude.com/docs/en/settings-reference.md)                                                | `76582550507f251beaa3688cfb35ad09b0241935d4a16b11b3a09077d19a7e5c` |
| [`env-vars.md`](https://code.claude.com/docs/en/env-vars.md)                                                                    | `a798c08983bcb2b77928a5ee9d6190018ce0ce608cc33edd0aeda8088d28b46b` |
| [`settings.md`](https://code.claude.com/docs/en/settings.md)                                                                    | `41ea72eb329492a8530f2d4730b093e930151422003f161656297e44cecbf074` |
| [Docs-linked schema](https://json.schemastore.org/claude-code-settings.json)                                                    | `6d4a6e3c7adedffce8079ccaef0a4bab5f5718b054421b4475c788a0ae4bedfe` |
| [Official documentation index](https://code.claude.com/docs/llms.txt)                                                           | `5c23f23c56ee0b7ac4d245fe0b79c59bcec5881e77a3ee80e021a07cb283df6b` |
| [Pinned repository archive](https://codeload.github.com/anthropics/claude-code/tar.gz/1c229fcd1e1e4e452e29a8f116b45fe4cfe2c528) | `542fb69ed71c3d6abb3bc36009b2ebd8beedc2281252ffcba2e69db5d11ed259` |

The public comparison covers **219 complete English Markdown pages**, the documentation index,
**1,524 UTF-8 text files** from the pinned repository, and the schema. All **1,525** repository
regular files were checked against the untruncated Git tree by path and Git blob identity; the
remaining file is a binary image.

There were **225 document requests**, including five legacy Agent SDK URLs. Six returned HTML:
the Claude Tag redirect and the five legacy URLs, which reached a generic landing page. The current
Agent SDK Markdown replacements were fetched through the index. Thus `225 − 6 = 219` accepted
Markdown snapshots. Translations, the excluded HTML pages, private/server configuration, and
unfetched sources remain outside the public-absence claim.

The exact-name comparison uses `\bCLAUDE_CODE_[A-Z0-9_]+\b` over complete source texts:

```text
234 names in docs + 134 in repository − 121 shared = 247
247 + 2 schema-only additions = 249 public names
```

The schema adds `CLAUDE_CODE_ENABLE_APPEND_SUBAGENT_PROMPT` and `CLAUDE_CODE_TEAM_NAME`.
The documentation index adds no further names. Historical mentions, examples, runtime outputs,
and schema entries count as public presence. Current supported behavior still needs the dedicated
reference and shipped consumer.

### Canonical settings and environment inventories

The settings index spans `settings-reference.md:592–834`. Its scope definition at line 578
identifies `Global config` as `~/.claude.json`. The normalization is:

```text
243 rows − 12 global-config rows − 60 dotted rows + 1 nested-only root = 172 roots
172 − keybindingFlavor − taskOutputMaxChars = 170 currently behavioral indexed roots
```

The nested-only root is `remote`, from `remote.defaultEnvironmentId` at line 738. The inactive rows
say “Deprecated and has no effect” (`keybindingFlavor`, line 697) and “Removed in v2.1.277”
(`taskOutputMaxChars`, line 810). The **378** environment rows occupy `env-vars.md:136–513`,
so `513 − 136 + 1 = 378`. This inventory includes context outputs and obsolete controls.

Reconstructing the previous index from the .239 report's complete lists and the .280 report's
listed changes gives **165 roots** and **366 environment names**. The current differences are
`165 + 7 = 172` and `366 + 12 = 378`, with no removed names in those reconstructed sets.
These are report-to-current-index differences; they establish documentation changes within that
comparison boundary.

| Added root index name               | Exact index evidence in `settings-reference.md`                                  |
| ----------------------------------- | -------------------------------------------------------------------------------- |
| `allowClaudeInChromeWithManagedMcp` | Line 596: “server run alongside a deployed”                                      |
| `allowedProviders`                  | Line 600: “a machine may use”                                                    |
| `appendPlugins`                     | Line 607: “after every mod a user installs”                                      |
| `availableModelsMatch`              | Line 624: “permit only the version it names”                                     |
| `deniedModels`                      | Line 645: “even ones `availableModels` permits”                                  |
| `maxProseWidth`                     | Line 703: “Cap how wide the prose in Claude's responses runs in a wide terminal” |
| `prependPlugins`                    | Line 732: “before every mod a user installs”                                     |

`appendPlugins` and `prependPlugins` were already declared in the previous binary report. Their
appearance here moves them into the public index; it does not date their implementation.

The 12 added environment rows are:

```text
CLAUDE_CODE_DISABLE_AUTH_REFRESH_LOCK
CLAUDE_CODE_DISABLE_DANGEROUS_RM_TIMEOUT
CLAUDE_CODE_DISABLE_MODEL_ACCESS_FALLBACK
CLAUDE_CODE_DISABLE_POWERSHELL_CMD_RM_DENY
CLAUDE_CODE_DISABLE_STRUCTURED_OUTPUTS
CLAUDE_CODE_DISABLE_SUBSTITUTION_RM_PROMPT
CLAUDE_CODE_DISABLE_WEB_FETCH
CLAUDE_CODE_MAX_MCP_DESCRIPTION_LENGTH
CLAUDE_CODE_NONSTREAMING_TIMEOUT_RETRIES
CLAUDE_CODE_PLUGIN_DIRS
CLAUDE_CODE_SKIP_MODEL_ACCESS_MEMORY
VERTEX_REGION_CLAUDE_5_5_SONNET
```

Their exact rows occur at `env-vars.md:238,249,257,267–269,274,311,328,342,379,508`.
Several affect security checks or authentication coordination; inclusion in this inventory is
informational. `CLAUDE_CODE_MAX_MCP_DESCRIPTION_LENGTH` already had release-history evidence in
2.1.280 and now has a canonical table row.

### Release history and schema reconciliation

The [pinned changelog](https://github.com/anthropics/claude-code/blob/1c229fcd1e1e4e452e29a8f116b45fe4cfe2c528/CHANGELOG.md)
starts the 2.1.288 section at line 1. Line 18 says “added `CLAUDE_CODE_DISABLE_STRUCTURED_OUTPUTS`
to turn structured outputs off”; `env-vars.md:268` also contains its row. It is a public control.
Line 83 says “Changed `/autocompact` to save the auto-compact window per model, so each model keeps
its own setting when you switch”; `settings-reference.md:1228–1260` describes the current
`modelSettings` fields. These statements have public-source evidence; the corresponding complete
integration flows were outside the loopback suite.

The settings guide still warns “The schema can lag behind the newest CLI releases”
(`settings.md:554`). The published schema matches the revision-pinned file byte for byte.
It has **142** root properties and `"additionalProperties": true` at line 270. Its intersection
with the 172-root index is **132**, leaving `172 − 132 = 40` index-only roots and
`142 − 132 = 10` schema-only roots.

The schema-only names are `$schema`, `autoConnectIde`, `autoInstallIdeExtension`, `diffTool`,
`externalEditorContext`, `permissionExplainerEnabled`, `requireCoworkFullVmSandbox`,
`skippedMarketplaces`, `skippedPlugins`, and `teammateDefaultModel`. Schema acceptance alone
provides no evidence of an effective setting.

## Runtime test scope and reproduction

The final matrix contains **25 initial-request cases + 7 multi-turn cases = 32 cases**.
Each multi-turn case sends three requests: `25 + 7 × 3 = 46`. The server returns two fixed
`Read` calls per multi-turn case, yielding `7 × 2 = 14` successful tool results. The second read
can use the unchanged-file optimization. **59 content assertions passed.** A separate smoke
case and three preliminary full passes preceded the final run; they are excluded from these totals.

Every case launches a fresh process with separate temporary HOME, configuration, cache, working,
and temporary directories. Child environments come from an explicit allowlist and a dummy,
non-secret API-key string used solely by the local protocol fixture. The server listens on
`127.0.0.1`, records request bodies, and returns fixed Messages API responses; it calls no model.
No real account credentials are supplied.

The CLI arguments are:

```text
-p "Respond with the fixture result."
--model <model-id>
--setting-sources ""
--settings <case-json>
--strict-mcp-config --mcp-config '{"mcpServers":{}}'
--no-session-persistence --dangerously-skip-permissions --no-chrome
--output-format json
```

The bypass-permissions flag is confined to the OS-sandboxed, fake-server fixture. Actual tool
execution is limited by the fixed responses to reading a task-owned file containing
`A fixed local test fixture.`. Initial-request cases return a final text response immediately;
multi-turn cases return two `Read` tool-use responses, then a final text response. Streaming
responses include message-start, content-block start/delta/stop, message-delta, and message-stop
events, with fixed usage values of 100 input and 10 output tokens.

The final macOS sandbox denies external networking, access under `/Users`, the system Claude Code
managed-settings directory, and execution of `security` and `defaults`; keychain/security-service
lookups are denied too. Loopback networking remains available. An external HTTPS probe with a
fixed DNS mapping failed, and a read of the repository's prior report failed with
`Operation not permitted`. This is a fixture-specific isolation profile; other filesystem paths
and loopback ports remain accessible.

Feature-flag fetching, auto-updates, telemetry, error reporting, IDE auto-connection, and auto-memory
are disabled in the fixture environment. Compiled model defaults still participate. These tests
cover the tested local branches; authenticated Auto Mode, remote consent, helper execution,
real compaction, and interactive UI flows require separate verification.

## Verification and practical conclusions

An independent check reproduced the distinct environment-name sets, the root-schema composition,
the public-name union and table normalization, and the final request assertions. The main audit
also compared all **2,183 extracted modules** with the pinned artifact and verified **122 focused
environment excerpts** plus **363 declaration/constructor/settings-consumer spans** by exact bytes
and source line. Markdown formatting, table structure, and local document links were checked.

The direct-read inventory is complete for its stated syntax criterion. Arbitrary alias propagation,
`require` aliases, object-spread enumeration, unresolved dynamic keys, native code, and general
control-flow reachability remain outside that criterion. The loopback cases connect selected source
claims to the actual executable; they establish those local paths within the recorded fixture.

The repository retains these findings, artifact identities, selected excerpts, and the reusable
method. Full vendor-source extractions, raw requests, and the complete rolling-document corpus
were temporary audit material. Repeating the historical public-absence calculation exactly requires
matching snapshots; the current URLs may serve changed content.

- Prefer the current documented setting when it covers the intended behavior.
- Re-check explicit off semantics at the final consumer. Bison preserves false; Larch and Amber
  allow model/capability fallback in the tested branches.
- Keep `precomputeCompactionEnabled` and `autoDreamEnabled` availability-qualified, even when their
  settings express an enabled preference.
- Treat the five limited/inert root entries as unverified features. A retained declaration,
  compatibility snapshot, or dormant draft builder gives a narrower result than an active feature.
- Evaluate authentication helpers, consent, remote execution, capability overrides, and host-data
  collection separately before applying them to a real profile.
