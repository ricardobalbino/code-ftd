## ?? O que é este Artefato?

Este é o **framework de priorização RICE** usado na Avanade Method para tomar decisões objetivas sobre o que construir primeiro. RICE combina:
- **R**each: Quantas pessoas/usuários impacta por período?
- **I**mpact: Quão significativo é o impacto por usuário?
- **C**onfidence: Qual nosso nível de certeza sobre Reach/Impact/Effort?
- **E**ffort: Quanto tempo/recursos necessários?

**Fórmula**: `RICE Score = (Reach × Impact × Confidence) / Effort`

---

## ?? Quando Usar

### ? USE para:
- Priorizar features no backlog (P0 vs P1 vs P2)
- Decidir entre múltiplas iniciativas competindo por recursos
- Validar se "hot request" de stakeholder merece prioridade imediata
- Comunicar decisões de priorização de forma objetiva
- Roadmap planning (O que vai em Q1 vs Q2 vs Q3?)

### ? NÃO USE para:
- Bugs críticos de produção (P0 sempre, sem RICE)
- Compliance requirements (obrigatórios, sem escolha)
- Decisões estratégicas top-down (CEO mandates)
- Features já comprometidas com clientes enterprise

---

## ?? Como Calcular RICE Score

### Step 1: Estimar REACH (Quantas pessoas por período)

**Definição**: Quantos usuários/customers serão impactados pelo feature no período de tempo escolhido (geralmente quarter)?

**Fontes de dados:**
- Analytics (MAU, DAU)
- Customer surveys
- Sales data
- User segmentation

**Exemplos:**
```yaml
feature: "Export Automation"
reach:
  calculation: "100 enterprise customers × 5 users/customer = 500 users/quarter"
  source: "Salesforce customer data + usage analytics"
  value: 500

feature: "Dark Mode"
reach:
  calculation: "10,000 MAU × 60% interested (survey) = 6,000 users/quarter"
  source: "User survey (n=500, 60% said 'would use dark mode')"
  value: 6000

feature: "Admin Dashboard Redesign"
reach:
  calculation: "50 admin users total (all would use)"
  source: "User role analytics"
  value: 50
```

**?? Importante**: Escolha período consistente (month vs quarter vs year). **Recomendado: quarter** para balancear short-term e long-term.

---

### Step 2: Estimar IMPACT (Qual tamanho do impacto por usuário)

**Definição**: Quanto cada usuário individual se beneficia? Escala de 0.25 a 3:

| Score | Label    | Descrição | Exemplo |
|-------|----------|-----------|---------|
| **3** | Massive  | Muda completamente como usuário trabalha, game-changer | Automação que elimina 2h/dia de trabalho manual |
| **2** | High     | Melhoria significativa, usuário nota claramente | Reduz tarefa de 30min para 5min |
| **1** | Medium   | Melhoria moderada, usuário percebe mas não é transformador | Adiciona atalho de teclado útil |
| **0.5** | Low    | Pequena melhoria, "nice to have" | Tooltip mais descritivo |
| **0.25** | Minimal | Quase imperceptível, polimento | Ajuste de padding de 8px para 12px |

**Exemplos práticos:**
```yaml
feature: "Export Automation"
impact:
  score: 3  # Massive
  reasoning: |
    Reduz 1.8h/dia de trabalho manual para 15min.
    Elimina 40% error rate (dados críticos).
    ROI mensurável: $120k/ano em time savings.
  source: "User interviews (n=12), time tracking data"

feature: "Dark Mode"
impact:
  score: 0.5  # Low
  reasoning: |
    Preferência estética, não melhora produtividade.
    Reduz eyestrain para alguns usuários (hard to quantify).
  source: "User survey feedback"

feature: "Real-time Collaboration (Google Docs-like)"
impact:
  score: 2  # High
  reasoning: |
    Elimina back-and-forth de emails/Slack para edição.
    Reduz conflitos de versão de 30% para <5%.
  source: "Benchmark de competidores + user pain points"
```

**Dica**: Se em dúvida entre dois scores, escolha o menor (conservative estimate).

---

### Step 3: Estimar CONFIDENCE (Certeza sobre Reach/Impact/Effort)

**Definição**: Quão confiantes estamos nos dados de Reach, Impact e Effort? Escala de 0% a 100%:

| Score | Label | Quando Usar |
|-------|-------|-------------|
| **100%** | High Confidence | Dados sólidos (analytics, A/B tests, produção) |
| **80%** | Medium Confidence | Pesquisa qualitativa boa (interviews, surveys n>50) |
| **50%** | Low Confidence | Assumptions educadas, dados limitados |

**Exemplos:**
```yaml
feature: "Export Automation"
confidence:
  score: 80%  # Medium-High
  reasoning: |
    Reach: 100% (dados exatos de Salesforce)
    Impact: 80% (time tracking + interviews, não A/B test)
    Effort: 70% (estimativa de engineering, não prototipado)
    Average: 83% ? round to 80%
  data_quality:
    reach: "High (exact analytics)"
    impact: "Medium (user research, no A/B test)"
    effort: "Medium (estimation, no spike)"

feature: "AI-Powered Recommendations"
confidence:
  score: 50%  # Low
  reasoning: |
    Reach: 80% (analytics sólidos)
    Impact: 30% (não sabemos se usuários confiarão em AI)
    Effort: 40% (nova tecnologia, muitas unknowns)
    Average: 50%
  data_quality:
    reach: "High"
    impact: "Low (hypothesis não validada)"
    effort: "Low (technical spike needed)"
```

**Red flags para LOW confidence (<50%):**
- ?? Nenhuma user research feita
- ?? Tecnologia nova sem experiência de equipe
- ?? Estimativas de effort baseadas em "gut feeling"
- ?? Impact é "achismo" sem dados

**Como aumentar confidence:**
- ? Fazer user interviews/surveys
- ? Rodar technical spike (proof of concept)
- ? Analisar competidores (benchmarking)
- ? A/B test ou prototype antes de commit

---

### Step 4: Estimar EFFORT (Pessoa-meses de trabalho)

**Definição**: Quanto trabalho total (todas equipes) necessário para lançar feature? Unidade: **person-months** (1 pessoa em tempo integral por 1 mês).

**Inclua no cálculo:**
- Development (frontend + backend)
- Design (UX research, wireframes, visual design)
- QA (test planning, automation, manual testing)
- DevOps (infra, monitoring, deployment)
- PM (coordination, documentation)
- Post-launch support (30 dias após GA)

**Exemplos de cálculo:**
```yaml
feature: "Export Automation"
effort:
  breakdown:
    backend_dev: 1.5  # 1 dev × 1.5 months
    frontend_dev: 1.0  # 1 dev × 1 month
    ux_design: 0.5    # 1 designer × 0.5 month
    qa: 0.5           # 1 QA × 0.5 month
    devops: 0.3       # 1 DevOps × 0.3 month (infra setup)
    pm: 0.2           # 1 PM × 0.2 month (coordination)
  total: 4.0  # person-months
  calendar_time: "6 weeks (with parallel work)"
  assumptions:
    - "Team disponível (não bloqueado por dependencies)"
    - "Scope não cresce (no scope creep)"
    - "Technical spike já foi feito"

feature: "UI Polishing (buttons, spacing, colors)"
effort:
  breakdown:
    frontend_dev: 0.3  # 1 dev × 0.3 month (1.5 weeks)
    ux_design: 0.2     # 1 designer × 0.2 month
    qa: 0.1            # 1 QA × 0.1 month (quick regression)
  total: 0.6  # person-months
  calendar_time: "2 weeks"
```

**?? Armadilhas comuns:**
```
? Subestimar: "Engineering disse 2 weeks" ? Esquecer design, QA, DevOps
? Incluir tudo: Engineering (2w) + Design (1w) + QA (0.5w) + DevOps (0.5w) = 1 month

? Confundir calendar time com person-months:
   "3 devs vão fazer em 1 month" ? Effort = 3 person-months (não 1!)

? Ignorar post-launch:
   "Feature pronto" ? Esquecer bug fixes, monitoring, documentação
```

---

### Step 5: Calcular RICE Score Final

**Fórmula**: `RICE Score = (Reach × Impact × Confidence) / Effort`

**Exemplo completo:**
```yaml
feature: "Export Automation"

reach: 500  # users/quarter
impact: 3   # Massive (3.0)
confidence: 0.8  # 80%
effort: 4.0  # person-months

rice_score: (500 × 3 × 0.8) / 4.0 = 1200 / 4 = 300

interpretation: "300 é ALTO ? Prioridade P0"
```

---

## ?? Interpretação de RICE Scores

### Score Ranges (Guia Geral)

| RICE Score | Prioridade | Ação Recomendada |
|------------|-----------|------------------|
| **>100** | ?? **P0 - Critical** | Roadmap imediato, começa próximo sprint |
| **50-100** | ? **P1 - High** | Próximo quarter, planning já |
| **20-50** | ? **P2 - Medium** | Backlog, considerar para futuro |
| **<20** | ?? **P3 - Low** | Ice box, apenas se tiver spare capacity |

**?? Importante**: Ranges são relativos ao seu contexto. Calibre com sua equipe!

### Exemplos de Comparação

```yaml
features:
  - name: "Export Automation"
    reach: 500
    impact: 3
    confidence: 0.8
    effort: 4.0
    rice: 300  # ?? P0
    reasoning: "High impact, feasible effort, clear user need"
  
  - name: "Real-time Collaboration"
    reach: 2000
    impact: 2
    confidence: 0.5
    effort: 12.0
    rice: 167  # ?? P0
    reasoning: "Massive reach, mas effort alto e confidence baixa (risk!)"
  
  - name: "Dark Mode"
    reach: 6000
    impact: 0.5
    confidence: 1.0
    effort: 2.0
    rice: 1500  # ???? P0+
    reasoning: "Reach enorme, effort baixo, easy win!"
  
  - name: "Advanced Analytics Dashboard"
    reach: 50
    impact: 2
    confidence: 0.8
    effort: 8.0
    rice: 10  # ?? P3
    reasoning: "Reach muito baixo (só admins), effort alto, não justifica"
  
  - name: "Keyboard Shortcuts"
    reach: 3000
    impact: 1
    confidence: 1.0
    effort: 1.0
    rice: 3000  # ?????? P0 (QUICK WIN!)
    reasoning: "Reach grande, effort mínimo, confidence alta = LOW-HANGING FRUIT"

sorted_by_rice:
  1. "Keyboard Shortcuts (3000)" ? DO FIRST (quick win)
  2. "Dark Mode (1500)" ? DO SECOND (quick win)
  3. "Export Automation (300)" ? DO THIRD (high value)
  4. "Real-time Collaboration (167)" ? SPIKE FIRST (reduce confidence risk)
  5. "Advanced Analytics Dashboard (10)" ? DEPRIORITIZE
```

---

## ?? Como Usar RICE no Workflow de PM

### Workflow Completo

```yaml
step_1_discovery:
  action: "Gather initial data for RICE inputs"
  tasks:
    - "User interviews para entender impact"
    - "Analytics para calcular reach"
    - "Engineering spike para estimar effort"
    - "Competitive analysis para calibrar impact"
  output: "RICE scores iniciais para top 10 features do backlog"

step_2_prioritization:
  action: "Score e rankear features"
  tasks:
    - "Calcular RICE para cada feature"
    - "Ordenar por score (descendente)"
    - "Agrupar em P0/P1/P2/P3"
  output: "Priorized backlog com justificativas"

step_3_validation:
  action: "Calibrar com stakeholders"
  tasks:
    - "Apresentar top 5 features com RICE scores"
    - "Discutir assumptions (reach, impact, effort)"
    - "Ajustar confidence se stakeholders discordam"
  output: "RICE scores validados e aprovados"

step_4_roadmap_planning:
  action: "Montar roadmap baseado em RICE"
  tasks:
    - "P0 features ? Q1 2025"
    - "P1 features ? Q2 2025"
    - "P2 features ? Backlog"
  output: "Roadmap quantitativo com RICE justifications"

step_5_retrospective:
  action: "Validar RICE predictions pós-launch"
  tasks:
    - "Reach real vs estimado?"
    - "Impact real vs estimado?"
    - "Effort real vs estimado?"
  output: "Calibrate future RICE estimates com learnings"
```

---

## ?? Template para Cálculo RICE

```yaml
feature: "[Nome do Feature]"

# ===== INPUTS =====
reach:
  value: [número]
  period: "quarter | month | year"
  calculation: "[Mostrar cálculo - ex: 100 customers × 5 users = 500]"
  source: "[Analytics, survey, sales data]"
  notes: "[Assumptions importantes]"

impact:
  value: [0.25 | 0.5 | 1 | 2 | 3]
  label: "[Minimal | Low | Medium | High | Massive]"
  reasoning: |
    [Por que esse score? Qual benefício concreto?]
  source: "[User interviews, A/B test, benchmarks]"
  notes: "[Se em dúvida entre dois scores, qual escolheu e por quê]"

confidence:
  value: [0.50 | 0.80 | 1.00]
  label: "[Low | Medium | High]"
  breakdown:
    reach_confidence: "[Low | Medium | High]"
    impact_confidence: "[Low | Medium | High]"
    effort_confidence: "[Low | Medium | High]"
  reasoning: |
    [Qual qualidade dos dados? O que aumentaria confidence?]
  data_quality: "[Ótima | Boa | Fraca]"

effort:
  value: [person-months]
  breakdown:
    backend: [person-months]
    frontend: [person-months]
    design: [person-months]
    qa: [person-months]
    devops: [person-months]
    pm: [person-months]
  calendar_time: "[weeks ou months]"
  assumptions:
    - "[Assumption 1]"
    - "[Assumption 2]"
  notes: "[Inclui post-launch support? Spike feito?]"

# ===== OUTPUT =====
rice_score:
  calculation: "(reach × impact × confidence) / effort"
  value: [número calculado]
  formula_filled: "([reach] × [impact] × [confidence]) / [effort] = [score]"

priority:
  level: "P0 | P1 | P2 | P3"
  reasoning: "[Por que esse nível de prioridade?]"
  recommended_action: "[Roadmap Q1, Backlog, Ice Box, etc]"

# ===== METADATA =====
metadata:
  author: "[Nome do PM]"
  date: "YYYY-MM-DD"
  status: "Draft | Reviewed | Approved"
  reviewers:
    - name: "[Nome]"
      role: "[Engineering Lead, VP Product, etc]"
      approved: true/false
      comments: "[Feedback]"
  change_log:
    - date: "YYYY-MM-DD"
      change: "[O que mudou]"
      reason: "[Por que mudou]"
```

---

## ?? Exemplos Reais Preenchidos

### Exemplo 1: Feature de Alto Impacto

```yaml
feature: "Automated Data Export System"

reach:
  value: 500
  period: "quarter"
  calculation: "100 enterprise customers × 5 data analysts/customer = 500 users/quarter"
  source: "Salesforce customer data + user role analytics (Jan 2025)"
  notes: "Conservative estimate - only counting active data analysts, not occasional users"

impact:
  value: 3
  label: "Massive"
  reasoning: |
    Current state: 1.8h/day manual exports com 40% error rate
    Future state: 15min/day automated com <5% error rate
    Time savings: 1.65h/day × 500 users × $50/hour = $412k/year
    Error reduction: Critical business data accuracy improved
  source: "User interviews (n=12), time tracking study (n=50), error logs analysis"
  notes: "Scored 3 (Massive) porque é game-changer para workflow diário"

confidence:
  value: 0.80
  label: "Medium-High"
  breakdown:
    reach_confidence: "High (exact data from Salesforce)"
    impact_confidence: "Medium (user research sólido, mas não A/B tested)"
    effort_confidence: "Medium (estimation de engineering, sem spike técnico)"
  reasoning: |
    Reach: 100% confidence (dados exatos)
    Impact: 70% confidence (user research + analytics, não production test)
    Effort: 80% confidence (similar features antes, mas nova tech stack)
    Average: ~83% ? Rounded to 80%
  data_quality: "Boa (mix de quantitativo e qualitativo)"

effort:
  value: 4.0
  breakdown:
    backend: 1.5  # Celery integration, validation service
    frontend: 1.0  # Export UI, scheduling interface
    design: 0.5   # Wireframes, user flows
    qa: 0.5       # Test plans, automation
    devops: 0.3   # Infrastructure setup (S3, Redis)
    pm: 0.2       # Coordination, documentation
  calendar_time: "6 weeks (com work paralelo)"
  assumptions:
    - "Team disponível (2 backend devs, 1 frontend, 1 designer, 1 QA)"
    - "Dependencies prontas (Auth Service, Data Warehouse)"
    - "No scope creep (scope locked após PRD approval)"
  notes: "Inclui 2 semanas de buffer para bug fixes pós-launch"

rice_score:
  calculation: "(reach × impact × confidence) / effort"
  value: 300
  formula_filled: "(500 × 3 × 0.8) / 4.0 = 1200 / 4 = 300"

priority:
  level: "P0"
  reasoning: |
    RICE 300 é alto score. Combinação de:
    - High reach (500 users)
    - Massive impact (transforma workflow diário)
    - Reasonable effort (4 months viável)
    - Strong business case ($412k/year ROI)
  recommended_action: "Roadmap para Q1 2025, começar desenvolvimento em próximo sprint"

metadata:
  author: "João Silva (PM)"
  date: "2025-02-03"
  status: "Approved"
  reviewers:
    - name: "Wilson Souza (Engineering Lead)"
      role: "Technical Feasibility"
      approved: true
      comments: "Effort estimate razoável, arquitetura validada"
    - name: "Maria Oliveira (VP Product)"
      role: "Business Value"
      approved: true
      comments: "Strong ROI, alinha com OKR Q1 de efficiency"
  change_log:
    - date: "2025-02-03"
      change: "Initial RICE calculation"
      reason: "PRD approval process"
```

---

### Exemplo 2: Quick Win (Low Effort, High Reach)

```yaml
feature: "Keyboard Shortcuts for Common Actions"

reach:
  value: 3000
  period: "quarter"
  calculation: "10,000 MAU × 30% power users = 3,000 users/quarter"
  source: "Mixpanel usage analytics (power users = >10 sessions/week)"
  notes: "Conservative - only power users will adopt shortcuts initially"

impact:
  value: 1
  label: "Medium"
  reasoning: |
    Saves 5-10 seconds per action × 50 actions/day = 4-8min/day
    Not transformational, but quality-of-life improvement for power users
    Reduces friction, improves user satisfaction (NPS boost)
  source: "User feature requests (n=120), competitive benchmarking"
  notes: "Scored 1 (Medium) - useful but não game-changer"

confidence:
  value: 1.0
  label: "High"
  breakdown:
    reach_confidence: "High (analytics precisos)"
    impact_confidence: "High (feature comum, benchmarks claros)"
    effort_confidence: "High (technology bem conhecida)"
  reasoning: |
    Reach: 100% (dados exatos de analytics)
    Impact: 100% (feature padrão, sabemos exatamente o que faz)
    Effort: 100% (já implementamos shortcuts antes, tech simples)
  data_quality: "Ótima"

effort:
  value: 1.0
  breakdown:
    backend: 0.0   # Nenhum backend work
    frontend: 0.7  # Keyboard event handlers, UI hints
    design: 0.2    # Shortcut cheatsheet, visual hints
    qa: 0.1        # Quick regression testing
    devops: 0.0    # No infrastructure changes
    pm: 0.0        # Minimal coordination
  calendar_time: "2 weeks"
  assumptions:
    - "Usando biblioteca existente (react-hotkeys)"
    - "Apenas 10-15 shortcuts mais usados (não comprehensive)"
  notes: "Low complexity feature, no dependencies"

rice_score:
  calculation: "(reach × impact × confidence) / effort"
  value: 3000
  formula_filled: "(3000 × 1 × 1.0) / 1.0 = 3000 / 1 = 3000"

priority:
  level: "P0 (Quick Win!)"
  reasoning: |
    RICE 3000 é ALTÍSSIMO por causa de:
    - Massive reach (3k power users)
    - Minimal effort (1 person-month)
    - High confidence (low risk)
    ? Clássico "low-hanging fruit" - alto ROI com baixo investimento
  recommended_action: "Implementar imediatamente (next sprint), easy win para equipe"

metadata:
  author: "João Silva (PM)"
  date: "2025-02-03"
  status: "Approved"
  change_log:
    - date: "2025-02-03"
      change: "Initial RICE - identified as quick win"
```

---

## ?? Armadilhas Comuns e Como Evitar

### Armadilha 1: "Gut Feeling" Prioritization
```
? ERRADO: "CEO quer esse feature, é P0"
? CORRETO: "Vamos calcular RICE para validar se alinha com dados"

Solução:
- Use RICE para TODAS features (mesmo CEO requests)
- Se RICE baixo mas CEO insiste ? Document como "Strategic override"
- Reavaliar quarterly: Feature teve impact esperado?
```

### Armadilha 2: Sandbagging (Inflating Scores)
```
? ERRADO: PM infla impact/reach do seu feature favorito
? CORRETO: Peer review de RICE scores com Engineering/Data

Solução:
- RICE scores precisam approval de pelo menos 2 stakeholders
- Use dados objetivos (analytics, não opinions)
- Retrospectives: Compare RICE predicted vs actual
```

### Armadilha 3: Analysis Paralysis
```
? ERRADO: Gastar 1 semana calculando RICE perfeitamente
? CORRETO: 80/20 rule - estimativas rápidas, iterar depois

Solução:
- Primeira passada: 30min por feature (ballpark RICE)
- Segunda passada (top 5 only): Deep dive com research
- RICE não é ciência exata, é framework de decisão
```

### Armadilha 4: Ignorar Confidence
```
? ERRADO: Feature com RICE 500 mas confidence 30% ? Priorizar P0
? CORRETO: Low confidence = HIGH RISK ? Spike first

Solução:
- RICE > 100 MAS confidence < 50% ? Fazer spike/prototype PRIMEIRO
- Aumentar confidence antes de commit a full development
- Re-calcular RICE após spike com confidence atualizada
```

---

## ?? Retrospective: Validar RICE Predictions

Após launch, compare predicted vs actual:

```yaml
feature: "Export Automation"

predicted_rice:
  reach: 500
  impact: 3
  confidence: 0.8
  effort: 4.0
  rice: 300

actual_results:
  reach: 450  # 90% of predicted (some customers delayed adoption)
  impact: 2.5 # 83% of predicted (time savings confirmed, but error reduction lower)
  effort: 5.5 # 137% of predicted (scope creep + bug fixes)
  confidence_validated: 0.7  # Lower than predicted

actual_rice_retrospective:
  calculation: "(450 × 2.5 × 0.7) / 5.5 = 144"
  variance: "-52% vs predicted (300 ? 144)"

learnings:
  - lesson: "Underestimated effort - scope creep com 'just one more format'"
    action: "Lock scope mais rigidamente em PRD, usar feature flags"
  
  - lesson: "Impact superestimado - error reduction teve challenges técnicos"
    action: "Validation layer needs Phase 2 improvements"
  
  - lesson: "Reach demorou mais - adoption slower than expected"
    action: "Invest more in onboarding/training para future features"

calibration:
  future_estimates:
    - "Multiplicar effort estimates por 1.2x (buffer para scope creep)"
    - "Ser mais conservative com impact scores (escolher lower bound)"
    - "Reach em quarter 1 é 70-80% de total reach (adoption curve)"
```

---

## ?? Integração com Outros Artefatos

- **${AVANADE_PRD_TEMPLATE_YAML}**: RICE score vai na seção `strategic_alignment.priority`
- **${AVANADE_PM_CHECKLIST_MD}**: RICE validation é gate para PRD approval
- **${AVANADE_DISCOVERY_TEMPLATE_YAML}**: Research alimenta inputs de RICE (reach, impact)
- **${AVANADE_MEMORY_PM_JOAO}**: Armazenar RICE scores históricos para calibração
