# Repository Guidelines

## Project Structure
- `chrome-wsl`: main bash entrypoint (packaged as the npm bin).
- `package.json`: npm metadata, bin mapping, publishConfig.
- `README.md`: user-facing usage and examples.
- `.github/workflows/publish.yml`: release/publish automation (npm).

## Build, Test, and Run
- Build/package: `npm pack` (uses local `chrome-wsl` file set).
- Run locally: `npx ./` or `./chrome-wsl`.
- Stop tunnel: `./chrome-wsl --stop`.
- Uninstall helpers: `./chrome-wsl --uninstall` (prompts for firewall rule removal and socat uninstall).
- Publish: GitHub release or `npm publish` (requires `NPM_TOKEN`).

## Coding Style & Conventions
- Bash script; keep `set -euo pipefail` intact.
- Indent with two spaces; prefer POSIX-ish bash; avoid bashisms unless needed.
- Keep PowerShell snippets double-escaped so `$` and `$_` survive from bash to PowerShell.
- Temp files live in `/mnt/c/Temp` for elevated PowerShell access.

## Testing Guidance
- No automated test suite. Validate manually:
  - `npx ./` to start the bridge and Chrome.
  - `./chrome-wsl --stop` to ensure cleanup.
  - `./chrome-wsl --uninstall` to verify prompts and removals.
  - Check `WINDOWS_CHROME_PATH`/port changes if edited.

## Commit & PR Guidelines
- Commit messages: short imperative (e.g., “Fix PowerShell quoting”).
- PRs: include what changed, why, and manual verification steps (e.g., `npx ./`, `--stop`, `--uninstall`). Mention portproxy/firewall scenarios if touched.

## Security & Config Tips
- PowerShell admin calls write temp scripts to `C:\Temp`; avoid storing secrets there.
- Remote debugging port default is 9222; adjust portproxy/firewall instructions and script if changing.
- Keep firewall/portproxy creation/removal gated and explicit (prompts + RUN_AS_ADMIN path).
