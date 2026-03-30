# Repository Guidelines

## Project Structure & Module Organization
Configurations live in `profiles/` (per-platform Conan profiles), `settings.yml` (compiler/OS matrix), `remotes.json` (remote ordering), and optional patches such as `sanitize.patch`. Create new profiles under `profiles/` using the naming pattern `toolchain-version-target[-cppstd-x]` so downstream teams can target them predictably.

## Build, Test, and Development Commands
- `conan config install .` installs the checked-out config into the active Conan client.
- `conan config install . --verify` ensures no local overrides conflict with the repo state; run before opening a PR.
- `conan profile list | grep <profile>` confirms that newly added profiles register correctly.

## Coding Style & Naming Conventions
Keep profiles in plain text with key/value pairs aligned via single spaces and no tabs. Group related options (e.g., `[settings]`, `[env]`, `[tool_requires]`) and add concise inline comments only where behavior might surprise consumers. YAML files (`settings.yml`, overrides) follow two-space indentation and lowercase keys unless Conan requires specific casing. File names use lowercase with dashes; avoid spaces.  All text files should end in a newline to prevent them from being added by certain editors that do so, and to prevent cat/type commands from messing up the terminal.

## Testing Guidelines
Smoke-test profile additions by running `conan install . -pr:<profile>` within a representative project before publishing the change, and note the project name in the PR. When altering `settings.yml`, ensure existing profiles still parse by running `conan profile show <profile>` for at least one Apple, one MSVC, and one Linux target. No automated CI exists, so local verification and screenshots of terminal output are expected for risky updates.

## Commit & Pull Request Guidelines
Write imperative commit subjects (`Add apple-clang-27 arm profile`). Squash small, related edits instead of stacking fixup commits. PRs must describe the motivation, highlight affected profiles or settings, and link any Jira/GitHub issue. Include reproduction steps (`conan install …`) plus verification evidence (command output or logs). If you change remotes or security-sensitive values, mention required downstream actions (e.g., “rerun `conan config install` on CI hosts”).

## Security & Configuration Tips
Treat remotes and credentials as sensitive: never commit tokens or passwords. Prefer environment-driven secrets and document required variables under `[env]` stubs instead of embedding values. For experimental changes, stage them in new profiles so existing pipelines remain stable; remove the `experimental-` prefix only after teams adopt them.
