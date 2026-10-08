# Claude Code hidden settings analysis — v2.1.293

**Snapshot:** 2026-10-08. **Target:** the installed Homebrew `claude-code@latest`
executable, Claude Code **2.1.293**. Public sources were fetched on 2026-10-08 UTC.
This report follows the [reverse-analysis method](claude-code-reverse-method.md) and uses the
[2.1.288 report](claude-code-hidden-settings-2.1.288.md) as its historical baseline.
The [machine inventory](claude-code-hidden-settings-2.1.293.inventory.json) retains sorted sets,
source witnesses, snapshot identities, and test outcomes for subsequent comparisons.

## Findings

- The executable exports **614 typed `CLAUDE_CODE_*` names**; **562** have a qualifying
  binding-resolved direct read. **337** of those are absent from the checked public corpus.
  These are investigation candidates with varied purposes and risk.
- The root factory declares **191 potential names**, including **21 outside the 172-root public
  index**. `idleCompaction` is additional to the previous report's unindexed list. It is a veto
  preference: false stops idle compaction; true still requires rollout and session/cache eligibility.
- `CLAUDE_CODE_SILENT_TURN_REMINDER_SECONDS` selects elapsed silent time in place of the turn-count
  criterion. A two-second threshold emitted the reminder despite a 999-turn threshold; a 3,600-second
  threshold suppressed it despite a one-turn threshold. The source's elapsed-time branch also omits
  the turn branch's three-reminder cap; that cap difference was traced statically.
- `precomputeCompactionEnabled` retains its gated **true fallback**. The prior tested Bison,
  Bash-first, token, delegation, and computed-reminder behaviors also reproduced.
- The suite now includes **Fable 5.1 with lean prompt**: JSON token-reminder off/fixed, Bison off/on,
  the three Thistle modes, and Gentle Parasol after tool results all passed their content checks.
- **44 final cases, 66 Messages requests, 84 content assertions, and 22 non-error Read results**
  passed. The canonical environment table has **388 rows**, ten more than the reconstructed
  previous table; the public root index remains unchanged.

Runtime results establish local request serialization against a fixed loopback server. Real
provider acceptance, model compliance, interactive UI, account rollout, and actual compaction
remain outside these tests. This audit left personal Claude Code configuration unchanged.

## Artifact and source identity

| Item                        | Observed value                                                                                                                                       |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Homebrew inventory          | `claude-code@latest 2.1.293`                                                                                                                         |
| Executable version          | `2.1.293 (Claude Code)`                                                                                                                              |
| Resolved path               | `/opt/homebrew/Caskroom/claude-code@latest/2.1.293/claude`                                                                                           |
| Size                        | `236,330,608` bytes                                                                                                                                  |
| SHA-256                     | `4e21122a227857da1178aca3299700c1fd7f2b77c93f12e73c2c76db796a105e`                                                                                   |
| Embedded build time         | `2026-10-07T06:36:42Z`                                                                                                                               |
| Embedded build revision     | `3abc54a9d60b4d12c627afad22d6e5f58a6199d2`                                                                                                           |
| Public release              | [`v2.1.293`](https://github.com/anthropics/claude-code/releases/tag/v2.1.293), published `2026-10-07T18:10:20Z`                                      |
| Public repository commit    | [`79babc372d64101f981bd2b52c3dbe588596dc56`](https://github.com/anthropics/claude-code/commit/79babc372d64101f981bd2b52c3dbe588596dc56)              |
| Docs-linked schema revision | SchemaStore [`d2cbdcde9855c1bf9ea99c336163cc6c93753e39`](https://github.com/SchemaStore/schemastore/commit/d2cbdcde9855c1bf9ea99c336163cc6c93753e39) |
| Extracted source            | `2,290` modules, `43,669,974` bytes                                                                                                                  |
| JavaScript syntax parser    | TypeScript `6.0.3`; zero parse diagnostics                                                                                                           |

At binary byte `183,745,698`, the build metadata contains:

```text
VERSION:"2.1.293",FEEDBACK_CHANNEL:"https://github.com/anthropics/claude-code/issues",BUILD_TIME:"2026-10-07T06:36:42Z",GIT_SHA:"3abc54a9d60b4d12c627afad22d6e5f58a6199d2"
```

Binary offsets are zero-based UTF-8 bytes in this exact artifact; ranges are end-exclusive.
`modules/module-184201435.js:11` names the extracted source beginning at byte `184201435`,
with line numbers measured before formatting. Short excerpts may lie within the cited function
span. The method's NUL-region extractor recreates these paths. The executable's build revision,
public repository commit, and schema revision identify separate artifacts.

The old 2.1.288 executable was absent from its former installation path. Comparisons below are
therefore against recorded reports and reconstructed public inventories. A newly examined name or
a changed count supplies no precise implementation-introduction version.

## Environment inventory

### Binding and parser checks

`modules/module-184201435.js:11`, byte `184,268,848`, composes the typed maps:

```text
bx={...P,...d,...G,...u,...m,...b,...B,...H}
```

The exported environment object is `a`. Its imported route,
`/$bunfs/root/chunk-70qqbqq4.js`, resolves uniquely to that module using the union of names
imported from the route and the actual export sets. Its lazy getter at byte `184,269,014` contains:

```text
let C=process.env[r];if(C!==n)e=s.parse(C),n=C;return e
```

The parser module is `modules/module-184199944.js:11`. The tri-state constructor at byte
`184,200,837` is `s=i((n)=>{if(Le(n))return!0;if(ps(n))return!1;return})`.
The actual recognizers in `modules/module-183920724.js:11`, bytes `183,921,339` and
`183,921,481`, lowercase and trim, then test `["1","true","yes","on"]` and
`["0","false","no","off"]` respectively.
`bool` applies the true-string test; `triBool` preserves false and undefined separately.
`str` trims input and maps blank strings to undefined. Consumer latching remains a separate step.

The census uses lexical binding symbols for named typed imports and global/imported Node
`process.env`. It includes property reads, literal or statically resolved constant-index reads,
direct object destructuring, and read/write compound operations. It excludes comments, strings,
write-only targets, deletions, and lexical shadowing. Fixtures distinguish real declarations from
TypeScript's synthesized symbols for property assignments, including named function/class
expressions and array-rest assignment targets.

Typed namespace-object access, arbitrary alias propagation, `require` aliases, spread enumeration,
unresolved computed keys, native code, and general control-flow reachability are outside this
bounded criterion. All extracted modules parsed; parsing establishes syntax coverage only.

| Set or operation                                  | Count |
| ------------------------------------------------- | ----: |
| Typed exported names                              |   614 |
| Typed names with a qualifying direct read         |   562 |
| Typed names outside that set: `614 − 562`         |    52 |
| Exact names in the checked public union           |   257 |
| Typed direct-read names present in that union     |   225 |
| Strict hidden-candidate set: `562 − 225`          |   337 |
| Separately traced hidden computed-reader controls |     2 |

The computed controls are `CLAUDE_CODE_TOASTY_THIMBLE` and `CLAUDE_CODE_GENTLE_PARASOL`,
both already included among the 52 outside the direct set. The dynamic scan has 153 access
occurrences across environment objects; those are unresolved access sites, not a count of named
hidden settings. Three direct-read names lack a typed export and stay outside the arithmetic:
`CLAUDE_CODE_COORDINATOR_EXTRA_TOOLS`, `CLAUDE_CODE_COORDINATOR_MODE`, and
`CLAUDE_CODE_ENABLE_TELEMETRY`.

The previous report's 588/537/320 totals and this report's 614/562/337 totals use different artifacts,
public corpora, and explicitly described scan boundaries. The retained current sets make future
comparisons possible without reconstructing a complete old environment inventory from prose.

## Prompt controls verified by the executable

The full-prompt cases use `CLAUDE_CODE_SIMPLE_SYSTEM_PROMPT=0` with
`claude-sonnet-4-5`, `claude-opus-5[1m]`, or `claude-fable-5[1m]` as indicated below.
The additional lean cases use `CLAUDE_CODE_SIMPLE_SYSTEM_PROMPT=1` and
`claude-fable-5-1[1m]`. These model IDs select local prompt behavior for the fixture.

| Control                                    | Cases and observed content                                                                                                                                                                                                                                                    |
| ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `CLAUDE_CODE_BISON_CAIRN`                  | Full Opus 5 and lean Fable 5.1: `0` removes `# Delivering work`; `1` includes it.                                                                                                                                                                                             |
| `CLAUDE_CODE_LARCH_CISTERN`                | Full Opus 5: `# Corrections` remains at both `0` and `1`.                                                                                                                                                                                                                     |
| `CLAUDE_CODE_AMBER_ASTROLABE`              | Full Fable 5: `You are operating autonomously.` remains at both values.                                                                                                                                                                                                       |
| `CLAUDE_CODE_THRIFTY_SONIC`                | Full Sonnet 4.5: strict Bash-first reminder absent at `0`, present at `1`, on the bypass-permissions branch.                                                                                                                                                                  |
| `CLAUDE_CODE_COZY_TEAPOT`                  | With Thrifty enabled, `strict` and `relaxed` each select their own wording and exclude the other.                                                                                                                                                                             |
| `CLAUDE_CODE_ACT_DONT_REDERIVE`            | Full Sonnet 4.5: `When you have enough information to act, act.` appears only at `1`.                                                                                                                                                                                         |
| `CLAUDE_CODE_INTRO_FRAME`                  | Full Sonnet 4.5: `You are an agent working with the user toward their goals` appears only at `1`.                                                                                                                                                                             |
| `CLAUDE_CODE_TOTAL_TOKENS_REMINDER`        | Full Sonnet 4.5: `off` removes `<total_tokens>`; `fixed` emits `<total_tokens>5000000 tokens left</total_tokens>`.                                                                                                                                                            |
| JSON `totalTokensReminder`                 | `off`/`fixed` reproduce that pair on full Sonnet 4.5 and lean Fable 5.1. A full-prompt conflict case confirms environment `off` wins over JSON `fixed`.                                                                                                                       |
| `CLAUDE_CODE_THISTLE_GREBE`                | Full Sonnet 4.5: `no_nudges` removes the broad-exploration instruction; `counter_steer` also adds the delegation-cost warning. Lean Fable 5.1: the broad-exploration instruction is absent in all three modes, and the counter-steer warning appears only in `counter_steer`. |
| `CLAUDE_CODE_TOASTY_THIMBLE`               | Full Fable 5: custom text appears after tool results. `0` and `1` do not become literal reminders.                                                                                                                                                                            |
| `CLAUDE_CODE_GENTLE_PARASOL`               | Full Fable 5 and lean Fable 5.1: custom text appears after tool results; `0` emits no literal reminder.                                                                                                                                                                       |
| `CLAUDE_CODE_SILENT_TURN_REMINDER`         | Full Fable 5 with threshold `1`: custom text appears after silent tool turns at enablement `1`, and stays absent at `0`.                                                                                                                                                      |
| `CLAUDE_CODE_SILENT_TURN_REMINDER_SECONDS` | Full Fable 5.1: a two-second threshold emits after a configured 2.3-second response delay despite `TURNS=999`; `SECONDS=3600` suppresses the same marker despite `TURNS=1`.                                                                                                   |
| `CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS`     | Full Sonnet 4.5: `# Committing changes with git` in the Bash description is present at `0`, absent at `1`. This public control checks the measurement path.                                                                                                                   |

The Bash wording markers are:

```text
Do your work through the Bash tool wherever it can accomplish the job
You can do much of your work through the Bash tool when it is the simpler route
```

The delegation markers are:

```text
For broad codebase exploration or research that'll take more than 3 queries
Delegate only when the payoff clearly exceeds that overhead.
```

Custom markers are exact text lines, separate from fixture paths and user prompts. Primary,
secondary, and ordinary silent reminders are absent on request one and present on requests two
and three in their enabled cases. The elapsed-seconds positive case is absent on requests one
and two and present on request three. Disabled and boolean-literal checks reach the same tool path.

### Source evidence for shared controls

| Control/path     | Source and byte                                   | Short exact excerpt                                                          |
| ---------------- | ------------------------------------------------- | ---------------------------------------------------------------------------- |
| Bison assembly   | `modules/module-192111083.js:2125`, `194,120,801` | `a.CLAUDE_CODE_BISON_CAIRN??(bCe(h)\|\|ais(s))?XLo:null`                     |
| Larch capability | `modules/module-187971458.js:11`, `187,980,134`   | `a.CLAUDE_CODE_LARCH_CISTERN\|\|void 0`                                      |
| Amber capability | `modules/module-187971458.js:11`, `187,979,968`   | `a.CLAUDE_CODE_AMBER_ASTROLABE\|\|void 0`                                    |
| Thrifty override | `modules/module-187971458.js:11`, `187,979,314`   | `if(a.CLAUDE_CODE_THRIFTY_SONIC!==void 0)return a.CLAUDE_CODE_THRIFTY_SONIC` |
| Cozy selection   | `modules/module-187971458.js:11`, `187,979,575`   | `a.CLAUDE_CODE_COZY_TEAPOT??Re().bashFirstSteerVariant()`                    |
| Act resolver     | `modules/module-192111083.js:2116`, `194,114,854` | `a.CLAUDE_CODE_ACT_DONT_REDERIVE,n=e??$r("tengu_cedar_lantern",!0)`          |
| Intro resolver   | `modules/module-192111083.js:2085`, `194,097,312` | `a.CLAUDE_CODE_INTRO_FRAME,n=e??$r("tengu_ochre_wren",!1)`                   |
| Git inclusion    | `modules/module-192111083.js:550`, `192,811,573`  | `if(e!==void 0)return!e;return ut().includeGitInstructions??!0`              |

The machine inventory preserves each typed export's getter, constructor, and direct-read witness.
Bison/Thrifty/Act/Intro use tri-state booleans; Larch/Amber use ordinary booleans and discard false
before capability fallback. The shared capability resolver checks the explicit argument first:
`if(r!==void 0)return r` at `modules/module-187971458.js:11`, byte `187,976,819`.
Amber has an additional outer gate, `if(!C("tengu_amber_sextant",!0))return null`, in
`modules/module-192111083.js:2079`, byte `194,095,844`. A force-on argument does not guarantee
that every outer condition permits the section.

Token selection now separates the environment/settings check into `TKt`:
`modules/module-192111083.js:2018`, bytes `194,076,014–194,076,144`:

```text
function TKt(){let e=a.CLAUDE_CODE_TOTAL_TOKENS_REMINDER;if(ufe(e))return e;let n=ut().totalTokensReminder;return ufe(n)?n:void 0}
```

The subsequent resolver handles client data and the remote/default `padded-countdown` value.
`bWt` at byte `194,075,643` caches the mode. The same source line declares
`QDo=5000000` at byte `194,073,825` and `bKt=15000000` at `194,073,837`.
These numbers govern reminder text and task-budget calculations; actual model context limits are
separate. The native JSON/environment conflict test exercises the loader and consumer together.

### Delegation: fixed modes, latching, and lean coverage

`modules/module-186400171.js:14`, byte `186,516,313`, validates the mode:

```text
function ks(e){return e==="default"||e==="no_nudges"||e==="counter_steer"?e:void 0}
```

The next resolver, `kR`, gives valid environment input priority over client data, feature data,
model floor, and finally `default`. `dF` at byte `186,516,790` returns the latched mode first.
The prompt initializer at `modules/module-192111083.js:2124`, byte `194,119,563`, registers
`Mfs((n)=>VIn(n)?"no_nudges":void 0)`. Unset therefore permits a model floor; it establishes no
universal active mode across sessions. These three names select built-in behavior, with no custom
text field in this selector.

The counter-steer section is gated by `q.has(yt)&&dF()==="counter_steer"?Nfs:null` at
`modules/module-192111083.js:2125`, byte `194,120,931`. Full-prompt guidance also checks a
fork-specific branch before mode selection. The lean Agent description can select alternate
model/rollout text instead of its ordinary `Jxr(Ye)` helper (`:3286`, byte `195,682,568`).
Consequently `no_nudges` removes specific encouragement. The Agent tool and separately gated
delegation guidance remain available. The lean tests establish the stated marker results,
without claiming an account-independent complete Agent description.

### Computed reminders and model eligibility

`modules/module-200436442.js:34`, byte `200,470,731`, contains the descriptor reader:

```text
function Wu(e,o){let n=a[e.envVar];if(n!==void 0)return Hu(n);
```

Descriptors at bytes `200,471,359` and `200,471,536` supply
`envVar:"CLAUDE_CODE_TOASTY_THIMBLE"` and `envVar:"CLAUDE_CODE_GENTLE_PARASOL"`.
The parser at byte `200,470,522` contains
`let o=e.trim();return ps(o)||Le(o)?null:rs(o,"env",void 0)`.
Explicit boolean-like text resolves to no custom reminder; blank typed input permits fallback.

`Yi` at byte `200,471,699` begins `if(!B7(n))return null` and caches non-null text by conversation
and normalized model. The insertion predicate at byte `200,473,861` requires a user message
whose content contains `tool_result`. Primary reminders have additional attachment/content
suppression; the secondary reminder retains the shared model and tool-result gates.

The current fallback for mid-conversation system support is in
`modules/module-186400171.js:51`, byte `186,797,569`. It checks HIPAA eligibility first,
then `CLAUDE_CODE_FORCE_MID_CONVERSATION_SYSTEM`, then model overrides and an Opus 4.8 exclusion,
and finally `Zy(r,"mid_conv_system",e)??!0`. The force flag changes local capability decisions;
provider acceptance remains untested. The loopback suite leaves that force flag unset.

Project/local scope filtering was not independently re-traced in this pass. The previous report's
filter findings remain version-scoped; this suite supplies reminder values directly in the child
process environment.

## Elapsed-time silent reminders

`CLAUDE_CODE_SILENT_TURN_REMINDER_SECONDS` is declared in
`modules/module-184201435.js:11`, byte `184,235,946`, as `M.int({min:1,wholeValue:!0})`.
It is absent from the checked public union and has a direct reader. The resolver is
`modules/module-192111083.js:3311`, byte `195,691,017`:

```text
function kfn(e){let n=a.CLAUDE_CODE_SILENT_TURN_REMINDER_SECONDS;if(n!==void 0)return n;let r=Qc(e)?.[iAr];return typeof r==="number"&&Number.isFinite(r)&&r>=1?Math.floor(r):void 0}
```

The decisive branch in `NAr`, source line 3324, byte `195,713,826`, is:

```text
let{turnsSinceLastReminder:s,remindersInStretch:g,silentSince:h}=LAr(e),b=h===void 0?NaN:r-Date.parse(h),w=Number.isFinite(b)?Math.floor(b/1000):void 0,M=kfn(n);
if(!(M===void 0?g<gfn&&s>=_fn():s>=1&&w!==void 0&&w>=M))return[];
```

The elapsed value is `floor((now − Date.parse(silentSince)) / 1000)` when finite. Without a
seconds threshold, the branch uses the configured turn count and `gfn=3` reminder cap;
`pfn=5` is the default turn threshold (`:3311`, bytes `195,690,455` and `195,690,461`).
With a seconds threshold, it requires at least one silent turn and sufficient elapsed seconds.
That branch omits both the turn threshold and the three-reminder cap.

Two runtime cases delay the fake server's second response by a configured **2.3 seconds**:

| Case            | Seconds / turns | Expected and observed marker                  |
| --------------- | --------------- | --------------------------------------------- |
| `seconds_short` | `2` / `999`     | Absent in requests 1–2, present in request 3. |
| `seconds_long`  | `3600` / `1`    | Absent in all three requests.                 |

Both explicitly enable silent reminders and use custom text `AUDIT_ELAPSED_REMINDER` on full
Fable 5.1. This tests branch selection and environment parsing. Repeated emission beyond three
reminders, unattended idle firing, and live wall-clock UX were outside the suite. Scheduling still
requires the ordinary foreground/conversation conditions; this is not an independent always-on timer.

## Root settings

### Actual loader, lazy shape, and counts

The loader chain is `NNe → mv → Vce → St → Ca(Oat())`.
`modules/module-184605458.js:20`, bytes `185,008,257–185,008,993`, contains
`c=Vce(i).safeParse(i)` after source cleanup. `Vce` at line 16, byte `184,889,390`, returns
`St().forDocument(e)`, and the root singleton at byte `184,889,335` is
`St=f(()=>new fn(Ca(Oat()),Rm))`.

The selected registry IDs are `Xg=["autoMode","deepLink","voice","briefView","screenReader"]`
at line 14, byte `184,790,083`. All five build gates are compiled true. `Xi` at byte
`184,792,574` merges their shapes; the terminal root spread at byte `184,889,224` is
`...Qr(Xi(e),(i)=>()=>i)`. Its imported map-values helper preserves property keys.
Eight other spreads are `...!1`; the only conditional property spread is XAA at line 16,
byte `184,822,652`: `...a.CLAUDE_CODE_ENABLE_XAA&&{xaaIdp:`.

The lazy class at `modules/module-184605458.js:14–16`, bytes `184,792,834–184,794,391`,
uses `u(this.shapeOf(this.keys)).passthrough()` for its whole schema and provides document-specific
shapes. A separate stripping entry point is
`function lSs(e){return St().forDocument(e,"strip")}` at line 16, byte `184,889,433`.
Unknown keys surviving ordinary parsing still need their own effective consumer.

| Set or operation                    | Count |
| ----------------------------------- | ----: |
| Direct unconditional declarations   |   183 |
| Registry additions                  |     7 |
| Unconditional union: `183 + 7`      |   190 |
| Conditional `xaaIdp`                |     1 |
| Potential explicit roots: `190 + 1` |   191 |
| Canonical public index              |   172 |
| Shipped/public intersection         |   167 |
| Annotation/alias exclusions         |     3 |
| Unindexed: `191 − 167 − 3`          |    21 |
| Unindexed without conditional XAA   |    20 |

The exclusions are `$schema`, `additionalMarketplaces`, and `allowedMarketplaces`.
The registry adds `skipAutoPermissionPrompt`, `useAutoModeDuringPlan`, `autoMode`,
`disableDeepLinkRegistration`, `voiceEnabled`, `defaultView`, and `axScreenReader`.
The five indexed roots absent from this explicit shape remain `browserExternalPageTools`,
`disableBrowserExternalNavigation`, `disableDesktopLocalSessions`, `disableMobileSimulatorTools`,
and `sshHostAllowlist`. Their host/platform implementations were outside this audit.

### Idle compaction: opt-out and threshold are separate controls

`idleCompaction` is declared at `modules/module-184605458.js:16`, byte `184,885,538`:

```text
idleCompaction:()=>H().optional().describe("Set to false to stop Claude Code from compacting a long conversation while the session is idle. Setting it to true does not turn idle compaction on.")
```

The preference reader is `function _xt(){return os("idleCompaction",!0).value}` at
`modules/module-192111083.js:732`, byte `192,984,527`. The ordinary preference resolver
`os`, in `modules/module-187894613.js:11`, bytes `187,895,727–187,896,112`, preserves a defined
source value and otherwise returns the supplied default: `return{value:i,source:"default"}`.

The rollout/threshold resolver `DPt`, `modules/module-192111083.js:748`, byte `193,075,042`,
selects only `log` or `compact` from `tengu_sunny_locket`; other values become `off`.
Its minimum-token input is:

```text
n=a.CLAUDE_CODE_IDLE_COMPACT_MIN_TOKENS??e?.minTokens
minTokens:IPt(n)?Math.max(nio,n):tio
```

`IPt` checks a finite number. Constants at bytes `193,074,925` and `193,074,936` are
`tio=200000` and `nio=1e5`: valid input is clamped to at least **100,000**, and invalid/missing input
falls back to **200,000**. The environment control is typed and absent from the public union.

The actual veto chain in `aio`, bytes `193,075,333–193,075,689`, includes:

```text
if(e.mode==="off")return"disabled";
if(!im())return"compaction_off";
if(!_xt())return"idle_compaction_off";
if(!h())return"prefix_changed";
if(o8()!==WR)return"not_one_hour";
```

`o8()` reads `requestJournal.lastMainThreadCacheTtlMs()` at
`modules/module-183949107.js:11`, byte `184,033,400`; `WR=3600000` at
`modules/module-187971458.js:52`, byte `188,029,813`, supplies the one-hour operand.
Additional checks require appropriate cache warmth, age, token count, request ordering, and usage
state. The scheduler eventually calls `h.compact()` only on its started path. Actual idle
compaction and threshold effects on a live session remain untested.

### All 21 unindexed roots

There are **16 active/gated static chains**, **four limited entries**, and **one inert retained
reader**. The table's classification is about traced source paths. The machine inventory supplies
all 191 declarations and the decisive evidence for each unindexed entry.

| Key                                | Current consumer and qualification                                                                                                                                                                                                                            |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `autoDreamEnabled`                 | `modules/module-200386854.js:11`, bytes `200,388,769–200,388,879`: `if(!JMt())return!1;let e=ut().autoDreamEnabled`. Availability precedes the setting; runner/session gates remain.                                                                          |
| `precomputeCompactionEnabled`      | `modules/module-192111083.js:2839`, byte `194,688,593`: `os("precomputeCompactionEnabled",!0).value`. Auto-compaction, remote-session, and rollout checks precede the true fallback.                                                                          |
| `idleCompaction`                   | `modules/module-192111083.js:748`, byte `193,075,333`: `if(!_xt())return"idle_compaction_off"`. False vetoes; rollout and cache prerequisites still decide availability.                                                                                      |
| `totalTokensReminder`              | `modules/module-192111083.js:2018`, byte `194,076,014`: `let n=ut().totalTokensReminder;return ufe(n)?n:void 0`. JSON/env priority and lean/full serialization were tested.                                                                                   |
| `totalTokensReminderBudget`        | `modules/module-192111083.js:2018`, byte `194,076,240`: `if(Number.isFinite(n)&&n>0)return n`. Environment priority and remote/default fallback are traced; source default is 15,000,000.                                                                     |
| `totalTokensReminderAfterUserTurn` | `modules/module-192111083.js:2018`, byte `194,077,502`: `let n=ut().totalTokensReminderAfterUserTurn;if(n!==void 0)return n`. Defined environment value wins; result latches.                                                                                 |
| `modelProposedGoals`               | `modules/module-186017741.js:24`, byte `186,126,740`: `if(await mNe("modelProposedGoals",e)!=="absent")return"alwaysAsk"`. Policy/flag/user values and raw-presence safeguards govern the gated tool.                                                         |
| `showMessageTimestamps`            | `modules/module-202089278.js:19`, bytes `202,146,428–202,149,698`: `showMessageTimestamps:os("showMessageTimestamps",!1).value`. The renderer additionally ANDs state with rollout; UI untested.                                                              |
| `defaultView`                      | `modules/module-201723974.js:49`, byte `201,856,942`: `ut().defaultView==="chat"` followed by `if(g())mRe(!0)`. Interactive-session and brief-entitlement conditions remain.                                                                                  |
| `remoteControl`                    | `modules/module-210100127.js:11`, byte `210,102,689`: `s?.remoteControl?.shareHostProfile`. Rollout and most-restrictive level selection govern host-profile sharing.                                                                                         |
| `remoteTools`                      | `modules/module-206100477.js:12`, byte `206,105,167`: `n?.remoteTools?.allowUnattendedServing===!1`. Policy/user false constrains unattended serving; separate consent remains required.                                                                      |
| `skipWorkflowUsageWarning`         | `modules/module-186017741.js:24`, byte `186,125,951`: `me("userSettings")?.skipWorkflowUsageWarning`. User/local/flag/policy record; workflow eligibility is separate.                                                                                        |
| `daemonColdStart`                  | `modules/module-191840850.js:11`, byte `191,842,450`: `let n=nM()?.settings.daemonColdStart;if(n!==void 0)return n`. Valid environment value wins, then settings, then rollout.                                                                               |
| `proxyAuthHelper`                  | `modules/module-184476632.js:15`, byte `184,507,576`: `if(!a.CLAUDE_CODE_ENABLE_PROXY_AUTH_HELPER)return`. Enablement, provenance, trust, and noninteractive conditions affect a credential-bearing command.                                                  |
| `policyHelpers`                    | `modules/module-186017741.js:16`, bytes `186,076,704–186,081,221`: `else if(n===null\|\|!Un.has(n))`. Admin-origin and remote payload/execution authorization constrain the per-platform helper map.                                                          |
| `xaaIdp`                           | `modules/module-220543767.js:12`, bytes `220,564,895–220,572,649`: `if(!v6())throw Error`. Conditionally declared, and the authentication flow rechecks enablement.                                                                                           |
| `breakReminder`                    | **Limited.** `modules/module-208610244.js:11`, bytes `208,612,993–208,613,971`: `this.cachedBreakThresholdMs=(r.breakThresholdMinutes??m)*60000`. The timer method schedules nothing; notification firing unverified.                                         |
| `quietHours`                       | **Inert retained reader.** `modules/module-224334021.js:11`, bytes `224,342,828–224,342,902`: `L=async(e)=>(e("Wellbeing settings are not available in this build"),null)`. The exported call is this stub; a dormant draft builder retains the setting read. |
| `doneMeansMerged`                  | **Limited.** `modules/module-224173844.js:12`, bytes `224,189,433–224,189,565`: `"doneMeansMerged"` in the Internal category. No enforcement consumer was established.                                                                                        |
| `todoFeatureEnabled`               | **Limited.** `modules/module-206133099.js:11`, byte `206,138,092`: `todoFeatureEnabled:s.todoFeatureEnabled??j(n,"todoFeatureEnabled")`. Compatibility/snapshot evidence supplies no task-panel activation proof.                                             |
| `autoUploadSessions`               | **Limited.** `modules/module-206133099.js:11`, byte `206,138,092`: `autoUploadSessions:s.autoUploadSessions??fRt("autoUploadSessions",n.autoUploadSessions)`. Compatibility/sanitizer evidence supplies no upload activation proof.                           |

The precompute gate remains:

```text
function WN(){if(!im())return!1;if(!IV())return!1;if(!C("tengu_sepia_moth",!1))return!1;return os("precomputeCompactionEnabled",!0).value}
```

Source: `modules/module-192111083.js:2839`, bytes `194,688,593–194,688,731`.
`im` resolves `autoCompactEnabled` after disable checks (`:732`, byte `192,984,454`), and
`IV` checks the remote-session gate (`:732`, byte `192,984,926`). The previous report's true
fallback conclusion therefore still applies to this traced path.

### Privacy, trust, and public-name qualifications

The merged getter is `function ut(){return ah().settings||{}}` at
`modules/module-186017741.js:16`, byte `186,114,343`. Source-specific consumers use `me`
and policy-tier readers; successful parsing does not grant trust or consent.

Host-profile levels are `YKe=["off","basic","full"]` at
`modules/module-184605458.js:16`, byte `184,802,819`. The collector returns at rollout off
before reading settings (`modules/module-210100127.js:11`, bytes `210,103,675–210,103,907`).
Remote-tools stored acceptance remains host/terms bound:
`dV(r.data.hostname)===dV(e.hostname())&&r.data.terms===Nfn` at
`modules/module-206100477.js:11`, bytes `206,102,764–206,103,299`.
The final consent predicate has additional refusal paths; setting true alone supplies no consent.

The ordinary proxy-helper setup uses merged settings and tracks project/local provenance;
a separate setup selects policy/user sources. The executor at
`modules/module-184476632.js:15`, bytes `184,507,860–184,508,969`, includes
`if(r&&ue(n))return null;if(ue(n)&&!Ee()&&!n.proxyAuthHelperConfig.trustAccepted())`.
`Ee()` reads the host's noninteractive state (`modules/module-183949107.js:11`, byte
`184,042,131`). This is a nuanced execution path, with a 30-second command timeout and stdout
used for proxy authorization. No helper, credential, or authentication flow was exercised.

The unindexed table preserves namespace distinctions. The current public policy-precedence table
mentions `proxyAuthHelper` (`settings-reference.md:6013`). `showMessageTimestamps` occurs in
VS Code's **extension settings** (`vs-code.md:547`); the page distinguishes that namespace from
Claude Code settings at lines 525–527. `todoFeatureEnabled` has historical changelog presence
(pinned `CHANGELOG.md:7826`). Other unindexed names, including `idleCompaction`, had no exact
spelling occurrence in the checked public corpus. Exact spelling alone establishes a narrower
claim than a supported root-setting contract.

## Public contract and changes since the previous report

### Snapshot coverage

The public corpus contains **220 accepted English Markdown pages** from all 221 indexed English
Markdown URLs. One Claude Tag URL redirected to HTML and is excluded; there were no duplicate
accepted bodies. The pinned repository has **1,527 regular files**, all checked against the
untruncated Git tree by path and Git blob identity. **1,526** decode as UTF-8; the binary image is
excluded from lexical scanning. The schema and documentation index are also scanned.

| Public snapshot                                                                                                                 | SHA-256                                                            |
| ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| [Settings reference](https://code.claude.com/docs/en/settings-reference.md)                                                     | `336052874e175d06cc9749156c3addc4bb06beac85aa2487e59dd72948c79104` |
| [Environment reference](https://code.claude.com/docs/en/env-vars.md)                                                            | `b1a2650f714c2adbe1bba56947ddb33da396d5e43882d7017b01f21a9e891048` |
| [Settings guide](https://code.claude.com/docs/en/settings.md)                                                                   | `5c41248aea5764d32b14f90b77f4aa149486ca6bad6517c400398d9096b0ee25` |
| [Documentation index](https://code.claude.com/docs/llms.txt)                                                                    | `ad0cfca1fb4500ee99f0a94f9a5eff7a34835c02fdb75dce2b9ac3b47b27571c` |
| [Published schema](https://json.schemastore.org/claude-code-settings.json)                                                      | `6d4a6e3c7adedffce8079ccaef0a4bab5f5718b054421b4475c788a0ae4bedfe` |
| [Pinned repository archive](https://codeload.github.com/anthropics/claude-code/tar.gz/79babc372d64101f981bd2b52c3dbe588596dc56) | `72aac96d40e03ba122bc8904e070f0228fa89fed80c072f9957707eae234b446` |

The exact-name expression is `\bCLAUDE_CODE_[A-Z0-9_]+\b`. It produces:

```text
242 docs names + 137 repository names − 124 shared = 255
255 + 2 schema-only names = 257 public names
```

The schema adds `CLAUDE_CODE_ENABLE_APPEND_SUBAGENT_PROMPT` and `CLAUDE_CODE_TEAM_NAME`;
the index adds no new names. History, examples, host outputs, and schema mentions count as public
presence. Translations, nonindexed pages, the excluded HTML target, and private/server sources
remain outside this lexical public-absence claim.

### Canonical tables and schema

The settings index at `settings-reference.md:592–834` still has 243 rows:

```text
243 − 12 global-config rows − 60 non-global dotted rows + remote = 172 roots
172 − keybindingFlavor − taskOutputMaxChars = 170 currently behavioral indexed roots
```

`remote.defaultEnvironmentId` at line 738 supplies the nested-only root. The inactive rows say
“Deprecated and has no effect” at line 697 and “Removed in v2.1.277” at line 810.
The environment table spans `env-vars.md:138–525`: `525 − 138 + 1 = 388` rows.
This includes context outputs and obsolete names as well as ordinary user inputs.

The settings guide repeats “The schema can lag behind the newest CLI releases” at line 554.
The published schema matches the pinned revision byte for byte, has 142 root properties, and
sets root `"additionalProperties": true` at line **270**. Its intersection with the canonical
index is 132: `172 − 132 = 40` index-only roots; `142 − 132 = 10` schema-only roots.

Reconstruction from the .239 complete lists and .280/.288 listed changes reproduces the previous
172 roots and 378 environment rows. Both root-set differences are empty. The environment
comparison is `378 + 10 additions − 0 removals = 388`:

```text
CLAUDE_CODE_DISABLE_INLINE_SHELL_RM_PROMPT
CLAUDE_CODE_DISABLE_REFUSAL_FALLBACK
CLAUDE_CODE_EMIT_SESSION_STATE_EVENTS
CLAUDE_CODE_FORCE_TERMINAL_IMAGES
CLAUDE_CODE_GZIP_REQUEST_BODIES
CLAUDE_CODE_PLUGIN_DIR_WATCH
CLAUDE_CODE_TRANSCRIPT_LOCAL_GC
CLAUDE_CODE_WEB_SEARCH_REFILLS_PER_HOUR
CLAUDE_CODE_WORKER_CHECKIN_SCHEDULE
VERTEX_REGION_CLAUDE_HAIKU_5_5
```

Their rows are `env-vars.md:258,271,282,301,311,350,408,419–420,525`. Several already name
versions preceding 2.1.288, so this is a canonical-index change list. The safety-related opt-outs
are listed as observations, with no recommendation to enable them.

`CLAUDE_CODE_WORKER_CHECKIN_SCHEDULE` controls successive background-subagent check-in waits
only with `CLAUDE_AUTO_BACKGROUND_TASKS=1`; its row says unset means no reminders.
`CLAUDE_CODE_TRANSCRIPT_LOCAL_GC` concerns long `-p`/SDK transcripts and is launch-environment-only.
These documented controls have their own session and lifecycle scope.

### Relevant public release notes

The [pinned changelog](https://github.com/anthropics/claude-code/blob/79babc372d64101f981bd2b52c3dbe588596dc56/CHANGELOG.md)
provides the following public claims. They were not all exercised in the fixture:

| Location           | Short exact excerpt / relevance                                                                                                                         |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `CHANGELOG.md:8`   | “Fixed Claude sometimes treating its own last actions before a context compaction as done after it”                                                     |
| `CHANGELOG.md:13`  | “Fixed Claude being told to continue or message subagents with `SendMessage`”                                                                           |
| `CHANGELOG.md:65`  | “Added an `effort` parameter to the Agent tool”                                                                                                         |
| `CHANGELOG.md:66`  | “Added `CLAUDE_CODE_OVERLOADED_RETRY_BASE_DELAY_MS` environment variable” — public release-history presence, with no current canonical environment row. |
| `CHANGELOG.md:69`  | “Added workflow agents to the `agent.spawn` mod hook”                                                                                                   |
| `CHANGELOG.md:126` | “`<system-reminder>` tags written in a hook's output are escaped before they reach Claude”                                                              |
| `CHANGELOG.md:182` | “Fixed scheduled tasks (`/loop` with an interval, reminders) silently not coming back on resume once the conversation was compacted”                    |

The complete old 249-name public union and 320-name hidden set were unavailable. Selected prior
hidden prompt names remain absent in this corpus, but a complete historical public-status transition
list cannot be reconstructed from the earlier aggregate counts alone.

## Runtime isolation, counts, and retained evidence

The final matrix has **33 initial-request cases + 11 three-request cases = 44 cases**.
Thus `33 + 11 × 3 = 66` Messages requests. Two Read tool uses per multi-turn case yield
`11 × 2 = 22` non-error tool results; the second read can use the unchanged-file optimization.
**84 content assertions passed.** A separate one-case smoke run and an initial 42-case pass
preceded the final matrix and are excluded from these totals.

Each child receives its own temporary HOME, config, cache, working, and temporary directories,
plus an explicit environment allowlist. The fixed server binds only to `127.0.0.1` and uses a
non-secret protocol-fixture API-key value. No live model or account credentials participate.
Unexpected routes and excess turns fail the fixture; final protocol errors were empty.

The CLI uses `-p`, the case's model and JSON settings, `--setting-sources ""`, empty strict MCP
configuration, `--no-session-persistence`, `--no-chrome`, `--output-format json`, and
`--dangerously-skip-permissions` inside the OS sandbox. The only model-directed tool calls are
predetermined Read calls on the task-owned fixture. The server returns fixed streaming Messages
responses with usage values of 100 input and 10 output tokens, then a final text response.
The two seconds cases additionally pause the second response by a configured 2.3 seconds.

The macOS sandbox denies external networking, real-home paths under `/Users`, system Claude Code
managed-settings paths, `security`/`defaults` execution, and keychain/security service lookups.
Before running Claude, a loopback health request succeeded, external HTTPS with a fixed DNS mapping
failed, and a read of the repository LICENSE canary failed with `Operation not permitted`.
Other filesystem paths and localhost ports remain accessible. These checks establish the recorded
fixture boundary; broader isolation was outside their scope.

Feature-flag fetching, telemetry, error reporting, auto-updates, IDE auto-connection, and auto-memory
are disabled in the test environment. Compiled model defaults still participate. The lean cases
are isolated representative configurations, with no personal hooks/plugins or saved account state.

The companion inventory retains the sorted current sets, first direct-read witnesses, root
provenance/classifications, public fetch identities, and exact case/assertion outcomes. Full vendor
source, raw API captures, and complete documentation bodies remain temporary audit material.
Historical public-absence replay requires matching snapshots; current rolling URLs may change.
Source/bytecode equivalence outside the exercised paths, arbitrary dynamic readers, interactive UI,
actual compaction, live cache behavior/billing, helper execution, consent, remote services, and model
compliance remain unverified.

## Verification

An independent pass recomputed the environment and root-setting sets, checked all 2,290 extracted
modules against the pinned binary, and replayed all 84 final request assertions. It confirmed
44 cases, 66 requests, 22 non-error Read results, and empty protocol errors. The retained inventory's
1,228 typed declaration/constructor spans, 777 other source witnesses, and 307 public citation
excerpts also matched their sources. Expanded shadowing/write-target fixtures passed with the
complete corpus read records unchanged. Markdown structure, JSON parsing, formatting, and local
links were checked before submission.
