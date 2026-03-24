---

## ?? Quando Usar Party Mode

### ? Cenários Ideais:
- **Arquitetura Complexa**: Requer input de Architect (Wilson), Dev (Tiago), QA (Carla)
- **Design de Features**: Necessita colaboração entre PO (Paula), UX (Sofia), Analyst (Maria)
- **Sprint Planning**: Envolve SM (Roberto), Dev (Tiago), QA (Carla), PO (Paula)
- **Decisões de Trade-off**: Múltiplas perspectivas (técnica, negócio, UX, qualidade)
- **Retrospectives**: Toda a equipe (todos os agentes)
- **Quality Gates**: Validação multi-dimensional antes de releases

### ? Quando NÃO Usar:
- Tarefas simples que um único agente pode resolver
- Perguntas diretas com resposta única
- Edições de código triviais
- Consultas de documentação

---

## ?? Configurações de Party Disponíveis

### 1. Architecture Party ???
**Participantes**: Wilson (Architect) + Tiago (Dev) + Carla (QA)  
**Foco**: Decisões arquiteturais, ADRs, tech stack  
**Formato**: Round-robin discussion

**Comando**:
```
#party-mode ? architecture-party
```

**Fluxo**:
1. **Wilson**: Propõe arquitetura baseada em requisitos
2. **Tiago**: Valida implementabilidade, complexidade técnica
3. **Carla**: Identifica riscos de qualidade, testabilidade
4. **Wilson**: Refina proposta com feedback
5. **Consenso**: ADR documentado

---

### 2. Design Party ??
**Participantes**: Paula (PO) + Sofia (UX) + Maria (Analyst)  
**Foco**: User experience, product decisions, discovery  
**Formato**: Simultaneous discussion

**Comando**:
```
#party-mode ? design-party
```

**Fluxo**:
1. **Maria**: Apresenta discovery insights, user needs
2. **Sofia**: Propõe wireframes, design patterns
3. **Paula**: Valida value proposition, prioridade
4. **Todos**: Iteração colaborativa até consenso

---

### 3. Quality Party ?
**Participantes**: Carla (QA) + Tiago (Dev) + Wilson (Architect)  
**Foco**: Quality gates, code review, performance  
**Formato**: Sequential review

**Comando**:
```
#party-mode ? quality-party
```

**Fluxo**:
1. **Carla**: Executa test coverage analysis, identifica gaps
2. **Tiago**: Code review (clean code, best practices)
3. **Wilson**: Architecture review (escalabilidade, segurança)
4. **Gate Decision**: Go/No-Go baseado em critérios

---

### 4. Sprint Party ??
**Participantes**: Roberto (SM) + Paula (PO) + Tiago (Dev) + Carla (QA)  
**Foco**: Sprint planning, refinement, retrospectives  
**Formato**: Scrum ceremony simulation

**Comando**:
```
#party-mode ? sprint-party
```

**Fluxo (Planning)**:
1. **Paula**: Apresenta product backlog priorities
2. **Roberto**: Facilita capacity planning
3. **Tiago + Carla**: Estimam stories (planning poker style)
4. **Roberto**: Finaliza sprint backlog commitment

**Fluxo (Retrospective)**:
1. **Roberto**: Facilita discussão (Start/Stop/Continue)
2. **Todos**: Contribuem com insights específicos de seu domínio
3. **Roberto**: Documenta action items

---

### 5. Full Party ??
**Participantes**: Todos os 8 agentes  
**Foco**: Decisões críticas, kickoffs, post-mortems  
**Formato**: Facilitado pelo Supervisor

**Comando**:
```
#party-mode ? full-party
```

**Fluxo**:
1. **João (PM)**: Define objetivos da discussão
2. **Supervisor**: Orquestra ordem de participação
3. **Cada agente**: Contribui com perspectiva especializada
4. **João**: Sintetiza decisões, next steps

---

## ?? Modos de Orquestração

### Round-Robin Discussion
- Cada agente fala sequencialmente
- Ordem definida por relevância ao tópico
- Iterações múltiplas até consenso

**Exemplo**:
```yaml
round_1:
  - Wilson: "Proposta inicial de arquitetura"
round_2:
  - Tiago: "Feedback de implementação"
  - Carla: "Riscos de qualidade"
round_3:
  - Wilson: "Arquitetura refinada"
```

### Simultaneous Discussion
- Todos os agentes contribuem em paralelo
- Útil para brainstorming, ideation
- Supervisor sintetiza ao final

### Sequential Review
- Cada agente revisa output do anterior
- Processo de refinamento em cadeia
- Garante múltiplas camadas de validação

---

## ?? Templates de Ativação

### Template 1: Architecture Decision
```markdown
?? Party Mode: Architecture Party

**Contexto**: [Descreva o problema arquitetural]

**Requisitos**:
- Performance: [target]
- Scalability: [target]
- Compliance: [requirements]

**Constraints**:
- Budget: [limite]
- Timeline: [prazo]
- Tech stack: [restrições]

**Agenda**:
1. Wilson: Proposta de arquitetura
2. Tiago: Avaliação de implementabilidade
3. Carla: Análise de testabilidade
4. Wilson: ADR final

**Output esperado**: Architecture Decision Record (ADR)
```

### Template 2: Sprint Planning
```markdown
?? Party Mode: Sprint Party

**Objetivo**: Planejar Sprint X

**Product Backlog Items**: [Lista de PBIs priorizados]

**Team Capacity**: [Story points disponíveis]

**Agenda**:
1. Paula: Apresentar PBIs (5min each)
2. Roberto: Facilitar planning poker
3. Tiago + Carla: Estimativas e refinamento
4. Roberto: Commitment final

**Output esperado**: Sprint Backlog committed
```

### Template 3: Quality Gate
```markdown
?? Party Mode: Quality Party

**Feature**: [Nome da feature]

**Checklist**:
- [ ] Code coverage > 80% (Carla)
- [ ] Clean code principles (Tiago)
- [ ] Security scan passed (Wilson)
- [ ] Performance benchmarks (Wilson)
- [ ] Accessibility compliance (Sofia - se aplicável)

**Agenda**:
1. Carla: Executar quality checks
2. Tiago: Code review
3. Wilson: Architecture & security review
4. Gate Decision: Go/No-Go

**Output esperado**: Quality Gate Report (Pass/Fail)
```

---

## ?? Comandos de Controle

### Ativação:
```bash
#party-mode ? [configuration]
#party-mode ? custom [agent1, agent2, agent3]
```

### Controle de Discussão:
```bash
#party ? next speaker
#party ? pause
#party ? summarize
#party ? vote [topic]
#party ? end session
```

### Configuração Customizada:
```yaml
custom_party:
  participants:
    - wilson  # Architect
    - tiago   # Developer
    - sofia   # UX Designer
  format: "round-robin"
  rounds: 3
  time_per_agent: "5min"
  output_format: "consensus-document"
```

---

## ?? Outputs de Party Mode

### 1. Consensus Document
- Decisão final acordada por todos
- Rationale de cada perspectiva
- Action items com owners

### 2. Multi-Perspective Report
- Seções separadas por agente
- Visão consolidada ao final
- Matriz de trade-offs

### 3. Decision Matrix
```yaml
criteria:
  - name: "Performance"
    weight: 30%
    scores:
      option_a: 8
      option_b: 6
  - name: "Maintainability"
    weight: 25%
    scores:
      option_a: 7
      option_b: 9
  - name: "Time to Market"
    weight: 20%
    scores:
      option_a: 5
      option_b: 9

recommendation: "Option B (weighted score: 8.05 vs 7.15)"
```

---

## ?? Memory Integration

Cada agente acessa sua própria memória durante Party Mode:

```yaml
maria_memory: ${AVANADE_MEMORY_ANALYST_MARIA}
wilson_memory: ${AVANADE_MEMORY_ARCHITECT_WILSON}
paula_memory: ${AVANADE_MEMORY_PO_PAULA}
roberto_memory: ${AVANADE_MEMORY_SM_ROBERTO}
carla_memory: ${AVANADE_MEMORY_QA_CARLA}
tiago_memory: ${AVANADE_MEMORY_DEV_TIAGO}
sofia_memory: ${AVANADE_MEMORY_UX_SOFIA}
joao_memory: ${AVANADE_MEMORY_PM_JOAO}
supervisor_memory: ${AVANADE_MEMORY_SUPERVISOR}
```

Memórias informam cada agente sobre:
- Padrões recorrentes em seu domínio
- Lições aprendidas de discussões anteriores
- Preferências de stakeholders
- Decisões históricas

---

## ? Best Practices

### DO:
- ? Definir objetivos claros antes de ativar Party Mode
- ? Escolher configuração adequada ao problema
- ? Documentar decisões tomadas (ADRs, meeting notes)
- ? Atualizar memórias dos agentes após sessões
- ? Limitar participantes ao necessário (não convocar todos sempre)

### DON'T:
- ? Usar Party Mode para tarefas triviais (overhead desnecessário)
- ? Convocar agentes irrelevantes ao contexto
- ? Pular documentação de decisões
- ? Ignorar dissenting opinions (explorar divergências)
- ? Executar Party Mode sem agenda definida

---

## ?? Integrações

### Com Workflows Avanade Method:
- **Create Architecture** ? Architecture Party
- **Sprint Planning** ? Sprint Party
- **Code Review** ? Quality Party
- **UX Design** ? Design Party

### Com Tasks:
- `${AVANADE_TASK_ADVANCED_ELICITATION}` ? Design Party
- `${AVANADE_TASK_ARCHITECTURE_QUALITY}` ? Architecture Party
- `${AVANADE_TASK_RETROSPECTIVE_FACILITATION}` ? Sprint Party

### Com Templates:
- `${AVANADE_ARCHITECTURE_TEMPLATE}` ? Output de Architecture Party
- `${AVANADE_PRD_TEMPLATE_YAML}` ? Output de Design Party
- `${AVANADE_STORY_TEMPLATE_YAML}` ? Output de Sprint Party

---

## ?? Referências

- Memória do Supervisor: `${AVANADE_MEMORY_SUPERVISOR}`
- Compliance Methodology: `${AVANADE_TASK_METHODOLOGY_COMPLIANCE}`
- Retrospective Guide: `${AVANADE_TASK_RETROSPECTIVE_FACILITATION}`

---
