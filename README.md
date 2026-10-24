# ADK Multi-Tool Agent

Estudo do [Google Agent Development Kit (ADK)](https://google.github.io/adk-docs/) para entender e debugar os eventos e o fluxo de chamada de tools de um agent.

O agent `weather_time_agent` usa o modelo `gemini-flash-latest` e expõe duas tools:

- `get_weather(city)` retorna um relatório de clima (mockado, só responde para "New York")
- `get_current_time(city)` retorna a hora atual na cidade (só "New York", via `zoneinfo`)

## Estrutura

```
.
├── parent_folder/
│   └── multi_tool_agent/
│       ├── __init__.py
│       ├── agent.py      # tools + definição do root_agent
│       └── .env          # GOOGLE_API_KEY (não versionado)
└── .gitignore
```

## Pré-requisitos

- Python 3.12+
- Uma API key do Gemini ([Google AI Studio](https://aistudio.google.com/apikey))

## Setup

```bash
python -m venv .venv
source .venv/bin/activate
pip install google-adk
```

Crie o arquivo `parent_folder/multi_tool_agent/.env`:

```env
GOOGLE_API_KEY=sua_chave_aqui
```

## Rodando

Os comandos do ADK são executados a partir de `parent_folder` (a pasta que contém o agent):

```bash
cd parent_folder

# interface web com o trace de eventos e chamadas de tools
adk web

# ou direto no terminal
adk run multi_tool_agent
```

Exemplos de perguntas:

- "What's the weather in New York?"
- "What time is it in New York?"
- "What's the weather in London?" (cai no caminho de erro da tool)
