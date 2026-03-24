### Design Patterns Validados
_Padrões arquiteturais que funcionaram bem_

**Exemplo**:
```yaml
- pattern: "CQRS + Event Sourcing"
  context: "Sistema de auditoria financeira (compliance heavy)"
  pros:
    - Auditoria completa (event store)
    - Read/Write optimization separada
    - Escalabilidade independente
  cons:
    - Complexidade aumentada
    - Eventual consistency
  recommendation: "Usar apenas se auditoria/compliance é requisito crítico"
  projects: ["FinanceHub", "ComplianceTracker"]
  
- pattern: "API Gateway + Microservices"
  context: "E-commerce com múltiplos canais (web, mobile, partners)"
  pros:
    - Escalabilidade granular
    - Tecnologias heterogêneas
    - Deployment independente
  cons:
    - Overhead operacional (monitoring, orchestration)
    - Distributed transactions complexas
  recommendation: "Adequado para teams >15 devs, múltiplos produtos"
  projects: ["ShopOnline", "MarketplaceX"]
```

---

### ADRs (Architecture Decision Records)
_Decisões arquiteturais documentadas_

**Exemplo**:
```yaml
- adr_id: "ADR-001"
  title: "Escolha de Azure SQL Database vs Cosmos DB"
  date: "2024-01-15"
  status: "Aceito"
  context: "Sistema de pedidos com 10k RPM esperados"
  decision: "Azure SQL Database (Elastic Pool)"
  rationale:
    - Dados relacionais estruturados
    - ACID transactions necessárias (pedidos)
    - Time tem expertise em SQL
    - Custo 40% menor que Cosmos DB para workload esperado
  consequences:
    - Single-region deployment inicialmente
    - Escalabilidade vertical até 80k RPM (Elastic Pool)
    - Fallback: Sharding se ultrapassar 80k RPM
  alternatives_considered:
    - Cosmos DB: Rejected (overkill para workload, custo alto)
    - PostgreSQL: Rejected (preferência Azure-native)
  
- adr_id: "ADR-002"
  title: "Autenticação via Azure AD B2C"
  date: "2024-02-01"
  status: "Aceito"
  context: "Portal cliente com 50k usuários externos"
  decision: "Azure AD B2C com social login (Google, Facebook)"
  rationale:
    - Managed service (menos manutenção)
    - MFA nativo
    - Compliance (LGPD/GDPR built-in)
    - Social login reduz friction (signup)
  consequences:
    - Vendor lock-in moderado (Azure)
    - Custo: R$ 0,015/MAU (Monthly Active User)
  alternatives_considered:
    - Auth0: Rejected (custo 3x maior)
    - Custom solution: Rejected (security risk, manutenção alta)
```

---

### Tech Stack Preferences & Trade-offs
_Tecnologias avaliadas e escolhas_

**Exemplo**:
```yaml
- category: "Backend Framework"
  preferred: ".NET 8 (C#)"
  rationale:
    - Performance excelente (benchmarks)
    - Azure-native (PaaS otimizado)
    - Time tem expertise
    - Long-term support (LTS)
  alternatives:
    - Node.js: Considerado para APIs simples (lightweight)
    - Python (FastAPI): Usado para ML/AI workloads
  
- category: "Frontend Framework"
  preferred: "React + TypeScript"
  rationale:
    - Ecossistema maduro
    - Fluent UI React components
    - TypeScript previne bugs
  alternatives:
    - Vue.js: Usado em projetos menores (learning curve menor)
    - Blazor: Evitado (ainda imaturo para SPAs complexos)
  
- category: "Database"
  preferred: "Azure SQL Database"
  rationale: "Relational data, ACID, managed service"
  alternatives:
    - Cosmos DB: Para scenarios de global distribution
    - PostgreSQL: Quando open-source é mandatório
```

---

### Performance Benchmarks
_Benchmarks de arquiteturas testadas_

**Exemplo**:
```yaml
- scenario: "API Gateway Comparison"
  date: "2024-01-10"
  load: "10k requests/sec"
  results:
    - Azure API Management: "Latency P95 = 45ms, throughput = 9.8k RPS"
    - Nginx: "Latency P95 = 12ms, throughput = 10.2k RPS"
    - Envoy: "Latency P95 = 18ms, throughput = 10k RPS"
  decision: "Azure APIM escolhido (features > raw performance)"
  notes: "Nginx mais rápido mas falta policy management, analytics built-in"
  
- scenario: "Caching Strategy"
  date: "2024-02-05"
  options_tested:
    - In-memory (no cache): "Response time 250ms"
    - Redis cache: "Response time 15ms (94% improvement)"
    - CDN (Azure Front Door): "Response time 8ms (97% improvement)"
  decision: "Redis para API data, CDN para assets estáticos"
```

---

### Anti-Patterns Evitados
_Decisões ruins evitadas (learnings)_

**Exemplo**:
```yaml
- anti_pattern: "Microservices Prematurely"
  context: "Projeto pequeno (3 devs, 1 produto)"
  problem: "Overhead de orchestration > benefícios"
  lesson: "Começar monolito modular, extrair microservices quando necessário"
  threshold: "Considerar microservices com time >10 devs ou múltiplos produtos"
  
- anti_pattern: "Over-engineering Security"
  context: "Internal tool, zero external access"
  problem: "Investido 3 sprints em security desnecessária"
  lesson: "Security proporcional ao risco (threat modeling first)"
  
- anti_pattern: "NoSQL para Relational Data"
  context: "Escolhido Cosmos DB para dados estruturados/relacionais"
  problem: "Queries complexas = application logic, custo alto"
  lesson: "NoSQL excelente para semi-structured, mas SQL ainda rei para relational"
```

---

### Cloud Patterns (Azure-Specific)
_Padrões testados na Azure_

**Exemplo**:
```yaml
- pattern: "Strangler Fig (Legacy Migration)"
  context: "Migração gradual on-prem ? Azure"
  implementation:
    - Azure API Management como facade
    - Rotear requests para novo sistema gradualmente (feature flags)
    - Monolito on-prem deprecado incrementalmente
  success_rate: "Alta"
  projects: ["LegacyMigrationX"]
  
- pattern: "Circuit Breaker"
  context: "Microservices com dependências externas"
  implementation:
    - Polly library (.NET)
    - Fallback para cache ou response padrão
  success_rate: "Alta"
  notes: "Prevenção de cascading failures"
```

---

### Scalability Insights
_Aprendizados sobre escalabilidade_

**Exemplo**:
```yaml
- insight: "Database é sempre o gargalo"
  evidence: "80% dos projetos com performance issues ? queries não otimizadas"
  solutions:
    - Indexação adequada (composite indexes)
    - Read replicas para queries pesadas
    - Caching agressivo (Redis)
  
- insight: "Stateless é chave para horizontal scaling"
  evidence: "Projetos com session in-memory falharam ao escalar"
  solutions:
    - Externalizar state (Redis, database)
    - JWT tokens (stateless auth)
```

---

## ?? Security Decisions
_Decisões de segurança documentadas_

**Exemplo**:
```yaml
- decision: "Azure Key Vault para secrets"
  date: "2024-01-20"
  rationale: "Centralizar secrets, rotation automática, audit logs"
  cost: "R$ 0,15/secret/month (baixo)"
  alternative_rejected: "Env vars hardcoded (risk de leak)"
  
- decision: "Managed Identity para Azure services"
  date: "2024-02-10"
  rationale: "Zero secrets no código, auth automática entre services"
  example: "App Service ? SQL Database (managed identity, sem connection string)"
```

---

## ?? Cost Optimization Learnings
_Otimizações de custo aplicadas_

**Exemplo**:
```yaml
- optimization: "Reserved Instances (VMs)"
  savings: "65% (1-year commitment)"
  context: "Production workloads estáveis"
  
- optimization: "Auto-shutdown Dev/Test environments"
  savings: "R$ 8k/month"
  implementation: "Azure Automation runbook (shutdown 19h-7h + weekends)"
  
- optimization: "Blob Storage Lifecycle Management"
  savings: "40% storage costs"
  implementation: "Hot ? Cool (30 days) ? Archive (90 days)"
```

---

## ?? Cross-References
_Links para artefatos relacionados_

- Architecture Template: `${AVANADE_ARCHITECTURE_TEMPLATE}`
- Architecture Quality Task: `${AVANADE_TASK_ARCHITECTURE_QUALITY}`
- Adversarial Review: `${AVANADE_TASK_ADVERSARIAL_REVIEW}`
- ADR Template: `${AVANADE_ADR_TEMPLATE}`

---

## ?? Como Usar Esta Memória

### ? ANTES de criar arquitetura:
1. Consultar **Design Patterns** ? reutilizar patterns validados
2. Revisar **ADRs** ? evitar decisões ruins passadas
3. Checar **Tech Stack Preferences** ? escolhas consistentes
4. Analisar **Anti-Patterns** ? não repetir erros

### ? DURANTE design:
1. Documentar **decisões críticas** como ADRs
2. Validar contra **Performance Benchmarks**
3. Aplicar **Cloud Patterns** adequados
4. Considerar **Cost Optimization** upfront

### ? APÓS implementação:
1. **Atualizar memória** com novos patterns/decisions
2. Documentar **Benchmarks** reais (vs estimados)
3. Registrar **Lessons Learned**
4. Atualizar **Tech Stack Preferences** se tecnologia nova validada

---

## ?? D365 CE Architecture Context - FTD Educação

### Decisões Arquiteturais JÁ TOMADAS
1. **Simulador Comercial**: Power Pages (frontend) + CRM (motor) - NÃO Canvas App, NÃO Custom Page
   - Motivo: responsividade mobile+desktop, liberdade de layout, único codebase, licença inclusa (Entra ID)
2. **Cálculos**: Frontend real-time (JS/PowerFX) + validação backend Azure Functions
   - Débounce: =50 produtos ? plugin sync; >50 ? Azure Function
3. **Integrações**: Time CRM consome endpoints (time Thiago Veiga constrói)
   - Sem API Management padrão
   - Simples ? Power Automate; Complexo ? Azure Function
4. **Repo**: Feature branches de `dev` (master não usado para deploy)
5. **Soluções**: 9 solutions numeradas por tipo de componente

### Problemas Arquiteturais a Resolver
- **Tabela de produtos poluída**: 1.283 linhas (deveria ser 15) - precisa separação prateleira vs customizado vs personalizado
- **12+ tabelas de preço**: workaround de visibilidade, não pricing ? redesenhar para tabela única
- **Campos duplicados Oportunidade ? Proposta**: oportunidade será "ressignificada"
- **1 proposta por canal de venda ? 1 proposta multi-canal**: mudança conceitual
- **Cadastro de contas**: sem owner da informação (CRM vs TOTVS race condition)
- **Contratos**: 50+ templates Word ? modularizar com jurídico
- **Vulcano**: eliminar aprovação externa duplicada
- **Security roles**: não equalizadas entre ambientes
- **Dataverse em nível crítico de armazenamento**

### Landscape de Integração
```
CRM D365 ? TOTVS/Datasul (ETLs bidirecionais, sync contas 1x/dia 6h)
CRM D365 ? ISA (cadastro de produto - a redesenhar)
CRM D365 ? Adobe Sign (plugin Adobe Agreement - migrando)
CRM D365 ? Data Lake (ETLs + events)
CRM D365 ? Área do Cliente (Canvas App, mesmas tabelas Dataverse, squad separada)
Monitoring: Datadog (APM) + Grafana (dashboards) + App Insights (AF telemetry)
```

### Ambientes
| Env | Pipeline | Status |
|-----|----------|--------|
| Dev | Manual (plugins) | Ativo |
| OAT | Automático | Ativo |
| Prod | Automático + GMUD/SMAX | Ativo |
| QA | Não configurado | Criado |
| Release Candidate | Não configurado | Criado |

### Architecture Artifacts
- Template: `d365-architecture-template.md`
- Config: `d365-config.yaml`
- **Knowledge Base**: `docs/ftd-knowledge-base.md` (LEITURA OBRIGATÓRIA)

---
