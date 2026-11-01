# Configuração de mundos e fases (`assets/live-config/`)

Esta pasta contém a **configuração de mundos e fases** do Harvest Tile. Os mesmos
arquivos vivem em dois lugares:

1. **empacotados no app** (esta pasta é declarada em `pubspec.yaml`);
2. **publicados no gh-pages** — o que o app baixa em produção
   (`kRemoteBaseUrl`, em `lib/config/remote_config_constants.dart`).

## Como o app escolhe a fonte

|| Modo | Fonte | Cache |
| --- | --- | --- |
| `flutter run` (debug / dev mode) | **esta pasta**, lida do bundle — sem rede | ignorado |
| release (build publicado) | gh-pages | `shared_preferences`; se a rede falhar, usa o bundle |

**Fluxo em release:** o app abre com o **cache** (ou com o bundle, se for a primeira
vez), e em segundo plano baixa o `manifest.json` e **só os mundos cuja `version`
mudou**. JSON inválido ou rede fora: o app **descarta o arquivo e mantém o que já
tem** — nunca fica sem fases. Os erros aparecem no console com o prefixo
`[remote-config]`.

O manifesto é sempre buscado com `?v=<timestamp>` e cada mundo com
`?v=<version>` — é o cache-busting contra o CDN do GitHub.

**Depois de editar:** em dev basta **rebuildar** (o asset é copiado no build).
Em produção, **suba a `version` do mundo** no manifesto e no arquivo do mundo;
sem isso o app não baixa de novo.

## Estrutura de Arquivos

```
assets/live-config/
├── README.md              (este arquivo)
├── manifest.json
└── worlds/
    ├── world_001_v1.json
    ├── world_001_v2.json
    ├── world_001_v3.json  (Ilha Iniciante — versão atual)
    ├── world_002_v1.json  (Mundo 2)
    ├── world_003_v1.json  (Vulcão Ardente)
    ├── world_004_v1.json  (Castelo das Crônicas)
    └── world_005_v1.json  (Atol das Ruínas)
```

**Versionamento:** Os arquivos de mundo incluem a versão no nome (`world_XX_vN.json`).
Isso permite rollback fácil, histórico completo, A/B testing e rollout gradual.
Ver skill `world-versioning` para detalhes.

## Documentação Detalhada

Para documentação técnica detalhada, consulte as skills do projeto:

| Skill | Conteúdo |
|-------|----------|
| `seed-format` | Formato do código de seed (HT-...) |
| `world-versioning` | Sistema de versionamento de arquivos |
| `world-schema` | Schema completo de manifest.json e world_XX_vN.json |
| `level-fields` | Campos de cada fase, mecânicas e validação |

As skills são a **fonte da verdade** para conhecimento técnico do projeto.

## Como Testar

1. **Dev mode (automático):** rode o app em debug — ele lê esta pasta e ignora o
   cache. Edite um JSON, **rebuild** e o efeito aparece.
2. **JSON inválido:** quebre algo (ex.: `tileCount: 5`, ou apague o `seedCode`)
   e rebuild. O console mostra `mundo 1 descartado: …` e o mundo anterior
   continua valendo.
3. **Atualização de versão:** suba `version` do mundo (arquivo **e** manifesto) e
   rode em **release** (`flutter run --release`). Em dev o cache não entra, então
   a comparação de versões não se aplica.
4. **Offline:** em release, desligue a rede. Sem cache, o app usa esta pasta
   empacotada; com cache, mantém o último válido.
5. **Resetar o cache local:** `LevelRepository.instance.clearCache()`.

## Visão Geral Rápida

### manifest.json

Índice dos mundos publicados. Lista quais arquivos de mundo usar e suas versões.

```json
{
  "configVersion": 1,
  "minAppVersion": "1.0.0",
  "worlds": [
    { "id": 1, "file": "worlds/world_001_v3.json", "version": 3 },
    { "id": 2, "file": "worlds/world_002_v1.json", "version": 1 },
    { "id": 3, "file": "worlds/world_003_v1.json", "version": 1 },
    { "id": 4, "file": "worlds/world_004_v1.json", "version": 1 },
    { "id": 5, "file": "worlds/world_005_v1.json", "version": 1 }
  ]
}
```

### world_XX_vN.json

Configuração de um mundo com suas fases. Contém:
- Nome do mundo (ex: "Ilha Iniciante")
- Bioma (tema visual do mapa)
- Lista de fases com seedCodes, dificuldade, recompensas, etc.
- Baús de recompensa por fase (`chests`, opcional): lista de **1 a 3** objetos
  `{ "type": "gold", "amount": N }` (🪙). O mapa da campanha desenha uma **bolha
  flutuante por baú** e o jogador coleta uma a uma (o baú só fica coletável com
  a fase concluída). O save guarda apenas **se o baú já foi coletado** — a
  recompensa vem daqui.

**Nota:** O campo `version` NÃO está no JSON. A versão vem do manifest.json
(entry.version) e do nome do arquivo (world_XX_vN.json). Isso evita redundância.

```json
{
  "worldId": 1,
  "name": "Ilha Iniciante",
  "biome": "island",
  "levels": [
    {
      "id": 1,
      "seedCode": "HT-1x3x1-M3-M-1777659001000000",
      "tileCount": 3,
      "rewardCoins": 25
    }
  ]
}
```

### seedCode

Código que define completamente uma fase:
- Grade (largura × altura × camadas)
- Tamanho de match (M3 = match-3)
- Template (forma do tabuleiro, opcional)
- Dificuldade (E/M/H)
- Seed aleatória (reprodutibilidade)

Ver skill `seed-format` para anatomia completa.

## Biomas (Temas Visuais)

O bioma define o tema do mundo nas DUAS telas — o mapa da campanha e o tabuleiro:
- `island` - Ilha (padrão)
- `forest` - Floresta
- `mountain` - Montanha
- `castle` - Castelo
- `ruins` - Ruínas

O bioma é usado para montar as cores do cenário, da trilha, do banner e as decorações do mapa; no tabuleiro (tela da fase) ele dá o fundo (degradê + manchas de cor — sem emoji no fundo, porque as peças já são emojis), a cor/ícone do header, o destaque dos selos e **os emojis das peças** (cada bioma tem o próprio pool de tiles — o tabuleiro e o título da home mudam de mundo para mundo).

## Acessando as Skills

As skills estão em `.skills/` e são automaticamente disponíveis para IAs:
- `.skills/seed-format/`
- `.skills/world-versioning/`
- `.skills/world-schema/`
- `.skills/level-fields/`

Ver `.skills/README.md` para mais informações sobre o sistema de skills.
