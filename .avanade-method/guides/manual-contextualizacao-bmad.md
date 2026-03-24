# Manual de Contextualização BMAD/Avanade Method para Novos Projetos

> **Versão**: 1.0 | **Data**: 18/Mar/2026 | **Baseado em**: Contextualização FTD Educação  
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
│   │   ├── avanade-supervisor.agent.md
│   │   ├── tiago-dev.agent.md
│   │   ├── carla-qa.agent.md
│   │   └── ... (10 agentes)
│   └── prompts/                             ← [VERIFICAR] Prompts (.prompt.md)
│
├── .avanade-method/
│   ├── config.yaml                          ← [ALTERAR] Config principal unificada
│   ├── _base/
│   │   ├── avanade-master.md                ← [NÃO ALTERAR] Template base
│   │   ├── config.yaml                      ← [NÃO ALTERAR] Config base
│   │   └── INHERITANCE-ARCHITECTURE.md      ← [NÃO ALTERAR] Doc de herança
│   ├── agents/                              ← [ALTERAR] Customizações por agente
│   │   ├── supervisor.customize.yaml
│   │   ├── tiago-dev.customize.yaml
│   │   └── ... (10 customize.yaml)
│   ├── configs/
│   │   ├── d365-config.yaml                 ← [ALTERAR] Config D365 com dados reais
│   │   └── ... (configs auxiliares)
│   ├── docs/                                ← [CRIAR] Documentos do projeto
│   │   ├── {projeto}-knowledge-base.md
│   │   ├── discovery/
│   │   ├── stories/
│   │   ├── prd/
│   │   └── roadmap/
│   ├── memory/                              ← [VERIFICAR] Memória dos agentes
│   ├── templates/                           ← [VERIFICAR] Templates disponíveis
│   ├── workflows/                           ← [VERIFICAR] Workflows disponíveis
│   ├── checklists/                          ← [VERIFICAR] Checklists
│   ├── standards/                           ← [VERIFICAR] Padrões de dev
│   ├── tasks/                               ← [VERIFICAR] Tasks reutilizáveis
│   └── guides/                              ← [VERIFICAR] Guias
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
  # + adicionar qualquer spec principal do projeto

# ═══ SEÇÃO: DEV SETTINGS ═══
devStoryLocation: docs/stories  # onde ficam as stories
```

**Não alterar:** `version`, `_base/`, workflows, tasks, checklists (são genéricos).

---

### 1.2 `.github/copilot-instructions.md` — Entry Point VSCode
> **Prioridade**: 🔴 Segundo arquivo  
> **Quem altera**: Supervisor

**O que alterar:**

```markdown
## Contexto do Projeto: [NOME DO CLIENTE]
**LEITURA OBRIGATÓRIA antes de qualquer tarefa:**
- `docs/{projeto}-knowledge-base.md` - [descrição]
- `docs/discovery/{projeto}-discovery.md` - [descrição]
- [+ specs do projeto]
- [+ config D365 se aplicável]
- [+ diretrizes BCA se D365]
- [+ inventário de docs se houver]

**Stack**: [tecnologias do projeto]
**Tipo**: Brownfield/Greenfield

## Personas MCP Ativas:
[manter lista padrão dos 9 agentes + supervisor]
```

---

### 1.3 `.github/instructions/{projeto}-context.instructions.md` — Contexto Global
> **Prioridade**: 🔴 Terceiro arquivo — CRIAR novo  
> **Quem altera**: Supervisor  
> **Efeito**: Injetado automaticamente em TODOS os arquivos pelo VS Code via `applyTo`

**Template:**

```markdown
---
applyTo: "**"
---

# [Nome do Cliente] - Contexto Global

**Projeto**: [Nome completo e tipo]
**Stack**: [Tecnologias]
**Tipo**: Brownfield/Greenfield

## Docs Mandatórios (ler antes de qualquer tarefa)
- [lista de docs mandatórios com caminhos relativos]

## Contexto Chave
- **[Feature principal]**: [descrição curta]
- **Pain point principal**: [descrição do problema]
- **Integrações**: [lista de sistemas]
- **Ambientes**: [lista de ambientes]

## Padrões de Desenvolvimento [se aplicável]
- **Backend**: [padrão]
- **Frontend**: [padrão]
- **Testing**: [padrão]
- **Pipelines**: [padrão]
- **Fluxo**: [padrão de trabalho diário]
- **Referência completa**: [caminho para doc de padrões]
```

---

## FASE 2: Configuração Específica da Plataforma

### 2.1 `.avanade-method/configs/d365-config.yaml` — Config D365 (se aplicável)
> **Quando**: Projeto usa Dynamics 365 CE  
> **Quem altera**: Wilson (Architect) ou Supervisor

**O que alterar:**

```yaml
project_context:
  client:
    name: "[Nome]"
    group: "[Grupo empresarial]"
    industry: "[Indústria]"
    headquarters: "[Localização]"
  business_context:
    peak_season: "[Período de pico]"
    sales_model: "[Modelo de vendas]"
    critical_processes: [lista]
  crm_current_state:
    users: [número]
    entities_customized: [lista]
    integrations: [lista]
  environments:
    dev: "[URL]"
    test: "[URL]"
    prod: "[URL]"
  pain_points: [lista com scores de severidade]
```

### 2.2 `.github/instructions/avanade-bca-guidelines.instructions.md` — Diretrizes BCA (se D365)
> **Quando**: Projeto usa Dynamics 365 CE com BCA  
> **Quem altera**: Supervisor / Wilson

**Template com frontmatter:**

```markdown
---
applyTo: "**/*.cs,**/*.ts,**/*.js,**/*.yaml,**/*.yml,**/*.xml,**/*.json,**/*.ps1"
---

# Avanade BCA — Diretrizes de Desenvolvimento
[Resumo condensado das regras BCA aplicáveis ao projeto]
```

**Seções obrigatórias:**
1. Backend (Plugin→Service→Repository)
2. Frontend (TypeScript, Contract/Controller)
3. Testing (Shift-Left, frameworks)
4. Pipelines (4 YAML)
5. Fluxo de Trabalho Diário
6. Architecture & Repo
7. Code Review Checklist

**Fonte**: Se o cliente Avanade forneceu docs BCA, ler todos, categorizar em inventário (`diretriz-avanade-inventory.md`) e condensar neste arquivo.

---

## FASE 3: Documentos do Projeto (criar conforme necessidade)

### 3.1 Knowledge Base — `docs/{projeto}-knowledge-base.md`
> **Quando**: SEMPRE — é o doc mais importante  
> **Quem cria**: Maria (Analyst) a partir de transcrições, docs do cliente, reuniões  
> **Fonte típica**: Transcrições de KT, docs existentes, entrevistas

**Conteúdo mínimo:**
- Processos de negócio (AS-IS)
- Integrações existentes
- Ambiente técnico atual
- Stakeholders e equipe
- Glossário de termos do negócio

### 3.2 Discovery — `docs/discovery/{projeto}-discovery.md`
> **Quando**: SEMPRE  
> **Quem cria**: Maria (Analyst)  
> **Fonte**: Reuniões de discovery, refinamento

**Conteúdo mínimo:**
- Fit-gap analysis
- Pain points priorizados
- Requisitos identificados
- Gaps de investigação

### 3.3 Especificações — `docs/especificacao-*.md`
> **Quando**: Quando há specs do cliente (Notion, Confluence, Word)  
> **Quem cria**: Maria ou Supervisor extrai do sistema do cliente

### 3.4 PRDs — `docs/prd/prd-{escopo}.md`
> **Quando**: Após discovery/refinamento  
> **Quem cria**: João (PM) — usar template `templates/d365-prd-template.md` ou `templates/prd-template.yaml`

### 3.5 Stories — `docs/stories/{projeto}-stories-{escopo}.md`
> **Quando**: Após PRD ou spec aprovada  
> **Quem cria**: Paula (PO) — usar template `templates/d365-story-template.md` ou `templates/story-template.yaml`

### 3.6 Roadmap — `docs/roadmap/{projeto}-roadmap.md`
> **Quando**: Opcional, quando há planejamento multi-sprint  
> **Quem cria**: João (PM) / Supervisor

---

## FASE 4: Customização dos Agentes

### 4.1 Agent Definitions — `.github/agents/*.agent.md`
> **Quando**: Raramente — são genéricos  
> **O que alterar**: Apenas se precisar adicionar contexto de projeto no `modeInstructions`

**Normalmente NÃO mexer.** Os agentes leem contexto via `config.yaml` → `devLoadAlwaysFiles`.

### 4.2 Agent Customizations — `.avanade-method/agents/*.customize.yaml`
> **Quando**: Para dar contexto específico de projeto a agentes-chave  
> **Quem altera**: Supervisor

**Agentes que TIPICAMENTE precisam de customização:**

| Agente | Quando customizar | O que adicionar |
|--------|-------------------|-----------------|
| `tiago-dev.customize.yaml` | Sempre (se há código) | Padrões de dev específicos (BCA, frameworks), referência a guidelines |
| `carla-qa.customize.yaml` | Sempre | Referência a padrões de code review, checklists BCA |
| `wilson-architect.customize.yaml` | Se há decisões técnicas | Padrões de arquitetura, tools obrigatórias |
| `maria-analyst.customize.yaml` | Raramente | Contexto geralmente vem dos docs |
| `joao-pm.customize.yaml` | Raramente | Idem |
| `paula-po.customize.yaml` | Raramente | Idem |
| `roberto-sm.customize.yaml` | Raramente | Idem |
| `sofia-ux.customize.yaml` | Se há UI guidelines | Design system, acessibilidade |
| `paige-tech-writer.customize.yaml` | Se há doc standards | Padrões de doc do cliente |
| `supervisor.customize.yaml` | Se há projeto específico no greeting | Contexto do projeto no step 9 |

**Formato de customização (seção `core_principles`):**

```yaml
core_principles:
  - "[princípios existentes - NÃO REMOVER]"
  - "NOVO: [regra específica do projeto]"
  - "REFERÊNCIA: [caminho para doc de referência]"
```

### 4.3 Agent Memory — `.avanade-method/memory/*.md`
> **Quando**: Automaticamente preenchido pelos agentes durante uso  
> **O que fazer**: Verificar se os templates existem (1 por agente)

**Não alterar manualmente.** Os agentes populam conforme trabalham.

---

## FASE 5: Checklist de Validação

### Antes de começar a usar o BMAD no projeto, valide:

#### Configuração Core
- [ ] `config.yaml` atualizado com dados do projeto
- [ ] `copilot-instructions.md` com lista de docs mandatórios
- [ ] `{projeto}-context.instructions.md` criado com `applyTo: "**"`
- [ ] `devLoadAlwaysFiles` com todos os docs mandatórios

#### Documentos Mínimos
- [ ] Knowledge Base criado (mesmo que inicial)
- [ ] Discovery criado (mesmo que parcial)
- [ ] Config específica da plataforma (d365-config.yaml, etc.)

#### Agentes (se necessário)
- [ ] `tiago-dev.customize.yaml` com padrões de dev do projeto
- [ ] `carla-qa.customize.yaml` com referência a checklists
- [ ] `wilson-architect.customize.yaml` se há decisões de arquitetura

#### Padrões de Dev (se aplicável)
- [ ] `avanade-bca-guidelines.instructions.md` criado (D365)
- [ ] Inventário de docs do cliente (se fornecidos)

---

## FASE 6: Fluxo de Alimentação Contínua

Após setup inicial, o BMAD é alimentado continuamente:

```
Transcrição de reunião → Maria (Analyst)
  ├→ Atualiza knowledge-base.md
  ├→ Atualiza discovery.md
  └→ Cria/atualiza especificações

Especificação aprovada → João (PM) + Paula (PO)
  ├→ João cria PRD (docs/prd/)
  └→ Paula cria Stories (docs/stories/)

Refinamento técnico → Wilson (Architect)
  └→ Cria ADRs e decisões (docs/arquitetura/)

Sprint começa → Roberto (SM)
  └→ Sprint planning (sprint-status.yaml)

Implementação → Tiago (Dev)
  └→ Código seguindo padrões BCA

Review → Carla (QA) + Tiago
  └→ Code review via checklist BCA

Documentação → Paige (Tech Writer)
  └→ Docs técnicos quando solicitado
```

---

## Referência Rápida: Arquivos por Tipo de Ação

### "Preciso contextualizar um novo projeto"
1. `.avanade-method/config.yaml` — dados do projeto
2. `.github/copilot-instructions.md` — docs mandatórios
3. `.github/instructions/{projeto}-context.instructions.md` — **CRIAR**
4. `.avanade-method/configs/d365-config.yaml` — se D365
5. `docs/{projeto}-knowledge-base.md` — **CRIAR**
6. `docs/discovery/{projeto}-discovery.md` — **CRIAR**

### "Preciso adicionar padrões de desenvolvimento"
1. `.github/instructions/avanade-bca-guidelines.instructions.md` — **CRIAR**
2. `.avanade-method/agents/tiago-dev.customize.yaml` — adicionar referência
3. `.avanade-method/agents/carla-qa.customize.yaml` — adicionar referência
4. `.avanade-method/agents/wilson-architect.customize.yaml` — adicionar referência
5. `.avanade-method/config.yaml` → `devLoadAlwaysFiles` — adicionar guidelines

### "Preciso gerar stories/PRD a partir de uma transcrição"
1. Coloque a transcrição em `docs/transcriçao/`
2. Peça à Maria para atualizar discovery/knowledge-base
3. Peça à Paula para gerar stories (referencia specs + discovery)
4. Peça ao João para gerar PRD (referencia specs + stories)

### "Preciso adicionar uma nova spec do cliente"
1. Salve em `docs/especificacao-{nome}.md`
2. Adicione ao `devLoadAlwaysFiles` em `config.yaml`
3. Adicione à lista de docs mandatórios em `copilot-instructions.md`
4. Adicione à lista em `{projeto}-context.instructions.md`

---

## Arquivos que NUNCA devem ser alterados

| Arquivo | Razão |
|---------|-------|
| `.avanade-method/_base/avanade-master.md` | Template base de todos agentes — herança |
| `.avanade-method/_base/config.yaml` | Config base — sobrescrito pelo config.yaml raiz |
| `.avanade-method/workflows/*.workflow.md` | Workflows genéricos — servem para qualquer projeto |
| `.avanade-method/workflows/WF-*.yaml` | Workflow manifests genéricos |
| `.avanade-method/tasks/*.task.md` | Tasks reutilizáveis — genéricas |
| `.avanade-method/checklists/*.md` | Checklists padrão Avanade |
| `.avanade-method/standards/*.md` | Padrões genéricos |
| `.avanade-method/templates/*.md` | Templates para geração de docs |
| `.github/agents/*.agent.md` | Definições de agentes (raramente alterar) |

---

## Exemplo Completo: Contextualização FTD Educação

### Arquivos CRIADOS (novos):
| Arquivo | Quem criou | Propósito |
|---------|-----------|-----------|
| `.github/instructions/ftd-context.instructions.md` | Supervisor | Contexto global com `applyTo: "**"` |
| `.github/instructions/avanade-bca-guidelines.instructions.md` | Supervisor | 180 linhas de regras BCA condensadas |
| `docs/ftd-knowledge-base.md` | Maria | ~350 linhas de processos, integrações, glossário |
| `docs/discovery/ftd-discovery.md` | Maria | Discovery com fit-gap, 16 seções |
| `docs/especificacao-simulador-notion.md` | Supervisor | Spec do Simulador extraída do Notion (~500 linhas) |
| `docs/especificacao-aprovacao-notion.md` | Supervisor | Spec de Aprovação extraída do Notion |
| `docs/diretriz-avanade-inventory.md` | Subagent | Inventário de 197 docs BCA categorizados |
| `docs/roadmap/ftd-roadmap-sugestao.md` | Supervisor | Roadmap 6 fases, 41 itens |
| `docs/stories/ftd-stories-refinamento.md` | Paula | 6 épicos, 22 stories, 104 tasks |
| `docs/prd/prd-refinamento-processo.md` | João | PRD D365 completo com 13 seções |

### Arquivos ALTERADOS (existentes):
| Arquivo | O que mudou |
|---------|-------------|
| `.avanade-method/config.yaml` | `project:`, `devLoadAlwaysFiles` com docs FTD |
| `.github/copilot-instructions.md` | Lista de leitura obrigatória com 6 docs FTD |
| `.avanade-method/configs/d365-config.yaml` | Dados reais FTD (pain points, ambientes, integrações) |
| `.avanade-method/agents/tiago-dev.customize.yaml` | +2 princípios BCA |
| `.avanade-method/agents/carla-qa.customize.yaml` | +2 princípios BCA |
| `.avanade-method/agents/wilson-architect.customize.yaml` | +2 princípios BCA |

### Arquivos NÃO alterados:
- `_base/*` (avanade-master.md, config.yaml)
- `workflows/*`, `tasks/*`, `checklists/*`, `standards/*`, `templates/*`
- `.github/agents/*.agent.md` (definições de agentes)
- `memory/*.md` (preenchidos automaticamente)
