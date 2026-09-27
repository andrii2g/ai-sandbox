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

## 2. Configure PATH

Make npm's installed commands available in this terminal:

```bash
export PATH="$(npm prefix -g)/bin:$PATH"
hash -r
```

With nvm, keep using the same Node version: global packages belong to that version. To make Node.js 24 the default for new nvm-enabled terminals:

```bash
nvm alias default 24
```

## 3. Check Pi

```bash
command -v pi
pi --version
```

You should see the executable path and installed version.

## 4. Check Pi inside nono

```bash
nono run --profile pi --block-net -- pi --version
```

This should print the same version inside the sandbox, with networking blocked. If nono reports `cannot find binary path`, repeat the nvm and PATH steps in that same terminal.
### WSL 2 warning

If you see `Landlock ABI V3 lacks TCP network filtering; using seccomp full-network-block fallback`, nono is using another Linux kernel mechanism (seccomp) to enforce `--block-net`. Network blocking still works; Landlock continues to enforce filesystem restrictions.

The `degraded` notice lists advanced features unavailable on your WSL kernel, such as per-port filtering. It does not mean the entire sandbox is disabled. If Pi prints its version, this startup check succeeded and no fix is needed. The version check does not independently test every sandbox restriction. See [nono's WSL 2 documentation](https://nono.sh/docs/cli/internals/wsl2).

Pi is now installed and can execute inside nono. Provider login and permissions for a full coding session come next.

[Official Pi installation instructions](https://pi.dev/docs/latest/quickstart)
