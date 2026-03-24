# Manual de Contextualização BMAD/Avanade Method para Novos Projetos

> **Versão**: 1.1 | **Data**: 24/Mar/2026 | **Contexto**: Consolidação Root Docs
> **Objetivo**: Guia passo a passo de todos os arquivos que precisam ser criados ou alterados ao contextualizar o Avanade Method para um novo projeto/cliente.

---

## Visão Geral da Estrutura

```
projeto/
├── .github/
│   ├── copilot-instructions.md              ← [ALTERAR] Entry point do Copilot
│   ├── instructions/
│   │   ├── {projeto}-context.instructions.md ← [CRIAR] Contexto global do projeto
│   │   └── avanade-bca-guidelines.instructions.md ← [CRIAR se D365] Diretrizes BCA
│   ├── agents/                              ← [VERIFICAR] Agent definitions (.agent.md)
│   └── prompts/                             ← [VERIFICAR] Prompts (.prompt.md)
│
├── .avanade-method/
│   ├── config.yaml                          ← [ALTERAR] Config principal unificada
│   ├── _base/                               ← [NÃO ALTERAR] Core do método
│   ├── agents/                              ← [ALTERAR] Customizações por agente
│   ├── configs/                             ← [ALTERAR] Configs de ambientes (D365)
│   ├── memory/                              ← [AUTO] Memória dos agentes
│   ├── templates/                           ← [USAR] Templates de PRD/Story
│   ├── workflows/                           ← [USAR] Automações
│   └── guides/                              ← [VER] Manuais
│
├── docs/                                ← [CRIAR/MANTER] Documentos do Projeto (RAIZ)
│   ├── {projeto}-knowledge-base.md
│   ├── arquitetura/                         ← [TÉCNICO] Specs e Onboarding Técnico
│   ├── discovery/                           ← [ANALISTA] Fits, Gaps e Atas
│   ├── framework/                           ← [MÉTODO] Como o projeto é gerido
│   ├── prd/                                 ← [PM] Specs de Negócio
│   ├── stories/                             ← [PO] User Stories
│   └── transcriçao/                         ← [BRUTO] Atas e áudios brutos
└── README.md
```

---

## FASE 1: Configuração Base (Obrigatória)

### 1.1 `.avanade-method/config.yaml` — Config Principal
> **Prioridade**: 🔴 PRIMEIRO arquivo a alterar  
> **Quem altera**: Supervisor

**O que alterar:**

```yaml
# ═══ SEÇÃO: PROJECT CONTEXT ═══
project:
  client: "[Nome do Cliente]"
  type: brownfield | greenfield
  platform: "dynamics-365-ce-online | power-platform | azure | custom"
  stack: "[Lista de tecnologias]"

# ═══ SEÇÃO: MANDATORY READING ═══
devLoadAlwaysFiles:
  - docs/{projeto}-knowledge-base.md
  - docs/discovery/{projeto}-discovery.md
  - .avanade-method/configs/d365-config.yaml           # se D365
  - .github/instructions/avanade-bca-guidelines.instructions.md  # se D365
  # + adicionar qualquer spec principal do projeto (ex: docs/especificacao-x.md)

# ═══ SEÇÃO: DEV SETTINGS ═══
devStoryLocation: docs/stories  # onde ficam as stories
```

---

### 1.2 `.github/copilot-instructions.md` — Entry Point VSCode
> **Prioridade**: 🔴 Segundo arquivo  

**O que alterar:**

```markdown
## Contexto do Projeto: [NOME DO CLIENTE]
**LEITURA OBRIGATÓRIA antes de qualquer tarefa:**
- `docs/{projeto}-knowledge-base.md` - [descrição]
- `docs/discovery/{projeto}-discovery.md` - [descrição]
- `docs/arquitetura/` - Documentação técnica de apoio
```

---

## FASE 2: Configuração Específica da Plataforma

### 2.1 `.avanade-method/configs/d365-config.yaml`  
(Manter configuração conforme padrões do Wilson Architect)

### 2.2 `.github/instructions/avanade-bca-guidelines.instructions.md`  
(Condensar regras de desenvolvimento BCA aqui).

---

## FASE 3: Documentos do Projeto

> **IMPORTANTE**: Todos os documentos de negócio e técnicos devem residir na pasta **`docs/` na raiz**, fora de `.avanade-method/`. Não duplique a pasta docs.

### 3.1 Knowledge Base — `docs/{projeto}-knowledge-base.md`
É a "fonte da verdade" do negócio. Deve ser mantido pela Maria Analyst.

### 3.2 Discovery — `docs/discovery/`
Espaço para fit-gaps, atas de reuniões de descoberta e refinamentos de processo.

### 3.3 Arquitetura — `docs/arquitetura/`
**Obrigatório**: Colocar aqui manuais de onboarding técnico, desenhos de solução, modelos de dados e transcrições de KT Técnico.

### 3.4 Framework — `docs/framework/`
Documentação sobre o setup inicial do time, workflows de trabalho e normas de convivência técnica.

---

## FASE 4: Customização e Alimentação

Ao final de cada reunião/sessão:
1. Coloque os áudios/textos brutos em `docs/transcriçao/`.
2. Peça à Maria para atualizar os documentos em `docs/discovery/`.
3. Mova os conhecimentos técnicos (Diagramas, Onboarding) para `docs/arquitetura/`.
4. Garanta que o `config.yaml` aponta para os arquivos corretos.

---

## Checklist de Validação Final (Perfeição)

- [ ] **Consolidação**: Verificado se NÃO há pastas `docs/` dentro de `.avanade-method/`.
- [ ] **Links**: Todos os links no README.md e na Wiki apontam para `docs/` na raiz.
- [ ] **Onboarding**: Documentos técnicos e transcrições de setup estão em `docs/arquitetura/`.
- [ ] **Config**: `config.yaml` carrega os arquivos obrigatórios usando caminhos diretos.
- [ ] **Git**: Alterações comitadas localmente para evitar perdas em pull/merge.
