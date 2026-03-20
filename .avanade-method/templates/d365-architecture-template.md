# ============================================================
# D365 ARCHITECTURE TEMPLATE - Avanade Method v4.29.0
# ============================================================
# Template Owner: Wilson Architect
# Extends: architecture-template.md com seções D365-specific

# USAGE: Documento de arquitetura para customizações D365 CE.
# Inclui Dataverse model, solution architecture, integration e security.

---

# 🏗️ Arquitetura - [Nome do Projeto/Feature]

## 1. Executive Summary

| Item | Valor |
|------|-------|
| **Projeto** | [Nome] |
| **Módulos D365** | [Sales, Service, Marketing, Field Service] |
| **Tipo** | [Brownfield - Customização CRM existente] |
| **Versão** | 1.0 |
| **Arquiteto** | Wilson (Avanade Method) |
| **Data** | YYYY-MM-DD |

### Visão Geral
[Descrição da solução arquitetural em 3-5 linhas]

### Decisões-Chave
| ADR | Decisão | Justificativa |
|-----|---------|---------------|
| ADR-001 | [Decisão] | [Por quê] |
| ADR-002 | [Decisão] | [Por quê] |

---

## 2. Solution Architecture (C4 - Context)

```mermaid
C4Context
    title System Context - [Nome do Projeto]

    Person(user, "Usuário CRM", "Usuário Dynamics 365 CE")
    Person(admin, "Admin", "Administrador D365")
    Person_Ext(external, "Usuário Externo", "Cliente/Parceiro via Portal")

    System(d365, "Dynamics 365 CE", "CRM - Sales, Service, Marketing")
    System(powerplatform, "Power Platform", "Power Automate, Canvas Apps")
    System(azure, "Azure Functions", "Backend de integração")

    System_Ext(erp, "Sistema ERP", "SAP / D365 F&O / Outro")
    System_Ext(email, "Exchange/Outlook", "Server-side sync")
    System_Ext(external_api, "API Externa", "Serviço de terceiros")

    Rel(user, d365, "Usa")
    Rel(admin, d365, "Configura")
    Rel(external, powerplatform, "Acessa via Power Pages")
    Rel(d365, azure, "Webhooks / Service Bus")
    Rel(azure, erp, "REST API")
    Rel(azure, external_api, "REST API")
    Rel(d365, email, "Server-side sync")
    Rel(powerplatform, d365, "Dataverse connector")
```

---

## 3. Dataverse Data Model (ERD)

### 3.1 Tabelas Novas

| Tabela (logical_name) | Display Name | Ownership | Relacionamento Principal |
|----------------------|--------------|-----------|------------------------|
| [prefix_tabela] | [Nome] | User/Team | [Lookup para account/contact/etc] |

### 3.2 Tabelas Modificadas

| Tabela | Mudança | Tipo | Impacto |
|--------|---------|------|---------|
| [tabela] | [Nova coluna / Relação / View / Form] | [Configuration] | [Baixo/Médio/Alto] |

### 3.3 ERD Diagram

```mermaid
erDiagram
    ACCOUNT ||--o{ OPPORTUNITY : "has"
    ACCOUNT ||--o{ CONTACT : "has"
    CONTACT ||--o{ INCIDENT : "reports"
    %% Add custom entities and relationships below
```

---

## 4. Solution Architecture (Packaging)

### 4.1 Solution Map

```mermaid
graph TB
    subgraph "Core Layer"
        Core[FTDCore<br/>Tables, Security Roles, Option Sets]
    end
    subgraph "Module Layer"
        Sales[FTDSales<br/>Sales customizations]
        Service[FTDService<br/>Service customizations]
    end
    subgraph "Code Layer"
        Plugins[FTDPlugins<br/>Plugin assemblies]
        PCF[FTDPCF<br/>PCF Controls]
    end
    subgraph "Integration Layer"
        Integration[FTDIntegration<br/>Webhooks, Custom APIs]
    end

    Sales --> Core
    Service --> Core
    Plugins --> Core
    PCF --> Core
    Integration --> Core
    Integration --> Plugins
```

### 4.2 Solution Components

| Solution | Tipo | Componentes | Dependências |
|----------|------|-------------|--------------|
| FTDCore | Core | [Listar] | Nenhuma |
| FTDSales | Module | [Listar] | FTDCore |
| FTDPlugins | Code | [Listar] | FTDCore |
| FTDIntegration | Integration | [Listar] | FTDCore, FTDPlugins |

---

## 5. Plugin Architecture

### 5.1 Plugin Execution Pipeline

```mermaid
sequenceDiagram
    participant U as Usuário/System
    participant D as Dataverse
    participant PV as Pre-Validation (10)
    participant PRE as Pre-Operation (20)
    participant DB as Database
    participant POST as Post-Operation (40)
    participant ASYNC as Async Service

    U->>D: Create/Update/Delete
    D->>PV: Execute Pre-Validation plugins
    PV-->>D: Validate or Throw
    D->>PRE: Execute Pre-Operation plugins
    PRE-->>D: Modify target or Throw
    D->>DB: Persist to database
    DB-->>D: Success
    D->>POST: Execute Post-Operation (sync)
    POST-->>D: Process or Throw
    D-->>U: Response
    D->>ASYNC: Queue async Post-Operation
    ASYNC-->>ASYNC: Execute async plugins
```

### 5.2 Plugin Registry

| Plugin Class | Entity | Message | Stage | Mode | Filtering Attributes |
|-------------|--------|---------|-------|------|---------------------|
| [Namespace.Class] | [entity] | [Create/Update/Delete] | [10/20/40] | [Sync/Async] | [attr1, attr2] |

### 5.3 Plugin Design Patterns

```
STANDARD PLUGIN PATTERN:
┌─────────────────────────────────────┐
│ 1. Validate Context                 │
│    - Check message, entity, depth   │
│ 2. Extract Data                     │
│    - Get target, pre/post images    │
│ 3. Execute Business Logic           │
│    - Service calls, calculations    │
│ 4. Handle Errors                    │
│    - InvalidPluginExecutionException│
│    - ITracingService for logging    │
└─────────────────────────────────────┘
```

---

## 6. Integration Architecture

### 6.1 Integration Landscape

```mermaid
graph LR
    subgraph "Dynamics 365 CE"
        DV[Dataverse]
        WH[Webhooks]
        CA[Custom APIs]
    end

    subgraph "Azure"
        AF[Azure Functions]
        SB[Service Bus]
        EG[Event Grid]
        KV[Key Vault]
    end

    subgraph "External Systems"
        ERP[ERP System]
        API[External APIs]
        FS[File Storage]
    end

    DV -->|webhook| AF
    DV -->|event| EG
    EG -->|trigger| AF
    AF -->|publish| SB
    SB -->|subscribe| AF
    AF -->|REST| ERP
    AF -->|REST| API
    AF -->|secrets| KV
    CA -->|call| AF
```

### 6.2 Integration Patterns

| Integration | Pattern | Source → Target | Frequency | Technology |
|------------|---------|-----------------|-----------|------------|
| [Nome] | [Webhook/ServiceBus/Polling/Batch] | [Origem → Destino] | [Real-time/Schedule] | [AF/PA/etc] |

### 6.3 Error Handling & Resilience

| Pattern | Implementation |
|---------|---------------|
| Retry Policy | Exponential backoff (3 retries, 1s/5s/30s) |
| Dead Letter | Service Bus dead-letter queue |
| Circuit Breaker | Polly library in Azure Functions |
| Idempotency | Correlation ID + dedup table |
| Alerting | Application Insights + Azure Monitor |

---

## 7. Power Automate Architecture

### 7.1 Flow Inventory

| Flow Name | Type | Trigger | Purpose | Error Handling |
|-----------|------|---------|---------|----------------|
| FTD - [Module] - [Action] - [Trigger] | [Auto/Schedule/Instant] | [Trigger] | [Purpose] | Scope Try-Catch |

### 7.2 Flow Design Pattern

```
STANDARD FLOW PATTERN:
┌──────────────────────────────────────┐
│ Trigger                              │
├──────────────────────────────────────┤
│ Scope: Initialize                    │
│   └─ Initialize variables            │
├──────────────────────────────────────┤
│ Scope: Try                           │
│   ├─ Condition/Switch                │
│   ├─ Dataverse operations            │
│   └─ External calls                  │
├──────────────────────────────────────┤
│ Scope: Catch (run after: failed)     │
│   ├─ Compose error details           │
│   └─ Send notification/log error     │
├──────────────────────────────────────┤
│ Scope: Finally (run after: all)      │
│   └─ Cleanup / audit log             │
└──────────────────────────────────────┘
```

---

## 8. Security Architecture

### 8.1 Security Model

```mermaid
graph TB
    subgraph "Business Units"
        RootBU[Root BU]
        BU1[BU: Region 1]
        BU2[BU: Region 2]
    end

    subgraph "Security Roles"
        SR1[Sales Manager]
        SR2[Sales Rep]
        SR3[Service Agent]
        SR4[Admin]
    end

    subgraph "Teams"
        T1[Owner Team: Sales BR]
        T2[Access Team: Deal Room]
        T3[AAD Group: All Sales]
    end

    RootBU --> BU1
    RootBU --> BU2
    BU1 --> T1
    BU2 --> T2
    SR1 --> T1
    SR2 --> T1
    SR3 --> T2
```

### 8.2 Security Roles Matrix

| Role | Table | Create | Read | Write | Delete | Append | Append To |
|------|-------|--------|------|-------|--------|--------|-----------|
| [Role] | [Table] | [User/BU/Org/None] | [...] | [...] | [...] | [...] | [...] |

### 8.3 Field Security Profiles

| Profile | Table.Field | Read | Update | Create |
|---------|------------|------|--------|--------|
| [Profile] | [table.field] | ✅/❌ | ✅/❌ | ✅/❌ |

---

## 9. Environment Strategy

```mermaid
graph LR
    DEV[DEV<br/>Unmanaged<br/>Development] -->|Export + Pack| BUILD[BUILD<br/>CI/CD<br/>Solution Checker]
    BUILD -->|Import Managed| TEST[TEST<br/>QA Testing<br/>Integration Tests]
    TEST -->|Import Managed| UAT[UAT<br/>Business Validation<br/>User Acceptance]
    UAT -->|Import Managed| PROD[PROD<br/>Production<br/>Managed Only]
```

| Environment | Purpose | Solution Type | Data | Access |
|-------------|---------|---------------|------|--------|
| Dev | Development | Unmanaged | Sample data | Dev team |
| Build | CI/CD | N/A (pipeline) | N/A | Automated |
| Test | QA | Managed | Test data | QA team |
| UAT | User Acceptance | Managed | Sanitized prod | Business users |
| Prod | Production | Managed | Production | All users |

---

## 10. Performance Considerations

### 10.1 Dataverse Performance

| Concern | Mitigation |
|---------|------------|
| Plugin execution time | Target < 2s, use async for heavy operations |
| Query performance | Use indexed columns, limit FetchXML results |
| Batch operations | Use ExecuteMultipleRequest (max 1000/batch) |
| Concurrent users | Optimize form load (lazy-load sub-grids) |
| Large datasets | Pagination, server-side filtering |

### 10.2 Integration Performance

| Concern | Mitigation |
|---------|------------|
| API throttling | Respect Dataverse API limits (6000 req/5min) |
| Service Bus throughput | Batch messages, appropriate tier |
| Azure Function cold start | Premium plan or warm-up trigger |
| Data migration volume | Parallel imports, off-peak scheduling |

---

## 11. ADR Log

### ADR-001: [Título da Decisão]

| Item | Detalhe |
|------|---------|
| **Status** | [Proposed / Accepted / Deprecated / Superseded] |
| **Contexto** | [Situação que requer decisão] |
| **Decisão** | [O que foi decidido] |
| **Alternativas** | [Opções consideradas e descartadas] |
| **Consequências** | [Trade-offs e impactos] |

---

## 12. Riscos & Mitigações

| Risco | Probabilidade | Impacto | Mitigação |
|-------|--------------|---------|-----------|
| [Risco 1] | [Alto/Médio/Baixo] | [Alto/Médio/Baixo] | [Ação] |

---

## 13. Tech Debt & Future Improvements

| Item | Prioridade | Effort | Descrição |
|------|-----------|--------|-----------|
| [Item 1] | [Alta/Média/Baixa] | [S/M/L] | [O que melhorar] |
