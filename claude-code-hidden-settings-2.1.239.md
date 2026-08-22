# Claude Code hidden settings analysis — v2.1.239

**Snapshot:** 2026-08-22. **Scope:** public Anthropic documentation, the public
`anthropics/claude-code` repository, and the JSON schema linked by Anthropic's settings guide.
No locally installed executable or local `settings.json` was inspected.

## Bottom line

At this revision, the public documentation is unusually broad:

- The settings reference indexes **145 canonical top-level behavioral keys** accepted in a
  `settings.json`-family file. It also documents two accepted marketplace aliases and the optional
  `$schema` editor annotation.
- The environment-variable reference has **339 named table rows**. Some rows are exported runtime
  context rather than user inputs, and seven rows are explicitly deprecated, removed, or no-ops.
- Standard OpenTelemetry, provider, and runtime-injected variables are documented on dedicated
  pages outside that 339-row table.
- A name appearing only in `CHANGELOG.md` is release-history evidence, not a current support
  contract. Those names are separated below.

## Revision and evidence rules

- Current public release/tag: [`v2.1.239`](https://github.com/anthropics/claude-code/releases/tag/v2.1.239),
  published 2026-08-21 19:54:23 UTC.
- Pinned repository commit: [`16440d0f6ee8c47f34169687044b89eafa8b0f8d`](https://github.com/anthropics/claude-code/commit/16440d0f6ee8c47f34169687044b89eafa8b0f8d),
  2026-08-21 19:54:17 UTC. The pinned changelog is
  [`CHANGELOG.md`](https://github.com/anthropics/claude-code/blob/16440d0f6ee8c47f34169687044b89eafa8b0f8d/CHANGELOG.md).
- Canonical settings source:
  [`settings-reference.md`](https://code.claude.com/docs/en/settings-reference.md), sitemap
  `lastmod` 2026-08-22 03:28:33.639 UTC. The settings guide is
  [`settings.md`](https://code.claude.com/docs/en/settings.md), `lastmod`
  2026-08-21 23:28:56.556 UTC.
- Canonical environment source:
  [`env-vars.md`](https://code.claude.com/docs/en/env-vars.md), sitemap `lastmod`
  2026-08-22 03:01:14.848 UTC.
- The settings guide links the published
  [Claude Code JSON schema](https://json.schemastore.org/claude-code-settings.json), but explicitly
  warns that it can lag the CLI. Its source revision was
  [`d2cbdcde9855c1bf9ea99c336163cc6c93753e39`](https://github.com/SchemaStore/schemastore/commit/d2cbdcde9855c1bf9ea99c336163cc6c93753e39)
  (2026-08-03), not an `anthropics/claude-code` commit.
- The pinned `anthropics/claude-code` tree has no settings schema. Its
  [`examples/settings/README.md`](https://github.com/anthropics/claude-code/blob/16440d0f6ee8c47f34169687044b89eafa8b0f8d/examples/settings/README.md)
  calls those examples “community-maintained” and possibly unsupported or incorrect. They are
  corroboration only, not the canonical inventory.

“Documented/supported” below means the current reference gives the name and behavior. It does not
mean stable forever: entries explicitly labeled experimental, beta, deprecated, or removed retain
that label.

## Top-level `settings.json` keys

The following are the **145 canonical root keys** in the current “All settings” index. Nested keys
such as `permissions.allow`, `sandbox.network.allowedDomains`, and `worktree.baseRef` are not counted
again.

- **Model and responses (15):** `advisorModel`, `alwaysThinkingEnabled`, `availableModels`,
  `effortLevel`, `enforceAvailableModels`, `fallbackModel`, `fastMode`, `fastModePerSessionOptIn`,
  `language`, `model`, `modelOverrides`, `outputStyle`, `showThinkingSummaries`,
  `switchModelsOnFlag`, `ultracode`.
- **Agents, sessions, worktrees (7):** `agent`, `crossSessionInbound`, `disableAgentView`,
  `isolatePeerMachines`, `processWrapper`, `teammateMode`, `worktree`.
- **Remote, desktop, notifications (11):** `agentPushNotifEnabled`, `awaySummaryEnabled`,
  `disableArtifact`, `disableDeepLinkRegistration`, `disableRemoteControl`, `enableArtifact`,
  `inputNeededNotifEnabled`, `preferredNotifChannel`, `remoteControlAtStartup`, `sshConfigs`,
  `sshHostAllowlist`.
- **MCP (8):** `allowAllClaudeAiMcps`, `allowedMcpServers`, `allowManagedMcpServersOnly`,
  `deniedMcpServers`, `disableClaudeAiConnectors`, `disabledMcpjsonServers`,
  `enableAllProjectMcpServers`, `enabledMcpjsonServers`.
- **Plugins and skills (15):** `allowedChannelPlugins`, `blockedMarketplaces`, `channelsEnabled`,
  `disableBundledSkills`, `disableCommandPluginSources`, `disableSkillShellExecution`,
  `enabledPlugins`, `extraKnownMarketplaces`, `pluginConfigs`, `pluginSuggestionMarketplaces`,
  `pluginTrustMessage`, `skillOverrides`, `strictKnownMarketplaces`,
  `strictPluginOnlyCustomization`, `syncClaudeAiSkills`.
- **Hooks and automation (9):** `allowedHttpHookUrls`, `allowManagedHooksOnly`, `disableAllHooks`,
  `disableWorkflows`, `enableWorkflows`, `hooks`, `httpHookAllowedEnvVars`,
  `workflowKeywordTriggerEnabled`, `workflowSizeGuideline`.
- **Permissions (7):** `allowManagedPermissionRulesOnly`, `autoMode`, `disableAutoMode`,
  `permissions`, `skipAutoPermissionPrompt`, `skipDangerousModePermissionPrompt`,
  `useAutoModeDuringPlan`.
- **Authentication and providers (8):** `apiKeyHelper`, `awsAuthRefresh`, `awsCredentialExport`,
  `forceLoginGatewayUrl`, `forceLoginMethod`, `forceLoginOrgUUID`, `gcpAuthRefresh`,
  `otelHeadersHelper`.
- **Interface and terminal (34):** `askUserQuestionTimeout`, `autoScrollEnabled`, `axScreenReader`,
  `companyAnnouncements`, `defaultShell`, `dialogExpiry`, `editorMode`,
  `emojiCompletionEnabled`, `fileSuggestion`, `footerLinksRegexes`, `keybindingFlavor`,
  `prefersReducedMotion`, `promptSuggestionEnabled`, `respectGitignore`, `respondToBashCommands`,
  `showClearContextOnPlanAccept`, `showTurnDuration`, `spellcheck`, `spinnerTipsEnabled`,
  `spinnerTipsOverride`, `spinnerVerbs`, `statusLine`, `subagentStatusLine`,
  `syntaxHighlightingDisabled`, `terminalProgressBarEnabled`, `terminalTitleFromRename`, `theme`,
  `tui`, `verbose`, `viewMode`, `vimInsertModeRemaps`, `voice`, `voiceEnabled`,
  `wheelScrollAccelerationEnabled`.
- **Git and attribution (4):** `attribution`, `includeCoAuthoredBy`, `includeGitInstructions`,
  `prUrlTemplate`.
- **Memory and context (11):** `autoCompactEnabled`, `autoCompactWindow`, `autoMemoryDirectory`,
  `autoMemoryEnabled`, `claudeMd`, `claudeMdExcludes`, `env`, `fileCheckpointingEnabled`,
  `plansDirectory`, `skillListingBudgetFraction`, `skillListingMaxDescChars`.
- **Updates and versioning (4):** `autoUpdatesChannel`, `minimumVersion`,
  `requiredMaximumVersion`, `requiredMinimumVersion`.
- **Tools (3):** `browserExternalPageTools`, `disableBrowserExternalNavigation`,
  `disableMobileSimulatorTools`.
- **Privacy and telemetry (3):** `cleanupPeriodDays`, `feedbackSurveyRate`,
  `skipWebFetchPreflight`.
- **Enterprise and managed (5):** `disableSideloadFlags`, `forceRemoteSettingsRefresh`,
  `parentSettingsBehavior`, `policyHelper`, `wslInheritsWindowsSettings`.
- **Sandbox (1):** `sandbox`.

### Scope qualifications

Unless listed here, the reference says **Any file** (user, shared project, project-local, or managed):

- **Managed only:** `allowAllClaudeAiMcps`, `allowedChannelPlugins`, `allowManagedHooksOnly`,
  `allowManagedMcpServersOnly`, `allowManagedPermissionRulesOnly`, `blockedMarketplaces`,
  `browserExternalPageTools`, `channelsEnabled`, `claudeMd`, `disableBrowserExternalNavigation`,
  `disableCommandPluginSources`, `disableMobileSimulatorTools`, `disableSideloadFlags`,
  `forceLoginGatewayUrl`, `forceRemoteSettingsRefresh`, `parentSettingsBehavior`,
  `pluginSuggestionMarketplaces`, `pluginTrustMessage`, `policyHelper`, `requiredMaximumVersion`,
  `requiredMinimumVersion`, `sshHostAllowlist`, `strictKnownMarketplaces`,
  `strictPluginOnlyCustomization`, `wslInheritsWindowsSettings`.
- **User or managed:** `askUserQuestionTimeout`, `autoMode`, `dialogExpiry`, `enableArtifact`,
  `footerLinksRegexes`, `pluginConfigs`, `processWrapper`, `skipAutoPermissionPrompt`, `spellcheck`,
  `sshConfigs`, `vimInsertModeRemaps`.
- **User, local, or managed (not shared project):** `skipDangerousModePermissionPrompt`,
  `syncClaudeAiSkills`, `useAutoModeDuringPlan`.

Two additional spellings are explicitly accepted on v2.1.232+: `additionalMarketplaces` aliases
`extraKnownMarketplaces`, and `allowedMarketplaces` aliases `strictKnownMarketplaces`. The canonical
spelling wins when both are present, and older clients ignore the aliases. `$schema` is documented as
an optional editor annotation, not a behavioral key. `includeCoAuthoredBy` is deprecated in favor of
`attribution`.

The six keys `autoConnectIde`, `autoInstallIdeExtension`, `diffTool`, `externalEditorContext`,
`permissionExplainerEnabled`, and removed `teammateDefaultModel` belong to `~/.claude.json` global
config; the current reference says Claude Code ignores them in `settings.json`.

### Prompt, tools, permissions, agents, and context

- **Prompt/instructions:** `outputStyle`, `language`, `includeGitInstructions`, managed `claudeMd`,
  `claudeMdExcludes`, and `agent` can alter what is loaded or which agent prompt is used. `model`,
  `fallbackModel`, `modelOverrides`, `effortLevel`, `alwaysThinkingEnabled`, and
  `showThinkingSummaries` affect request/model behavior rather than adding arbitrary prompt text.
- **Tools:** `permissions`, `sandbox`, the MCP keys, plugin/skill keys, `hooks`,
  `disableBundledSkills`, `disableSkillShellExecution`, workflow keys, and the three desktop-tool
  policy keys control availability, execution, or interception.
- **Permissions:** `permissions` owns `allow`, `ask`, `deny`, `additionalDirectories`, `defaultMode`,
  and `disableBypassPermissionsMode`; `autoMode`, `disableAutoMode`, `useAutoModeDuringPlan`, and the
  managed-only lock keys govern higher-level policy. Deny rules apply in every permission mode.
- **Agents:** `agent` selects a named agent and applies its prompt/tools/model;
  `disableAgentView`, `teammateMode`, `crossSessionInbound`, `isolatePeerMachines`, `worktree`,
  `subagentStatusLine`, workflow keys, and `strictPluginOnlyCustomization` affect spawning,
  coordination, isolation, or customization sources.
- **Context:** `autoCompactEnabled`, `autoCompactWindow`, `autoMemoryEnabled`,
  `autoMemoryDirectory`, `claudeMd`, `claudeMdExcludes`, `skillListingBudgetFraction`, and
  `skillListingMaxDescChars` directly govern loaded context or compaction. `plansDirectory` and
  `fileCheckpointingEnabled` govern persisted working state.

### Schema reconciliation

The docs-linked schema has **142 root properties**, `additionalProperties: true`, and is older than
these docs. Compared with the canonical current settings index:

- **Current docs but absent from schema (14):** `autoCompactWindow`, `crossSessionInbound`,
  `dialogExpiry`, `disableCommandPluginSources`, `enableWorkflows`, `isolatePeerMachines`,
  `keybindingFlavor`, `promptSuggestionEnabled`, `skipAutoPermissionPrompt`, `spellcheck`,
  `switchModelsOnFlag`, `syncClaudeAiSkills`, `terminalTitleFromRename`, `ultracode`.
- **Schema-only behavioral names absent from the current reference (4):** `managedMcpServers`,
  `requireCoworkFullVmSandbox`, `skippedMarketplaces`, `skippedPlugins`. Treat these as stale or
  non-public, not as documented support.
- The remaining schema-only properties are `$schema` and the six global-config names above. The
  current reference, rather than schema presence, determines whether a key belongs in
  `settings.json`.

## Canonical environment-variable inventory

These are the **339 rows** in the current [`env-vars.md`](https://code.claude.com/docs/en/env-vars.md)
Variables table. Grouping is only for readability; spelling is exact.

### `ANTHROPIC_*` (46)

```text
ANTHROPIC_API_KEY, ANTHROPIC_AUTH_TOKEN, ANTHROPIC_AWS_API_KEY,
ANTHROPIC_AWS_BASE_URL, ANTHROPIC_AWS_WORKSPACE_ID, ANTHROPIC_BASE_URL,
ANTHROPIC_BEDROCK_BASE_URL, ANTHROPIC_BEDROCK_MANTLE_BASE_URL,
ANTHROPIC_BEDROCK_REGION_PREFIX, ANTHROPIC_BEDROCK_SERVICE_TIER, ANTHROPIC_BETAS,
ANTHROPIC_CUSTOM_HEADERS, ANTHROPIC_CUSTOM_MODEL_OPTION,
ANTHROPIC_CUSTOM_MODEL_OPTION_DESCRIPTION, ANTHROPIC_CUSTOM_MODEL_OPTION_NAME,
ANTHROPIC_CUSTOM_MODEL_OPTION_SUPPORTED_CAPABILITIES, ANTHROPIC_DEFAULT_FABLE_MODEL,
ANTHROPIC_DEFAULT_FABLE_MODEL_DESCRIPTION, ANTHROPIC_DEFAULT_FABLE_MODEL_NAME,
ANTHROPIC_DEFAULT_FABLE_MODEL_SUPPORTED_CAPABILITIES, ANTHROPIC_DEFAULT_HAIKU_MODEL,
ANTHROPIC_DEFAULT_HAIKU_MODEL_DESCRIPTION, ANTHROPIC_DEFAULT_HAIKU_MODEL_NAME,
ANTHROPIC_DEFAULT_HAIKU_MODEL_SUPPORTED_CAPABILITIES, ANTHROPIC_DEFAULT_MODEL,
ANTHROPIC_DEFAULT_OPUS_MODEL, ANTHROPIC_DEFAULT_OPUS_MODEL_DESCRIPTION,
ANTHROPIC_DEFAULT_OPUS_MODEL_NAME, ANTHROPIC_DEFAULT_OPUS_MODEL_SUPPORTED_CAPABILITIES,
ANTHROPIC_DEFAULT_SONNET_MODEL, ANTHROPIC_DEFAULT_SONNET_MODEL_DESCRIPTION,
ANTHROPIC_DEFAULT_SONNET_MODEL_NAME, ANTHROPIC_DEFAULT_SONNET_MODEL_SUPPORTED_CAPABILITIES,
ANTHROPIC_FEDERATION_RULE_ID, ANTHROPIC_FOUNDRY_API_KEY, ANTHROPIC_FOUNDRY_AUTH_TOKEN,
ANTHROPIC_FOUNDRY_BASE_URL, ANTHROPIC_FOUNDRY_RESOURCE, ANTHROPIC_MODEL,
ANTHROPIC_ORGANIZATION_ID, ANTHROPIC_PROFILE, ANTHROPIC_SMALL_FAST_MODEL,
ANTHROPIC_SMALL_FAST_MODEL_AWS_REGION, ANTHROPIC_VERTEX_BASE_URL,
ANTHROPIC_VERTEX_PROJECT_ID, ANTHROPIC_WORKSPACE_ID
```

### API, AWS, Bash, and runtime (7)

```text
API_FORCE_IDLE_TIMEOUT, API_TIMEOUT_MS, AWS_BEARER_TOKEN_BEDROCK,
BASH_DEFAULT_TIMEOUT_MS, BASH_MAX_OUTPUT_LENGTH, BASH_MAX_TIMEOUT_MS, CCR_FORCE_BUNDLE
```

### Other standard/runtime names (12)

```text
CLAUDECODE, DEBUG, DO_NOT_TRACK, HTTP_PROXY, HTTPS_PROXY, MAX_MCP_OUTPUT_TOKENS,
MAX_STRUCTURED_OUTPUT_RETRIES, MAX_THINKING_TOKENS, NO_PROXY,
SLASH_COMMAND_TOOL_CHAR_BUDGET, TASK_MAX_OUTPUT_LENGTH, USE_BUILTIN_RIPGREP
```

### Other `CLAUDE_*` (24)

```text
CLAUDE_AFK_COUNTDOWN_MS, CLAUDE_AFK_TIMEOUT_MS, CLAUDE_AGENT_SDK_DISABLE_BUILTIN_AGENTS,
CLAUDE_AGENT_SDK_MCP_NO_PREFIX, CLAUDE_ASYNC_AGENT_STALL_TIMEOUT_MS,
CLAUDE_AUTOCOMPACT_PCT_OVERRIDE, CLAUDE_AUTO_BACKGROUND_TASKS, CLAUDE_AX_PREPARK_MS,
CLAUDE_AX_SCREEN_READER, CLAUDE_AX_STARTUP_QUIET_MS,
CLAUDE_BASH_MAINTAIN_PROJECT_WORKING_DIR, CLAUDE_BYTE_STREAM_IDLE_TIMEOUT_MS,
CLAUDE_CLIENT_PRESENCE_FILE, CLAUDE_CONFIG_DIR, CLAUDE_DISABLE_ADOPT, CLAUDE_EFFORT,
CLAUDE_ENABLE_BYTE_WATCHDOG, CLAUDE_ENABLE_BYTE_WATCHDOG_BEDROCK,
CLAUDE_ENABLE_STREAM_WATCHDOG, CLAUDE_ENV_FILE, CLAUDE_PID,
CLAUDE_REMOTE_CONTROL_SESSION_NAME_PREFIX, CLAUDE_STREAM_IDLE_TIMEOUT_MS,
CLAUDE_SUBAGENT_BG_SHELL_MAX_MS
```

### `CLAUDE_CODE_*` (178)

```text
CLAUDE_CODE_ACCESSIBILITY, CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD,
CLAUDE_CODE_ALT_SCREEN_FULL_REPAINT, CLAUDE_CODE_ALWAYS_ENABLE_EFFORT,
CLAUDE_CODE_API_KEY_HELPER_TTL_MS, CLAUDE_CODE_ARTIFACT_AUTO_OPEN,
CLAUDE_CODE_ARTIFACT_COMMENTS, CLAUDE_CODE_ARTIFACT_COMMENTS_AUTOREACT,
CLAUDE_CODE_ATTRIBUTION_HEADER, CLAUDE_CODE_AUTO_COMPACT_WINDOW,
CLAUDE_CODE_AUTO_CONNECT_IDE, CLAUDE_CODE_AWS_CHAIN_RESOLVE_TIMEOUT_MS,
CLAUDE_CODE_BRIDGE_SESSION_ID, CLAUDE_CODE_BS_AS_CTRL_BACKSPACE, CLAUDE_CODE_CERT_STORE,
CLAUDE_CODE_CHILD_SESSION, CLAUDE_CODE_CLIENT_CERT, CLAUDE_CODE_CLIENT_KEY,
CLAUDE_CODE_CLIENT_KEY_PASSPHRASE, CLAUDE_CODE_CONNECT_TIMEOUT_MS,
CLAUDE_CODE_DEBUG_LOGS_DIR, CLAUDE_CODE_DEBUG_LOG_LEVEL, CLAUDE_CODE_DISABLE_1M_CONTEXT,
CLAUDE_CODE_DISABLE_ADAPTIVE_THINKING, CLAUDE_CODE_DISABLE_ADMIN_ENV_UNION,
CLAUDE_CODE_DISABLE_ADVISOR_TOOL, CLAUDE_CODE_DISABLE_AGENT_VIEW,
CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN, CLAUDE_CODE_DISABLE_ARTIFACT,
CLAUDE_CODE_DISABLE_ATTACHMENTS, CLAUDE_CODE_DISABLE_AUTO_MEMORY,
CLAUDE_CODE_DISABLE_BACKGROUND_TASKS, CLAUDE_CODE_DISABLE_BEDROCK_CONTENT_TYPE_GUARD,
CLAUDE_CODE_DISABLE_BG_EXIT_HANDOFF, CLAUDE_CODE_DISABLE_BG_SHELL_PRESSURE_REAP,
CLAUDE_CODE_DISABLE_BUNDLED_SKILLS, CLAUDE_CODE_DISABLE_CLAUDE_MDS,
CLAUDE_CODE_DISABLE_CRON, CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS,
CLAUDE_CODE_DISABLE_EXPLORE_PLAN_AGENTS, CLAUDE_CODE_DISABLE_FAST_MODE,
CLAUDE_CODE_DISABLE_FEEDBACK_SURVEY, CLAUDE_CODE_DISABLE_FILE_CHECKPOINTING,
CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS, CLAUDE_CODE_DISABLE_LEGACY_MODEL_REMAP,
CLAUDE_CODE_DISABLE_MOUSE, CLAUDE_CODE_DISABLE_MOUSE_CLICKS,
CLAUDE_CODE_DISABLE_MTLS_RELOAD_ON_STALE_CONNECTION,
CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC, CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK,
CLAUDE_CODE_DISABLE_NOTIFICATION_PRESENCE_CHECK,
CLAUDE_CODE_DISABLE_OFFICIAL_MARKETPLACE_AUTOINSTALL,
CLAUDE_CODE_DISABLE_PERMISSION_PROMPT_NOTIFY_HOOKS, CLAUDE_CODE_DISABLE_POLICY_SKILLS,
CLAUDE_CODE_DISABLE_TERMINAL_TITLE, CLAUDE_CODE_DISABLE_THINKING,
CLAUDE_CODE_DISABLE_UNKNOWN_MODEL_WINDOW_ENFORCEMENT,
CLAUDE_CODE_DISABLE_VIRTUAL_SCROLL, CLAUDE_CODE_DISABLE_WORKFLOWS,
CLAUDE_CODE_EFFORT_LEVEL, CLAUDE_CODE_ENABLE_APPEND_SUBAGENT_PROMPT,
CLAUDE_CODE_ENABLE_AUTO_MODE, CLAUDE_CODE_ENABLE_AWAY_SUMMARY,
CLAUDE_CODE_ENABLE_BACKGROUND_PLUGIN_REFRESH, CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL,
CLAUDE_CODE_ENABLE_FINE_GRAINED_TOOL_STREAMING,
CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY, CLAUDE_CODE_ENABLE_OPUS_4_7_FAST_MODE,
CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION, CLAUDE_CODE_ENABLE_TASKS,
CLAUDE_CODE_ENABLE_TELEMETRY, CLAUDE_CODE_ENABLE_TODO_TOOLS,
CLAUDE_CODE_EXIT_AFTER_STOP_DELAY, CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS,
CLAUDE_CODE_EXTRA_BODY, CLAUDE_CODE_FILE_READ_MAX_OUTPUT_TOKENS,
CLAUDE_CODE_FORCE_SESSION_PERSISTENCE, CLAUDE_CODE_FORCE_STRIKETHROUGH,
CLAUDE_CODE_FORCE_SYNC_OUTPUT, CLAUDE_CODE_FORK_SUBAGENT,
CLAUDE_CODE_FORWARD_SUBAGENT_TEXT, CLAUDE_CODE_GIT_BASH_PATH, CLAUDE_CODE_GLOB_HIDDEN,
CLAUDE_CODE_GLOB_NO_IGNORE, CLAUDE_CODE_GLOB_TIMEOUT_SECONDS,
CLAUDE_CODE_GOAL_CHECKIN_MINUTES, CLAUDE_CODE_HIDE_CWD, CLAUDE_CODE_IDE_HOST_OVERRIDE,
CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL, CLAUDE_CODE_IDE_SKIP_VALID_CHECK,
CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS, CLAUDE_CODE_MAX_CONTEXT_TOKENS,
CLAUDE_CODE_MAX_OUTPUT_TOKENS, CLAUDE_CODE_MAX_RETRIES,
CLAUDE_CODE_MAX_SUBAGENTS_PER_SESSION, CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH,
CLAUDE_CODE_MAX_TOOL_USE_CONCURRENCY, CLAUDE_CODE_MAX_TURNS,
CLAUDE_CODE_MAX_WEB_SEARCHES_PER_SESSION, CLAUDE_CODE_MCP_ALLOWLIST_ENV,
CLAUDE_CODE_MCP_AUTO_BACKGROUND_MS, CLAUDE_CODE_MCP_TOOL_IDLE_TIMEOUT,
CLAUDE_CODE_MESSAGING_SOCKET, CLAUDE_CODE_MESSAGING_TOKEN, CLAUDE_CODE_NATIVE_CURSOR,
CLAUDE_CODE_NEW_INIT, CLAUDE_CODE_NO_FLICKER, CLAUDE_CODE_OAUTH_REFRESH_TOKEN,
CLAUDE_CODE_OAUTH_SCOPES, CLAUDE_CODE_OAUTH_TOKEN,
CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE, CLAUDE_CODE_OTEL_CONTENT_MAX_LENGTH,
CLAUDE_CODE_OTEL_DIAG_STDERR, CLAUDE_CODE_OTEL_FLUSH_TIMEOUT_MS,
CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS, CLAUDE_CODE_OTEL_SHUTDOWN_TIMEOUT_MS,
CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE, CLAUDE_CODE_PERFORCE_MODE,
CLAUDE_CODE_PLUGIN_CACHE_DIR, CLAUDE_CODE_PLUGIN_GIT_TIMEOUT_MS,
CLAUDE_CODE_PLUGIN_KEEP_MARKETPLACE_ON_FAILURE, CLAUDE_CODE_PLUGIN_PREFER_HTTPS,
CLAUDE_CODE_PLUGIN_SEED_DIR, CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY,
CLAUDE_CODE_PRINT_BG_WAIT_CEILING_MS, CLAUDE_CODE_PROCESS_WRAPPER,
CLAUDE_CODE_PROJECT_DIR_NAME, CLAUDE_CODE_PROPAGATE_TRACEPARENT,
CLAUDE_CODE_PROVIDER_MANAGED_BY_HOST, CLAUDE_CODE_PROXY_RESOLVES_HOSTS,
CLAUDE_CODE_REMOTE, CLAUDE_CODE_REMOTE_SESSION_ID, CLAUDE_CODE_RESUME_INTERRUPTED_TURN,
CLAUDE_CODE_RESUME_INTERRUPTED_TURN_MAX_AGE_MS, CLAUDE_CODE_RESUME_PROMPT,
CLAUDE_CODE_RETRY_WATCHDOG, CLAUDE_CODE_SAFE_MODE, CLAUDE_CODE_SCRIPT_CAPS,
CLAUDE_CODE_SCROLL_SPEED, CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS,
CLAUDE_CODE_SESSION_ID, CLAUDE_CODE_SHELL, CLAUDE_CODE_SHELL_PREFIX, CLAUDE_CODE_SIMPLE,
CLAUDE_CODE_SIMPLE_SYSTEM_PROMPT, CLAUDE_CODE_SKIP_ANTHROPIC_AWS_AUTH,
CLAUDE_CODE_SKIP_AWS_CRED_CACHE, CLAUDE_CODE_SKIP_BEDROCK_AUTH,
CLAUDE_CODE_SKIP_FAST_MODE_NETWORK_ERRORS, CLAUDE_CODE_SKIP_FAST_MODE_ORG_CHECK,
CLAUDE_CODE_SKIP_FOUNDRY_AUTH, CLAUDE_CODE_SKIP_MANTLE_AUTH,
CLAUDE_CODE_SKIP_PROMPT_HISTORY, CLAUDE_CODE_SKIP_VERTEX_AUTH,
CLAUDE_CODE_STOP_HOOK_BLOCK_CAP, CLAUDE_CODE_SUBAGENT_MODEL,
CLAUDE_CODE_SUBPROCESS_ENV_SCRUB, CLAUDE_CODE_SYNC_PLUGIN_INSTALL,
CLAUDE_CODE_SYNC_PLUGIN_INSTALL_TIMEOUT_MS, CLAUDE_CODE_SYNC_SKILLS,
CLAUDE_CODE_SYNC_SKILLS_INSTALL_TIMEOUT_MS, CLAUDE_CODE_SYNC_SKILLS_WAIT_TIMEOUT_MS,
CLAUDE_CODE_SYNTAX_HIGHLIGHT, CLAUDE_CODE_TASK_LIST_ID,
CLAUDE_CODE_TEAM_TEARDOWN_PARK_TIMEOUT_MS, CLAUDE_CODE_TMPDIR,
CLAUDE_CODE_TMUX_TRUECOLOR, CLAUDE_CODE_TOOL_MEMORY_LIMIT,
CLAUDE_CODE_USER_DIALOG_TIMEOUT_MS, CLAUDE_CODE_USE_ANTHROPIC_AWS,
CLAUDE_CODE_USE_BEDROCK, CLAUDE_CODE_USE_FOUNDRY, CLAUDE_CODE_USE_MANTLE,
CLAUDE_CODE_USE_NATIVE_FILE_SEARCH, CLAUDE_CODE_USE_POWERSHELL_TOOL,
CLAUDE_CODE_USE_VERTEX, CLAUDE_CODE_WEBFETCH_CACHE_TTL_MS,
CLAUDE_CODE_WORKFLOW_PREFIX_STAGGER_MS
```

### `DISABLE_*` (22)

```text
DISABLE_AUTOUPDATER, DISABLE_AUTO_COMPACT, DISABLE_COMPACT, DISABLE_COST_WARNINGS,
DISABLE_DOCTOR_COMMAND, DISABLE_ERROR_REPORTING, DISABLE_EXTRA_USAGE_COMMAND,
DISABLE_FEEDBACK_COMMAND, DISABLE_GROWTHBOOK, DISABLE_INSTALLATION_CHECKS,
DISABLE_INSTALL_GITHUB_APP_COMMAND, DISABLE_INTERLEAVED_THINKING, DISABLE_LOGIN_COMMAND,
DISABLE_LOGOUT_COMMAND, DISABLE_PROMPT_CACHING, DISABLE_PROMPT_CACHING_FABLE,
DISABLE_PROMPT_CACHING_HAIKU, DISABLE_PROMPT_CACHING_OPUS,
DISABLE_PROMPT_CACHING_SONNET, DISABLE_TELEMETRY, DISABLE_UPDATES,
DISABLE_UPGRADE_COMMAND
```

### Enable/force/feature controls (9)

```text
ENABLE_CLAUDEAI_MCP_SERVERS, ENABLE_PROMPT_CACHING_1H,
ENABLE_PROMPT_CACHING_1H_BEDROCK, ENABLE_TOOL_SEARCH, FALLBACK_FOR_ALL_PRIMARY_MODELS,
FORCE_AUTOUPDATE_PLUGINS, FORCE_HYPERLINK, FORCE_PROMPT_CACHING_5M, IS_DEMO
```

### `MCP_*` (14)

```text
MCP_CLIENT_SECRET, MCP_CONNECTION_NONBLOCKING, MCP_CONNECT_TIMEOUT_MS,
MCP_DISCOVERY_CACHE, MCP_DISCOVERY_CACHE_MAX_STALE_S, MCP_DISCOVERY_CACHE_STRIKES,
MCP_DISCOVERY_CACHE_TTL_S, MCP_OAUTH_CALLBACK_PORT, MCP_PROTOCOL_NEGOTIATION,
MCP_REMOTE_SERVER_CONNECTION_BATCH_SIZE, MCP_SDK_GENERATION,
MCP_SERVER_CONNECTION_BATCH_SIZE, MCP_TIMEOUT, MCP_TOOL_TIMEOUT
```

### Claude-specific `OTEL_*` (11)

```text
OTEL_ATTRIBUTE_VALUE_LENGTH_LIMIT, OTEL_LOG_ASSISTANT_RESPONSES,
OTEL_LOG_RAW_API_BODIES, OTEL_LOG_TOOL_CONTENT, OTEL_LOG_TOOL_DETAILS,
OTEL_LOG_USER_PROMPTS, OTEL_METRICS_INCLUDE_ACCOUNT_UUID,
OTEL_METRICS_INCLUDE_ENTRYPOINT, OTEL_METRICS_INCLUDE_RESOURCE_ATTRIBUTES,
OTEL_METRICS_INCLUDE_SESSION_ID, OTEL_METRICS_INCLUDE_VERSION
```

### `VERTEX_REGION_*` (16)

```text
VERTEX_REGION_CLAUDE_3_5_HAIKU, VERTEX_REGION_CLAUDE_3_5_SONNET,
VERTEX_REGION_CLAUDE_3_7_SONNET, VERTEX_REGION_CLAUDE_4_0_OPUS,
VERTEX_REGION_CLAUDE_4_0_SONNET, VERTEX_REGION_CLAUDE_4_1_OPUS,
VERTEX_REGION_CLAUDE_4_5_OPUS, VERTEX_REGION_CLAUDE_4_5_SONNET,
VERTEX_REGION_CLAUDE_4_6_OPUS, VERTEX_REGION_CLAUDE_4_6_SONNET,
VERTEX_REGION_CLAUDE_4_7_OPUS, VERTEX_REGION_CLAUDE_4_8_OPUS,
VERTEX_REGION_CLAUDE_5_OPUS, VERTEX_REGION_CLAUDE_5_SONNET,
VERTEX_REGION_CLAUDE_FABLE_5, VERTEX_REGION_CLAUDE_HAIKU_4_5
```

## Focused environment-variable behavior map

### Prompts and loaded instructions

- `CLAUDE_CODE_SIMPLE=1` is documented as equivalent to `--bare`: a minimal system prompt, only
  Bash/read/edit plus explicitly supplied MCP tools, and no automatic hooks, skills, commands,
  subagents, plugins, MCP, auto memory, or CLAUDE.md discovery.
- `CLAUDE_CODE_SIMPLE_SYSTEM_PROMPT=1` shortens the system prompt and tool descriptions without
  removing normal tools or discovery. Setting it to `0` explicitly opts out of any experiment or
  server-side choice that would enable it.
- `CLAUDE_CODE_DISABLE_CLAUDE_MDS=1` removes all CLAUDE.md memory, including user, project, and auto
  memory; `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1` does the opposite for `--add-dir`
  directories. `CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS=1` removes built-in git workflow instructions
  and the git-status snapshot from the system prompt. `CLAUDE_CODE_DISABLE_POLICY_SKILLS=1` skips
  system-wide managed skills.
- `CLAUDE_CODE_ENABLE_APPEND_SUBAGENT_PROMPT=1` enables text appended by
  `--append-subagent-system-prompt`; the flag sets the variable automatically. It excludes forked
  subagents. `CLAUDE_CODE_RESUME_PROMPT` replaces the continuation message injected after resuming
  an interrupted turn.
- `CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION` governs UI prompt suggestions and overrides the
  `promptSuggestionEnabled` setting; it does not append instructions to the model.
- `CLAUDE_CODE_SKIP_PROMPT_HISTORY=1` prevents prompt history and session transcripts from being
  written, so sessions cannot be resumed. `SLASH_COMMAND_TOOL_CHAR_BUDGET` controls how much skill
  metadata enters the Skill tool listing.

### Tools and permissions

- Documented tool switches include `CLAUDE_CODE_DISABLE_ADVISOR_TOOL`,
  `CLAUDE_CODE_DISABLE_ARTIFACT`, `CLAUDE_CODE_DISABLE_ATTACHMENTS`,
  `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS`, `CLAUDE_CODE_DISABLE_BUNDLED_SKILLS`,
  `CLAUDE_CODE_DISABLE_CRON`, `CLAUDE_CODE_DISABLE_EXPLORE_PLAN_AGENTS`,
  `CLAUDE_CODE_DISABLE_WORKFLOWS`, `CLAUDE_CODE_ENABLE_TASKS`,
  `CLAUDE_CODE_ENABLE_TODO_TOOLS`, `CLAUDE_CODE_USE_POWERSHELL_TOOL`, and
  `ENABLE_TOOL_SEARCH`.
- Limits and shaping controls include the three `BASH_*` variables,
  `CLAUDE_CODE_FILE_READ_MAX_OUTPUT_TOKENS`, `CLAUDE_CODE_GLOB_*`,
  `CLAUDE_CODE_MAX_TOOL_USE_CONCURRENCY`, `CLAUDE_CODE_MAX_WEB_SEARCHES_PER_SESSION`,
  `CLAUDE_CODE_TOOL_MEMORY_LIMIT`, `CLAUDE_CODE_WEBFETCH_CACHE_TTL_MS`,
  `MAX_MCP_OUTPUT_TOKENS`, and `TASK_MAX_OUTPUT_LENGTH`.
- `CLAUDE_CODE_MCP_ALLOWLIST_ENV=1` gives stdio MCP servers only a safe baseline plus their
  configured environment. `CLAUDE_CODE_SAFE_MODE=1` disables local customization for diagnosis,
  but managed policy still applies.
- There is **no documented environment variable for arbitrary tool allow/ask/deny rules**. Use the
  `permissions` setting or documented CLI flags. `CLAUDE_CODE_ENABLE_AUTO_MODE` is retained only for
  compatibility and is a no-op in v2.1.239.

### Agents and sessions

- `CLAUDE_CODE_SUBAGENT_MODEL` overrides models for subagents, teammates, and workflow agents,
  including per-invocation and agent-frontmatter choices. `CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS`
  and `CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH` cap concurrency and nesting.
- `CLAUDE_AGENT_SDK_DISABLE_BUILTIN_AGENTS=1` removes built-in agent types in `-p`/SDK mode;
  `CLAUDE_CODE_DISABLE_EXPLORE_PLAN_AGENTS=1` removes only built-in Explore and Plan agents.
- `CLAUDE_CODE_FORK_SUBAGENT` controls fork mode; `CLAUDE_CODE_DISABLE_AGENT_VIEW` disables
  background agents and agent view; `CLAUDE_AUTO_BACKGROUND_TASKS` forces automatic backgrounding;
  `CLAUDE_ASYNC_AGENT_STALL_TIMEOUT_MS` and `CLAUDE_SUBAGENT_BG_SHELL_MAX_MS` bound background work.
- `CLAUDE_CODE_ENABLE_APPEND_SUBAGENT_PROMPT` and `CLAUDE_CODE_FORWARD_SUBAGENT_TEXT` affect
  subagent prompt construction and stream-json forwarding. `CLAUDE_CODE_TASK_LIST_ID` shares a task
  list across sessions.
- `CLAUDE_CODE_MESSAGING_SOCKET` and `CLAUDE_CODE_MESSAGING_TOKEN` are explicitly set by Claude
  Code, not by users, and cannot be supplied through a settings `env` block.

### Context, memory, thinking, and output

- `CLAUDE_CODE_AUTO_COMPACT_WINDOW` sets a 100K–1M token compaction window and overrides the
  command, flag, and setting. `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` can move compaction earlier, not
  later. `DISABLE_AUTO_COMPACT` leaves manual `/compact`; `DISABLE_COMPACT` removes both.
- `CLAUDE_CODE_DISABLE_1M_CONTEXT` holds native-1M models to 200K and removes 1M variants;
  `CLAUDE_CODE_MAX_CONTEXT_TOKENS` corrects the assumed window for gateway/custom model IDs.
  `CLAUDE_CODE_MAX_OUTPUT_TOKENS` reduces context available before compaction as it increases.
- `CLAUDE_CODE_DISABLE_AUTO_MEMORY` controls creation and loading of auto memory, with `0` able to
  force it on despite `--bare` or the setting. CLAUDE.md controls are listed above.
- Thinking is controlled by `CLAUDE_CODE_EFFORT_LEVEL`, `CLAUDE_CODE_DISABLE_ADAPTIVE_THINKING`,
  `CLAUDE_CODE_DISABLE_THINKING`, `DISABLE_INTERLEAVED_THINKING`, and `MAX_THINKING_TOKENS`.
  Prompt-cache controls are `DISABLE_PROMPT_CACHING` and its per-family variants,
  `ENABLE_PROMPT_CACHING_1H`, and `FORCE_PROMPT_CACHING_5M`.

### Experimental and remotely flagged behavior

- `ANTHROPIC_BETAS` adds caller-selected `anthropic-beta` headers.
  `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS=1` strips Anthropic beta headers and beta tool-schema
  fields; it also disables MCP tool search except where a managed override is documented.
- `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` enables agent teams, explicitly labeled experimental and
  off by default. `CLAUDE_CODE_NO_FLICKER=1` selects fullscreen rendering, labeled a research
  preview. `MCP_SDK_GENERATION` and `MCP_PROTOCOL_NEGOTIATION` pin the documented MCP runtime and
  protocol-probe behavior.
- `DISABLE_GROWTHBOOK=1|true`, `DO_NOT_TRACK=1`, any non-empty `DISABLE_TELEMETRY`, or any non-empty
  `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` disables feature-flag fetching. The last two
  “non-empty” variables still disable it when set to `0` or `false`.
- Without feature-flag fetching, the docs say these degrade or disappear: account-default auto
  mode, VS Code's settings-based starting permission mode, Remote Control, cross-session messages,
  `claude import`, CLI routine creation, advisor, model-selected `/loop` intervals and built-in
  maintenance, normal artifact-comment behavior, adaptive PR-status refresh, and automatic v2 MCP
  runtime/protocol-probe selection. Explicit `MCP_SDK_GENERATION=v2` and
  `MCP_PROTOCOL_NEGOTIATION=auto` can restore the MCP selections.

## Environment-variable qualifications

### `env` setting and precedence

A settings `env` value overwrites the same shell variable. Settings-file precedence then applies
among `env` blocks. Project/local values normally wait for workspace trust, although variables
classified as safe (for example model, timeout, limit, feature-toggle, and telemetry values) apply
at startup. Changes are reapplied in a running session, but startup-only consumers require a
restart; removing a key does not unset it until restart.

Project/local `env` cannot set `CLAUDE_CODE_PROCESS_WRAPPER`, `CLAUDE_CODE_SYNC_SKILLS`,
`CLAUDE_CODE_SYNC_PLUGINS`, `CLAUDE_CODE_PLUGIN_CACHE_DIR`, or `CLAUDE_CODE_PLUGIN_SEED_DIR`.
Host-owned identity values, `CLAUDE_CODE_MESSAGING_SOCKET`, `CLAUDE_CODE_MESSAGING_TOKEN`, and
`CLAUDE_CODE_PROJECT_DIR_NAME` are ignored from settings `env` as documented.

Notable precedence exceptions: `--model` and `/model` override `ANTHROPIC_MODEL`, which overrides
`model`; `CLAUDE_CODE_EFFORT_LEVEL` overrides `/effort`, `--effort`, and `effortLevel`;
`CLAUDE_CODE_PROVIDER_MANAGED_BY_HOST` lets an embedding host override managed model configuration.

### Rows that are not active ordinary controls

- **Deprecated but still documented:** `ANTHROPIC_SMALL_FAST_MODEL` and
  `ENABLE_PROMPT_CACHING_1H_BEDROCK` (use `ANTHROPIC_DEFAULT_HAIKU_MODEL` and
  `ENABLE_PROMPT_CACHING_1H`).
- **Removed/no-op:** `CLAUDE_CODE_CONNECT_TIMEOUT_MS` (removed v2.1.186),
  `CLAUDE_CODE_ENABLE_AUTO_MODE` (compatibility no-op),
  `CLAUDE_CODE_ENABLE_OPUS_4_7_FAST_MODE` (removed v2.1.142),
  `CLAUDE_CODE_MAX_SUBAGENTS_PER_SESSION` (removed v2.1.224), and
  `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE` (removed v2.1.160).
- **Runtime outputs rather than normal inputs:** `CLAUDECODE`, `CLAUDE_CODE_CHILD_SESSION`,
  `CLAUDE_CODE_BRIDGE_SESSION_ID`, `CLAUDE_CODE_REMOTE`, `CLAUDE_CODE_REMOTE_SESSION_ID`,
  `CLAUDE_CODE_SESSION_ID`, `CLAUDE_CODE_MESSAGING_SOCKET`, `CLAUDE_CODE_MESSAGING_TOKEN`,
  `CLAUDE_EFFORT`, and `CLAUDE_PID`. `CLAUDE_ENV_FILE` is both a documented input path and a file
  populated by lifecycle hooks.

### Documented outside the 339-row table

These are public, but belong to specialized interfaces rather than the canonical control table:

- [`Monitoring`](https://code.claude.com/docs/en/monitoring-usage.md) documents standard OTel names:
  `OTEL_METRICS_EXPORTER`, `OTEL_LOGS_EXPORTER`, `OTEL_EXPORTER_OTLP_PROTOCOL`,
  `OTEL_EXPORTER_OTLP_ENDPOINT`, generic and per-signal `OTEL_EXPORTER_OTLP_*_HEADERS`,
  `OTEL_EXPORTER_OTLP_{METRICS,LOGS,TRACES}_{PROTOCOL,ENDPOINT}`, `OTEL_METRIC_EXPORT_INTERVAL`,
  `OTEL_LOGS_EXPORT_INTERVAL`, `OTEL_TRACES_EXPORT_INTERVAL`,
  `OTEL_EXPORTER_OTLP_METRICS_TEMPORALITY_PREFERENCE`, `OTEL_RESOURCE_ATTRIBUTES`, and OTLP client
  certificate/key variables. Beta tracing uses `CLAUDE_CODE_ENHANCED_TELEMETRY_BETA` (alias
  `ENABLE_ENHANCED_TELEMETRY_BETA`) plus `OTEL_TRACES_EXPORTER`; detailed hook tracing additionally
  names `ENABLE_BETA_TRACING_DETAILED` and `BETA_TRACING_ENDPOINT`. Tool subprocesses receive
  `TRACEPARENT` when tracing is active.
- [`MCP`](https://code.claude.com/docs/en/mcp.md) documents runtime outputs
  `CLAUDE_CODE_MCP_SERVER_NAME`, `CLAUDE_CODE_MCP_SERVER_URL`, and `CLAUDE_PROJECT_DIR` for the
  appropriate server/helper processes.
- [`Hooks`](https://code.claude.com/docs/en/hooks.md) and
  [`plugins-reference`](https://code.claude.com/docs/en/plugins-reference.md) document
  `CLAUDE_PROJECT_DIR`, `CLAUDE_PLUGIN_ROOT`, `CLAUDE_PLUGIN_DATA`, and generated
  `CLAUDE_PLUGIN_OPTION_<KEY>` values. [`Agent view`](https://code.claude.com/docs/en/agent-view.md)
  documents the per-background-session `CLAUDE_JOB_DIR`.
- [`Skills`](https://code.claude.com/docs/en/skills.md) documents `${CLAUDE_SESSION_ID}` and
  `${CLAUDE_SKILL_DIR}` as string substitutions; they are not presented as general launch-time
  controls.
- [`Amazon Bedrock`](https://code.claude.com/docs/en/amazon-bedrock.md) documents standard AWS
  variables including `AWS_REGION`, `AWS_DEFAULT_REGION`, `AWS_PROFILE`,
  `AWS_SHARED_CREDENTIALS_FILE`, and `AWS_CONFIG_FILE`. The network guide documents lowercase proxy
  aliases plus `NODE_EXTRA_CA_CERTS` and `NODE_TLS_REJECT_UNAUTHORIZED`. `NO_COLOR` and
  `FORCE_COLOR` placed in settings `env` affect subprocesses only, not Claude Code's own UI.

## Changelog-only is not current documented support

The pinned changelog mentions the following environment-style names without a current canonical
reference entry or a dedicated current user-facing contract. They should not be treated as supported
configuration solely because release history names them:

- `AI_AGENT` (v2.1.120): described only as a value Claude Code sets for subprocess attribution.
- `ANTHROPIC_LOG` (v0.2.125): an old logging instruction; the current reference instead documents
  `DEBUG`, `CLAUDE_CODE_DEBUG_LOG_LEVEL`, and `CLAUDE_CODE_DEBUG_LOGS_DIR`.
- `CLAUDE_BASH_NO_LOGIN` (v1.0.124): later changelog text in v2.1.51 says skipping the login shell
  became the default when a shell snapshot is available and the variable was previously required.
- `CLAUDE_CODE_USER_EMAIL` and `CLAUDE_CODE_ORGANIZATION_UUID` (v2.1.51), and the behavior of
  `CLAUDE_CODE_ACCOUNT_UUID`: introduced for SDK hosts/telemetry. Current settings docs mention
  `CLAUDE_CODE_ACCOUNT_UUID` only as an example of a host-owned identity variable ignored in every
  settings `env` block; none has a normal user-control row.
- `CLAUDE_MEMORY_STORES` (v2.1.172): appears only in a remote-session memory bug fix.
- `ANTHROPIC_DEFAULT_{OPUS,SONNET,HAIKU}_MODEL_SUPPORTS` (v2.1.84): old changelog spelling. Current
  documented names end in `_SUPPORTED_CAPABILITIES` and also include the Fable family.

Conversely, a recent changelog mention is not “changelog-only” when the current reference also has a
row. Examples include `CLAUDE_CODE_RETRY_WATCHDOG` (behavior updated in v2.1.239),
`CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION` (v2.1.238), `CLAUDE_CODE_ENABLE_TODO_TOOLS`,
`CLAUDE_CODE_WEBFETCH_CACHE_TTL_MS`, and `CLAUDE_CODE_TOOL_MEMORY_LIMIT` (v2.1.233).

## Local binary analysis

### Artifact and definition of “hidden”

The local executable analyzed was Claude Code `2.1.239` at
`/opt/homebrew/Caskroom/claude-code@latest/2.1.239/claude`:

- size: `324,973,552` bytes
- SHA-256: `2b4f7aafdaa65bcc2335f56a4b276317837203f2c5587b1f2a17ca78ad14e36f`
- embedded build revision: `9bf8e9521fe06414183309865310e27c9b8db3dd`

The bundle exports 453 names matching `CLAUDE_CODE_*` in its typed environment schema. Of those,
405 also have a direct `G.<name>` or `process.env.<name>` read in executable code. Comparing those
405 names with every `CLAUDE_CODE_*` name found in the public-source baseline above leaves **234
runtime-read candidates absent from the public sources checked**. That number is deliberately not
presented as “234 useful flags”: many are host protocol fields, test fixtures, credentials,
telemetry plumbing, or implementation handshakes rather than reasonable user configuration.

Here, **hidden** means “read by this exact executable but absent from the current public sources
checked.” It does not mean supported, stable, safe, or intentionally user-configurable.

### Method

1. Locate the installed executable and pin its version and SHA-256.
2. Extract the generated environment export map (`NAME:()=>symbol`) and its schema constructor
   (`str`, `bool`, `triBool`, `int`, or `enum`). Merely finding a string in the binary is not enough.
3. Require a direct runtime read (`G.NAME` or `process.env.NAME`), eliminating 48 schema/string-table
   names that had no direct read.
4. Trace each useful candidate to the condition and final prompt, tool, or feature it controls.
5. Compare the candidate names against the current official environment reference, settings
   reference, public repository, and changelog pinned above.
6. For prompt controls, run clean `claude -p` processes against a loopback fake Messages API and
   inspect the serialized request. Each negative test has a positive control so “string absent” is
   not mistaken for “code path never ran.” No prompt payload was sent to Anthropic during these
   tests.

This method also exposes an important semantic distinction in the generated parser (binary byte
~285,041,561):

| Parser/use pattern | Accepted input | Consequence |
|---|---|---|
| `triBool` | true: `1`, `true`, `yes`, `on`; false: `0`, `false`, `no`, `off` | Can usually override a remote gate both on and off. |
| `bool` used directly | the same true spellings enable it; everything else is false | Can normally be toggled locally. |
| `bool` combined as `env || model || remoteGate` | true spellings force it on | `0` does **not** force it off; the model bundle or remote gate can still enable it. |
| `str`/`enum`/`int` | Call-site-specific validation | Invalid values usually fall through to server/default behavior rather than reliably disabling it. |

### Hidden prompt and behavior controls

These names are absent from the public-source baseline and have a traced runtime use. “Verified”
means the emitted request changed in the loopback differential test; “static” means the direct
control path was confirmed in the bundle but its multi-turn or UI condition was not reproduced.

| Variable | Values | Actual behavior in 2.1.239 | Disable semantics | Evidence |
|---|---|---|---|---|
| `CLAUDE_CODE_THRIFTY_SONIC` | `triBool` | Controls `bashFirst`; when active, Auto/bypass mode tells the model to read, search, and edit through Bash instead of dedicated tools and trims the Bash description. | `0` is a real off override. | Verified `0` absent / `1` present; gate ~288,928,449, attachment ~300,616,042, renderer ~300,787,035. |
| `CLAUDE_CODE_ACT_DONT_REDERIVE` | `triBool` | Adds “act; do not re-derive established facts or re-litigate decisions.” | `0` is a real off override. | Verified; gate ~301,184,874, text ~301,178,328. |
| `CLAUDE_CODE_INTRO_FRAME` | `triBool` | Replaces the default introductory identity sentence with “working with the user toward their goals.” | `0` is a real off override. | Verified; ~301,184,630. |
| `CLAUDE_CODE_BISON_CAIRN` | boolean | Force-enables the long `# Delivering work` section about scope, assumptions, blockers, and completion. | One-way enable: `0` cannot defeat a model/remote enablement. | Verified on Sonnet; gate ~288,929,012, section ~301,178,612. |
| `CLAUDE_CODE_LARCH_CISTERN` | boolean | Force-enables `# Corrections`, discouraging unnecessary correction narration and re-auditing. | One-way enable. | Verified on Sonnet; gate ~288,929,072, section ~301,180,661. |
| `CLAUDE_CODE_AMBER_ASTROLABE` | boolean | Force-enables an autonomy appendix that says to proceed without mid-task questions for reversible work. | One-way enable; Fable 5 can enable the same section independently. | Verified on Sonnet; gate ~288,928,943, section ~301,148,544. |
| `CLAUDE_CODE_GAULT_KESTREL` | boolean | Removes a caution sentence that says to stop when the target contradicts its description or was not created by the agent. | One-way enable; it weakens a guardrail and should not be enabled casually. | Static; gate ~288,928,817, use ~301,146,867. |
| `CLAUDE_CODE_BASALT_COVE` | boolean | Force-selects the expanded “Communicating with the user” guidance for matching models. | One-way enable. | Static; gate ~288,928,214, use ~301,142,360. |
| `CLAUDE_CODE_GORSE_PLOVER` | boolean | Adds a Bash-description nudge saying commands are cheap and errors are informative. | One-way enable; only relevant on the applicable Bash-description path. | Static; ~298,633,664. |
| `CLAUDE_CODE_PARCHMENT_FERN` | boolean | Selects stricter/read-scope-specific pre-read wording for Write/Edit on eligible models. | One-way enable; ignored for several older model families. | Static; gate ~288,929,274, uses ~290,132,172 and ~293,784,008. |
| `CLAUDE_CODE_THISTLE_GREBE` | `default`, `no_nudges`, `counter_steer` | Selects subagent delegation guidance. `no_nudges` removes the normal delegation encouragement; `counter_steer` adds a strong “delegate only when payoff exceeds overhead” section. | Explicit enum override, latched per session. | Verified request-size/content changes; parser ~288,045,573, counter-steer text ~288,046,240. |
| `CLAUDE_CODE_TOASTY_THIMBLE` | arbitrary non-boolean text | Injects that text as a `batching_reminder` after an eligible tool-result turn, latched per model and conversation. | `0`, `false`, `no`, `off`, and also true-like strings all resolve to no custom reminder. | Static; parser/injector ~295,914,119–295,915,294. |
| `CLAUDE_CODE_SILENT_TURN_REMINDER` | `triBool` | Enables a reminder after enough silent tool turns; at most three reminders occur in one stretch. | `0` is a real off override. | Static; gate ~300,564,917, injection ~300,607,833 and ~300,613,247. |
| `CLAUDE_CODE_SILENT_TURN_REMINDER_TEXT` | string | Replaces the silent-turn reminder text. | A supplied string wins over the remote/default text. | Static; ~300,564,717. |
| `CLAUDE_CODE_SILENT_TURN_REMINDER_TURNS` | positive integer | Sets the silent-turn threshold; default is five. | Direct numeric override. | Static; ~300,565,073. |
| `CLAUDE_CODE_TOTAL_TOKENS_REMINDER` | `off`, `infinite`, `fixed`, `countdown`, `padded-countdown` | Controls `<total_tokens>…</total_tokens>` blocks. Default is `padded-countdown`; `fixed` emits 5,000,000. | `off` reliably removes the block. | Verified; resolver ~295,985,316. |
| `CLAUDE_CODE_TOTAL_TOKENS_REMINDER_BUDGET` | positive integer | Sets the starting budget for `padded-countdown`; default is 15,000,000. | Direct numeric override. | Static; ~295,985,826. |
| `CLAUDE_CODE_TOTAL_TOKENS_REMINDER_AFTER_USER_TURN` | `triBool` | Controls whether the token reminder is also emitted/re-anchored after a normal user turn. | `0` is a real off override. | Static; ~295,986,270. |
| `CLAUDE_CODE_TODO_REMINDER_MODE` | `baseline`, `off` | Enables or disables TodoWrite/task reminders after ten turns without maintenance. | `off` reliably disables this reminder. | Static; resolver ~300,605,017; cadence ~300,645,980. |
| `CLAUDE_CODE_TURN_UPDATES` | `triBool` | Selects a shorter prompt asking for a pre-tool sentence, brief progress updates, and a standalone recap. | `0` is a real off override. | Static; ~301,142,310. |
| `CLAUDE_CODE_BASH_OUTPUT_AUDIENCE_NOTE` | `triBool` | Gates a reminder after qualifying Bash output that command output is not reliably visible to the user. | `0` is a real off override. | Static; ~298,341,027. |
| `CLAUDE_CODE_ENABLE_NARRATION` | `triBool` | Enables delayed narration during tool rounds; explicit enable uses a 30-second fallback interval when no server interval exists. | `0` is a real off override. | Static; ~298,341,139. |
| `CLAUDE_CODE_PEWTER_OWL` | `triBool` | Overrides both internal “brief” variants, where ordinary assistant text may be hidden and a dedicated user-message tool is expected. | Explicit on/off override. | Static; ~296,129,144. |
| `CLAUDE_CODE_PEWTER_OWL_TOOL` | `triBool` | Overrides only the tool-based brief variant. | Explicit on/off override. | Static; ~296,129,342. |

### Other high-confidence hidden controls

| Variable | Values/default | Behavior and qualification | Binary evidence |
|---|---|---|---|
| `CLAUDE_CODE_WORKFLOWS` | `triBool` | `0` disables workflow availability; `1` opts in only if the server availability gate is also on. This differs from the documented `CLAUDE_CODE_DISABLE_WORKFLOWS`. | ~288,272,208–444 |
| `CLAUDE_CODE_EXPERIMENTAL_OBSERVER_AGENTS` | boolean | Enables observer-agent declarations, still subject to a remote gate and disabled in simple mode. | ~293,963,619 |
| `CLAUDE_CODE_WEB_FETCH_AGENT` | `triBool` | Overrides the WebFetch-agent experiment, but organization policy and execution mode can still prohibit it. | ~291,856,439 |
| `CLAUDE_CODE_PLAN_V2_AGENT_COUNT` | integer `1..10` | Overrides the main Plan-v2 agent count; defaults vary by plan/account. | ~300,710,000 |
| `CLAUDE_CODE_PLAN_V2_EXPLORE_AGENT_COUNT` | integer `1..10` | Overrides Plan-v2 exploration fan-out; default is 3. | ~300,710,638 |
| `CLAUDE_CODE_JUNIPER_SUNDIAL` | positive integer | Overrides turns between task-maintenance attachments; default is 10. | ~300,605,266 and ~300,645,980 |
| `CLAUDE_CODE_NO_MODEL_FALLBACK` | boolean | Suppresses automatic model fallback. | Direct runtime reads in the model selection/request path. |
| `CLAUDE_CODE_HARBOR_KITE` | boolean | Force-enables cross-session messaging, except for additional platform/rollout constraints. | ~288,811,752 |
| `CLAUDE_CODE_HARBOR_KITE_PACING_OFF` | boolean | Disables outbound cross-session message pacing. | ~288,825,027 |
| `CLAUDE_CODE_WALNUT_SPIRE` | boolean | Enables the early-access `claude plugin eval` command. The embedded help explicitly permits shell, user `settings.json.env`, or managed `env`, and rejects relying on project/local `env`. | ~290,063,793–290,064,078 |
| `CLAUDE_CODE_LANTERN_PRISM` | boolean | Enables the early-access `/skill-doctor` command and related plugin stats surface. | ~290,063,714 and downstream `roe()` calls |
| `CLAUDE_CODE_PROACTIVE` | boolean | Passes `assistantMode` into the proactive UI/session controller. | ~310,402,959 |
| `CLAUDE_CODE_TOASTY_THIMBLE` and the three silent-turn variables | user/managed scope | The binary explicitly filters these out of project and local settings; put them only in user or managed `env`. | settings-env filter ~291,672,656 and deny set ~291,679,500 |

### Unindexed top-level `settings.json` keys

Parsing the executable's root settings object found 159 accepted keys. The current public index has 145
canonical keys. After removing `$schema` and the two documented aliases (`additionalMarketplaces` and
`allowedMarketplaces`), **21 accepted keys are absent from the canonical index**:

| Key | Shape | Embedded behavior | Caution |
|---|---|---|---|
| `breakReminder` | `{enabled, intervalMinutes, breakThresholdMinutes, message}` | Non-blocking break reminder; defaults are 30 minutes of use and 10 minutes of inactivity to reset. | Low risk. |
| `quietHours` | `{enabled, start, end}` | One soft reminder per session inside a local `HH:MM` window. | Low risk. |
| `showMessageTimestamps` | boolean | Shows each message's arrival time. | Low risk. |
| `todoFeatureEnabled` | boolean | Controls the todo/task tracking panel. | UI behavior may change. |
| `feedbackDrafts` | `notify`, `quiet`, `off` | Controls model-drafted feedback and its notification; `off` removes the tool. | Experimental surface. |
| `precomputeCompactionEnabled` | boolean | Computes the compaction summary in the background before it is needed. | Only applies when auto-compact is enabled. |
| `modelSettings` | object keyed by canonical model | Currently persists per-model `effortLevel` (`low`, `medium`, `high`, `xhigh`). | Schema is passthrough and may change. |
| `autoDreamEnabled` | boolean | Overrides server default for background memory consolidation. | Can read/write memory when auto-memory is enabled. |
| `doneMeansMerged` | boolean | Adds a completion policy: continue until a PR is merge-ready, a monitor is armed, or a complete handoff is provided. | Changes agent autonomy and task duration. |
| `modelProposedGoals` | `auto`, `alwaysAsk`, `disabled` | Controls the ProposeGoal tool; typed `/goal` is unaffected. | User/managed/flag sources only because it affects consent. |
| `totalTokensReminder` | enum listed above | Native settings equivalent of the hidden environment control. | Changes model-visible prompt content. |
| `totalTokensReminderBudget` | positive integer | Native budget for `padded-countdown`. | Changes model-visible prompt content. |
| `totalTokensReminderAfterUserTurn` | boolean | Controls re-emission and re-anchoring after user turns. | Changes model-visible prompt content. |
| `autoContinueAtUsageLimit` | boolean | Waits for a claude.ai usage limit to reset and then continues automatically. | May extend unattended runtime. |
| `autoUploadSessions` | boolean | Mirrors local sessions to claude.ai as view-only sessions. | Privacy/data-transfer impact; do not enable casually. |
| `skipWorkflowUsageWarning` | boolean | Records acceptance of the multi-agent workflow usage warning. | Consent-related; do not preseed without intent. |
| `daemonColdStart` | `transient`, `ask` | Chooses whether a missing background service is spawned for the login session or prompts for persistent installation. | Process-lifecycle behavior. |
| `proxyAuthHelper` | string command | Produces a `Proxy-Authorization` header. | Credential-bearing; prefer managed configuration. |
| `remote` | `{defaultEnvironmentId}` | Selects the default cloud-session environment. | External execution target. |
| `policyHelpers` | per-OS object | Internal multi-platform managed-policy helper/fallback mechanism. | Admin-only; executing helpers is security-sensitive. |
| `xaaIdp` | `{issuer, clientId, callbackPort?}` | Internal XAA/SEP-990 identity-provider configuration, only present when another hidden gate enables it. | Enterprise authentication; do not experiment on a live profile. |

Five of these are already present in the local user settings: `autoDreamEnabled`,
`modelProposedGoals`, `modelSettings`, `precomputeCompactionEnabled`, and
`skipWorkflowUsageWarning`.

### Runtime differential results

The loopback tests captured the serialized `/v1/messages` body from isolated temporary profiles:

| Case | Result |
|---|---|
| `CLAUDE_CODE_THRIFTY_SONIC=0` vs `1` | Exact Bash-first instruction absent with `0`, present with `1`. |
| `CLAUDE_CODE_ACT_DONT_REDERIVE=0` vs `1` | Section absent with `0`, present with `1`; baseline was on. |
| `CLAUDE_CODE_INTRO_FRAME=0` vs `1` | Alternate intro absent with `0`, present with `1`. |
| `CLAUDE_CODE_BISON_CAIRN=1` | Added `# Delivering work` and increased request by about 2 KB. |
| `CLAUDE_CODE_LARCH_CISTERN=1` | Added `# Corrections` and increased request by about 1.25 KB. |
| `CLAUDE_CODE_AMBER_ASTROLABE=1` | Added the autonomy appendix and increased request by about 1.37 KB. |
| `CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS=1` | Removed the built-in git section and reduced the request by about 6.4 KB. This variable is documented, so it is a control rather than a hidden finding. |
| `CLAUDE_CODE_SIMPLE_SYSTEM_PROMPT=1` | Switched to the lean harness and reduced this test request from about 113 KB to 73 KB. This variable is documented. |
| `CLAUDE_CODE_TOTAL_TOKENS_REMINDER=off` | Removed the `<total_tokens>` block; `fixed` retained it. |
| `CLAUDE_CODE_DISABLE_ATTACHMENTS=1` | Removed the token block, confirming its broad effect; it is documented and too broad to recommend merely for prompt cleanup. |
| `CLAUDE_CODE_THISTLE_GREBE=no_nudges` | Removed the normal positive subagent guidance. |
| `CLAUDE_CODE_THISTLE_GREBE=counter_steer` | Removed that guidance and added the stronger anti-overdelegation section. |

The model bundle matters independently of environment overrides:

- With the local fake API and `claude-opus-5[1m]`, the bundled `# Delivering work`, `# Corrections`,
  and two-line `heron_brook` section were present. Setting `BISON_CAIRN=0` or `LARCH_CISTERN=0`
  did not remove them.
- With `claude-fable-5[1m]`, the autonomy appendix was present. Setting
  `AMBER_ASTROLABE=0` did not remove it.

That confirms the static `env || modelBundle || remoteGate` reading: these boolean codenames are
force-on switches, not symmetric kill switches. `THRIFTY_SONIC` is different because its explicit
`triBool` value is checked before model and remote assignment.

Headless `--permission-mode auto` did not initialize an Auto Mode attachment against the fake API,
so it was not treated as a valid positive control. The THRIFTY differential used the
`bypassPermissions` branch, which the 2.1.239 code routes through the same `dci()` → `bashFirst` →
attachment renderer as Auto Mode. This establishes the environment parsing and emitted prompt;
the Auto-specific branch is supported by direct code tracing rather than a live authenticated call.

### Practical conclusions

- The most reliable hidden **off switches** for model-visible reminders are the `triBool` or enum
  controls: `THRIFTY_SONIC=0`, `ACT_DONT_REDERIVE=0`, `SILENT_TURN_REMINDER=0`,
  `TOTAL_TOKENS_REMINDER=off`, `TODO_REMINDER_MODE=off`, and
  `THISTLE_GREBE=no_nudges`. `TOASTY_THIMBLE=0` suppresses its custom reminder because boolean-like
  strings are treated as “no custom text.”
- Do not assume every `...=0` disables a feature. `BISON_CAIRN`, `LARCH_CISTERN`,
  `AMBER_ASTROLABE`, `GAULT_KESTREL`, `GORSE_PLOVER`, `PARCHMENT_FERN`, and `BASALT_COVE` use
  one-way OR gates at their relevant call sites.
- Prefer a documented top-level setting when one exists. For example, use `includeGitInstructions`,
  `disableWorkflows`, `autoMemoryEnabled`, or `fileCheckpointingEnabled` instead of an environment
  alias.
- Avoid experimenting with auth, host-protocol, remote-session, policy, test-fixture, or
  `DISABLE_*` safety variables. Several are blocked from lower-trust settings scopes, and some can
  weaken approval or policy behavior.
- Re-run this analysis after every Claude Code update. These names are internal implementation
  details, and the byte offsets and behavior claims apply only to the pinned `2.1.239` binary.
