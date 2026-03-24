### Value Prioritization Patterns
_Frameworks de priorização validados_

**Exemplo**:
```yaml
- framework: "RICE Score (Reach × Impact × Confidence / Effort)"
  example:
    feature_a:
      reach: 10000  # users affected
      impact: 3     # 0=minimal, 1=low, 2=medium, 3=high
      confidence: 80%
      effort: 13    # story points
      rice_score: 1846  # (10000 × 3 × 0.8) / 13
    feature_b:
      reach: 2000
      impact: 3
      confidence: 100%
      effort: 5
      rice_score: 1200
  decision: "Feature A prioritized (higher RICE)"
  pros: "Quantitativo, menos bias, transparente"
  cons: "Estimativas de reach podem ser imprecisas"
  best_for: "B2C products, data-driven orgs"
  
- framework: "MoSCoW (Must/Should/Could/Won't)"
  example:
    must_have:
      - "User authentication (security)"
      - "Core CRUD operations"
    should_have:
      - "Email notifications"
      - "Export to Excel"
    could_have:
      - "Dark mode"
      - "Advanced filters"
    wont_have:
      - "Mobile app (future release)"
  pros: "Simples, stakeholders entendem facilmente"
  cons: "Subjetivo (tudo vira 'Must' se não facilitar bem)"
  best_for: "Fixed-scope projects, MVP definition"
  
- framework: "Kano Model (Basic/Performance/Delight)"
  example:
    basic: "Sistema deve ser rápido (< 2s load time)" # Absence = dissatisfaction
    performance: "Mais features de relatórios" # More = better
    delight: "Smart suggestions baseadas em ML" # Unexpected wow
  decision: "Basicos primeiro, depois performance, delight opcional"
  pros: "Foco em user satisfaction, não apenas features"
  cons: "Requer research (surveys, user testing)"
  best_for: "Consumer products, competitive markets"
```

---

### Backlog Grooming Insights
_Estratégias eficazes de manutenção de backlog_

**Exemplo**:
```yaml
- practice: "Epics > Features > Stories (3-tier hierarchy)"
  rationale: "Clareza de roadmap (epics), flexibilidade de execution (stories)"
  example:
    epic: "E-commerce Checkout Optimization"
    features:
      - "One-click purchase"
      - "Guest checkout"
      - "Multiple payment methods"
    stories:
      - "Como usuário, quero salvar meu cartão..."
      - "Como guest, quero comprar sem criar conta..."
  benefit: "Roadmap communication (João PM) + team execution (Roberto SM)"
  
- practice: "Backlog grooming cadence: bi-weekly"
  activities:
    - "Review top 20 PBIs (priority order)"
    - "Refine top 5-8 (next sprint candidates)"
    - "Archive/delete stale items (>6 months old)"
  participants: "Paula (PO) + Roberto (SM) + Tiago (Dev) + Maria (Analyst se needed)"
  duration: "90 minutos"
  impact: "Planning meetings -50% duration (PBIs já prontos)"
  
- practice: "User Story Splitting (quando PBI > 8 SP)"
  techniques:
    - "Workflow steps (vertical slice cada step)"
    - "CRUD operations (Create, Read separados)"
    - "Happy path vs edge cases"
    - "Simple/complex variants"
  benefit: "Stories fit in sprint, deploy incrementally"
```

---

### Stakeholder Management Patterns
_Estratégias de gestão de expectativas_

**Exemplo**:
```yaml
- stakeholder_type: "Executive Sponsor (C-level)"
  communication_style: "High-level, outcome-focused"
  cadence: "Monthly (roadmap review)"
  artifacts:
    - "Product roadmap (3-6 months)"
    - "OKRs progress (Objectives & Key Results)"
    - "Business metrics dashboard (revenue, users, NPS)"
  pitfall_avoided: "Status updates técnicos detalhados ? bore executives"
  success_pattern: "Business value storytelling - 'Feature X increased retention 15%'"
  
- stakeholder_type: "End Users (Beta testers)"
  communication_style: "Hands-on, feedback-driven"
  cadence: "Bi-weekly (demo + feedback sessions)"
  artifacts:
    - "Clickable prototypes (Sofia UX)"
    - "Beta releases (staging environment)"
    - "Feedback surveys (Google Forms)"
  pitfall_avoided: "Aceitar todo feedback sem filtro ? feature creep"
  success_pattern: "Validar feedback com data (analytics) antes de priorizar"
  
- stakeholder_type: "Internal Teams (Sales, Support)"
  communication_style: "Collaborative, early involvement"
  cadence: "Sprint reviews (showcase new features)"
  artifacts:
    - "Release notes"
    - "Training materials (for sales enablement)"
    - "FAQs (for support team)"
  pitfall_avoided: "Surpresas em production ? unprepared teams"
  success_pattern: "Involve early (discovery phase) ? they sell/support better"
```

---

### User Story Quality Patterns
_Características de boas user stories_

**Exemplo**:
```yaml
- pattern: "INVEST Criteria (Good Story)"
  independent: "Não depende de outras stories para ser deployada"
  negotiable: "Detalhes de implementação flexíveis (converse com dev)"
  valuable: "Entrega valor ao usuário/negócio (não apenas tarefa técnica)"
  estimable: "Time consegue estimar esforço (se não: refinar ou spike)"
  small: "Cabe em 1 sprint (<= 8 SP idealmente)"
  testable: "Acceptance criteria claros, QA consegue validar"
  
- anti_pattern: "Technical Task disfarçada de User Story"
  bad_example: "Como desenvolvedor, quero refatorar o módulo de autenticação..."
  problem: "Não entrega valor direto ao usuário"
  fix: "Como usuário, quero login mais rápido (<2s) para acessar sistema rapidamente"
  lesson: "Frame em termos de user value, não tarefa técnica"
  
- pattern: "Acceptance Criteria SMART"
  specific: "Sistema deve enviar email em até 5 minutos após ação"
  measurable: "QA pode validar (não vago como 'sistema deve ser rápido')"
  achievable: "Tecnicamente possível com stack atual"
  relevant: "Alinhado com objetivo da story"
  testable: "Carla (QA) consegue escrever test case"
  
  example:
    story: "Como usuário, quero recuperar senha via email"
    acceptance_criteria:
      - "Email enviado em até 5min após request"
      - "Link de reset expira em 24h"
      - "Usuário consegue criar nova senha (min 8 chars, 1 número)"
      - "Email contém branding da empresa (logo, cores)"
```

---

## ?? Product Metrics & Analytics

### Key Product Metrics Tracked
_Métricas que Paula monitora para decisões_

**Exemplo**:
```yaml
- category: "Acquisition"
  metrics:
    - name: "Signup conversion rate"
      current: "12%"
      target: "15%"
      actions: "Simplificar onboarding (Sofia UX + Tiago Dev)"
    - name: "Cost per acquisition (CPA)"
      current: "R$ 50"
      target: "R$ 40"
      actions: "Melhorar SEO, viral loops"
  
- category: "Activation"
  metrics:
    - name: "Time to first value (TTFV)"
      current: "8 minutos"
      target: "< 5 minutos"
      actions: "Onboarding interativo (tooltips, quick wins)"
    - name: "Feature adoption (% users using Feature X)"
      current: "30%"
      target: "50%"
      actions: "In-app messaging, user education"
  
- category: "Retention"
  metrics:
    - name: "Monthly Active Users (MAU)"
      current: "10k"
      target: "15k (50% growth)"
      actions: "Email campaigns, push notifications"
    - name: "Churn rate"
      current: "5%/month"
      target: "< 3%/month"
      actions: "Exit surveys (identify reasons), retention features"
  
- category: "Revenue"
  metrics:
    - name: "Monthly Recurring Revenue (MRR)"
      current: "R$ 100k"
      target: "R$ 150k"
      actions: "Upsell features, new pricing tiers"
    - name: "Customer Lifetime Value (LTV)"
      current: "R$ 1200"
      target: "R$ 1500"
      actions: "Reduce churn, increase ARPU"
```

---

### A/B Testing Insights
_Experimentos que informaram decisões de produto_

**Exemplo**:
```yaml
- experiment: "Onboarding flow (5 steps vs 2 steps)"
  hypothesis: "Shorter onboarding ? higher completion rate"
  variants:
    control: "5 steps (email, name, company, role, preferences)"
    variant_a: "2 steps (email, password - resto opcional)"
  metric: "Signup completion rate"
  results:
    control: "65% completion"
    variant_a: "82% completion (+26%)"
  decision: "Ship variant A - significativo (p<0.05)"
  learning: "Reduce friction em signup, collect data progressively"
  
- experiment: "Pricing page (3 tiers vs 4 tiers)"
  hypothesis: "Mais opções ? mais conversões (choice paradox test)"
  variants:
    control: "3 tiers (Basic R$49, Pro R$99, Enterprise custom)"
    variant_a: "4 tiers (Basic, Plus R$79, Pro, Enterprise)"
  metric: "Paid conversion rate"
  results:
    control: "8% conversion"
    variant_a: "6% conversion (-25%)"
  decision: "Keep control - menos opções é melhor (paradox of choice confirmed)"
  learning: "3 tiers é sweet spot, 4+ confunde usuários"
```

---

### Feature Success/Failure Post-Mortems
_Learnings de features shipped_

**Exemplo**:
```yaml
- feature: "Dark Mode (SUCCESS)"
  motivation: "User requests (top 5 no feedback forum)"
  effort: "5 story points (Tiago + Sofia)"
  impact:
    - "User satisfaction +10% (NPS survey)"
    - "Session duration +15% (less eye strain ? longer usage)"
    - "App Store rating 4.2 ? 4.5"
  learning: "Low effort, high impact - listen to vocal users"
  
- feature: "Advanced Search Filters (FAILURE)"
  motivation: "Assumption: power users need complex filters"
  effort: "34 story points (1 sprint completo)"
  impact:
    - "Usage: 2% of users (98% never touched)"
    - "Complexity added ? other features delayed"
  learning: "Validate assumptions BEFORE building - data > opinions"
  action: "Deprecate feature, simplify UI (remove clutter)"
  
- feature: "Social Sharing (MIXED)"
  motivation: "Viral growth strategy"
  effort: "13 story points"
  impact:
    - "Shares: 500/month (lower than expected 2k/month)"
    - "Acquisition from shares: 50 users (not enough)"
    - "However: Existing users loved it (NPS +5%)"
  learning: "Not a growth driver, but good retention feature - repurpose"
```

---

## ?? Roadmap Planning Insights

### Roadmap Horizon Strategies
_Como estruturar roadmap temporal_

**Exemplo**:
```yaml
- horizon: "Now (Current Sprint)"
  detail_level: "Stories (INVEST, estimadas)"
  commitment: "100% (committed sprint backlog)"
  change_tolerance: "Baixa (apenas emergências)"
  
- horizon: "Next (1-2 sprints)"
  detail_level: "Features (refined PBIs)"
  commitment: "80% (high confidence)"
  change_tolerance: "Média (reprioritize se needed)"
  
- horizon: "Later (3-6 months)"
  detail_level: "Epics (high-level themes)"
  commitment: "50% (roadmap indicativo)"
  change_tolerance: "Alta (pivot-friendly)"
  example: "Q3: Mobile app, Q4: Integrations marketplace"
  
- horizon: "Future (6+ months)"
  detail_level: "Vision (strategic bets)"
  commitment: "20% (exploratório)"
  change_tolerance: "Muito alta"
  example: "AI-powered recommendations, blockchain integration"
```

---

### OKR Integration with Backlog
_Como OKRs informam priorização_

**Exemplo**:
```yaml
- quarter: "Q1 2024"
  objective: "Aumentar user retention"
  key_results:
    - kr1: "Reduce churn de 5% para 3%"
    - kr2: "Increase DAU/MAU ratio de 0.3 para 0.4"
    - kr3: "NPS score de 35 para 45"
  
  backlog_impact:
    high_priority:
      - "Onboarding improvements (kr1, kr2)"
      - "Push notifications (kr2)"
      - "In-app user education (kr1)"
    medium_priority:
      - "Gamification (kr2)"
    deprioritized:
      - "New acquisition features (não alinhado com OKR Q1)"
  
  result:
    - kr1: "? 2.8% churn (exceeded)"
    - kr2: "?? 0.38 DAU/MAU (missed slightly)"
    - kr3: "? NPS 47 (exceeded)"
  learning: "OKRs focused backlog ? clear wins, some misses acceptable"
```

---

## ?? Cross-Functional Collaboration

### Collaboration with Maria (Analyst)
_Patterns de trabalho conjunto_

**Exemplo**:
```yaml
- workflow: "Discovery ? PRD"
  steps:
    1. "Paula define business problem/opportunity"
    2. "Maria executes Discovery Protocol (user research)"
    3. "Paula + Maria co-create PRD (business value + user needs)"
  artifact: "${AVANADE_PRD_TEMPLATE_YAML}"
  benefit: "PRDs com user validation, não apenas assumptions"
  
- anti_pattern_avoided: "Paula escreve PRD sozinha (sem user validation)"
  problem: "Build wrong thing (não valida com usuários)"
  fix: "Maria sempre envolvida em discovery antes de PRD"
```

---

### Collaboration with Sofia (UX)
_Design + Product partnership_

**Exemplo**:
```yaml
- workflow: "Wireframes ? PRD iteration"
  steps:
    1. "Paula compartilha user stories draft"
    2. "Sofia cria low-fi wireframes (validação visual)"
    3. "Paula + Sofia validam com usuários (usability testing)"
    4. "Paula refina stories baseado em feedback"
  artifact: "Figma prototypes + ${AVANADE_PRD_TEMPLATE_YAML}"
  benefit: "UX validation antes de dev ? menos rework"
  
- pattern: "Design Sprints (1 semana)"
  when: "New features complexas ou incertas"
  participants: "Paula + Sofia + Maria + 2-3 stakeholders"
  output: "Validated prototype + PRD"
  effectiveness: "Alta (economiza 2-3 sprints de dev se feature não validar)"
```

---

### Collaboration with Roberto (SM)
_PO + SM partnership_

**Exemplo**:
```yaml
- workflow: "Backlog refinement colaborativo"
  cadence: "Bi-weekly (90min)"
  participants: "Paula + Roberto + Tiago (tech input)"
  activities:
    - "Paula: Apresenta PBIs, business value, acceptance criteria"
    - "Roberto: Facilita discussion, identifica dependencies"
    - "Tiago: Technical feasibility, estimativas iniciais"
  output: "PBIs ready for Planning (INVEST compliant)"
  
- boundary_definition:
  paula_owns: "WHAT (features) e WHY (value)"
  roberto_owns: "HOW (process) e WHEN (sprint planning)"
  shared: "Refinement, priority trade-offs"
  lesson: "Clear boundaries evitam conflitos de responsabilidade"
```

---

## ?? Cross-References

### Artifacts Relacionados:
- PRD Template: `${AVANADE_PRD_TEMPLATE_YAML}`
- Story Template: `${AVANADE_STORY_TEMPLATE_YAML}`
- Value Validation: `${AVANADE_TASK_VALUE_VALIDATION}`
- Story Readiness: `${AVANADE_TASK_STORY_READINESS}`

### Agent Memories:
```yaml
supervisor: ${AVANADE_MEMORY_SUPERVISOR}
analyst: ${AVANADE_MEMORY_ANALYST_MARIA}
ux: ${AVANADE_MEMORY_UX_SOFIA}
sm: ${AVANADE_MEMORY_SM_ROBERTO}
pm: ${AVANADE_MEMORY_PM_JOAO}
```

---

## ?? Como Usar Esta Memória

### ? ANTES de planejar features:
1. Consultar **Value Prioritization Patterns** ? framework adequado ao contexto
2. Revisar **Product Metrics** ? decisões data-driven
3. Consultar **Feature Success/Failure** ? evitar erros passados

### ? DURANTE backlog management:
1. Aplicar **User Story Quality Patterns** ? INVEST compliance
2. Usar **Backlog Grooming Insights** ? manter backlog saudável
3. Aplicar **Stakeholder Management** ? expectativas alinhadas

### ? PARA roadmap planning:
1. Usar **Roadmap Horizon Strategies** ? nível de detalhe apropriado
2. Integrar **OKRs** ? alignment estratégico
3. Considerar **A/B Testing Insights** ? validate assumptions

### ? APÓS features shipped:
1. **Atualizar memória** com metrics de impacto
2. Documentar **Feature Post-Mortems** ? learning loop
3. Atualizar **Product Metrics** ? track progress

---

## ?? D365 CE PO Context - FTD Educação

### Processo Comercial FTD (Jornada Completa)
Conta ? Contato (Rep. Legal) ? Oportunidade ? Produtos ? Proposta (6 etapas) ? Aprovação (4 alçadas) ? Contrato ? Adobe Sign ? TOTVS

### 6 Etapas do Simulador Comercial
1. Dados da Proposta (cliente, sócio, tipo, safra, vigência, alunado)
2. Produtos e Serviços (grid, adição individual/lote, totalizadores real-time)
3. Benefícios, Doações, Patrocínios, Adiantamento
4. Configuração de Vendas/Canais (FTD com Você, Venda Direta, Smart POS, etc.)
5. Matriz de Serviços (em revisão)
6. Análise e Aprovação (big numbers, comparativo ano anterior, alçadas visuais)

### Linhas de Negócio e Regras
| Linha | Majoração | Lote | Especificidade |
|-------|-----------|------|----------------|
| Sistema de Ensino | ? | ? | Coleções (Trilhas ~15 materiais) |
| Didático | ? | ? | - |
| Bilíngue | ? | ? | - |
| Literatura | ? | ? | Produtos individuais |
| Espanhol | ? | ? | - |

### Tipos de Produto
- Prateleira (catálogo padrão)
- Customizado Compartilhado (leve: tirar capítulo)
- Personalizado (pesado: capa, mascote - vinculado a escola)
- Grade (ensino médio, escola específica)

### Epic Patterns FTD
1. **Simulador Comercial** - Power Pages (Onda 1 pós-MVP: Avanade)
2. **Faxina de Dados** - Produtos, tabelas de preço, contas CNPJ
3. **Integração Cadastro Produto** (ISA ? TOTVS ? CRM)
4. **Aprovação de Propostas** - eliminar Vulcano, novas regras
5. **Benefícios/Doações/Patrocínios** - Etapa 3 do simulador
6. **Canais de Venda Unificados** - 1 proposta multi-canal
7. **Higienização de Base** - CNPJ CRM?TOTVS, security roles

### Roadmap
- **MVP (31/mar/2026)**: Adição individual de produtos (time FTD)
- **Onda 1 pós-MVP (~ago/2026)**: Lote, copiar proposta, benefícios, aprovação (Avanade)
- **Pós-Venda**: Comissionamento, inteligência comercial

### Knowledge Base: `docs/ftd-knowledge-base.md` (LEITURA OBRIGATÓRIA)

---
