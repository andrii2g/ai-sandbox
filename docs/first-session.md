# First session: Pi inside nono

[Back to overview](../README.md) · [Pi installation](pi.md)

Complete the [nono setup](nono.md) and [Pi setup](pi.md) first. Their startup configuration makes `nono` and `pi` available in new Bash terminals.

## 1. Start Pi in your project

Replace `/path/to/your/project` with the folder you want Pi to work on:

```bash
cd /path/to/your/project
nono run --profile pi --allow . --allow "$HOME/.pi/agent" -- pi
```

What these commands do:

- `cd /path/to/your/project` selects the project Pi will work on. Replace the path with your own, for example `cd ~/ai`.
- `nono run` starts a program inside the sandbox. `--profile pi` applies your saved policy, `--allow .` grants read/write access to the current project, and `--allow "$HOME/.pi/agent"` permits Pi to read and save its configuration and sessions. The `--` separates nono options from the program to launch: `pi`.

These permissions let Pi work with project files and retain its configuration between runs. They apply to this launch and do not modify the saved profile.

We omit `--block-net` because login and hosted models need networking. This initial setup allows outbound connections and makes Pi's stored credentials accessible to sandboxed code. Keep `~/.pi/agent/auth.json` private and out of Git. Always use the nono launch command for sandboxed work; bare `pi` runs outside nono.

## 2. Log in

Inside Pi, type:

```text
/login
```

Choose your provider and follow its subscription or API-key login prompts. If a browser callback cannot reach WSL, follow Pi's instructions to paste the authorization code or redirect URL. See [provider authentication](https://pi.dev/docs/latest/providers).

### OpenAI Codex: browser or device code?

Both options sign you into your OpenAI account:

- **Browser login** opens a sign-in page and normally returns the result through a local callback to the terminal application.
- **Device code login** displays a link and a temporary code. Open the link in your Windows browser, sign in, and enter the code; no local callback is needed.

For Pi inside nono on WSL, we recommend **device code login** to avoid browser-opening and callback issues. You may need to enable it in your ChatGPT security settings, or have your workspace administrator enable it. Follow the link and code shown by Pi, then wait for login to complete. See [OpenAI authentication guidance](https://learn.chatgpt.com/docs/auth#login-on-headless-devices).

## 3. Select a model

Inside Pi, type:

```text
/model
```

Choose an available model for the provider you authenticated with. You can use `/model` again later to change it. See the [official quickstart](https://pi.dev/docs/latest/quickstart).

For local models, follow the [Ollama guide](ollama.md) to register and select your exact model variant.

## 4. Try a first task

For a project with a README, enter:

```text
Read this project's README and summarize it without changing any files.
```

Review the tool activity and response. A successful answer confirms your model connection works. The request to avoid edits is an instruction to Pi; the sandbox still grants the project read/write access.

To return to the latest session later, run from the same project folder:

```bash
nono run --profile pi --allow . --allow "$HOME/.pi/agent" -- pi --continue
```
