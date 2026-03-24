## ?? O que é este Workflow?

O **create-product-brief** é um workflow Avanade Method v6 de discovery colaborativo que cria um product brief através de facilitação passo-a-passo. É o **kick-off opcional do Phase 1-Discovery** - uma alternativa mais leve que começar direto no PRD.

**Filosofia**: "Collaborative discovery antes de commitment - validar visão antes de PRD completo"

---

## ?? Quando Usar?

### ? Use create-product-brief quando:
- **Greenfield project** com visão ainda nebulosa (precisa discovery antes de PRD)
- **Stakeholder alignment** necessário ANTES de PRD detalhado
- **Quick validation** de ideia (brief = 2-3 páginas, PRD = 10-20 páginas)
- **Kickoff meeting** estruturado com stakeholders
- **Budget approval** precisa de documento conciso de visão
- **Multiple options** sendo consideradas (brief compara alternativas)

### ? NÃO use quando:
- Visão já está clara e aprovada (vá direto para PRD)
- Projeto urgente sem tempo para discovery (use quick-spec)
- Brief já existe e está validado (vá para PRD ou research)
- Projeto muito simples (<1 semana, use quick-dev)

---

## ?? STEP 0: Carregar Contexto FTD (OBRIGATÓRIO)

**Antes de iniciar qualquer step deste workflow:**
1. Ler `.avanade-method/config.yaml` ? `devLoadAlwaysFiles`
2. Carregar docs mandatórios:
   - `ftd-knowledge-base.md` (processos, integrações, glossário)
   - `ftd-discovery.md` (fit-gap, pain points)
   - `especificacao-simulador-notion.md` (spec do Simulador Comercial)
   - `d365-config.yaml` (ambientes, naming, stack)
3. Usar terminologia FTD (Safra, Spartan, Alçada, etc.)
4. Respeitar regras D365 CE + Power Pages + Azure Functions

---

## ?? Workflow Process (6 Steps)

### STEP 1: init
**Objetivo**: Detectar continuação ou novo brief
**Ações**:
- Perguntar: "Criar novo brief ou continuar existente?"
- Se existente: Carregar `{planning_artifacts}/product-brief.md`
- Se novo: Inicializar template vazio
- Configurar contexto de facilitação

**Output**: Brief template ou brief existente carregado

---

### STEP 2: vision (Problem & Solution)
**Objetivo**: Articular problema e solução de alto nível
**Descoberta Guiada**:
```yaml
Perguntas Facilitadoras:
1. "Qual problema específico este produto resolve?" 
   ? Dor real de usuários/negócio, não feature list
2. "Para quem é este problema?" 
   ? User personas primárias (não "todos")
3. "Qual a solução proposta (1-2 frases)?"
   ? WHAT not HOW - "Automatizar exports" não "Build API REST"
4. "Qual o diferencial desta solução?"
   ? O que faz melhor que alternativas atuais
5. "Por que resolver AGORA?"
   ? Market timing, regulatory, competitive pressure
```

**Documentação**:
```markdown
## Problem Statement
[Problema específico com contexto de negócio]

## Target Users
[Personas primárias - não mais que 2-3]

## Proposed Solution
[Descrição high-level da solução - 2-3 parágrafos]

## Value Proposition
[Por que esta solução? Diferencial competitivo?]

## Why Now?
[Urgency drivers - market, regulatory, competitive]
```

**Validação**: Problem Statement claro? Target Users específicos? Solution WHAT not HOW?

---

### STEP 3: users (User Segments & Journeys)
**Objetivo**: Definir user segments e suas jornadas de alto nível
**Descoberta Guiada**:
```yaml
Perguntas Facilitadoras:
1. "Quais são os user segments principais?"
   ? Segmentar por comportamento/necessidades, não demografia
2. "Para cada segment, qual o workflow atual (as-is)?"
   ? Pain points, manual steps, frustrations
3. "Qual a jornada ideal (to-be) com este produto?"
   ? High-level user journey, não detailed UI flows
4. "Quais são os key touchpoints?"
   ? Onde usuário interage com produto
```

**Documentação**:
```markdown
## User Segments

### Segment 1: [Nome]
- **Characteristics**: [Quem são, role, context]
- **Current Pain Points**: [3-5 frustrações específicas]
- **Goals with Product**: [O que querem alcançar]

### Segment 2: [Nome]
...

## High-Level User Journeys

### Journey 1: [Nome - ex: "Export Monthly Report"]
**As-Is (Current State)**:
1. [Manual step 1]
2. [Manual step 2]
...
**Pain Points**: [Time, errors, complexity]

**To-Be (With Product)**:
1. [Automated/improved step 1]
2. [Automated/improved step 2]
...
**Benefits**: [Time saved, error reduction, ease]
```

**Validação**: User segments claros? Pain points específicos? Journeys focam em outcomes não features?

---

### STEP 4: metrics (Success Criteria)
**Objetivo**: Definir métricas de sucesso mensuráveis
**Descoberta Guiada**:
```yaml
Perguntas Facilitadoras:
1. "Como saberemos que este produto teve sucesso?"
   ? Métricas de negócio, não vanity metrics
2. "Quais KPIs existem hoje (baseline)?"
   ? Estabelecer linha de base para comparação
3. "Qual a meta realista em 3-6 meses?"
   ? Targets específicos e alcançáveis
4. "Quais métricas de adoção rastreamos?"
   ? User engagement, retention, satisfaction
```

**Documentação**:
```markdown
## Success Metrics

### Business Impact
- **Metric 1**: [ex: "Reduzir tempo de export de 1.8h para 15min"]
  - Baseline: 1.8h/day per user
  - Target: <15min automated
  - Measurement: Time tracking logs
  
- **Metric 2**: [ex: "Reduzir error rate de 40% para <5%"]
  - Baseline: 40% de exports com erros
  - Target: <5% error rate
  - Measurement: Error logs analysis

### User Adoption
- **Active Users**: [Target após 3 meses]
- **User Satisfaction**: [NPS ou CSAT target]
- **Feature Adoption**: [% de users usando feature core]

### Technical Metrics
- **Performance**: [Response time, uptime targets]
- **Reliability**: [Error rates, availability]
```

**Validação**: Métricas mensuráveis? Baselines definidos? Targets realistas?

---

### STEP 5: scope (High-Level Scope & Constraints)
**Objetivo**: Definir escopo inicial e constraints conhecidos
**Descoberta Guiada**:
```yaml
Perguntas Facilitadoras:
1. "Quais são as capabilities core (MVP)?"
   ? 3-5 capabilities essenciais, não feature list completa
2. "O que está OUT OF SCOPE explicitamente?"
   ? Evitar scope creep - ser claro sobre limites
3. "Quais constraints conhecidos?"
   ? Tech stack, budget, timeline, regulatory, integrations
4. "Quais dependencies de outros sistemas?"
   ? Integrações requeridas, APIs externas
```

**Documentação**:
```markdown
## High-Level Scope

### In Scope (MVP)
1. **Capability 1**: [ex: "Multi-format export (Excel, PDF, CSV)"]
2. **Capability 2**: [ex: "Scheduled automated exports"]
3. **Capability 3**: [ex: "Email delivery of reports"]
...

### Explicitly Out of Scope
- [Feature/capability excluída - ex: "Real-time streaming exports"]
- [Feature/capability excluída - ex: "Custom report builder UI"]

## Constraints

### Technical Constraints
- Tech stack: [Known technologies - ex: "Must use existing .NET infrastructure"]
- Integrations: [Required systems - ex: "Integrate with SAP ERP"]
- Performance: [Non-negotiable limits - ex: "<2s export generation"]

### Business Constraints
- Budget: [If known - ex: "$50k development budget"]
- Timeline: [If known - ex: "Launch Q2 2026"]
- Regulatory: [Compliance requirements - ex: "GDPR compliant"]

### Resource Constraints
- Team size: [If known]
- Expertise gaps: [Skills needed but missing]
```

**Validação**: MVP capabilities claros? Out-of-scope explícito? Constraints documentados?

---

### STEP 6: complete (Finalize & Next Steps)
**Objetivo**: Finalizar brief e sugerir próximos workflows
**Ações**:
1. **Review Completeness**: Todas seções preenchidas?
2. **Executive Summary**: Gerar resumo de 1 parágrafo do brief
3. **Save Brief**: Salvar `{planning_artifacts}/product-brief.md`
4. **Suggest Next Steps**:
   ```yaml
   Próximos Workflows Recomendados:
   - research (se domain/market/tech unknowns) ? avanade-method-bmm-research
   - create-prd (transformar brief em PRD detalhado) ? avanade-method-bmm-create-prd
   - brainstorming (se precisar explorar soluções alternativas) ? avanade-method-brainstorming
   ```

**Output Final**:
```markdown
# Product Brief: [Nome do Produto]

## Executive Summary
[1 parágrafo condensando problem, solution, value, metrics]

## Problem Statement
...

## Target Users
...

## Proposed Solution
...

## Success Metrics
...

## High-Level Scope
...

## Constraints
...

---
**Created**: [timestamp]
**Author**: Maria Analyst (Avanade Method)
**Next Steps**: [Workflows sugeridos]
```

**Validação Final**: Brief completo? Executive summary claro? Next steps sugeridos?

---

## ?? OUTPUT FORMAT

### Product Brief Structure (Template)

```markdown
# Product Brief: [Nome do Produto]

**Created**: [Date]  
**Author**: Maria Analyst  
**Status**: Draft | In Review | Approved

---

## Executive Summary
[1-2 parágrafos: problema, solução, valor, timeline]

---

## Problem Statement
### Context
[Business/market context]

### Problem
[Problema específico que resolve]

### Impact
[Impacto atual do problema - costs, time, errors, frustration]

---

## Target Users
### Primary Segment: [Nome]
- Characteristics: [Who they are]
- Pain Points: [3-5 specific frustrations]
- Goals: [What they want to achieve]

### Secondary Segment: [Nome]
...

---

## Proposed Solution
### Overview
[High-level solution description - 2-3 parágrafos]

### Value Proposition
[Why this solution? Diferencial vs alternativas]

### Why Now?
[Timing drivers - market, competitive, regulatory]

---

## User Journeys (High-Level)

### Journey 1: [Nome]
**As-Is**: [Current manual process]  
**Pain Points**: [Specific issues]  
**To-Be**: [Improved process with product]  
**Benefits**: [Quantified improvements]

---

## Success Metrics

### Business Impact
- **Metric 1**: [Name] - Baseline: [X] ? Target: [Y]
- **Metric 2**: [Name] - Baseline: [X] ? Target: [Y]

### User Adoption
- Active Users: [Target]
- User Satisfaction: [NPS/CSAT target]

### Technical Metrics
- Performance: [Targets]
- Reliability: [Targets]

---

## High-Level Scope

### In Scope (MVP)
1. [Core capability 1]
2. [Core capability 2]
3. [Core capability 3]

### Out of Scope
- [Explicitly excluded feature 1]
- [Explicitly excluded feature 2]

---

## Constraints

### Technical
- Tech Stack: [Requirements]
- Integrations: [Required systems]
- Performance: [Non-negotiable limits]

### Business
- Budget: [If known]
- Timeline: [If known]
- Regulatory: [Compliance requirements]

---

## Next Steps
1. [Recommended workflow 1 - ex: research if unknowns]
2. [Recommended workflow 2 - ex: create-prd for detailed requirements]
```

---

## ?? Integration Points

### Prerequisites (Optional):
- **brainstorming** (se precisa explorar soluções) ? Pode alimentar "Proposed Solution"
- **Stakeholder interviews** (informal) ? Inputs para problem/users/metrics

### Next Steps (Recommended):
1. **research** (se existem unknowns) ? `avanade-method-bmm-research` (market/domain/technical)
   - Use quando: Market não validado, domain novo, tech feasibility incerta
2. **create-prd** (transformar brief em PRD) ? `avanade-method-bmm-create-prd`
   - Use quando: Brief aprovado, ready para requirements detalhados
3. **brainstorming** (se soluções alternativas) ? `avanade-method-brainstorming`
   - Use quando: Proposed Solution precisa exploração criativa

### Artifact Dependencies:
- **Input artifacts**: Nenhum (início do discovery)
- **Output artifacts**: `product-brief.md`
- **Used by**: create-prd (pode referenciar brief como contexto)

---

## ? Best Practices

### DO:
- ? **Focus em problema primeiro** - Entender dor ANTES de solução
- ? **Métricas mensuráveis** - Baselines + targets específicos
- ? **User segments específicos** - "Financial Analysts em mid-size companies" não "everyone"
- ? **Out-of-scope explícito** - Previne scope creep posteriormente
- ? **Constraints realistas** - Budget, timeline, tech stack desde início
- ? **Keep it concise** - Brief = 2-4 páginas, não 20 páginas
- ? **Stakeholder validation** - Review brief com stakeholders antes de PRD

### DON'T:
- ? **Não liste features** - Brief é sobre problema/valor, não feature list
- ? **Não seja vago** - "Melhorar experiência" ? específico "Reduzir tempo de export de 1.8h para 15min"
- ? **Não pule métricas** - "Success" sem números = impossível medir
- ? **Não ignore constraints** - Tech debt, budget, timeline são realidades
- ? **Não faça brief virar PRD** - Se está >5 páginas, está detalhado demais
- ? **Não ignore out-of-scope** - Scope creep começa aqui

---

## ?? Common Pitfalls

### Pitfall 1: **Brief Turns Into PRD**
**Sintoma**: Brief com 15+ páginas, functional requirements detalhados  
**Problema**: Brief perdeu propósito - deve ser conciso para alignment  
**Solução**: Limitar a 2-4 páginas. Detalhes vão no PRD posteriormente

### Pitfall 2: **Vague Success Metrics**
**Sintoma**: "Melhorar satisfação", "Aumentar eficiência" sem números  
**Problema**: Impossível medir success sem baselines e targets  
**Solução**: SEMPRE: Baseline atual + Target específico + Como medir

### Pitfall 3: **Solution Without Problem**
**Sintoma**: Brief foca em features ("Queremos AI chatbot") sem articular problema  
**Problema**: Solução pode não resolver dor real  
**Solução**: Começar SEMPRE com "Qual problema?" ANTES de "Qual solução?"

### Pitfall 4: **No Out-of-Scope**
**Sintoma**: Brief só lista o que está IN scope  
**Problema**: Scope creep inevitável - stakeholders assumem features não documentadas  
**Solução**: Seção OUT-OF-SCOPE explícita - "Não faremos X, Y, Z"

### Pitfall 5: **Ignoring Constraints**
**Sintoma**: Brief otimista sem mencionar budget, timeline, tech constraints  
**Problema**: PRD/Architecture depois descobrem impossibilidades  
**Solução**: Constraints upfront - budget, tech stack, timeline, regulatory

---

## ?? Examples

### Example 1: Good Problem Statement

**GOOD** ?:
```markdown
## Problem Statement

### Context
Financial analysts em mid-size companies (50-500 employees) precisam gerar 
relatórios mensais consolidando dados de múltiplas fontes (ERP, CRM, Excel).

### Problem
Processo atual é 100% manual:
- 1.8 horas/dia por analista copiando dados entre sistemas
- 40% dos exports contêm erros (copy/paste mistakes, formulas quebradas)
- Reports atrasam 2-3 dias após fim do mês (impacta decisões de negócio)

### Impact
- **Time Cost**: 1.8h × 20 dias × $45/hour = $1,620/mês por analista
- **Error Cost**: Decisões baseadas em dados errados, retrabalho, perda de credibilidade
- **Opportunity Cost**: Analistas gastam tempo em tarefa manual vs análise estratégica
```

**BAD** ?:
```markdown
## Problem Statement
Users want better reports. Current process is slow and error-prone.
```
**Por que BAD**: Vago ("better reports"), sem contexto de quem são users, sem quantificação de "slow" ou "error-prone", sem business impact.

---

### Example 2: Good Success Metrics

**GOOD** ?:
```markdown
## Success Metrics

### Business Impact
- **Time Savings**: 
  - Baseline: 1.8h/day manual export process
  - Target: <15min automated process
  - Measurement: Time tracking logs before/after
  - ROI: $1,620/month × 12 months = $19,440/year per analyst

- **Error Reduction**:
  - Baseline: 40% de exports com erros
  - Target: <5% error rate
  - Measurement: Error logs + user reports
  - Impact: Decisions based on accurate data, reduced rework

### User Adoption
- **Active Users**: 80% dos financial analysts usando dentro de 3 meses
- **User Satisfaction**: NPS >40 (currently NPS -10 com processo manual)
- **Feature Adoption**: >90% usando scheduled exports (core feature)
```

**BAD** ?:
```markdown
## Success Metrics
- Improve user satisfaction
- Make process faster
- Reduce errors
```
**Por que BAD**: Sem baselines, sem targets numéricos, sem como medir. "Faster" é 10% ou 90%? "Reduce errors" de quanto para quanto?

---

## ?? Troubleshooting

### Issue: Brief ficou muito longo (>5 páginas)
**Sintoma**: Brief detalhando functional requirements, user stories, wireframes  
**Solução**: Mover detalhes para PRD. Brief deve ter:
- Problem (1 página)
- Solution high-level (1 página)
- Metrics + Scope + Constraints (1-2 páginas)

### Issue: Stakeholders querem features específicas no brief
**Sintoma**: "Adicionar chatbot, mobile app, analytics dashboard" no brief  
**Solução**: Brief documenta CAPABILITIES, não features:
- ? "Chatbot with NLP"
- ? "Self-service support capability"
Features específicas vão no PRD

### Issue: Métricas de success não estão disponíveis
**Sintoma**: "Não temos baseline de quanto tempo leva hoje"  
**Solução**: 
1. Estimar com SMEs ("Financial analysts estimam 1-2h/dia")
2. Documentar como estimativa: "Baseline (estimated): ~1.5h/day"
3. Adicionar em Constraints: "Need to establish proper metrics tracking"

### Issue: Brief aprovado mas visão mudou
**Sintoma**: Durante PRD, stakeholders mudaram direção  
**Solução**: Re-run create-product-brief workflow em EDIT mode:
- Carregar brief existente
- Atualizar seções que mudaram
- Marcar como "v2" e documentar changes

---

## ?? References

- **Avanade Method Workflow Path**: `_avanade-method/bmm/workflows/1-analysis/create-product-brief/`
- **Workflow Manifest Entry**: `workflow-manifest.csv` line 3
- **Command**: `avanade-method-bmm-create-brief`
- **Owner Agent**: Maria Analyst
