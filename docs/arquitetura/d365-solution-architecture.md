# Arquitetura de Solução — FTD Educação
## Dynamics 365 CE + Power Platform + Azure

**Cliente**: FTD Educação S/A (Grupo Marista)  
**Projeto**: Modernização CRM — Transformação CX  
**Tipo**: Brownfield  
**Versão**: 1.0  
**Data**: 20/03/2026  
**Autores**: Avanade (João Carlos Figueirôa, Rodrigo Silva — Arquitetos)

---

## 1. VISÃO GERAL

### 1.1 Contexto do Cliente

A **FTD Educação S/A**, parte do **Grupo Marista**, é uma das maiores editoras e distribuidoras de materiais didáticos do Brasil, com +120 anos de história, 28 filiais nacionais e impacto em ~1,5 milhão de estudantes.

- **Pico sazonal**: Novembro a Janeiro (adoção escolar), com até **5.000 contratos/dia**
- **Processo crítico**: Criação de propostas comerciais para escolas públicas e privadas
- **Pain point central**: Consultor leva até **3 horas** para criar 1 proposta (200 produtos × 3 cliques)

### 1.2 Objetivos Arquiteturais

1. Modernizar a jornada comercial completa via **Simulador Comercial** (Power Pages)
2. Eliminar ferramentas legadas externas (Vulcano, Pede Livreiro)
3. Estabelecer CRM D365 como **fonte de verdade** para contas
4. Racionalizar integrações com TOTVS, ISA, Lumisfera, Adobe Sign
5. Garantir escalabilidade para pico sazonal (5-10x volume normal)

---

## 2. DIAGRAMA DE CONTEXTO (C4 — Level 1)

```mermaid
C4Context
    title Sistema FTD Educação — Visão de Contexto

    Person(consultor, "Consultor Comercial", "Cria propostas, negocia com escolas")
    Person(escola, "Escola / Mantenedora", "Recebe e aprova propostas")
    Person(aprovador, "Aprovador Interno", "Coordenador, Gerente, Diretor")
    Person(admin, "Administrador CRM", "Configura e mantém o sistema")

    System(crm, "Dynamics 365 CRM", "Sistema central de gestão comercial: Spartan, PNLD, Hub SAC")
    System(simulador, "Simulador Comercial", "Power Pages: interface de negociação consultiva")
    System(areaCliente, "Área do Cliente", "Portal escolas: visualização e aprovação de propostas")

    System_Ext(totvs, "TOTVS / Datasul", "ERP corporativo: faturamento, estoque, financeiro")
    System_Ext(isa, "ISA", "Cadastro de produtos (família de produto)")
    System_Ext(adobeSign, "Adobe Acrobat Sign", "Assinatura digital de contratos")
    System_Ext(lumisfera, "Lumisfera (Adobe Commerce)", "E-commerce: FTD com Você, B2B Livreiros")
    System_Ext(datadog, "Datadog + Grafana", "Monitoramento e observabilidade")

    Rel(consultor, simulador, "Cria e gerencia propostas")
    Rel(consultor, crm, "Gestão de contas, oportunidades, contratos")
    Rel(escola, areaCliente, "Aprova/recusa propostas, visualiza histórico")
    Rel(aprovador, crm, "Aprova propostas (Power Automate Approvals)")
    Rel(admin, crm, "Configuração e administração")

    Rel(simulador, crm, "Lê/escreve via Dataverse Web API")
    Rel(crm, totvs, "ETL bidirecional: contas, pedidos, faturamento")
    Rel(crm, isa, "Sincronização de catálogo de produtos")
    Rel(crm, adobeSign, "Envio e recebimento de contratos assinados")
    Rel(crm, lumisfera, "Configuração de sublistas de produtos por canal")
    Rel(crm, datadog, "Telemetria, logs e alertas")
```

---

## 3. DIAGRAMA DE CONTAINERS (C4 — Level 2)

```mermaid
C4Container
    title Containers — Plataforma FTD CRM

    Person(consultor, "Consultor", "404 consultores ativos")

    Container_Boundary(d365, "Dynamics 365 CE Online") {
        Container(spartan, "App Spartan", "Model-Driven App", "App principal de vendas comerciais")
        Container(pnld, "App PNLD", "Model-Driven App", "Programa Nacional do Livro Didático")
        Container(hubSac, "Hub SAC", "Model-Driven App", "Atendimento ao cliente (CRC)")
        Container(dataverse, "Microsoft Dataverse", "Data Platform", "Repositório central de dados: contas, propostas, produtos, oportunidades")
        Container(plugins, "Plugins C#", "Dataverse Plugins", "Lógica de negócio server-side: validações, cálculos, triggers")
        Container(flows, "Power Automate Flows", "Cloud Flows", "Aprovações, notificações, sincronizações automatizadas")
        Container(customApis, "Custom APIs", "Dataverse Custom APIs", "Endpoints reutilizáveis expostos internamente")
        Container(pcf, "PCF Controls", "TypeScript/React", "Componentes UI avançados: grids de produtos, totalizadores")
        Container(webRes, "Web Resources JS", "JavaScript", "Lógica de formulário: show/hide, validações client-side, eventos")
    }

    Container_Boundary(powerPages, "Power Pages") {
        Container(simulador, "Simulador Comercial", "Power Pages SPA", "Interface de negociação: 6 etapas, cálculos real-time, cache heavy")
        Container(areaCliente, "Área do Cliente", "Power Pages", "Portal escolas: visualização, aprovação, histórico de propostas (squad separada)")
    }

    Container_Boundary(azure, "Microsoft Azure") {
        Container(azFunctions, "Azure Functions", ".NET 8 Isolated", "Cálculos pesados (>50 produtos), integrações ETL, processamento assíncrono")
        Container(serviceBus, "Azure Service Bus", "Topics/Queues", "Mensageria: desacoplamento D365 ↔ TOTVS, garantia de entrega")
        Container(eventGrid, "Azure Event Grid", "Events", "Roteamento de eventos: Data Lake, sincronizações event-driven")
        Container(keyVault, "Azure Key Vault", "Secrets", "Tokens, URLs, connection strings para Azure Functions")
        Container(appInsights, "Application Insights", "Telemetry", "Telemetria de Azure Functions")
        Container(devOps, "Azure DevOps", "ALM", "Repositório, pipelines CI/CD, PBIs (em migração para unificação)")
    }

    Rel(consultor, simulador, "HTTPS / Entra ID Auth")
    Rel(consultor, spartan, "HTTPS / Unified Interface")
    Rel(simulador, dataverse, "Dataverse Web API")
    Rel(spartan, dataverse, "Dataverse SDK (managed)")
    Rel(pnld, dataverse, "Dataverse SDK (managed)")
    Rel(hubSac, dataverse, "Dataverse SDK (managed)")
    Rel(plugins, dataverse, "IOrganizationService (in-process)")
    Rel(flows, dataverse, "Dataverse Connector")
    Rel(azFunctions, dataverse, "Dataverse Web API / SDK")
    Rel(azFunctions, serviceBus, "Service Bus SDK")
    Rel(dataverse, serviceBus, "Webhooks → Service Bus")
    Rel(serviceBus, eventGrid, "Event forwarding")
    Rel(azFunctions, keyVault, "Managed Identity")
    Rel(azFunctions, appInsights, "ILogger / TelemetryClient")
    Rel(devOps, dataverse, "pac CLI / Solution Packager deploy")
```

---

## 4. APLICATIVOS D365

| App | Área | Usuários | Módulo D365 |
|-----|------|----------|-------------|
| **Spartan** | Comercial | Consultores, Anjas, Coordenadores, Gerentes, Diretores | Sales |
| **PNLD** | Setor Público | Consultores PNLD (NÃO criam contas) | Sales |
| **Hub SAC** | CRC (Atendimento) | Atendentes, Supervisores | Customer Service |
| **Adobe Sign** | Contratos | Consultores, Jurídico | Extensão ISV |
| **Área do Cliente** | Portal | Representantes legais de escolas | Power Pages (squad separada) |

---

## 5. SOLUTIONS ARCHITECTURE (9 Soluções)

```mermaid
flowchart TD
    subgraph DEP["Ordem de Deploy (Dependências)"]
        S1["1️⃣ FTDCore\n(Tabelas, Security Roles,\nOption Sets, Publisher)"]
        S2["2️⃣ FTDDataModel\n(Extensões de entidades\npadrão + relacionamentos)"]
        S3["3️⃣ FTDSales\n(Forms, Views, BPF\ndo módulo Sales)"]
        S4["4️⃣ FTDService\n(Forms, Views\ndo módulo Service)"]
        S5["5️⃣ FTDPlugins\n(Assemblies C#\nregistro de steps)"]
        S6["6️⃣ FTDIntegration\n(Webhooks, Custom APIs,\nAzure Function configs)"]
        S7["7️⃣ FTDControls\n(PCF Controls\ncompartilhados)"]
        S8["8️⃣ FTDClientExtensions\n(JavaScript Web Resources,\nHTML Resources)"]
        S9["9️⃣ FTDSiteMap\n(Configuração de\nnavegação dos apps)"]
    end

    S1 --> S2
    S2 --> S3
    S2 --> S4
    S2 --> S5
    S5 --> S6
    S3 --> S7
    S3 --> S8
    S8 --> S9
```

**Estratégia**: Segmentação por tipo de componente. Deploy em sequência com patches. Managed solutions para OAT e PROD; Unmanaged para DEV.

**Publisher**: `ftd` (prefixo `ftd_` para todos os componentes customizados)

---

## 6. AMBIENTES E PIPELINE

### 6.1 Ambientes D365

| Ambiente | Nome | Status | Pipeline | Observações |
|----------|------|--------|----------|-------------|
| **DEV** | FTD-Dev | ✅ Ativo | ❌ Manual | Plugin deploy manual. Source of truth = branch `dev` |
| **Build** | FTD-Build | ✅ Implícito | ✅ Automático | Export → Pack Managed → Solution Checker |
| **OAT (UAT)** | FTD-OAT | ✅ Ativo | ✅ Automático | Import Managed. Aprovador no pipeline |
| **QA** | FTD-QA | 🔧 Criado | ❌ Não conf. | Pendente configuração pipeline |
| **RC** | FTD-RC | 🔧 Criado | ❌ Não conf. | Pendente configuração pipeline |
| **PROD** | FTD-Prod | ✅ Ativo | ✅ Manual (GMUD) | Requer GMUD via SMAX. Aprovação infra by Giselle |

### 6.2 Pipeline ALM (4 YAMLs Azure DevOps)

```mermaid
flowchart LR
    subgraph Repo["Repositório: FTD Dynamics"]
        F[Feature Branch\nfeat/STORY-XXX]
        D[Branch dev\n= Source of Truth]
    end

    subgraph Pipeline["Azure DevOps Pipelines"]
        P1["1️⃣ Guard (CI)\nBuild + Solution Checker\n→ Bloqueia se Critical/High"]
        P2["2️⃣ Deploy Code\nExport Unmanaged\n+ Unpack → Commit"]
        P3["3️⃣ Deploy MANUAL\nImport Managed\n→ OAT / PROD"]
        P4["4️⃣ Export-Unpack\nSincroniza repo\ncom ambiente"]
    end

    F -- PR → dev --> D
    D --> P1
    P1 -- Aprovado --> P3
    P2 --> D
    D --> P4
```

**Fluxo de trabalho**: `Customization Master → pac unpack → commit #workitem → PR → guard build → deploy manual`

**Batch projects**: Executados em VM separada (não na máquina do dev). Solicitado ao time de infra.

---

## 7. MODELO DE SEGURANÇA

```mermaid
graph TD
    BU_BR["Business Unit: FTD Brasil"]
    BU_BR --> BU_SP["BU: Filial SP"]
    BU_BR --> BU_RJ["BU: Filial RJ"]
    BU_BR --> BU_XX["BU: ... (28 filiais)"]

    BU_SP --> U1["👤 Consultor A\n(vê só suas contas)"]
    BU_SP --> U2["👤 Consultor B\n(vê só suas contas)"]
    BU_SP --> U3["👥 Coordenador\n(vê todas contas da filial)"]
    BU_SP --> U4["👥 Gerente\n(vê todas contas da filial)"]
    BU_BR --> U5["👥 Diretor Adjunto\n(vê todas as filiais)"]
    BU_BR --> U6["👥 Diretor Comercial\n(vê todas as filiais)"]
```

**Regras confirmadas (Marcel - 16/Mar/2026)**:
- Consultores: visibilidade **somente suas próprias contas** (Owner = Consultor)
- Coordenadores/Gerentes: visibilidade da **filial inteira** (Business Unit)
- Diretores: visibilidade **nacional** (Parent BU)
- Consultores PNLD: **NÃO criam contas** — apenas administram/compartilham existentes

**Usuário de serviço**: `FTDMaxFlow` — propietário de todos os Power Automate flows e conexões. Acesso restrito a Julio, Fernando e Thiago.

⚠️ **Risco**: Limite de conexões do FTDMaxFlow para child flows — investigar múltiplos service accounts ou Managed Identity.

---

## 8. PROCESSO COMERCIAL — JORNADA COMPLETA

```mermaid
flowchart LR
    A["🏫 Conta\n(Escola/Mantenedora)"]
    B["👤 Contato\n(Rep. Legal - CPF obrig.)"]
    C["💼 Oportunidade\n(modelo venda, safra,\nvigência, tipo contrato)"]
    D["📋 Produtos\n(individual ou lote)"]
    E["📝 Proposta\n(6 etapas Simulador)"]
    F["✅ Aprovação\n(4 alçadas)"]
    G["📄 Contrato\n(50+ templates Word)"]
    H["✍️ Adobe Sign\n(Assinatura digital)"]
    I["🏭 TOTVS\n(Pedido, faturamento)"]

    A --> B --> C --> D --> E --> F --> G --> H --> I
```

### Etapas da Proposta (Simulador Comercial — Power Pages)

| Etapa | Nome | MVP | Responsável |
|-------|------|-----|-------------|
| 1 | Contexto da Negociação | ✅ Em dev | FTD |
| 2 | Produtos e Serviços | ✅ MVP (adição individual) | FTD → Avanade (lote) |
| 3 | Benefícios, Doações, Patrocínio, Adiantamento | 📅 Pós-MVP | Avanade |
| 4 | Configuração de Vendas (Canais) | 📅 Pós-MVP | Avanade |
| 5 | Matriz de Serviços | 🔄 Em revisão | Avanade |
| 6 | Revisão e Compartilhamento / Aprovação | 📅 Pós-MVP | Avanade |

---

## 9. ALÇADAS DE APROVAÇÃO (Power Automate)

| Nível | Aprovador | Gatilho (exemplos) |
|-------|-----------|-------------------|
| **1** | Consultor (auto-aprovação) | Dentro dos limites padrão |
| **2** | Coordenador + Gerente | % royalty, % adiantamento, taxa admin acima do limite N1 |
| **3** | + Diretor Adjunto | Regras mais elevadas de desconto/patrocínio |
| **4** | + Diretor Comercial (Quintela) | Casos extremos (hoje: regras mal definidas → tudo cai aqui) |

⚠️ **Situação atual**: Regras de alçada sendo revistas pela equipe comercial (Mônica). Motor de aprovação será entregue independentemente das regras.

**Tecnologia**: Power Automate Cloud Flows com Approvals nativo (UI Teams/Outlook). **Vulcano será ELIMINADO**.

---

## 10. ROADMAP DE ENTREGAS

| Fase | Deadline | Responsável | Escopo |
|------|----------|-------------|--------|
| **MVP Simulador** | 31/Mar/2026 | FTD | Adição individual de produtos (Power Pages Etapas 1+2) |
| **Habilitadores** | Pré-MVP | FTD+Avanade | 7 itens de base: environments, pipelines, security roles, cadastros |
| **Simulador Faxina** | Pós-MVP | Avanade | Limpeza de dados: produtos (1.283→15), preços (12+→5), contas |
| **Onda 1** | ~Ago/2026 | Avanade | Lote, copiar proposta, benefícios, aprovação, migrar Pede Livreiro |
| **Onda 2 — Funil** | TBD | Avanade | Funil de vendas, higienização 101K contas |
| **Pós-Venda** | TBD | Avanade | Comissionamento, inteligência comercial |
| **PNLD** | TBD | Avanade | Setor público (4 itens específicos) |

---

## 11. ARCHITECTURE DECISION RECORDS (ADRs)

| ADR | Decisão | Status |
|-----|---------|--------|
| ADR-001 | Power Pages (não Canvas App) para Simulador Comercial | ✅ Aceito |
| ADR-002 | Entra ID (não Azure AD B2C) para autenticação Power Pages | ✅ Aceito |
| ADR-003 | Débounce: ≤50 produtos → Plugin Sync; >50 → Azure Function | ✅ Aceito |
| ADR-004 | Cache heavy client-side (JavaScript) para mínimo de roundtrips Dataverse | ✅ Aceito |
| ADR-005 | CRM como source of truth para contas (vs TOTVS) — alinhamento Jovanello | ✅ Em formalização |
| ADR-006 | Azure Service Bus para integração assíncrona D365 ↔ TOTVS | ✅ Aceito |
| ADR-007 | Eliminar Vulcano — migrar aprovação para Power Automate nativo D365 | ✅ Aceito |

> **Nota**: Ver `.avanade-method/templates/adr-template.md` para template formal de ADR.

---

## 12. RISCOS ARQUITETURAIS

| # | Risco | Impacto | Probabilidade | Mitigação |
|---|-------|---------|--------------|-----------|
| R1 | Dataverse storage crítico (já passou do limite) | 🔴 Crítico | ✅ Ocorrendo | Escalar com arquiteto infra imediatamente + análise de dados para arquivamento |
| R2 | Cadastro de produtos não limpo a tempo (MVP) | 🟠 Alto | 🟡 Média | Adição individual não depende de lote — MVP seguro; lote depende da limpeza |
| R3 | Resistência dos 404 consultores ao Simulador | 🟠 Alto | 🟡 Média | Validação UX com 4 grupos focais + rollout gradual por filial |
| R4 | Limite de conexões FTDMaxFlow | 🟡 Médio | 🟡 Média | Investigar múltiplos service accounts ou Managed Identity para flows |
| R5 | Pipeline concorrido com outras áreas (fila) | 🟡 Médio | 🟠 Alta | Slots de deploy reservados + comunicação antecipada |
| R6 | QA e RC environments não configurados | 🟡 Médio | ✅ Ocorrendo | Configurar antes da Onda 1 (Avanade) |
| R7 | Regras de alçada ainda indefinidas | 🟡 Médio | ✅ Ocorrendo | Entregar motor independente das regras; parametrizar depois |

---

*Gerado automaticamente com base em: transcrições de onboarding (9-12/Mar/2026), especificação Simulador Notion, refinamento de processo (16/Mar/2026), d365-config.yaml v4.29.0*
