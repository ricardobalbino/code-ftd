---

## ?? O que é este Artefato?

Este é o **checklist de validação** para garantir qualidade e completude de documentação de produto na Avanade Method. Cobre:
- ? **PRD Quality Gates**: Validação antes de approval
- ? **Discovery Completeness**: Perguntas estratégicas respondidas
- ? **Stakeholder Alignment**: Sign-offs necessários obtidos
- ? **Technical Feasibility**: Viabilidade técnica validada
- ? **Business Value**: ROI e métricas definidas

---

## ?? Quando Usar

### ? USE para:
- Antes de compartilhar PRD para review
- Antes de solicitar approval de stakeholders
- Antes de iniciar desenvolvimento (Definition of Ready)
- Após mudanças significativas no scope
- Como gate para transição entre fases (Discovery ? Planning ? Execution)

### ? NÃO USE para:
- Bugs simples (use issue tracker checklist)
- Refactoring técnico (use ADR checklist)
- Experimentos/protótipos (validação leve)

---

## ? CHECKLIST COMPLETO

### 1. DISCOVERY PHASE - Contexto e Problema

#### 1.1 Problem Definition
- [ ] **Problema claramente articulado** - Qual dor estamos resolvendo?
- [ ] **Impacto quantificado** - Métricas atuais documentadas (tempo desperdiçado, custos, erros)?
- [ ] **Público-alvo identificado** - Para quem estamos resolvendo? Qual persona?
- [ ] **Urgência justificada** - Por que resolver isso agora? Qual o custo de não fazer?
- [ ] **Competição analisada** - Como concorrentes resolvem isso? O que podemos aprender?

**Exemplo de validação:**
```
? VAGO: "Usuários têm dificuldade com exports"
? ESPECÍFICO: "Usuários enterprise gastam 1.8h/dia em exports manuais, resultando em 40% error rate e $150k/ano em custos operacionais (fonte: Analytics Q4 2024)"
```

#### 1.2 User Research
- [ ] **Pesquisa com usuários realizada** - Pelo menos 5 entrevistas ou survey com 50+ respostas?
- [ ] **Pain points documentados** - Lista de frustrações validadas com quotes dos usuários?
- [ ] **Current workarounds identificados** - Como usuários resolvem hoje? Por que é inadequado?
- [ ] **User personas criadas** - Pelo menos 1 persona primária com goals, frustrations, context?
- [ ] **Jobs-to-be-Done definidos** - Qual "job" o usuário está "hiring" nosso produto para fazer?

**Red flags:**
```
?? "Achamos que usuários querem X" (sem pesquisa)
?? "O CEO disse que é importante" (stakeholder opinion ? user need)
?? "Competidor Y tem esse feature" (copy sem validação)
```

#### 1.3 Strategic Alignment
- [ ] **OKRs da empresa mapeados** - Feature alinha com qual OKR? Qual Key Result?
- [ ] **Roadmap do produto** - Feature encaixa em qual theme/initiative?
- [ ] **Priorização justificada** - Por que P0 vs P1? RICE score calculado?
- [ ] **Trade-offs considerados** - O que NÃO faremos para fazer isso?

**RICE Scoring:**
```yaml
reach: [Quantos usuários impacta por quarter?]
impact: [1=Minimal, 2=Low, 3=Medium, 5=High, 8=Massive]
confidence: [% de certeza - 50%, 80%, 100%]
effort: [Person-months estimado]

RICE Score = (Reach × Impact × Confidence) / Effort

Exemplo:
reach: 500 users/quarter
impact: 5 (High)
confidence: 80%
effort: 2 person-months
RICE = (500 × 5 × 0.8) / 2 = 1000 ? Alta prioridade
```

---

### 2. PRD DOCUMENTATION - Estrutura e Clareza

#### 2.1 Executive Summary
- [ ] **TL;DR presente** - Executivo consegue entender em <3min?
- [ ] **Problem statement conciso** - 1-2 parágrafos máximo?
- [ ] **Solução proposta clara** - Qual abordagem em high-level?
- [ ] **Expected impact quantificado** - Métricas de sucesso específicas?
- [ ] **Strategic alignment explícito** - OKRs referenciados diretamente?

**Template de validação:**
```
Problema: [1 sentence]
Para quem: [1 sentence]
Solução: [2 sentences]
Impacto: [3 metrics com targets]
Timing: [Por que agora - 1 sentence]
```

#### 2.2 Goals & Success Metrics
- [ ] **Primary goal SMART** - Specific, Measurable, Achievable, Relevant, Time-bound?
- [ ] **3-5 KPIs definidos** - Métricas mensuráveis e rastreáveis?
- [ ] **Baselines documentadas** - Estado atual de cada métrica conhecido?
- [ ] **Targets realistas** - Metas baseadas em benchmarks/análise?
- [ ] **Tracking method especificado** - Como/onde mediremos? (dashboard, tool)

**Exemplo de KPI bem definido:**
```yaml
? GOOD:
metric: "Time to Export (TTE)"
baseline: "1.8 hours (average, Q4 2024 data)"
target: "15 minutes (p95, by end of Q1 2025)"
measurement: "Datadog dashboard - Export Analytics"
tracking_frequency: "Daily"

? BAD:
metric: "Make exports faster"
target: "Improve significantly"
measurement: "TBD"
```

#### 2.3 User Stories & Requirements
- [ ] **Pelo menos 5 user stories** - Cobertura suficiente do feature?
- [ ] **Acceptance criteria específicos** - Cada story tem Given-When-Then?
- [ ] **Story points estimados** - Complexidade quantificada (1, 2, 3, 5, 8, 13)?
- [ ] **Priorização clara** - P0 (must-have) vs P1 (should-have) vs P2 (could-have)?
- [ ] **Technical notes incluídas** - Dependencies, tech choices, constraints?

**User Story Quality Check:**
```
? GOOD:
"As a Data Analyst, I want to schedule daily exports
so I don't manually trigger them every morning"

Acceptance Criteria:
- Given I have configured an export template
- When I set a daily schedule at 6AM UTC
- Then the export runs automatically and emails me the result

Story Points: 8
Priority: P0
Technical Notes: Use Celery Beat for scheduling, PostgreSQL for storage

? BAD:
"User wants to schedule exports"
(Sem persona, sem reasoning, sem acceptance criteria, sem estimativa)
```

#### 2.4 Non-Functional Requirements
- [ ] **Performance targets** - Latência, throughput, response time definidos?
- [ ] **Scalability requirements** - Crescimento esperado (users, data, load)?
- [ ] **Security & compliance** - GDPR, SOC2, encryption requirements?
- [ ] **Reliability SLAs** - Uptime, error rate, retry policies?
- [ ] **Usability goals** - Onboarding time, task completion rate?

**Checklist NFRs:**
```
Performance:
- [ ] Latência máxima especificada (ex: p95 < 200ms)
- [ ] Throughput mínimo (ex: 1000 req/s)
- [ ] Tamanho máximo de payload (ex: 10MB)

Scalability:
- [ ] Usuários concorrentes suportados (ex: 10k MAU)
- [ ] Crescimento projetado (ex: 3x em 12 meses)
- [ ] Horizontal scaling possível?

Security:
- [ ] Autenticação method (OAuth, JWT, API keys)
- [ ] Autorização model (RBAC, ABAC)
- [ ] Encryption (at rest, in transit)
- [ ] Compliance frameworks (GDPR, HIPAA, SOC2)
```

---

### 3. DESIGN & UX - Experiência do Usuário

#### 3.1 Wireframes & Mockups
- [ ] **Wireframes de todas screens principais** - Pelo menos low-fidelity?
- [ ] **User flows documentados** - Happy path e edge cases?
- [ ] **Figma/design tool links** - Designs acessíveis para toda equipe?
- [ ] **Design review realizado** - UX designer aprovou?
- [ ] **Responsive design considerado** - Mobile, tablet, desktop?

**User Flow Validation:**
```
Happy Path:
- [ ] Fluxo principal tem <5 steps?
- [ ] Call-to-actions claros em cada step?
- [ ] Success states definidos?

Edge Cases:
- [ ] Error states desenhados? (validation errors, server errors)
- [ ] Loading states? (skeleton screens, spinners)
- [ ] Empty states? (no data, first-time user)
```

#### 3.2 Accessibility
- [ ] **WCAG 2.1 Level AA compliance** - Color contrast, keyboard nav, screen reader?
- [ ] **Keyboard navigation testada** - Tab order lógico, atalhos úteis?
- [ ] **ARIA labels em inputs** - Screen readers funcionam?
- [ ] **Color não é única forma de informação** - Ícones/texto suplementam cores?

---

### 4. TECHNICAL ARCHITECTURE - Viabilidade Técnica

#### 4.1 Architecture Review
- [ ] **High-level diagram presente** - Componentes principais identificados?
- [ ] **Technology stack definido** - Linguagens, frameworks, databases?
- [ ] **Data models especificados** - Schemas de banco, entities?
- [ ] **API contracts documentados** - Endpoints, request/response formats?
- [ ] **Engineering Lead aprovou** - Arquitetura é viável?

**Architecture Checklist:**
```
Components:
- [ ] Frontend stack escolhido (React, Vue, Angular)?
- [ ] Backend framework (FastAPI, Django, Spring)?
- [ ] Database(s) (PostgreSQL, MongoDB, Redis)?
- [ ] Message queue se necessário (Celery, RabbitMQ)?
- [ ] File storage (S3, Azure Blob, local)?

Scalability:
- [ ] Stateless design? (horizontal scaling fácil)
- [ ] Caching strategy? (Redis, Memcached)
- [ ] Load balancing? (NGINX, ALB)
- [ ] Database sharding/replication considerado?
```

#### 4.2 Dependencies & Integrations
- [ ] **Dependências internas mapeadas** - Outros serviços/times envolvidos?
- [ ] **Dependências externas** - Third-party APIs, vendors?
- [ ] **SLAs validados** - Dependências têm uptime/performance aceitável?
- [ ] **Fallback strategies** - O que fazemos se dependência cair?
- [ ] **Contacts documentados** - Email/Slack de owners de cada dependência?

**Dependency Risk Assessment:**
```yaml
dependency: "Auth Service"
type: "Hard Dependency"
sla: "99.99% uptime"
fallback: "Cached tokens (15min validity)"
risk_level: "Low"
contact: "auth-team@example.com"

dependency: "Third-party Analytics API"
type: "Soft Dependency"
sla: "95% uptime (external vendor)"
fallback: "Queue analytics events, retry later"
risk_level: "Medium"
contact: "vendor-support@thirdparty.com"
```

---

### 5. RISKS & MITIGATION - Gestão de Incertezas

#### 5.1 Risk Identification
- [ ] **Pelo menos 5 risks identificados** - Technical, business, operational?
- [ ] **Likelihood scored** - Low/Medium/High para cada risk?
- [ ] **Impact scored** - Low/Medium/High/Critical?
- [ ] **Mitigation plans** - Ações concretas para cada risk?
- [ ] **Risk owners atribuídos** - Quem monitora cada risk?

**Risk Matrix:**
```
         Impact ?
    Low    Med    High   Critical
L   ?     ??     ??     ??
M   ??     ??     ??     ??
H   ??     ??     ??     ??

Legend:
? Accept risk (monitor)
?? Mitigate (plan B)
?? Mitigate + escalate (active management)
?? Block until resolved (unacceptable)
```

**Risk Template:**
```yaml
risk: "Performance degradation com datasets >100k rows"
likelihood: "Medium"
impact: "High"
score: "?? High Priority"
mitigation: |
  - Implementar pagination (chunks de 50k rows)
  - Streaming exports para datasets grandes
  - Load testing com 500k rows antes de GA
owner: "Engineering Lead"
status: "In Progress"
```

#### 5.2 Open Questions
- [ ] **Todas open questions documentadas** - Incertezas capturadas?
- [ ] **Owners atribuídos** - Quem vai responder cada pergunta?
- [ ] **Deadlines definidos** - Quando precisamos de resposta?
- [ ] **Impact assessment** - O que acontece se resposta mudar decisão?

**Open Question Template:**
```yaml
question: "Qual retention policy para exports? 30 dias suficiente?"
owner: "Compliance Officer"
deadline: "2025-02-10"
impact: "Medium - afeta storage costs ($500/month) e GDPR compliance"
blocker: false  # Podemos começar dev enquanto isso é resolvido?
```

---

### 6. ROADMAP & DELIVERY - Plano de Execução

#### 6.1 Phased Rollout
- [ ] **MVP claramente definido** - Qual subset de features é suficiente para launch?
- [ ] **Fases posteriores planejadas** - Phase 2, 3 com objectives claros?
- [ ] **Success criteria por fase** - Como sabemos se fase foi bem-sucedida?
- [ ] **Timeline realista** - Estimativas validadas com Engineering?
- [ ] **Team capacity confirmada** - Recursos disponíveis para timeline proposto?

**MVP Checklist:**
```
? Must-Have (MVP blocker):
- [ ] Core user story #1 implementada
- [ ] Core user story #2 implementada
- [ ] Autenticação funcional
- [ ] Performance targets atingidos (p95)
- [ ] Security audit passou

? Should-Have (Phase 2):
- [ ] Advanced features
- [ ] Optimizações de UX
- [ ] Analytics integrado

?? Could-Have (Phase 3):
- [ ] Nice-to-have features
- [ ] API pública
```

#### 6.2 Milestones & Gates
- [ ] **Milestones definidos** - 4-6 checkpoints ao longo do projeto?
- [ ] **Quality gates** - Critérios de passagem para cada milestone?
- [ ] **Gate owners identificados** - Quem aprova cada gate?
- [ ] **Gantt chart ou timeline** - Visualização do plano?

**Milestone Template:**
```yaml
milestone: "MVP Beta Launch"
date: "2025-03-08"
gate_criteria:
  - "10 early adopters onboarded"
  - "Monitoring dashboards live (Datadog)"
  - "Error rate <5% for 3 consecutive days"
  - "All P0 bugs resolved"
gate_owner: "Engineering Lead"
status: "Not Started"
```

---

### 7. STAKEHOLDER ALIGNMENT - Comunicação e Aprovações

#### 7.1 Stakeholder Identification
- [ ] **Primary stakeholders listados** - Quem tem poder de veto?
- [ ] **Secondary stakeholders** - Quem será impactado mas não aprova?
- [ ] **Responsibilities claras** - Quem faz o quê?
- [ ] **Communication plan** - Frequência e formato de updates?
- [ ] **Escalation path** - Quem resolve deadlocks?

**RACI Matrix Example:**
```
Activity            João PM   Eng Lead   UX    Compliance   Exec
PRD Creation        R         C          C     I            I
Design Review       A         C          R     I            I
Architecture        C         R          I     C            I
Security Audit      I         C          I     R            A
Final Approval      A         C          C     C            R

R = Responsible (faz o trabalho)
A = Accountable (aprova)
C = Consulted (input necessário)
I = Informed (mantido no loop)
```

#### 7.2 Sign-offs & Approvals
- [ ] **PRD approval checklist** - Quem precisa aprovar antes de dev começar?
- [ ] **Design sign-off obtido** - UX designer aprovou wireframes?
- [ ] **Technical feasibility confirmada** - Engineering Lead aprovou arquitetura?
- [ ] **Compliance sign-off** - Se aplicável (GDPR, SOC2, HIPAA)?
- [ ] **Executive sponsor confirmado** - Budget e recursos aprovados?

**Approval Tracking:**
```yaml
approvals:
  - approver: "VP of Product"
    role: "Executive Sponsor"
    status: "? Approved"
    date: "2025-02-10"
    comments: "Strong business case, proceed with MVP"
  
  - approver: "Engineering Lead"
    role: "Technical Feasibility"
    status: "? Pending"
    date: null
    comments: "Reviewing architecture, feedback by 2025-02-12"
  
  - approver: "Compliance Officer"
    role: "Security & Compliance"
    status: "? Rejected"
    date: "2025-02-11"
    comments: "PII masking needs stronger validation - see notes"
    blockers:
      - "Add automated PII detection tests"
      - "Document data retention policy"
```

---

### 8. DEFINITION OF READY - Pronto para Desenvolvimento

#### 8.1 Engineering Handoff
- [ ] **All P0 user stories têm acceptance criteria** - Engineers sabem o que construir?
- [ ] **Technical spikes concluídos** - Incertezas técnicas resolvidas?
- [ ] **Dependencies disponíveis** - APIs, services, dados necessários prontos?
- [ ] **Designs finalizados** - Não há TBDs em wireframes?
- [ ] **Test data disponível** - Engineers podem testar localmente?

**DoR Checklist:**
```
Technical:
- [ ] Architecture diagram aprovado
- [ ] Data models finalizados (schemas SQL)
- [ ] API contracts definidos (OpenAPI/Swagger)
- [ ] Third-party integrations testadas (sandbox)

Process:
- [ ] Stories quebradas em tasks <1 dia
- [ ] Story points estimados por equipe
- [ ] Definition of Done acordada
- [ ] Sprint capacity confirmada

Support:
- [ ] Monitoring dashboards configurados
- [ ] Error tracking setup (Sentry, Rollbar)
- [ ] Logging strategy definida
- [ ] Runbook para on-call criado
```

#### 8.2 QA Readiness
- [ ] **Test plan criado** - Casos de teste mapeados?
- [ ] **Test data preparado** - Datasets para QA disponíveis?
- [ ] **Performance benchmarks** - Como QA valida SLAs?
- [ ] **Accessibility testing plan** - WCAG checklist para QA?

---

### 9. COMMUNICATION PLAN - Transparência e Updates

#### 9.1 Regular Updates
- [ ] **Weekly updates agendados** - Standup, Slack, email?
- [ ] **Status dashboard** - KPIs visíveis em tempo real?
- [ ] **Risk register atualizado** - Risks/issues tracked publicamente?
- [ ] **Changelog mantido** - Mudanças de scope documentadas?

**Communication Cadence:**
```yaml
daily:
  audience: "Engineering Team"
  format: "Slack #export-feature"
  content: "Blockers, progress, quick questions"

weekly:
  audience: "Engineering + PM + UX"
  format: "30min sync (Fridays 2PM)"
  content: "Sprint review, next week planning"

bi_weekly:
  audience: "All Stakeholders"
  format: "Email update (5min read)"
  content: "KPI progress, milestone updates, risks"

monthly:
  audience: "Executives"
  format: "Slide deck (5 slides)"
  content: "Business impact, ROI, roadmap adjustments"
```

#### 9.2 Retrospectives
- [ ] **Retrospective agendada pós-launch** - Lessons learned capturados?
- [ ] **Feedback loop com usuários** - Como coletamos feedback pós-GA?
- [ ] **Iteration plan** - Como ajustamos roadmap baseado em learnings?

---

## ?? APPROVAL GATES

### Gate 1: PRD Approval (Before Development)
```yaml
required_approvals:
  - "? VP Product (business case)"
  - "? Engineering Lead (technical feasibility)"
  - "? UX Designer (design sign-off)"
  - "? Compliance (if applicable)"

checklist_completion: "95%"  # Algumas open questions OK, mas <5%

status: "?? APPROVED TO PROCEED"
```

### Gate 2: Design Review (Before Implementation)
```yaml
required_approvals:
  - "? UX Designer (wireframes final)"
  - "? Accessibility review (WCAG audit)"
  - "? Engineering (design feasibility)"

deliverables:
  - "Figma links com all screens"
  - "User flows documentados"
  - "Component library atualizado"

status: "?? APPROVED TO BUILD"
```

### Gate 3: Architecture Review (Before Coding)
```yaml
required_approvals:
  - "? Engineering Lead (architecture)"
  - "? DevOps (infrastructure feasibility)"
  - "? Security team (threat model)"

deliverables:
  - "Architecture diagram (C4 model)"
  - "Data models (SQL schemas)"
  - "API contracts (OpenAPI spec)"

status: "?? APPROVED TO CODE"
```

### Gate 4: Pre-Launch Review (Before GA)
```yaml
required_approvals:
  - "? QA (all tests passed)"
  - "? Security (pen test passed)"
  - "? Performance (load test passed)"
  - "? Product (UAT approved)"

metrics_validation:
  - "Error rate <5% for 5 days straight"
  - "p95 latency meets SLA"
  - "10 beta users successfully onboarded"

status: "?? APPROVED TO LAUNCH"
```

---

## ?? SCORECARD - Auto-Avaliação

Use este scorecard para avaliar qualidade do seu PRD:

```yaml
categories:
  discovery:
    weight: 20%
    score: 0-10  # Quantas perguntas de Discovery respondidas?
    questions: 9
  
  documentation:
    weight: 25%
    score: 0-10  # Estrutura PRD completa?
    questions: 15
  
  design_ux:
    weight: 15%
    score: 0-10  # Wireframes, flows, accessibility?
    questions: 8
  
  technical:
    weight: 20%
    score: 0-10  # Arquitetura, viabilidade, dependencies?
    questions: 10
  
  risk_management:
    weight: 10%
    score: 0-10  # Risks identificados e mitigados?
    questions: 6
  
  delivery:
    weight: 10%
    score: 0-10  # Roadmap, milestones, team capacity?
    questions: 8

total_score:
  calculation: "sum(category.score × category.weight)"
  threshold:
    excellent: ">90% - Ready for approval"
    good: "75-90% - Minor gaps, addressable"
    needs_work: "60-75% - Significant gaps, revise"
    not_ready: "<60% - Major gaps, restart discovery"
```

**Exemplo de cálculo:**
```
Discovery: 8/10 × 20% = 1.6
Documentation: 9/10 × 25% = 2.25
Design/UX: 7/10 × 15% = 1.05
Technical: 9/10 × 20% = 1.8
Risk Mgmt: 6/10 × 10% = 0.6
Delivery: 8/10 × 10% = 0.8

Total Score: 8.1/10 = 81% ? GOOD (ready for approval com minor gaps)
```

---

## ?? Integração com Outros Artefatos

- **${AVANADE_PRD_TEMPLATE_YAML}**: Template de PRD a ser validado com este checklist
- **${AVANADE_TASK_RICE_PRIORITIZATION}**: Método de priorização para "Strategic Alignment"
- **${AVANADE_DISCOVERY_TEMPLATE_YAML}**: Discovery doc alimenta seção "Context & Background"
- **${AVANADE_MEMORY_PM_JOAO}**: Armazena PRDs aprovados que passaram por checklist

---
