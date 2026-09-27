# Pi: quick setup

[Back to overview](../README.md) · [nono setup](nono.md)

Run these commands in Linux Bash or WSL 2. You need Node.js 22.19 or newer and the existing nono profile named `pi`.

## 1. Install Pi

If you use nvm, load it and select Node.js 24 first:

```bash
. "$HOME/.nvm/nvm.sh"
nvm use 24
```

Install the latest Pi:

```bash
npm install -g --ignore-scripts @earendil-works/pi-coding-agent
```

## 2. Keep Pi available in new terminals (once)

With nvm, Pi belongs to the Node version used during installation. Make Node.js 24 the default for new terminals:

```bash
nvm alias default 24
```

Ensure your `~/.bashrc` contains the nvm initialization lines below. The nvm installer normally adds them; add them only if missing (use your existing `NVM_DIR` if it differs):

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
```

Reload Bash configuration and clear any remembered executable paths:

```bash
source "$HOME/.bashrc"
hash -r
```

nvm adds the selected Node installation's `bin` directory to PATH, including `pi`. No repeated PATH export is needed. If you deliberately switch Node versions later, switch back with `nvm use 24` to use this Pi installation.

Without nvm, run `npm prefix -g` and add `<that-prefix>/bin` to PATH in your shell's startup file. The preceding [nono setup](nono.md) already configures its `~/.local/bin` directory and Bash login startup.

## 3. Create Pi's storage directory (once)

```bash
mkdir -p "$HOME/.pi/agent"
```

This prepares storage for settings, credentials, and sessions before granting nono access to it. Run it once per Linux user; `$HOME` is your home directory. The `-p` option creates missing parents and safely leaves an existing directory intact, so repeating it is harmless.

## 4. Check Pi

```bash
command -v pi
pi --version
```

Run these checks in a newly opened Bash terminal too. You should see the executable path and installed version without repeating the setup commands.

## 5. Check Pi inside nono

```bash
nono run --profile pi --block-net -- pi --version
```

This should print the same version inside the sandbox, with networking blocked. The previous steps have configured PATH and selected the Node version used by Pi.

### WSL 2 warning

If you see `Landlock ABI V3 lacks TCP network filtering; using seccomp full-network-block fallback`, nono is using another Linux kernel mechanism (seccomp) to enforce `--block-net`. Network blocking still works; Landlock continues to enforce filesystem restrictions.

The `degraded` notice lists advanced features unavailable on your WSL kernel, such as per-port filtering. It does not mean the entire sandbox is disabled. If Pi prints its version, this startup check succeeded and no fix is needed. The version check does not independently test every sandbox restriction. See [nono's WSL 2 documentation](https://nono.sh/docs/cli/internals/wsl2).

Next: [start your first Pi session inside nono](first-session.md), log in, and select a model.

[Official Pi installation instructions](https://pi.dev/docs/latest/quickstart)
