# ADK Multi-Agent

Estudo do [Google Agent Development Kit (ADK)](https://google.github.io/adk-docs/) para entender e debugar os eventos e o fluxo de chamada de tools de agents.

Todos os agents ficam em `agents/`, então um único `adk web` mostra todos eles no seletor.

## Agents

### `multi_tool_agent`

O agent `weather_time_agent` usa o modelo `gemini-flash-latest` e expõe duas tools:

- `get_weather(city)` retorna um relatório de clima (mockado, só responde para "New York")
- `get_current_time(city)` retorna a hora atual na cidade (só "New York", via `zoneinfo`)

### `google_search_agent`

O agent `basic_search_agent` usa a tool nativa `google_search` para responder com grounding na busca do Google. Usa um modelo com suporte a Live API (streaming).

## Estrutura

```
.
├── agents/                      # rode o adk a partir daqui
│   ├── multi_tool_agent/
│   │   ├── __init__.py
│   │   ├── agent.py             # tools + definição do root_agent
│   │   └── .env                 # não versionado
│   └── google_search_agent/
│       ├── __init__.py
│       ├── agent.py             # root_agent com google_search
│       └── .env                 # não versionado
└── .gitignore
```

Para adicionar um novo agent, crie uma pasta em `agents/` com `__init__.py` (`from . import agent`) e um `agent.py` que define `root_agent`.

## Pré-requisitos

- Python 3.12+
- Uma API key do Gemini ([Google AI Studio](https://aistudio.google.com/apikey))

## Setup

```bash
python -m venv .venv
source .venv/bin/activate
pip install google-adk
```

Crie um `.env` em cada pasta de agent (`agents/<agent>/.env`):

```env
GOOGLE_API_KEY=sua_chave_aqui
```

## Rodando

Os comandos do ADK são executados a partir de `agents/` (a pasta que contém os agents):

```bash
cd agents

# interface web com todos os agents no seletor + trace de eventos e chamadas de tools
adk web

# ou direto no terminal
adk run multi_tool_agent
adk run google_search_agent
```

Também dá para rodar da raiz com `adk web agents`.

Exemplos de perguntas para o `multi_tool_agent`:

- "What's the weather in New York?"
- "What time is it in New York?"
- "What's the weather in London?" (cai no caminho de erro da tool)
