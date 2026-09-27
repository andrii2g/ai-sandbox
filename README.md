# ai-sandbox

A step-by-step guide to preparing a sandbox for AI agents using [nono](https://nono.sh) and [Pi](https://pi.dev). nono controls filesystem and network access; Pi runs the coding workflow inside the sandbox using one dedicated profile.

## Setup guides

1. [nono](docs/nono.md) — install the sandbox, save Bash PATH permanently, and create the `pi` profile.
2. [Pi](docs/pi.md) — install the latest Pi, persist the Node environment, and check it runs inside nono.
3. [First session](docs/first-session.md) — start Pi inside nono, log in, select a model, and try a task.
4. [Ollama](docs/ollama.md) — configure Qwen 3.8 27B or another existing Ollama model and select it in Pi.

Commands target Linux Bash, including WSL 2. Complete the startup configuration once per Linux user; new Bash terminals should find both `nono` and `pi` automatically. Follow the guides in order before starting a session.
