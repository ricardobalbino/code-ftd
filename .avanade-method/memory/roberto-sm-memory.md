### Sprint Patterns & Velocity
_Padrões de sprints que funcionam_

**Exemplo**:
```yaml
- sprint_length: "2 semanas"
  team_size: "5-7 pessoas (devs + QA)"
  avg_velocity: "22-28 story points"
  pros:
    - "Ritmo sustentável"
    - "Feedback loop rápido"
    - "Planning não muito longo (2-3h)"
  cons:
    - "Overhead de ceremonies (20% do tempo)"
  best_for: "Maioria dos projetos (sweet spot)"
  
- sprint_length: "1 semana"
  team_size: "3-4 pessoas"
  avg_velocity: "12-15 story points"
  pros:
    - "Feedback ultra-rápido"
    - "Adaptação ágil a mudanças"
  cons:
    - "Overhead ceremonies alto (30% do tempo)"
    - "Stories precisam ser bem pequenas"
  best_for: "Startups, MVPs, high uncertainty"
  
- sprint_length: "3-4 semanas"
  team_size: "8-10 pessoas"
  avg_velocity: "40-55 story points"
  pros:
    - "Overhead ceremonies baixo (12% do tempo)"
    - "Mais tempo para features complexas"
  cons:
    - "Feedback loop lento"
    - "Risco de drift (muito tempo sem validação)"
  best_for: "Regulated industries, waterfall-hybrid"
```

---

### Ceremony Optimization Patterns
_Otimizações de ceremonies que funcionaram_

**Exemplo**:
```yaml
- ceremony: "Sprint Planning"
  standard_duration: "4h (2-week sprint)"
  optimization:
    - "Pre-refinement obrigatório (PBIs já estimados)"
    - "Timeboxing: 5min por story (timer visível)"
    - "Parking lot para discussões técnicas profundas"
  optimized_duration: "2h (50% redução)"
  impact: "Team morale +15% (menos meetings), quality mantida"
  
- ceremony: "Daily Standup"
  standard_duration: "15min"
  optimization:
    - "Async updates (Slack) + sync apenas se blockers"
    - "Walk the board (foco em stories, não pessoas)"
    - "Timeboxing: 1min por pessoa max"
  optimized_duration: "5-10min (ou async)"
  impact: "Time savings 50h/sprint (team de 7 pessoas)"
  
- ceremony: "Sprint Review"
  standard_duration: "1h"
  optimization:
    - "Demo pré-gravada (stakeholders assistem antes)"
    - "Review meeting = Q&A + feedback apenas"
    - "Showcases bi-weekly (não toda sprint)"
  optimized_duration: "30min"
  impact: "Stakeholder engagement +30% (respect their time)"
  
- ceremony: "Retrospective"
  standard_duration: "1.5h"
  optimization:
    - "Anonymous feedback pre-collected (Google Forms)"
    - "Dot voting para priorizar discussões"
    - "Action items MAX 3 (foco > quantidade)"
  optimized_duration: "1h"
  impact: "Action item completion rate 60% ? 85%"
```

---

### Backlog Refinement Strategies
_Técnicas de refinement eficazes_

**Exemplo**:
```yaml
- technique: "Continuous Refinement"
  description: "15min/dia durante sprint (vs 2h session semanal)"
  pros:
    - "Menos disruptivo (pequenos chunks)"
    - "Contexto fresco (discussões mais focadas)"
  cons:
    - "Requer disciplina (fácil de pular)"
  effectiveness: "Média-Alta"
  best_for: "Times co-localizados, disciplinados"
  
- technique: "Refinement Sprints"
  description: "1 sprint de refinement a cada 4 sprints de dev"
  pros:
    - "Backlog super refinado (ready para múltiplas sprints)"
    - "Discovery profundo (Maria + Paula + Roberto)"
  cons:
    - "Velocity zero durante refinement sprint"
  effectiveness: "Alta (para projetos complexos)"
  best_for: "Fixed-price, regulated industries"
  
- technique: "Just-in-Time Refinement"
  description: "Refinar apenas top 3-5 PBIs (próxima sprint)"
  pros:
    - "Mínimo waste (não refinar o que pode mudar)"
    - "Flexibilidade alta (pivots fáceis)"
  cons:
    - "Risco de PBIs não prontos para Planning"
  effectiveness: "Média"
  best_for: "Startups, produtos early-stage"
```

---

### Estimation Techniques Validadas
_Métodos de estimativa que funcionam_

**Exemplo**:
```yaml
- technique: "Planning Poker (Modified Fibonacci)"
  sequence: "1, 2, 3, 5, 8, 13, 20, 40, 100"
  pros:
    - "Consenso de time (não top-down)"
    - "Discussão de assumptions (quando discordância grande)"
  cons:
    - "Demorado (5-10min por story)"
  accuracy: "±30% (médio prazo)"
  best_for: "Teams maduros, estimativa de sprints"
  
- technique: "T-Shirt Sizing (XS/S/M/L/XL)"
  pros:
    - "Rápido (1-2min por story)"
    - "Útil para epics/features (high-level)"
  cons:
    - "Menos preciso que Planning Poker"
  accuracy: "±50%"
  best_for: "Roadmap planning, portfolio management (João PM)"
  
- technique: "No Estimates (Cycle Time)"
  pros:
    - "Zero overhead de estimativa"
    - "Focus em flow (throughput metrics)"
  cons:
    - "Difícil para fixed-price projects (cliente quer estimates)"
  accuracy: "N/A (forecasting via cycle time histórico)"
  best_for: "Kanban, continuous delivery, retainer contracts"
```

---

## ?? Impediment Management

### Common Impediments & Resolutions
_Blockers recorrentes e como removê-los_

**Exemplo**:
```yaml
- impediment: "Dependency em time externo (API não pronta)"
  frequency: "Alta (40% das sprints)"
  resolution_pattern:
    - "Mock/Stub API (contract-first development)"
    - "Escalation para João (PM) se delay > 3 dias"
    - "Parallel work (front-end avança com mock)"
  avg_resolution_time: "2 dias"
  prevention: "Dependency mapping no Planning, early engagement"
  
- impediment: "Unclear requirements (PBI volta para refinement)"
  frequency: "Média (25% das sprints)"
  resolution_pattern:
    - "Emergency refinement session (Roberto + Maria + Paula)"
    - "Acceptance criteria re-written com exemplos"
    - "Spike se technical uncertainty"
  avg_resolution_time: "1 dia"
  prevention: "Definition of Ready obrigatório (checklist)"
  
- impediment: "Technical blocker (bug de infra, ambiente down)"
  frequency: "Média (20% das sprints)"
  resolution_pattern:
    - "DevOps/Infra escalation imediata"
    - "Workaround temporário se possível"
    - "Swap stories (time trabalha em outras tasks)"
  avg_resolution_time: "4h - 1 dia"
  prevention: "Infra-as-code, monitoring proativo"
  
- impediment: "Team member ausente (doença, férias)"
  frequency: "Média (15% das sprints)"
  resolution_pattern:
    - "Pair programming (knowledge sharing preventivo)"
    - "Replan sprint (reduzir commitment)"
    - "Cross-training (T-shaped team)"
  avg_resolution_time: "N/A (adjustment imediato)"
  prevention: "Bus factor mitigation (documentation, pairing)"
```

---

### Escalation Patterns
_Quando e como escalar para João (PM) ou sponsor_

**Exemplo**:
```yaml
- escalation_trigger: "Blocker > 3 dias sem resolução"
  escalation_path: "Roberto ? João (PM) ? Sponsor (se needed)"
  communication: "Slack DM + formal email (paper trail)"
  
- escalation_trigger: "Scope creep (stakeholder request direct para dev)"
  escalation_path: "Roberto ? Paula (PO) ? João (PM)"
  communication: "Educar stakeholder sobre change control process"
  
- escalation_trigger: "Team conflict não resolvido em retro"
  escalation_path: "Roberto ? João (PM) + HR (se necessário)"
  communication: "1-on-1s primeiro, escalation se não resolver"
```

---

## ?? Metrics & Team Health

### Velocity Tracking Insights
_Padrões de velocity e o que indicam_

**Exemplo**:
```yaml
- pattern: "Velocity crescente (steady upward trend)"
  interpretation: "Team learning, processo melhorando"
  action: "Manter o curso, documentar learnings"
  
- pattern: "Velocity volátil (altos e baixos)"
  interpretation: "Estimativas inconsistentes OU workload irregular"
  action: "Review estimation process, stabilize backlog"
  
- pattern: "Velocity decrescente (downward trend)"
  interpretation: "?? RED FLAG - burnout, technical debt, ou impediments"
  action: "Emergency retrospective, identify root cause"
  
- pattern: "Velocity estável mas baixa (< team potential)"
  interpretation: "Ceremonies overhead alto OU team sub-utilized"
  action: "Optimize ceremonies, check for blockers silenciosos"
```

---

### Team Morale Indicators
_Sinais de que time está feliz/unhappy_

**Exemplo**:
```yaml
- green_flag: "Daily standup animada (piadas, colaboração)"
  action: "Continue doing what you're doing"
  
- green_flag: "Retros com feedback construtivo (não apenas complaints)"
  action: "Psychological safety alta - maintain"
  
- yellow_flag: "Standup apática (respostas robóticas)"
  action: "1-on-1s com team, investigar causas"
  
- yellow_flag: "Velocity estável mas silêncio em ceremonies"
  action: "Team pode estar funcionando mas não engajada - check wellbeing"
  
- red_flag: "Absences aumentando (sick days, late starts)"
  action: "?? Burnout warning - workload review urgente"
  
- red_flag: "Retros sem action items (apathy)"
  action: "?? Team gave up on improvement - reset culture"
```

---

### Definition of Done (DoD) Evolution
_Como DoD evoluiu ao longo de projetos_

**Exemplo**:
```yaml
- version: "v1.0 (initial)"
  criteria:
    - "Code committed to main branch"
    - "Manual testing passed"
    - "Deployed to dev environment"
  problems: "Bugs escapando para produção (50% bug rate)"
  
- version: "v2.0 (improved)"
  criteria:
    - "Code committed + code review approved (Tiago)"
    - "Unit tests written (>70% coverage)"
    - "Manual testing + regression tests passed (Carla)"
    - "Deployed to staging + smoke tests"
  problems: "Processo lento (throughput -20%)"
  
- version: "v3.0 (optimized - CURRENT)"
  criteria:
    - "Code committed + automated code review (Sonar)"
    - "Unit + integration tests (>80% coverage)"
    - "Automated regression tests passed"
    - "Deployed to staging via CI/CD"
    - "Acceptance criteria validated (demo to Paula)"
  benefits: "Bug rate -70%, throughput restored (automation)"
```

---

## ?? Facilitation Techniques

### Retrospective Formats que Funcionam
_Variações de retro para evitar monotonia_

**Exemplo**:
```yaml
- format: "Start/Stop/Continue (classic)"
  description: "3 colunas - o que começar, parar, continuar fazendo"
  pros: "Simples, estruturado, ação-orientado"
  cons: "Pode ficar repetitivo (usar 1x por mês max)"
  best_for: "Teams novos, retros iniciais"
  
- format: "Sailboat (visual metaphor)"
  description: "Ilha (goal), vento (helps), âncora (impediments), rochas (risks)"
  pros: "Visual, engaja pessoas não-verbais"
  cons: "Requer preparação (desenho)"
  best_for: "Teams remotos (Miro/Mural), brainstorming"
  
- format: "Mad/Sad/Glad"
  description: "Emocional - o que deixou time mad, sad, glad"
  pros: "Foco em sentimentos (psychological safety)"
  cons: "Pode ficar negativo (facilitar bem)"
  best_for: "Sprints difíceis (high stress), team conflicts"
  
- format: "Timeline Retrospective"
  description: "Linha do tempo da sprint - eventos key + sentimentos"
  pros: "Contextual, identifica patterns ao longo do tempo"
  cons: "Demorado (90min+)"
  best_for: "Retrospectives de release (não sprint)"
  
- format: "Lean Coffee"
  description: "Team propõe tópicos, vota, discute (timeboxed)"
  pros: "Democratic, foco no que time quer discutir"
  cons: "Pode perder foco (facilitar bem)"
  best_for: "Teams maduros, self-organizing"
```

---

### Conflict Facilitation Patterns
_Como facilitar conflitos em ceremonies_

**Exemplo**:
```yaml
- conflict_type: "Estimation disagreement (Tiago: 3 SP, Carla: 8 SP)"
  facilitation:
    - "Cada um explica rationale (2min each)"
    - "Identify assumptions divergentes"
    - "Time vote novamente (geralmente converge)"
    - "Se não convergir: escolher maior (conservative) OU spike"
  outcome: "Consenso alcançado 85% das vezes"
  
- conflict_type: "Priority clash (devs querem refactoring, Paula quer features)"
  facilitation:
    - "Quantify technical debt (Tiago: impacto em velocity)"
    - "Quantify business value (Paula: revenue/users)"
    - "Negociar ratio (e.g., 80% features, 20% tech debt)"
  outcome: "Compromise alcançado 90% das vezes"
  
- conflict_type: "Personal conflict (2 team members)"
  facilitation:
    - "1-on-1s separados (escutar ambos os lados)"
    - "Facilitated conversation (Roberto mediador)"
    - "Focus em behaviors, não pessoas (non-violent communication)"
    - "Escalate para João (PM) se não resolver"
  outcome: "Resolvido 70% das vezes sem escalation"
```

---

## ?? Process Improvement Learnings

### Process Anti-Patterns Evitados
_Processos ruins que abandonamos_

**Exemplo**:
```yaml
- anti_pattern: "Scrum Master como Project Manager"
  problem: "Roberto vira gargalo (single point of failure), time não self-organizing"
  solution: "Roberto facilitador apenas - time owns process, João (PM) owns delivery"
  impact: "Team autonomy +50%, Roberto bandwidth liberated"
  
- anti_pattern: "Story points = hours (1 SP = 1 hour)"
  problem: "Estimativas vistas como commitment (pressure), velocity manipulation"
  solution: "Story points abstratos (relative sizing), sem conversão para horas"
  impact: "Estimativas mais honestas, menos gaming"
  
- anti_pattern: "Daily Standup = status report para SM"
  problem: "Time fala para Roberto (não entre si), standup vira micro-management"
  solution: "Walk the board (focus em stories), time coordena entre si"
  impact: "Collaboration +40%, standup duration -50%"
```

---

### Adaptation to Remote/Hybrid
_Ajustes de processo para times remotos_

**Exemplo**:
```yaml
- ceremony: "Sprint Planning (remote)"
  tools: "Zoom + Miro (collaborative board)"
  adjustments:
    - "Pre-work: PBIs pre-refinados (docs compartilhados)"
    - "Breakout rooms para estimation (small groups)"
    - "Digital planning poker (PlanningPoker.com)"
  effectiveness: "Alta (comparable to in-person)"
  
- ceremony: "Daily Standup (hybrid)"
  tools: "Slack (async) + Zoom (sync opcional)"
  adjustments:
    - "Async updates obrigatórias (Slack thread)"
    - "Sync apenas se blockers (opt-in)"
    - "Recording para time members em outros timezones"
  effectiveness: "Alta (flexibilidade +80%)"
  
- challenge: "Team bonding (remote)"
  solutions:
    - "Virtual coffee breaks (Donut bot Slack)"
    - "Retrospectives com icebreakers"
    - "Quarterly off-sites (in-person)"
  impact: "Team cohesion mantida (NPS não caiu)"
```

---

## ?? Cross-References

### Artifacts Relacionados:
- Retrospective Facilitation: `${AVANADE_TASK_RETROSPECTIVE_FACILITATION}`
- Story Readiness: `${AVANADE_TASK_STORY_READINESS}`
- Methodology Compliance: `${AVANADE_TASK_METHODOLOGY_COMPLIANCE}`

### Agent Collaboration:
```yaml
supervisor: ${AVANADE_MEMORY_SUPERVISOR}
po: ${AVANADE_MEMORY_PO_PAULA}
pm: ${AVANADE_MEMORY_PM_JOAO}
dev: ${AVANADE_MEMORY_DEV_TIAGO}
qa: ${AVANADE_MEMORY_QA_CARLA}
```

### Templates:
- Story Template: `${AVANADE_STORY_TEMPLATE_YAML}`

---

## ?? Como Usar Esta Memória

### ? ANTES de Sprint Planning:
1. Consultar **Sprint Patterns** ? escolher sprint length adequado
2. Revisar **Backlog Refinement Strategies** ? garantir PBIs prontos
3. Consultar **Estimation Techniques** ? escolher método apropriado

### ? DURANTE Sprint:
1. Aplicar **Ceremony Optimization Patterns** ? meetings eficientes
2. Usar **Impediment Management** ? resolver blockers rapidamente
3. Monitorar **Team Morale Indicators** ? early warning de problemas

### ? EM Retrospectives:
1. Escolher **Retrospective Formats** ? evitar monotonia
2. Aplicar **Conflict Facilitation** ? resolver divergências construtivamente
3. Evitar **Process Anti-Patterns** ? melhoria contínua

### ? APÓS Sprint:
1. **Atualizar memória** com novos learnings
2. Documentar **Velocity Tracking** ? trends e interpretações
3. Atualizar **DoD Evolution** ? se mudanças no processo

---

## ?? D365 CE Sprint Context - FTD Educação

### Cerimônias FTD (Reais)
| Dia | Reunião | Participantes |
|-----|---------|---------------|
| Segunda | Squad trabalho (10:30) | Dev + UX + Oscar + Kevellin + Fabi |
| Terça | Negócio | Área de negócio + Oscar |
| Quinta | Tecnologia | Arquitetos + Oscar + Medella + Jovanello |

### Times Concorrentes
- **Squad CRM** (Julio): melhorias + sustentação (time pequeno)
- **Squad Área do Cliente**: Canvas App + mesmas tabelas Dataverse (squad link para coordenar)
- **Time de Integração** (Thiago Veiga): constrói endpoints
- **Avanade**: assumindo Onda 1 pós-MVP
- **Pipeline concorrido** com outras áreas da FTD

### Impedimentos Típicos FTD
- Pipeline ocupado por outras áreas (fila para deploy)
- Ambientes QA e RC ainda não configurados no pipeline
- Repo desacoplado dos PBIs (migração em andamento)
- Dependência de infra para VM de batch updates
- Dependência de Giselle para abertura de GMUD/SMAX
- Coexistência de 2 times no mesmo ambiente D365
- Dataverse em nível crítico de armazenamento
- Dependência de Oscar para regras de negócio (ele participa ativamente)

### Sprint Considerations FTD
- Débitos técnicos atacados APENAS quando sobra capacidade ("não planejados")
- Coordenar com Squad Área do Cliente para mudanças em tabelas compartilhadas
- Plugin deploy manual para Dev (pipeline só para OAT/Prod)
- GMUD para Produção requer antecipação (recurso infra)
- **Knowledge Base**: `docs/ftd-knowledge-base.md`

---
