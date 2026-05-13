# Riot Games API — Bruno Collection

Collection completa para desenvolvimento com a **Tournament API da Riot Games** para League of Legends, usando o [Bruno](https://www.usebruno.com/).

## 📁 Estrutura

```
LuB-Riot-API/
├── 01-setup/                        # Fluxo inicial: Provider → Tournament
│   ├── 01-create-provider.bru
│   └── 02-create-tournament.bru
├── 02-tournament-codes/             # Geração e consulta de tournament codes
│   ├── 01-generate-codes.bru
│   ├── 02-get-code-details.bru
│   └── 03-update-code.bru
├── 03-lobby-events/                 # Eventos do lobby antes da partida
│   └── 01-get-lobby-events.bru
├── 04-match-v5/                     # Dados de partidas completadas
│   ├── 01-get-match-by-id.bru
│   ├── 02-get-match-timeline.bru
│   └── 03-get-matches-by-puuid.bru
├── 05-summoner-account/             # Dados de jogadores
│   ├── 01-get-account-by-riot-id.bru
│   ├── 02-get-summoner-by-puuid.bru
│   └── 03-get-league-entries.bru
├── 06-callback-simulation/          # Simula callbacks da Riot no seu backend
│   ├── 01-simulate-tournament-callback.bru
│   └── 02-simulate-callback-team-blue-wins.bru
├── 07-spectator/                    # Partidas ao vivo
│   └── 01-get-active-game.bru
├── environments/
│   └── example.bru                  ← ✅ Versionar (template sem valores reais)
├── .gitignore                       # Protege stub.bru e production.bru
└── bruno.json
```

## 🚀 Configuração Inicial

### 1. Instalar o Bruno
```bash
# macOS
brew install --cask bruno

# Windows/Linux: https://www.usebruno.com/downloads
```

### 2. Clonar e abrir no Bruno
```bash
git clone https://github.com/luanscps/LuB-Riot-API.git
cd LuB-Riot-API
# Abra o Bruno → Open Collection → selecione esta pasta
```

### 3. Criar seu environment
```bash
cp environments/example.bru environments/stub.bru
# Edite stub.bru e cole sua RGAPI- key
```

### 4. Obter sua API Key (Development)
Acesse [developer.riotgames.com](https://developer.riotgames.com), faça login com sua conta Riot e gere sua **Development Key** (válida por 24h, renovável).

> ⚠️ A Development Key usa o `tournament-stub-v5` — os códigos gerados **não funcionam** no cliente real do LoL, mas todo o fluxo de backend funciona normalmente para testes.

## 🔄 Ordem de Execução (Fluxo Completo)

Execute as requests **nesta ordem** na primeira vez:

1. `05-summoner-account/01` → obtém PUUID
2. `05-summoner-account/02` → obtém summoner_id
3. `01-setup/01` → registra callback URL + salva provider_id
4. `01-setup/02` → cria torneio + salva tournament_id
5. `02-tournament-codes/01` → gera codes + salva tournament_code
6. `02-tournament-codes/02` → valida o código gerado
7. `06-callback-simulation/01` → testa seu backend Next.js

Os scripts `after-response` salvam automaticamente os IDs nos env vars.

## 🧪 Rodando via CLI

```bash
# Instalar CLI do Bruno
npm install -g @usebruno/cli

# Rodar toda a collection
bru run --env stub

# Rodar apenas uma pasta
bru run 01-setup --env stub

# Output detalhado
bru run --env stub --reporter cli
```

## ⚠️ Rate Limits

| Tipo de Key       | Requests/s | Requests/2min |
|-------------------|-----------|----------------|
| Development       | 20        | 100            |
| Production (base) | 500       | 30.000         |

## 🔐 Segurança — .gitignore

Jamais commite sua API key! O `.gitignore` já está configurado para ignorar:
```
environments/stub.bru
environments/production.bru
environments/local.bru
```

Versione apenas `environments/example.bru`.

## 📚 Referências

- [Riot Developer Portal](https://developer.riotgames.com/apis)
- [Docs LoL API](https://developer.riotgames.com/docs/lol)
- [Bruno — API Client](https://www.usebruno.com/)
- [Bruno CLI (@usebruno/cli)](https://www.npmjs.com/package/@usebruno/cli)
