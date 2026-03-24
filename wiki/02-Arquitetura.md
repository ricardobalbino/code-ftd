# Arquitetura de Solução — FTD Educação

> [← Voltar ao Início](./Home.md)

**Versão**: 1.0 | **Data**: 20/03/2026  
**Autores**: João Carlos Figueirôa, Rodrigo Silva (Avanade Architects)

---

## 1. Diagrama de Contexto (C4 — Level 1)

```mermaid
C4Context
    title Sistema FTD Educação — Visão de Contexto

    Person(consultor, "Consultor Comercial", "Cria propostas, negocia com escolas")
    Person(escola, "Escola / Mantenedora", "Recebe e aprova propostas")
    Person(aprovador, "Aprovador Interno", "Coordenador, Gerente, Diretor")
    Person(admin, "Administrador CRM", "Configura e mantém o sistema")

    System(crm, "Dynamics 365 CRM", "Sistema central: Spartan, PNLD, Hub SAC")
    System(simulador, "Simulador Comercial", "Power Pages: interface de negociação")
    System(areaCliente, "Área do Cliente", "Portal escolas: aprovação de propostas")

    System_Ext(totvs, "TOTVS / Datasul", "ERP: faturamento, estoque, financeiro")
    System_Ext(isa, "ISA", "Cadastro de produtos")
    System_Ext(adobeSign, "Adobe Acrobat Sign", "Assinatura digital")
    System_Ext(lumisfera, "Lumisfera (Adobe Commerce)", "E-commerce B2C/B2B")

    Rel(consultor, simulador, "Cria e gerencia propostas")
    Rel(consultor, crm, "Gestão de contas e contratos")
    Rel(escola, areaCliente, "Aprova/recusa propostas")
    Rel(aprovador, crm, "Aprova propostas (Power Automate)")
    Rel(simulador, crm, "Lê/escreve via Dataverse Web API")
    Rel(crm, totvs, "ETL bidirecional: contas, pedidos")
    Rel(crm, isa, "Sincronização de catálogo de produtos")
    Rel(crm, adobeSign, "Contratos assinados digitalmente")
    Rel(crm, lumisfera, "Sublistas de produtos por canal")
```

---

## 2. Diagrama de Containers (C4 — Level 2)

```mermaid
C4Container
    title Containers — Plataforma FTD CRM

    Person(consultor, "Consultor", "404 consultores ativos")

    Container_Boundary(d365, "Dynamics 365 CE Online") {
        Container(spartan, "App Spartan", "Model-Driven App", "App principal de vendas")
        Container(dataverse, "Microsoft Dataverse", "Data Platform", "Repositório central")
        Container(plugins, "Plugins C#", "Dataverse Plugins", "Lógica server-side")
        Container(flows, "Power Automate", "Cloud Flows", "Aprovações e notificações")
        Container(customApis, "Custom APIs", "Dataverse", "Endpoints reutilizáveis")
        Container(pcf, "PCF Controls", "TypeScript/React", "Grids de produtos, totalizadores")
        Container(webRes, "Web Resources JS", "JavaScript", "Validações client-side")
    }

    Container_Boundary(powerPages, "Power Pages") {
        Container(simulador, "Simulador Comercial", "SPA", "Negociação: 6 etapas, cálculos real-time")
        Container(areaCliente, "Área do Cliente", "Power Pages", "Portal escolas (squad separada)")
    }

    Container_Boundary(azure, "Microsoft Azure") {
        Container(azFunctions, "Azure Functions", ".NET 8 Isolated", "Cálculos pesados, ETL")
        Container(serviceBus, "Azure Service Bus", "Topics/Queues", "Desacoplamento D365 ↔ TOTVS")
        Container(keyVault, "Azure Key Vault", "Secrets", "Tokens, URLs, connection strings")
        Container(appInsights, "Application Insights", "Telemetry", "Monitoramento de funções")
    }

    Rel(consultor, simulador, "Acessa via browser (Entra ID)")
    Rel(simulador, dataverse, "Dataverse Web API")
    Rel(plugins, dataverse, "SDK Dataverse")
    Rel(azFunctions, serviceBus, "Publica/Consome mensagens")
    Rel(azFunctions, keyVault, "Lê segredos")
```

---

## 3. Decisões Arquiteturais

### 3.1 Tabela de Decisões

| Decisão | Escolha | Alternativas Descartadas | Motivo |
|---------|---------|--------------------------|--------|
| **Frontend Simulador** | Power Pages | Canvas App, Custom Page | Responsivo, liberdade de layout, licença inclusa, single codebase |
| **Autenticação** | Entra ID (Azure AD) | Portal externo | Usuários internos com licença Enterprise, sem custo extra |
| **Cálculos real-time** | JavaScript no frontend | Plugin síncrono a cada mudança | Performance, UX sem delay perceptível |
| **Validação backend** | Azure Functions | Apenas plugin | Proteção contra bypass, >50 produtos sem timeout |
| **Estratégia de débounce** | ≤50 → plugin sync; >50 → Azure Fn | Sempre async | Respeitar limite de 2 min de plugins |
| **Cache** | Client-side cache pesado | Sem cache | Minimizar roundtrips ao Dataverse (sem-cache por padrão) |
| **Integração ERP** | Consumir endpoints do time Veiga | CRM expõe APIs diretas | CRM não expõe APIs — time de integração separado |

### 3.2 Regra de Débounce: Plugin vs Azure Function

```
≤ 50 produtos → Plugin síncrono (Pre/Post-Operation, Stage 20/40)
> 50 produtos → Azure Function via Service Bus (assíncrono, sem timeout de 2 min)
```

> **Motivo**: Plugins têm timeout de 2 minutos. Propostas com 200 produtos precisam de Azure Function.

---

## 4. Soluções D365 (9 Soluções Segmentadas)

As soluções são numeradas com ordem de dependência para deploy sequencial:

| Ordem | Solution | Conteúdo |
|-------|----------|----------|
| 1 | **FTDCore** | Data model: entidades, campos, relationships |
| 2 | **FTDSecurity** | Security roles, field security profiles |
| 3 | **FTDPlugins** | Todos os plugins C# |
| 4 | **FTDFlows** | Power Automate flows |
| 5 | **FTDClientExtensions** | PCF controls, Web resources |
| 6 | **FTDSiteMap** | Navigation, App Modules |
| 7-9 | (+ outras ~3) | Módulos específicos (PNLD, SAC, etc.) |

**Regras de soluções**:
- Deploy SEMPRE em ordem numérica (dependência)
- Upper environments: soluções **managed** (nunca unmanaged)
- Trabalham com **patches** para releases incrementais
- **NUNCA** editar a solução Default

---

## 5. Fluxos de Dados Principais

### 5.1 Fluxo: Simulador → CRM → TOTVS

```
Consultor (Browser)
    → Power Pages Simulador
        → Dataverse Web API (leitura de produtos, contas)
        → Plugin C# (validação, cálculo de alçada)
        → Dataverse (persiste ftd_proposta)
            → Power Automate (fluxo de aprovação)
                → Aprovadores notificados (Teams/Email)
                    → Proposta Aprovada
                        → Azure Function (gera pedido)
                            → Service Bus
                                → TOTVS/Datasul (faturamento)
```

### 5.2 Fluxo: Integração de Dados (ETL 6h)

```
TOTVS/Datasul  ──[ETL 1x/dia às 6h]──▶  CRM D365
ISA (Produtos) ──[sync periódico]──────▶  Dataverse (Product catalog)
```

---

## 6. Segurança e Observabilidade

| Aspecto | Solução |
|---------|---------|
| **Autenticação** | Entra ID (usuários internos) |
| **Secrets** | Azure Key Vault (para Azure Functions) — Power Automate usa variáveis de ambiente |
| **Security Roles** | Por BU/Filial (controla visibilidade de contas) |
| **Logs** | ITracingService (plugins) + Application Insights (Azure Fn) |
| **Monitoramento** | Datadog + Grafana + Azure Application Insights |
| **Alertas** | Datadog (SLA, erros de integração, volume pico) |

---

## 7. Escalabilidade para Pico Sazonal

**Período crítico**: Novembro a Janeiro (adoção escolar)

- Pico: até **5.000 contratos/dia**
- Volume normal: ~500 contratos/dia
- Fator de pico: 10x volume normal

**Estratégias**:
- Cache client-side no Simulador (reduz queries ao Dataverse)
- Azure Functions auto-scale (isolado process)
- Service Bus para desacoplar picos de processamento
- Plugin timeout protection (débounce >50 produtos → Azure Function)

---

*Referências: [docs/arquitetura/d365-solution-architecture.md](../docs/arquitetura/d365-solution-architecture.md) | [docs/arquitetura/stack-tecnologica.md](../docs/arquitetura/stack-tecnologica.md)*
