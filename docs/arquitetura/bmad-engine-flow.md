# Como Funciona o Motor BMAD
## Fluxo Completo de Prompt → Resposta no Projeto FTD Educação

**Finalidade**: Entender como sua customização e contextualização FTD é carregada e utilizada  
**Data**: 20/03/2026 | **Versão**: 1.0

---

## 1. VISÃO GERAL — O QUE É O BMAD ENGINE

O **BMAD (Breakthrough Method for Agile Delivery)** é um framework de agentes de IA onde cada "persona" (Tiago, Wilson, Maria...) é montada dinamicamente no momento em que você faz uma solicitação. **Não existe um agente carregado permanentemente** — ele é construído em tempo real, camada por camada, toda vez que você interage.

```mermaid
graph LR
    PROMPT["🧑 Seu Prompt"] --> ENGINE["⚙️ Motor BMAD\n(montagem dinâmica)"]
    ENGINE --> RESPOSTA["💬 Resposta\ncontextualizada FTD"]

    ENGINE -.- C1["Camada 1: Copilot Global"]
    ENGINE -.- C2["Camada 2: Instructions FTD"]
    ENGINE -.- C3["Camada 3: Agent Base (master)"]
    ENGINE -.- C4["Camada 4: Customize YAML"]
    ENGINE -.- C5["Camada 5: Memory do Agente"]
    ENGINE -.- C6["Camada 6: devLoadAlwaysFiles"]
    ENGINE -.- C7["Camada 7: Architecture Docs"]
```

---

## 2. FLUXO PASSO A PASSO — DO PROMPT À RESPOSTA

```mermaid
sequenceDiagram
    actor VOCÊ as 👤 Você
    participant VS as 🖥️ VS Code / Copilot
    participant CI as 📋 copilot-instructions.md
    participant FI as 📋 ftd-context.instructions.md
    participant AG as 🤖 Agente .agent.md
    participant BM as 📜 avanade-master.md (base)
    participant CY as ⚙️ customize.yaml (FTD)
    participant MEM as 🧠 memory.md
    participant DOCS as 📚 devLoadAlwaysFiles
    participant ARCH as 🏗️ docs/arquitetura/*

    VOCÊ->>VS: "Tiago, crie um plugin para validar CNPJ duplicado"
    
    Note over VS,CI: PASSO 1: Injeção Automática Global
    VS->>CI: Carrega .github/copilot-instructions.md
    VS->>FI: Carrega .github/instructions/ftd-context.instructions.md
    Note over CI,FI: applyTo: "**" → aplica em TUDO automaticamente

    Note over VS,AG: PASSO 2: Identificação do Agente
    VS->>AG: Carrega .github/agents/tiago-dev.agent.md
    AG-->>VS: Persona básica + menu + whenToUse

    Note over AG,BM: PASSO 3: Base Template (herança)
    AG->>BM: Carrega _base/avanade-master.md
    BM-->>AG: Protocolo de ativação, steps 1-6, regras base

    Note over AG,CY: PASSO 4: Customização FTD (override)
    AG->>CY: Carrega agents/tiago-dev.customize.yaml
    CY-->>AG: ftd_project_context + d365_dev_rules + testing_stack + base_rule + rule

    Note over AG,MEM: PASSO 5: Memória Persistente
    AG->>MEM: Carrega memory/tiago-dev.memory.md
    MEM-->>AG: Histórico de decisões anteriores, aprendizados

    Note over AG,DOCS: PASSO 6: Documentos Obrigatórios (devLoadAlwaysFiles)
    AG->>DOCS: ftd-knowledge-base.md
    AG->>DOCS: ftd-discovery.md
    AG->>DOCS: especificacao-simulador-notion.md
    AG->>DOCS: d365-config.yaml
    DOCS-->>AG: Contexto completo de negócio + técnico FTD

    Note over AG,ARCH: PASSO 7: Documentos de Arquitetura
    AG->>ARCH: d365-data-model.md → entidades disponíveis
    AG->>ARCH: padroes-desenvolvimento.md → Plugin→Service→Repository
    AG->>ARCH: stack-tecnologica.md → FakeXrmEasy, Early Bound, etc.
    ARCH-->>AG: Modelo de dados + padrões específicos FTD

    Note over AG,VOCÊ: PASSO 8: Geração da Resposta
    AG->>VOCÊ: Plugin C# com namespace FTD.Plugins.Account.PreCreate\nEarly Bound, depth check, FakeXrmEasy tests, ColumnSet correto
```

---

## 3. MAPA DE ARQUIVOS — O QUE É CARREGADO E QUANDO

```mermaid
graph TD
    subgraph GLOBAL["🌐 CAMADA 1 — Global (sempre, automático)"]
        COP[".github/copilot-instructions.md\n✅ Lido em TODA interação Copilot"]
        FTD[".github/instructions/ftd-context.instructions.md\napplyTo: ** → aplica em qualquer arquivo aberto"]
    end

    subgraph AGENT["🤖 CAMADA 2 — Agente (ao ativar @agente)"]
        AGT[".github/agents/tiago-dev.agent.md\nIdentidade, menu, whenToUse"]
        BASE[".avanade-method/_base/avanade-master.md\nProtocolo base, steps de ativação, regras gerais"]
    end

    subgraph CUSTOM["⚙️ CAMADA 3 — Customização FTD (merge com base)"]
        CUS[".avanade-method/agents/tiago-dev.customize.yaml\n• ftd_project_context ✅ NOVO\n• d365_dev_rules ✅ NOVO\n• testing_stack ✅ NOVO\n• architecture_docs ✅ NOVO\n• base_rule original ✅ RESTAURADO\n• rule especializada ✅ NOVO"]
    end

    subgraph MEMORY["🧠 CAMADA 4 — Memória (persistente entre sessões)"]
        MEM[".avanade-method/memory/tiago-dev.memory.md\nDecisões passadas, aprendizados, contexto acumulado"]
    end

    subgraph DOCS["📚 CAMADA 5 — Conhecimento FTD (devLoadAlwaysFiles)"]
        KB["docs/ftd-knowledge-base.md\n379 linhas: glossário, stakeholders, dores, KPIs"]
        DIS["docs/discovery/ftd-discovery.md\n347 linhas: 20 pain points, fit-gap, roadmap"]
        SIM["docs/especificacao-simulador-notion.md\nSimulador Comercial: 6 etapas, regras de cálculo"]
        D365[".avanade-method/configs/d365-config.yaml\n357 linhas: entidades, soluções, decision framework"]
    end

    subgraph ARCH["🏗️ CAMADA 6 — Arquitetura (docs/arquitetura/)"]
        SA["d365-solution-architecture.md\nC4 diagrams, ambientes, pipeline ALM, segurança"]
        DM["d365-data-model.md\nEntidades, campos, ERD, gaps"]
        IL["d365-integration-landscape.md\nTOTVS, ISA, Adobe, Lumisfera, fluxos"]
        PD["padroes-desenvolvimento.md\nPlugin→Service→Repository, PCF, PA, Azure Functions"]
        ST["stack-tecnologica.md\nStack completa, ferramentas, testing, decision matrix"]
    end

    GLOBAL --> AGENT
    AGENT --> CUSTOM
    CUSTOM --> MEMORY
    MEMORY --> DOCS
    DOCS --> ARCH
    ARCH --> RESP["💬 Resposta com contexto\nFTD + D365 + Azure completo"]
```

---

## 4. O QUE CADA ARQUIVO FAZ NO SEU PROMPT

### Quando você escreve: *"Crie um plugin para validar CNPJ duplicado"*

| Arquivo carregado | O que ele contribui para a resposta |
|-------------------|-------------------------------------|
| `copilot-instructions.md` | Identifica que estamos no projeto FTD, ativa modo Avanade Method |
| `ftd-context.instructions.md` | Lembra que é brownfield D365, que existe FTDMaxFlow, que há 9 solutions |
| `tiago-dev.agent.md` | Ativa a persona Tiago: Dev Full Stack, protocolo dev-story |
| `avanade-master.md` | Impõe o protocolo: ler story → implementar → testar → marcar [x] |
| `tiago-dev.customize.yaml` | Injeta: `depth_check`, `no_magic_strings`, `early_bound: OBRIGATÓRIO`, `testing_stack.plugins: FakeXrmEasy` |
| `tiago-dev.memory.md` | Lembra decisões anteriores (ex: qual namespacing já foi usado) |
| `ftd-knowledge-base.md` | Sabe que CNPJ duplicado é pain point #3, que existem 101K contas |
| `ftd-discovery.md` | Entende o contexto: Pre-Validation Stage 10 já é padrão no projeto |
| `d365-config.yaml` | Confirma: publisher = `ftd_`, solution = `FTDCore`, filtro = `ftd_cnpj` |
| `d365-data-model.md` | Usa a entidade `Account` com campo `ftd_cnpj` que já está documentado |
| `padroes-desenvolvimento.md` | Gera: `Plugin<T> → Service → Repository`, `[CrmPluginRegistration]`, `FakeXrmEasy` tests |

**Resultado**: Um plugin gerado com o namespace correto `FTD.Plugins.Account.PreValidate`, Early Bound, depth check, ColumnSet específico para `ftd_cnpj`, `InvalidPluginExecutionException` com mensagem em PT-BR, e testes FakeXrmEasy com cobertura ≥ 80%.

---

## 5. FLUXO DE DECISÃO DO AGENTE (Motor Interno)

```mermaid
flowchart TD
    START["Prompt recebido"] --> PERSONA{"Qual agente\né @mencionado?"}

    PERSONA -->|"@tiago-dev\nou implícito"| LOAD_T["Carrega contexto Tiago:\nd365_dev_rules + testing_stack\n+ solution_mapping"]
    PERSONA -->|"@wilson-architect"| LOAD_W["Carrega contexto Wilson:\nadrs_aceitos + adrs_pendentes\n+ architectural_risks"]
    PERSONA -->|"@supervisor\nou orquestrar"| LOAD_S["Carrega contexto Supervisor:\nrouting_knowledge\n+ environment_status\n+ critical_blockers"]
    PERSONA -->|"Nenhum agente\nespecífico"| LOAD_G["Usa copilot-instructions\n+ ftd-context global"]

    LOAD_T --> BASE_RULE["Aplica base_rule:\nLer docs mandatórios ANTES\nde gerar artefato"]
    LOAD_W --> BASE_RULE
    LOAD_S --> BASE_RULE
    LOAD_G --> BASE_RULE

    BASE_RULE --> LOAD_DOCS["Carrega devLoadAlwaysFiles:\nftd-knowledge-base\nftd-discovery\nd365-config\nespecificacao-simulador"]

    LOAD_DOCS --> LOAD_ARCH["Carrega architecture_docs:\nd365-data-model\npadroes-desenvolvimento\nstack-tecnologica\n..."]

    LOAD_ARCH --> SPECIALIST["Aplica rule especializada\ndo agente (foco de papel)"]

    SPECIALIST --> CHECK{"O prompt é sobre\ncomponenteD365?"}

    CHECK -->|"Sim: Plugin C#"| R1["Usa: Plugin→Service→Repository\nEarly Bound, FakeXrmEasy\nStage correto (10/20/40)"]
    CHECK -->|"Sim: Power Automate"| R2["Usa: FTD-[Módulo]-[Ação]-[Trigger]\nScope Try-Catch-Finally\nConnection References"]
    CHECK -->|"Sim: Azure Function"| R3["Usa: .NET 8 Isolated\nManaged Identity + Key Vault\nPolly retry, Correlation ID"]
    CHECK -->|"Sim: Power Pages"| R4["Usa: Cache-first JS\nEntra ID auth\nMobile First (404 consultores)"]
    CHECK -->|"Negócio/Processo"| R5["Usa: Jornada FTD\nGlossário (Safra, Anja, Alçada)\n20 pain points + roadmap"]

    R1 --> RESP["✅ Resposta gerada\ncom contexto FTD + D365 completo"]
    R2 --> RESP
    R3 --> RESP
    R4 --> RESP
    R5 --> RESP
```

---

## 6. HIERARQUIA DE CONTEXTO — QUEM "GANHA" EM CONFLITO

Quando há informações conflitantes entre arquivos, esta é a ordem de **prioridade** (maior = mais autoritativo):

```
┌─────────────────────────────────────────────────────────────────┐
│  PRIORIDADE 1 (MAIS ALTO) — customize.yaml                      │
│  ftd_project_context específico do agente                       │
│  "Plugin < 2 segundos" → governa decisão de performance         │
├─────────────────────────────────────────────────────────────────┤
│  PRIORIDADE 2 — d365-config.yaml (devLoadAlwaysFiles)           │
│  Decision framework: 11 cenários documentados                   │
│  "≤50 produtos → Plugin | >50 → Azure Function"                 │
├─────────────────────────────────────────────────────────────────┤
│  PRIORIDADE 3 — avanade-master.md (base template)               │
│  Protocolo geral Avanade Method                                 │
│  Steps de ativação: 1-6                                         │
├─────────────────────────────────────────────────────────────────┤
│  PRIORIDADE 4 — ftd-context.instructions.md (global)            │
│  Contexto geral do projeto FTD                                  │
│  Brownfield, ambientes, stakeholders                            │
├─────────────────────────────────────────────────────────────────┤
│  PRIORIDADE 5 (MAIS BAIXO) — copilot-instructions.md            │
│  Regras globais do Copilot                                      │
│  Idioma, formatação, comportamento geral                        │
└─────────────────────────────────────────────────────────────────┘
```

---

## 7. VALIDAÇÃO — COMO SABER SE SUA CUSTOMIZAÇÃO ESTÁ FUNCIONANDO

### ✅ Sinais de que está funcionando corretamente

| Sinal | O que significa |
|-------|-----------------|
| Agente usa `ftd_` nos nomes de campos | Leu `d365-config.yaml` (publisher = ftd) |
| Agente menciona `FakeXrmEasy` nos testes | Leu `tiago-dev.customize.yaml` (testing_stack) |
| Agente fala em "Safra" e "Anja" corretamente | Leu `ftd-knowledge-base.md` |
| Agente avisa sobre Dataverse storage | Leu `critical_blockers` do Supervisor |
| Agente propõe `Plugin<T> → Service → Repository` | Leu `padroes-desenvolvimento.md` |
| Agente diferencia `≤50 / >50 produtos` | Leu o `debounce_rule` do Tiago |
| Agente menciona os ADRs ao tomar decisão | Leu `wilson-architect.customize.yaml` (adrs_aceitos) |
| Agente fala em "Mobile First" para UX | Leu `sofia-ux.customize.yaml` (specialist_focus) |
| Agente menciona GMUD para PROD | Leu `roberto-sm.customize.yaml` (specialist_focus) |
| Agente referencia `FTD - [X] - [Y] - [Trigger]` em flows | Leu o `namespace_pa` do Tiago |

### ❌ Sinais de problema (contexto não carregado)

| Sinal | Diagnóstico |
|-------|-------------|
| Agente usa nomes de entidade genéricos (Account sem `ftd_`) | `d365-config.yaml` não foi lido |
| Agente gera plugin sem `depth check` | `customize.yaml` não foi aplicado |
| Agente não sabe o que é "Safra" | `ftd-knowledge-base.md` não carregado |
| Agente sugere Canvas App para o Simulador | ADRs não lidos (ADR-001 diz Power Pages) |
| Agente cria secrets no `appsettings.json` | `d365_dev_rules.secrets` não aplicado |
| Agente ignora o pico sazonal Nov-Jan | `ftd-discovery.md` não carregado |

---

## 8. TESTANDO SUA CONTEXTUALIZAÇÃO — PROMPTS DE DIAGNÓSTICO

Use estes prompts para verificar se o contexto FTD está sendo aplicado corretamente:

### Teste 1: Contexto de Negócio
```
@tiago-dev O que é "Safra" no contexto FTD e como ela aparece nas entidades D365?
```
**Resposta esperada**: Citar `ftd_safra` como campo em `ftd_proposta` e `Opportunity`, explicar que é o ano letivo/comercial (ex: Safra 26 = 2026), mencionar que é usado para filtrar vigor de produtos.

---

### Teste 2: Decision Framework D365
```
@tiago-dev Preciso calcular o total de uma proposta com 200 produtos. Qual tecnologia usar e por quê?
```
**Resposta esperada**: Citar a regra de débounce (200 > 50 → Azure Function), mencionar timeout de 2min do plugin, sugerir Service Bus para processamento assíncrono.

---

### Teste 3: Padrão de Código
```
@tiago-dev Mostre a estrutura básica de um plugin D365 para criar uma proposta.
```
**Resposta esperada**: `Plugin<T> → Service → Repository`, `[CrmPluginRegistration]` com Stage correto, `ITracingService`, `if (context.Depth > 1) return;`, Early Bound, `ColumnSet` específico.

---

### Teste 4: Decisão Arquitetural
```
@wilson-architect Devo usar Canvas App ou Power Pages para o Simulador Comercial?
```
**Resposta esperada**: Citar ADR-001 (já aceito: Power Pages), não reabrir a decisão, mencionar que licença Enterprise cobre uso interno, citar Entra ID (ADR-002).

---

### Teste 5: Contexto Completo de Orquestração
```
@supervisor Quero criar uma nova feature de geração de contratos. Por onde começo?
```
**Resposta esperada**: Rotear para Maria (discovery), depois João PM (PRD), Wilson (ADR se necessário), Paula (stories), Tiago (implementação). Mencionar que contratos têm 50+ templates Word como pain point existente.

---

## 9. RESUMO VISUAL — CAMADAS DE CONTEXTO FTD

```mermaid
graph TB
    subgraph FTDCTX["🎯 CONTEXTO FTD COMPLETO (o que o agente sabe)"]
        direction LR

        subgraph NEG["📊 Negócio"]
            N1["120 anos história\n28 filiais\n1.5M estudantes"]
            N2["Pico Nov-Jan\n5.000 contratos/dia"]
            N3["404 consultores\n3h por proposta"]
            N4["20 pain points\n9 linhas de negócio"]
        end

        subgraph TEC["⚙️ Técnico"]
            T1["D365 CE Online\n9 solutions\nPublisher: ftd_"]
            T2["Power Pages\n(Simulador 6 etapas)"]
            T3["Azure Functions\n.NET 8 + Service Bus"]
            T4["Power Automate\nFTDMaxFlow owner"]
        end

        subgraph PAD["📐 Padrões"]
            P1["Plugin→Service→Repository\nEarly Bound obrigatório"]
            P2["Débounce ≤50/>50 produtos"]
            P3["FakeXrmEasy 80%\njest 70% cobertura"]
            P4["FTD.[Module].[Function]\nnaming conventions"]
        end

        subgraph INT["🔌 Integrações"]
            I1["TOTVS\n(ETL + Service Bus)"]
            I2["ISA\n(Produtos)"]
            I3["Adobe Sign\n(Contratos)"]
            I4["Lumisfera\n(E-commerce)"]
        end

        subgraph ALM["🚀 ALM"]
            A1["Dev: manual\nOAT: automático\nPROD: GMUD"]
            A2["4 pipelines YAML\nSolution Checker:\n0 Critical, 0 High"]
            A3["Branch: dev\n= source of truth"]
        end
    end

    FTDCTX --> AGENTE["🤖 Agente BMAD\n(qualquer um dos 10)"]
    AGENTE --> PROMPT["Seu Prompt\n→ Resposta contextualizada FTD"]
```

---

## 10. CONCLUSÃO — SUA CUSTOMIZAÇÃO ESTÁ FUNCIONANDO?

### ✅ O que foi implementado e garante o contexto:

| Camada | Arquivo | Conteúdo FTD | Status |
|--------|---------|--------------|--------|
| Global | `copilot-instructions.md` | Avanade Method + FTD | ✅ |
| Global | `ftd-context.instructions.md` | Contexto FTD completo | ✅ |
| Agente | `*.agent.md` (10 agentes) | Personas + menus | ✅ |
| Base | `avanade-master.md` | Protocolo de ativação | ✅ |
| Customize | `*.customize.yaml` (10 agentes) | `ftd_project_context` enriquecido | ✅ **Atualizado** |
| Memória | `*.memory.md` (10 agentes) | Histórico por agente | ✅ |
| Conhecimento | `ftd-knowledge-base.md` | 379 linhas de contexto FTD | ✅ |
| Discovery | `ftd-discovery.md` | 20 pain points, fit-gap | ✅ |
| Config | `d365-config.yaml` | 357 linhas D365 técnico | ✅ |
| Arquitetura | `docs/arquitetura/*.md` (5 docs) | Modelo de dados, integrações, padrões | ✅ **Novo** |

> [!IMPORTANT]
> Todos os 10 agentes têm `base_rule` (original) + `rule` (especializada) + `architecture_docs` + `specialist_focus` personalizados para seu papel. Isso garante que tanto a regra fundamental quanto o contexto especializado sejam aplicados em qualquer interação.

> [!TIP]
> Use os **Prompts de Diagnóstico** da seção 8 para validar se o contexto está sendo aplicado corretamente em uma nova sessão. Se o agente responde com os termos FTD corretos (Safra, Anja, débounce, FakeXrmEasy), o contexto está funcionando.

> [!WARNING]
> O contexto é **carregado em tempo real** — se o arquivo estiver quebrado (YAML inválido) ou o caminho errado, o agente pode não ter aquela camada. Execute sempre uma verificação de sintaxe YAML nos `customize.yaml` após edições.

---

*Documento criado em 20/03/2026 como referência de arquitetura do framework BMAD para o projeto FTD Educação.*
