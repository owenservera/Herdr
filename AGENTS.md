# Herdr workspace instructions

## Purpose and scope
- This is Owen's dedicated workspace for researching, configuring, and validating [Herdr](https://herdr.dev/docs/) on his local Windows machine and integrating it with agentic coding CLIs. Herdr is a terminal workspace manager whose background server owns panes and agent processes.
- This directory is not the upstream Herdr source repository. At initialization (2026-10-08), it was empty, had no Git metadata, package manifest, application code, or build/test scripts. Do not invent project commands or scaffold an application unless requested.
- Keep project notes, reproducible setup scripts, and sanitized configuration examples here. Actual user configuration and agent integrations live outside this workspace; changes there have a separate impact.

## Read before changing setup
- Start with https://herdr.dev/agent-guide.md for setup and troubleshooting; use https://herdr.dev/docs/ for detailed documentation.
- Installation/platform constraints: https://herdr.dev/docs/install/ and https://herdr.dev/docs/windows-beta/ (the latter URL documents generally available native Windows support despite its name).
- Configuration/integrations: https://herdr.dev/docs/configuration/, https://herdr.dev/docs/config-reference/, and https://herdr.dev/docs/integrations/.
- Prefer the installed binary's `herdr --help` and subcommand help over examples from a different release. Record the installed version and source URLs in findings; distinguish verified local facts from vendor documentation and inference.

## Local environment and discovery
- Host: Windows x64; ZCode's command shell is Git Bash. Workspace: `C:\0-BlackBoxProject-0\Herdr`. Quote paths with spaces and label PowerShell, Command Prompt, and Bash snippets explicitly; their syntax is not interchangeable.
- Initial PATH discovery found `claude`, `codex`, `opencode`, Node/npm/pnpm, Bun, Git/GitHub CLI, and uv. This proves command availability, not authentication or working integrations. `herdr` was not on this shell's PATH; that does not prove it is uninstalled. Recheck current state before setup work.
- Official Windows installs use `%USERPROFILE%\.herdr\packages\standalone\releases`, with `%LOCALAPPDATA%\Programs\Herdr\bin` as a compatibility alias. Keep `herdr.exe` with its bundled ConPTY runtime; copying only the executable breaks the supported package layout.
- Default Windows config: `%APPDATA%\herdr\config.toml`; `HERDR_CONFIG_PATH` can override it. Use `herdr --help` to resolve the actual path. Herdr works without a config; `herdr --default-config` prints defaults. Never overwrite an existing config with the defaults.

## Setup and validation
- Do not install or update software as part of research or instruction-file maintenance. For an authorized installation, use the current official Windows instructions, not Unix shell installers or guessed package names.
- Once available, useful checks are `herdr --help`, `herdr -V`, `herdr integration status`, and `herdr status` when a server is running. There are currently no workspace build, typecheck, lint, or test commands.
- `herdr integration install <name>` modifies the selected agent's configuration. Inspect and back up the relevant files first; preserve unrelated settings and credentials. Verify support on the installed Windows release before using an integration.
- `herdr server reload-config` applies reloadable config changes. Startup-only settings and existing pane shells may require recreation; do not restart merely because a setting did not change immediately.

## Agent/runtime safety and Windows gotchas
- Starting `herdr` launches or attaches a session. Never launch it inside an existing Herdr pane (`HERDR_ENV=1`); nested launch is blocked.
- For agents operating panes, consult https://herdr.dev/docs/agent-skill/ and the release-matched output of `herdr --skill`. That operator skill requires `HERDR_ENV=1`; outside Herdr, limit work to setup/diagnostics rather than blindly driving a focused session.
- Identify the intended session and pane before sending text, keys, prompts, or closing anything. `herdr server stop` terminates pane processes; confirm before stopping servers or deleting sessions. Closing a client/detaching is not the same as stopping its server.
- Native Windows does not support direct `herdr terminal attach` or live server handoff. Do not prescribe Unix sockets, file-descriptor handoff, or Unix process-group assumptions.
- Default pane shell is PowerShell 7 when available, otherwise Windows PowerShell 5.1—not necessarily ZCode's Git Bash. Live cwd detection after PowerShell `cd` is partial; verify the actual working directory before issuing filesystem commands.
- Never commit tokens, OAuth material, private agent transcripts, or unsanitized machine configuration. Keep changes scoped to the request; do not alter parent SwarmVault inputs or generated pages during Herdr-only work. If working in that vault, follow the inherited SwarmVault rules.
