### Project Archetypes Identificados
_Tipos de projetos e padrões de orquestração_

**Exemplo**:
```yaml
- archetype: "Enterprise Integration"
  characteristics:
    - Múltiplos sistemas legacy
    - Compliance pesado (LGPD, SOX)
    - Stakeholders técnicos e de negócio
  recommended_workflow: "Waterfall-Agile hybrid"
  key_agents:
    - primary: [Wilson, Tiago, Carla]
    - secondary: [Maria, João]
  party_mode_usage: "Alta (Architecture Parties frequentes)"
  timeline_pattern: "3-6 meses, releases mensais"
  
- archetype: "MVP/Startup Product"
  characteristics:
    - Time pequeno (3-5 pessoas)
    - Pivot-friendly
    - Focus em speed-to-market
  recommended_workflow: "Lean Startup + Scrum"
  key_agents:
    - primary: [Paula, Sofia, Tiago]
    - secondary: [Maria, Roberto]
  party_mode_usage: "Média (Design Parties)"
  timeline_pattern: "2-4 semanas, releases semanais"
  
- archetype: "E-commerce Platform"
  characteristics:
    - Alta carga (performance crítica)
    - UX é diferencial competitivo
    - Escalabilidade horizontal necessária
  recommended_workflow: "Scrum com UX sprints antecipados"
  key_agents:
    - primary: [Sofia, Wilson, Tiago]
    - secondary: [Paula, Carla]
  party_mode_usage: "Alta (Design + Architecture Parties)"
  timeline_pattern: "4-8 meses, releases bi-semanais"
```

---

### Agent Coordination Patterns
_Padrões de orquestração entre agentes_

**Exemplo**:
```yaml
- pattern: "Discovery-Driven Development"
  sequence:
    1. Maria (Analyst): Discovery Protocol execution
    2. Sofia (UX): Wireframes baseados em discovery
    3. Paula (PO): PRD com value proposition validado
    4. Wilson (Architect): Arquitetura com NFRs claros
    5. Roberto (SM): Breakdown em stories
    6. Tiago (Dev): Implementação
    7. Carla (QA): Quality gates
  effectiveness: "Alta"
  best_for: "Greenfield projects, requirements incertos"
  
- pattern: "Architecture-First"
  sequence:
    1. Wilson (Architect): Spike técnico, ADR inicial
    2. Maria (Analyst): Discovery focado em constraints técnicos
    3. Paula (PO): Priorização baseada em capacidade técnica
    4. Tiago (Dev): PoC de decisões críticas
    5. Fluxo normal (Roberto → Tiago → Carla)
  effectiveness: "Média-Alta"
  best_for: "Projetos com constraints técnicos fortes (compliance, performance)"
  
- pattern: "Lean UX Loop"
  sequence:
    1. Sofia (UX): Low-fi prototypes
    2. Maria (Analyst): Validação com usuários
    3. Sofia (UX): Refino baseado em feedback
    4. Paula (PO): Spec de features validadas
    5. Tiago (Dev): Implementação incremental
  effectiveness: "Alta"
  best_for: "Produtos consumer-facing, UX é prioridade"
```

---

### Workflow Optimization Insights
_Aprendizados sobre execução de workflows_

**Exemplo**:
```yaml
- workflow: "Create Architecture"
  insight: "ADRs devem ser escritos DURANTE discussão (não depois)"
  rationale: "Context loss se deixar para depois → decisões mal documentadas"
  improvement: "Wilson documenta ADR real-time durante Architecture Party"
  impact: "Qualidade de ADRs +40%, tempo de revisão -30%"
  
- workflow: "Sprint Planning"
  insight: "Estimar stories > 8 SP em Planning é bottleneck"
  rationale: "Stories grandes causam debates longos, estimativas imprecisas"
  improvement: "Refinement obrigatório para PBIs > 5 SP antes de Planning"
  impact: "Planning duration -50% (4h → 2h para sprint de 2 semanas)"
  
- workflow: "Code Review"
  insight: "Review apenas por Tiago perde perspectiva de qualidade"
  rationale: "Dev foca em code style, miss security/performance issues"
  improvement: "Quality Party para features críticas (Tiago + Carla + Wilson)"
  impact: "Bugs em produção -60%, vulnerabilidades detectadas +80%"
```

---

### Party Mode Effectiveness Data
_Análise de quando Party Mode agrega valor_

**Exemplo**:
```yaml
- party_type: "Architecture Party"
  activation_count: 23
  avg_duration: "90 minutos"
  success_rate: "87% (decisões não revertidas após)"
  value_indicators:
    - "ADRs mais completos (trade-offs documentados)"
    - "Implementação mais suave (menos surpresas)"
    - "Buy-in técnico maior (devs participaram de decisão)"
  anti_patterns_avoided:
    - "Architecture Astronaut (Wilson sozinho → overengineering)"
    - "YAGNI extremo (Tiago sozinho → technical debt)"
  
- party_type: "Sprint Party"
  activation_count: 45
  avg_duration: "120 minutos"
  success_rate: "92% (sprints commitments cumpridos)"
  value_indicators:
    - "Estimativas mais precisas (planning poker com contexto)"
    - "QA envolvido early (Carla identifica test scenarios up-front)"
    - "Dependencies identificadas cedo (menos blockers)"
  
- party_type: "Full Party"
  activation_count: 8
  avg_duration: "180 minutos"
  success_rate: "75% (consenso alcançado)"
  notes: "Usado para decisões críticas (pivot, tech stack change, major releases)"
  lesson: "Limitado a decisões realmente críticas - overhead alto"
```

---

### Deployment Patterns (Agent Terraform)
_Configurações de VSCode deployment que funcionaram_

**Exemplo**:
```yaml
- deployment_type: "Full Stack Team"
  personas_deployed: "Todos os 8 agentes"
  templates_deployed: "PRD, Architecture, Story, Discovery"
  copilot_instructions: "Global (todos agentes acessíveis)"
  project_type: "Enterprise application"
  team_size: ">10 pessoas"
  feedback: "Positivo - dropdown de personas muito usado"
  
- deployment_type: "Startup Lean Team"
  personas_deployed: "Paula, Sofia, Tiago (core 3)"
  templates_deployed: "PRD, Story"
  copilot_instructions: "Minimal (foco em speed)"
  project_type: "MVP development"
  team_size: "3-5 pessoas"
  feedback: "Positivo - menos overhead, mais agilidade"
  
- deployment_type: "Architecture Focus"
  personas_deployed: "Wilson, Tiago, Carla"
  templates_deployed: "Architecture, ADR"
  copilot_instructions: "Tech-heavy (NFRs, security)"
  project_type: "Modernização de legacy system"
  team_size: "5-8 pessoas (seniors)"
  feedback: "Positivo - ADRs documentados consistentemente"
```

---

### MCP Artifact Management Patterns
_Estratégias eficazes de uso de artifacts_

**Exemplo**:
```yaml
- pattern: "Artifact-Driven Generation"
  description: "Gerar .agent.md e .prompt.md automaticamente de artifacts"
  benefits:
    - "Single source of truth (MCP)"
    - "Atualização em todos projects simultaneamente"
    - "Versionamento centralizado"
  challenges:
    - "Requer MCP server funcionando"
    - "Troubleshooting mais complexo (layer adicional)"
  recommendation: "Usar para teams > 5 pessoas, múltiplos projects"
  
- pattern: "Hybrid (Artifacts + Manual)"
  description: "Personas core via artifacts, customizações manuais"
  benefits:
    - "Flexibilidade para project-specific needs"
    - "Fallback se MCP falhar"
  challenges:
    - "Risco de drift (manual diverge de artifacts)"
  recommendation: "Usar para experimentação, novos workflows"
```

---

## 🎓 Methodological Learnings

### Elicitation Patterns que Funcionam
_Técnicas de discovery multi-agente_

**Exemplo**:
```yaml
- technique: "Reverse Pitch"
  description: "Maria (Analyst) apresenta entendimento de requisitos DE VOLTA ao stakeholder"
  effectiveness: "Alta (95% accuracy em capturar requirements)"
  why_works: "Stakeholder corrige mal-entendidos early, valida alinhamento"
  when_use: "Após discovery inicial, antes de PRD"
  
- technique: "Five Whys (Party Mode)"
  description: "Design Party usa Five Whys colaborativamente"
  effectiveness: "Média-Alta (insights mais profundos)"
  participants: "Maria (facilitador) + Paula (business value) + Sofia (user needs)"
  why_works: "Múltiplas perspectivas revelam root causes complexos"
  when_use: "Problem spaces complexos, soluções não óbvias"
```

---

### Quality Gate Configurations
_Configurações de quality gates eficazes_

**Exemplo**:
```yaml
- gate: "Pre-Production Release Gate"
  checklist:
    - code_coverage: ">80% (Carla)"
    - security_scan: "No high/critical vulns (Wilson)"
    - performance_benchmark: "P95 < 200ms (Wilson)"
    - accessibility: "WCAG AA (Sofia)"
    - clean_code: "No major code smells (Tiago)"
  party_mode: "Quality Party (Carla + Tiago + Wilson)"
  pass_rate: "78%"
  lesson: "Gates rigorosos previnem 90% dos bugs em produção, mas aumentam cycle time +15%"
  
- gate: "Story Ready for Sprint"
  checklist:
    - invest_criteria: "Passed (Roberto)"
    - acceptance_criteria: "Testáveis (Carla)"
    - dependencies: "Identificadas (Roberto)"
    - estimation: "Team consensus (Tiago + Carla)"
  party_mode: "Sprint Party (refinement)"
  pass_rate: "92%"
  lesson: "Stories bem refinadas → sprints mais previsíveis"
```

---

## 🔄 Self-Evolution Insights

### Supervisor Capabilities Evolution
_Como supervisor melhorou ao longo do tempo_

**Exemplo**:
```yaml
- version: "v5.0"
  capability: "Basic orchestration (Maria → Paula → Tiago)"
  limitation: "Orquestração sequencial apenas, sem party mode"
  
- version: "v5.5"
  capability: "Party Mode introduzido (round-robin)"
  improvement: "Decisões arquiteturais 40% mais rápidas (consenso multi-agente)"
  
- version: "v6.0"
  capability: "Agent Terraform (VSCode auto-deploy)"
  improvement: "Setup time 0h (era 2-4h manuais), zero-config experience"
  next_evolution: "Self-healing deployments, auto-update artifacts"
```

---

### Retrospective Learnings (Meta)
_Aprendizados sobre como conduzir retrospectives_

**Exemplo**:
```yaml
- learning: "Retros precisam de facilitador neutro"
  context: "Roberto (SM) facilita, mas devs hesitam em criticar processo Scrum"
  solution: "Full Party com João (PM) facilitando - perspectiva externa"
  impact: "Feedback mais honesto, action items mais acionáveis"
  
- learning: "Action items sem owner morrem"
  context: "60% dos action items não eram executados"
  solution: "Roberto atribui owner + deadline durante retro"
  impact: "Completion rate 60% → 85%"
```

---

## 📊 Metrics & KPIs Tracking

### Agent Performance Indicators
_Métricas de eficácia de cada agente_

**Exemplo**:
```yaml
- agent: "Maria (Analyst)"
  metric: "Discovery completeness"
  measurement: "% de requisitos não descobertos em discovery"
  baseline: "25% miss rate"
  current: "8% miss rate"
  improvement_driver: "Advanced Elicitation Task + Memory usage"
  
- agent: "Wilson (Architect)"
  metric: "ADR quality"
  measurement: "ADRs revertidos ou heavily modified"
  baseline: "30% revert rate"
  current: "12% revert rate"
  improvement_driver: "Architecture Party (multi-perspective validation)"
  
- agent: "Tiago (Dev)"
  metric: "Code quality (Sonar)"
  measurement: "Code smells per 1k LOC"
  baseline: "15 smells/1k LOC"
  current: "6 smells/1k LOC"
  improvement_driver: "Clean Code Task enforcement"
```

---

## 🔗 Cross-References

### Artifacts Relacionados:
- Party Mode Guide: `${AVANADE_PARTY_MODE_GUIDE}`
- Methodology Compliance: `${AVANADE_TASK_METHODOLOGY_COMPLIANCE}`
- Retrospective Facilitation: `${AVANADE_TASK_RETROSPECTIVE_FACILITATION}`

### Agent Memories:
```yaml
analyst: ${AVANADE_MEMORY_ANALYST_MARIA}
architect: ${AVANADE_MEMORY_ARCHITECT_WILSON}
po: ${AVANADE_MEMORY_PO_PAULA}
sm: ${AVANADE_MEMORY_SM_ROBERTO}
qa: ${AVANADE_MEMORY_QA_CARLA}
dev: ${AVANADE_MEMORY_DEV_TIAGO}
ux: ${AVANADE_MEMORY_UX_SOFIA}
pm: ${AVANADE_MEMORY_PM_JOAO}
```

### Templates:
```yaml
prd: ${AVANADE_PRD_TEMPLATE_YAML}
architecture: ${AVANADE_ARCHITECTURE_TEMPLATE}
story: ${AVANADE_STORY_TEMPLATE_YAML}
discovery: ${AVANADE_DISCOVERY_TEMPLATE_YAML}
```

---

## 📌 Como Usar Esta Memória

### ✅ ANTES de orquestrar agentes:
1. Consultar **Project Archetypes** → identificar tipo de projeto
2. Revisar **Agent Coordination Patterns** → escolher fluxo adequado
3. Consultar **Party Mode Effectiveness** → decidir quando usar Party Mode
4. Revisar **Quality Gate Configurations** → configurar gates apropriados

### ✅ DURANTE orquestração:
1. Aplicar **Workflow Optimization Insights** → evitar anti-patterns conhecidos
2. Referenciar **Elicitation Patterns** → guiar discovery multi-agente
3. Monitorar **Agent Performance Indicators** → identificar gaps early

### ✅ APÓS conclusão de projeto:
1. **Atualizar memória** com novos insights
2. Documentar **Self-Evolution** → o que melhorou
3. Adicionar **Retrospective Learnings** → lições para próximos projetos
4. Atualizar **Deployment Patterns** → configurações VSCode eficazes

---

## 🏢 D365 CE Context - FTD Educação

### Project Context
```yaml
type: brownfield
platform: "Dynamics 365 CE + Power Platform"
client: "FTD Educação S/A (Grupo Marista, +120 anos, 28 filiais)"
modules: [Sales (Spartan, PNLD), Customer Service (Hub SAC)]
stack: [Dataverse, Plugins C#, PCF, Azure Functions, Power Automate, Power Pages, Service Bus]
prefix: "ftd"
simulator: "Power Pages (frontend) + CRM (motor)"
integrations: [TOTVS/Datasul, ISA, Adobe Sign, Data Lake, Área do Cliente, Vulcano (eliminar)]
monitoring: [Datadog, Grafana, Application Insights]
```

### Key Business Knowledge
- **Processo**: Conta → Contato → Oportunidade → Proposta (6 etapas) → Aprovação (4 alçadas) → Contrato → TOTVS
- **Dor principal**: 3 HORAS para criar 1 proposta (200 produtos × 3 cliques)
- **Pico sazonal**: Nov-Jan (~5.000 contratos/dia)
- **Canais de venda**: FTD com Você (Lumisfera), Venda Direta, Smart POS, Frente de Loja
- **Linhas de negócio**: Sistema de Ensino, Didático, Bilíngue, Espanhol, Literatura, APE
- **Roadmap**: MVP (31/mar) → Onda 1 pós-MVP (Avanade, ~ago/2026)

### Stakeholders Chave
- **Oscar de Rooij**: PO/Business Analyst (especificações, simulador)
- **Julio Cesar**: Tech Lead CRM FTD
- **Fernando**: Dev Power Pages/Azure Functions
- **Danilo Macedo**: Tech Lead Avanade (conhece FTD desde 2022)
- **Marcio Jovanello**: Manager Avanade
- **Thiago Veiga**: Arquiteto integrações (time separado)

### D365 Routing Matrix
| Request Type | Primary Agent | Secondary | Artifacts |
|-------------|--------------|-----------|-----------|
| Discovery / Fit-Gap | Maria Analyst | - | d365-discovery-template.md |
| PRD D365 | João PM | Maria | d365-prd-template.md |
| Architecture D365 | Wilson Architect | - | d365-architecture-template.md |
| Stories D365 | Paula PO | - | d365-story-template.md |
| Plugin Dev | Tiago Dev | Wilson | d365-plugin-dev.workflow.md |
| Integration Dev | Tiago Dev | Wilson | d365-integration.workflow.md |
| UX Model-driven | Sofia UX | - | d365-form-design patterns |
| QA / Code Review | Carla QA | Tiago | d365-solution-checklist.md |
| Sprint Planning | Roberto SM | - | d365-solution-transport.task.md |
| Documentation | Paige Tech Writer | - | ftd-knowledge-base.md |

### Knowledge Sources
- **FTD Knowledge Base**: `docs/ftd-knowledge-base.md` (FONTE PRIMÁRIA - OBRIGATÓRIA)
- **Transcrições originais**: `docs/transcriçao/` (cenario atual, onboarding, usabilidade)
- **Config D365**: `configs/d365-config.yaml`

---