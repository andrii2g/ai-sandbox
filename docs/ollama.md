# Ollama: configure and select models in Pi

[Back to overview](../README.md) · [First Pi session](first-session.md)

Register your existing Ollama models in Pi, then choose one for each session. They all use the same nono profile named `pi`.

## 1. Check your Ollama endpoint

Replace `your-ollama-host` with your Ollama hostname or IP address:

```bash
curl http://your-ollama-host:11434/api/tags
```

The response lists available model names. Use those exact names in Pi.

## 2. Configure the model in Pi

Edit `~/.pi/agent/models.json`:

```bash
nano "$HOME/.pi/agent/models.json"
```

This example registers Qwen 3.8 27B. Replace the endpoint and model ID as needed. If other providers already exist, merge the `ollama` entry into the existing `providers` object.

```json
{
  "providers": {
    "ollama": {
      "baseUrl": "http://your-ollama-host:11434/v1",
      "api": "openai-completions",
      "apiKey": "ollama",
      "models": [
        {
          "id": "qwen3.8:27b-mtp-q4_K_M",
          "name": "Qwen 3.8 27B"
        }
      ]
    }
  }
}
```

- `baseUrl`: your Ollama endpoint, including `/v1`.
- `id`: the exact model tag returned by Ollama.
- `name`: a display label in Pi.
- `apiKey`: a placeholder for an endpoint without authentication; it does not secure the server. Use your endpoint's credentials if authentication is required.

This minimal entry uses Pi's default context and output limits. Add `contextWindow` and `maxTokens` if needed to match the model's effective limits; these fields do not reconfigure Ollama.

## 3. Add another existing model

Find its exact name in `/api/tags`, then add another object to the same `models` array:

```json
{ "id": "EXACT_MODEL_NAME_FROM_OLLAMA", "name": "My other model" }
```

Replace the placeholder and separate model objects with commas. Adding an entry registers a model with Pi; it does not create or download it in Ollama.

## 4. Select a model

After completing the [Pi setup](pi.md), start Pi from your project directory in a Bash terminal:

```bash
nono run --profile pi --allow . --allow "$HOME/.pi/agent" -- pi
```

Enter `/model` to reload the configuration and open the picker. Search for `qwen3.8:27b-mtp-q4_K_M` or another configured ID, select the `ollama` entry, and press Enter. To save it as the startup default, highlight the model and press `Ctrl+S`.

To select the model directly when launching Pi:

```bash
nono run --profile pi --allow . --allow "$HOME/.pi/agent" -- \
  pi --provider ollama --model qwen3.8:27b-mtp-q4_K_M
```

Replace the model ID to select another registered model. No `/login` is needed for an endpoint without authentication. Omit `--block-net` so Pi can reach Ollama.

## 5. Check the configuration

```bash
nono run --profile pi --allow "$HOME/.pi/agent" -- pi --list-models ollama
```

Your configured models should appear. Select one and send a short prompt to confirm it responds.

[Pi's compatible-endpoint configuration](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/models.md#configure-a-compatible-endpoint)
