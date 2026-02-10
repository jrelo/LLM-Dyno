# LLM-Dyno

Single-file large language model chat console for [Ollama](https://ollama.com). No build step, no dependencies. Open the HTML and connect to your local models.

<img width="2047" height="988" alt="image" src="https://github.com/user-attachments/assets/343f84df-464b-47df-acd7-9f5621e4ef6c" />

## Quick Start

1. [Install Ollama](https://ollama.com/download)
2. Pull a model: `ollama pull llama3.2`
3. Open `llm-dyno.html` in a browser

Ollama serves on `localhost:11434` by default. LLM-Dyno connects there automatically. Change the URL in settings if your server lives elsewhere on the network.

## Loading GGUF Models from HuggingFace

Most models on HuggingFace ship as `.gguf` files. To run one in Ollama, create a **Modelfile** that points at it:

```
FROM ./mistral-7b-instruct-v0.3.Q5_K_M.gguf
```

That's it. One line. Then import it:

```sh
ollama create mistral-7b -f Modelfile
```

You can add optional parameters and a chat template if the model needs one:

```
FROM ./deepseek-coder-6.7b-instruct.Q4_K_M.gguf

PARAMETER temperature 0.7
PARAMETER top_p 0.9
PARAMETER stop "<|EOT|>"

TEMPLATE """{{- if .System }}{{ .System }}
{{ end }}### Instruction:
{{ .Prompt }}
### Response:
"""
```

Most well-known models (Llama, Mistral, Qwen, Phi, Gemma) have their chat template baked into the GGUF metadata, so the single `FROM` line usually works. Add a `TEMPLATE` block when Ollama doesn't pick up the format automatically or when you want to override it.

```sh
ollama create deepseek-coder -f Modelfile
ollama run deepseek-coder   # quick sanity check from the terminal
```

The model now appears in Dyno's dropdown.

## Features

- Streaming chat with token-by-token debug view
- Real-time <think> tags view
- Full parameter control (temperature, top_p, top_k, repeat penalty, seed)
- Editable system prompt
- Raw API inspector, per-request stats, message history, model info
- Session export to JSON
- All settings persist in localStorage
