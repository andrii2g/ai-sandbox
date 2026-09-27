# ai-sandbox

A step-by-step guide to preparing a sandbox for AI agents using [nono](https://nono.sh) and [Pi](https://pi.dev). We will use nono to control the agent's access to files and the network, with one dedicated profile for Pi.

## 1. Install nono

Run these commands in a Linux Bash terminal (or inside your WSL 2 distribution on Windows):

```bash
curl -fsSL https://nono.sh/install.sh | sh

# Make user-installed commands available in this terminal.
export PATH="$HOME/.local/bin:$PATH"

# Keep this setting for future interactive Bash terminals; safe to repeat.
grep -qxF 'export PATH="$HOME/.local/bin:$PATH"' "$HOME/.bashrc" ||
  printf '\n%s\n' 'export PATH="$HOME/.local/bin:$PATH"' >> "$HOME/.bashrc"

command -v nono
nono --version
```

These startup instructions are for Bash. Non-interactive scripts should set PATH explicitly or use the full executable path. Linux requires a kernel with Landlock support. See the [official installation guide](https://nono.sh/docs/cli/getting_started/installation).

## 2. Create a profile

Create a reusable sandbox policy named `pi`: `--extends default` inherits nono's baseline rules, and `--groups node_runtime` adds access to Node.js runtime paths needed by Pi. This prepares its permissions; it does not install or launch Pi:

```bash
nono profile init pi --extends default --groups node_runtime
```

The profile is saved to `~/.config/nono/profiles/pi.json`, or `$XDG_CONFIG_HOME/nono/profiles/pi.json` if that variable is set. Inspect and validate it:

```bash
nono profile show pi
nono profile validate "${XDG_CONFIG_HOME:-$HOME/.config}/nono/profiles/pi.json"
```

Check that a sandboxed process can start, with networking blocked for this check:

```bash
nono run --profile pi --block-net -- /bin/echo 'Sandbox ready'
```

Here, `run` starts a sandboxed program, `--profile pi` applies your profile, and `--block-net` blocks networking for this run. The `--` separates nono options from the command: `/bin/echo` prints `Sandbox ready` and exits. This does not launch Pi or change the profile. A successful run confirms basic sandbox startup, not every restriction.

Select a profile with `--profile NAME` on each run; no global switch is needed. Profiles define permissions, and the default base does not automatically grant access to your project directory. See [profiles and groups](https://nono.sh/docs/cli/features/profiles-groups).

This is a starting profile. Next, we will install Pi and configure its workspace, credentials, and network access before running the agent.
