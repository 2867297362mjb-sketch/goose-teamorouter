# goose-teamorouter

[goose](https://github.com/aaif-goose/goose) custom provider files for [TeamoRouter](https://teamorouter.com), a pay-as-you-go gateway with one API key for Claude, GPT, Gemini, DeepSeek, GLM, Kimi, and Qwen models.

> Maintained by TeamoRouter (the author works at TeamoRouter). Not affiliated with the goose project.

goose recommends that LLM services publish a custom-provider file that users install themselves ([CONTRIBUTING](https://github.com/aaif-goose/goose/blob/main/CONTRIBUTING.md#llm-provider-integrations)). This repository is that file.

## Files

| File | goose provider | Endpoint | Use it for |
|---|---|---|---|
| `teamorouter.json` | `teamorouter` | OpenAI-compatible, `https://api.teamorouter.com/v1` | Every model, Claude included. The model list is fetched live from the API. |
| `teamorouter_claude.json` | `teamorouter_claude` | Anthropic-compatible Messages, `https://api.teamorouter.com` | Claude models on the native Anthropic API (prompt caching, native tool use). |

Install one or both; they read the same key.

## Install

macOS / Linux:

```bash
mkdir -p ~/.config/goose/custom_providers
curl -fsSL https://raw.githubusercontent.com/2867297362mjb-sketch/goose-teamorouter/main/teamorouter.json \
  -o ~/.config/goose/custom_providers/teamorouter.json
curl -fsSL https://raw.githubusercontent.com/2867297362mjb-sketch/goose-teamorouter/main/teamorouter_claude.json \
  -o ~/.config/goose/custom_providers/teamorouter_claude.json
```

Windows: put the same files in `%APPDATA%\Block\goose\config\custom_providers\`.

## Use

1. Create an API key in the [TeamoRouter dashboard](https://teamorouter.com/dashboard).
2. Either export it:

   ```bash
   export TEAMOROUTER_API_KEY="your-api-key"
   goose session --provider teamorouter --model deepseek-v4-flash
   goose session --provider teamorouter_claude --model claude-sonnet-5-5
   ```

   or open goose Desktop → Settings → Models, pick **TeamoRouter**, and paste the key, which goose stores in its keychain.

Model IDs are listed at [teamorouter.com/models](https://teamorouter.com/models). The machine-readable version, with context windows and prices, is at [api.teamorouter.com/v1/models/pricing](https://api.teamorouter.com/v1/models/pricing).

## What the files send

- Your API key, to `api.teamorouter.com` only.
- `X-Client-Source: goose-teamorouter` on every request, so TeamoRouter can tell which traffic comes from goose. Delete the `headers` block if you don't want it.

## Tested

Tested with goose 1.58.0 (`goose run --provider ... --model ... -t "Read the file hello.txt ..."`). Each model read the file with the shell tool and returned its contents:

- `teamorouter`: `deepseek-v4-flash`, `claude-sonnet-5-5`, `glm-5.3`.
- `teamorouter_claude`: `claude-sonnet-5-5`.

## License

MIT
