## Objetivo
Validar aderência do projeto à Avanade Method e best practices de delivery.

---

## ?? Avanade Method Pillars

### 1. Discovery First (Entender Antes de Construir)
**Critério**: Problema e contexto profundamente entendidos antes de soluções

**Checklist**:
- [ ] **Problem Statement**: Problema claramente definido
- [ ] **Stakeholders**: Mapeados (poder vs interesse)
- [ ] **User Research**: Entrevistas, surveys, ou dados qualitativos
- [ ] **Current State**: As-Is documentado (processos, sistemas)
- [ ] **Success Criteria**: Métricas de sucesso definidas

**Red Flags**:
- ? "Vamos construir X porque sim"
- ? Pulando direto para solução técnica
- ? Stakeholders descobertos durante implementação

**Artifacts**:
- Discovery Document (${AVANADE_DISCOVERY_TEMPLATE_YAML})
- User Personas
- Stakeholder Map
- Current State Assessment

---

### 2. Artifacts-Driven (Documentação Viva)
**Critério**: Artefatos estruturados, completos, e mantidos atualizados

**Checklist**:
- [ ] **PRD**: Product Requirements Document atualizado
- [ ] **Architecture**: Documentação técnica (ADRs, diagramas C4)
- [ ] **Stories**: Backlog com User Stories INVEST-compliant
- [ ] **Tests**: Test plans e casos de teste documentados
- [ ] **Design**: Wireframes, UI specs, Figma/Sketch files
- [ ] **API Specs**: OpenAPI/Swagger para APIs
- [ ] **Deployment**: Runbooks, infra-as-code (Terraform/Bicep)

**Padrão de Qualidade**:
Todos artefatos devem passar:
- ${AVANADE_TASK_EDITORIAL_REVIEW_PROSE} (clareza)
- ${AVANADE_TASK_EDITORIAL_REVIEW_STRUCTURE} (completude)

**Red Flags**:
- ? "Código é a documentação"
- ? Artefatos desatualizados (>1 sprint)
- ? Decisões arquiteturais não documentadas (sem ADRs)

---

### 3. Quality Gates (Validação em Cada Fase)
**Critério**: Validação rigorosa antes de avançar para próxima fase

**Quality Gates por Fase**:

#### ?? Discovery ? Planning
- [ ] **Discovery completo**: Todos stakeholders entrevistados
- [ ] **Problem validation**: Dados suportam necessidade
- [ ] **Success metrics**: KPIs definidos e mensuráveis
- [ ] **Scope agreed**: PO e stakeholders alinhados

#### ?? Planning ? Design
- [ ] **PRD aprovado**: PO validou requisitos
- [ ] **Stories escritas**: Backlog priorizado (RICE/INVEST)
- [ ] **Dependencies mapeadas**: Integrações/APIs identificadas
- [ ] **Risks identified**: Riscos técnicos e de negócio documentados

#### ??? Design ? Development
- [ ] **Architecture reviewed**: ${AVANADE_TASK_ARCHITECTURE_QUALITY}
- [ ] **ADRs criados**: Decisões críticas documentadas
- [ ] **Wireframes aprovados**: UX validada com usuários/stakeholders
- [ ] **DoD definido**: Definition of Done acordado

#### ?? Development ? QA
- [ ] **Code review**: Aprovado por peer (${AVANADE_TASK_CODE_REVIEW})
- [ ] **Unit tests**: Coverage = 80% (${AVANADE_TASK_TEST_COVERAGE})
- [ ] **Documentation**: README, comments, API docs atualizados
- [ ] **CI/CD**: Pipeline verde (build + tests passando)

#### ? QA ? Production
- [ ] **Test plan executed**: Todos cenários testados
- [ ] **UAT approved**: PO/usuários validaram funcionalidade
- [ ] **Performance tested**: Load tests executados
- [ ] **Security reviewed**: Scan de vulnerabilidades (Snyk, SonarQube)
- [ ] **Deployment plan**: Runbook pronto, rollback strategy definida

**Red Flags**:
- ? Pular fases ("vamos codificar direto")
- ? Aprovar fase com issues conhecidos
- ? QA começando apenas no final

---

### 4. Self-Evolution (Melhoria Contínua)
**Critério**: Time reflete e melhora processos regularmente

**Checklist**:
- [ ] **Retrospectives**: Executadas a cada sprint
- [ ] **Action items**: Rastreados e implementados
- [ ] **Metrics tracking**: Velocity, quality, happiness medidos
- [ ] **Knowledge sharing**: Learnings compartilhados (wiki, demos)
- [ ] **Process improvements**: Workflow ajustado baseado em feedback

**Artifacts**:
- Sprint Retrospective Notes (${AVANADE_TASK_RETROSPECTIVE_FACILITATION})
- Action Items Tracker
- Metrics Dashboard (Velocity, Defect Density, Lead Time)

**Red Flags**:
- ? Mesmos problemas recorrentes (3+ sprints)
- ? Retrospectives canceladas ou superficiais
- ? Ações nunca implementadas

---

### 5. Multi-Agent Coordination (Colaboração Estruturada)
**Critério**: Agentes/personas trabalham em sinergia, não silos

**Checklist**:
- [ ] **Handoffs claros**: Discovery ? PM ? Architect ? Dev ? QA ? PO
- [ ] **Shared context**: Todos agentes acessam mesmos artefatos
- [ ] **Cross-functional reviews**: Architect revisa stories, QA participa de design
- [ ] **Party Mode**: Discussões colaborativas quando apropriado (${AVANADE_PARTY_MODE_GUIDE})
- [ ] **Memory sharing**: Aprendizados de 1 agente disponíveis para outros

**Exemplos de Colaboração**:
```
? Bom: Wilson (Architect) + Tiago (Dev) + Carla (QA) discutem design juntos
? Ruim: Architect desenha sozinho ? joga para Dev ? problemas surgem depois
```

**Party Mode Scenarios**:
- Decisões arquiteturais críticas ? Architecture Party
- Sprint retrospectives ? Full Party
- Design reviews ? UX + Dev + Architect

---

## ?? Memory System (Knowledge Management)
**Critério**: Conhecimento é capturado, armazenado, e reutilizado

**Checklist**:
- [ ] **Agent-specific memories**: Cada agente mantém memória (decisions, patterns, learnings)
- [ ] **Consulta pré-execução**: Agentes consultam memória ANTES de executar
- [ ] **Atualização pós-execução**: Memória atualizada APÓS cada interação
- [ ] **Cross-agent access**: Supervisor acessa memória de todos agentes

**Estrutura de Memória**:
```
_memory/
+-- analyst-sidecar/        ? ${AVANADE_MEMORY_ANALYST_MARIA}
+-- architect-sidecar/      ? ${AVANADE_MEMORY_ARCHITECT_WILSON}
+-- po-sidecar/             ? ${AVANADE_MEMORY_PO_PAULA}
+-- sm-sidecar/             ? ${AVANADE_MEMORY_SM_ROBERTO}
+-- qa-sidecar/             ? ${AVANADE_MEMORY_QA_CARLA}
+-- dev-sidecar/            ? ${AVANADE_MEMORY_DEV_TIAGO}
+-- ux-sidecar/             ? ${AVANADE_MEMORY_UX_SOFIA}
+-- supervisor-sidecar/     ? ${AVANADE_MEMORY_SUPERVISOR}
```

**Conteúdo Típico**:
- Decisions & rationale (por que escolhemos X?)
- Patterns validados (code patterns, design patterns)
- Common mistakes (bugs recorrentes, anti-patterns)
- User preferences (templates customizados)

**Red Flags**:
- ? Mesma pergunta feita múltiplas vezes (memória não consultada)
- ? Decisões não documentadas (lost knowledge)
- ? Zero reuso de padrões validados

---

## ?? Compliance Scoring

**Pontos por pilar** (0-20):
- Discovery First: /20
- Artifacts-Driven: /20
- Quality Gates: /20
- Self-Evolution: /20
- Multi-Agent Coordination: /20

**Total: /100**

**Interpretação**:
- **85-100**: ? **EXCELENTE** - Avanade Method totalmente implementado
- **70-84**: ?? **BOM** - Aderência forte, gaps menores
- **50-69**: ?? **ADEQUADO** - Avanade Method parcialmente seguido
- **30-49**: ?? **FRACO** - Desvios significativos da metodologia
- **0-29**: ?? **INADEQUADO** - Metodologia não está sendo seguida

---

## ?? Ações Corretivas por Faixa

### 85-100 (Excelente)
**Ações**:
- Continuar práticas atuais
- Compartilhar learnings com outros projetos
- Considerar tornar-se projeto referência

### 70-84 (Bom)
**Ações**:
- Identificar gaps específicos (scoring detalhado)
- Priorizar 2-3 melhorias para próximo sprint
- Revisar retrospectives (estão gerando ações?)

### 50-69 (Adequado)
**Ações**:
- Revisar processos com Supervisor
- Training sessions em áreas fracas
- Aumentar frequência de quality gates

### 30-49 (Fraco)
**Ações**:
- **Intervenção necessária**: Revisão completa de processo
- Workshop de Avanade Method com todo time
- Supervisor ativamente facilitando handoffs

### 0-29 (Inadequado)
**Ações**:
- **Red Alert**: Escalação para liderança
- Parar e reavaliar se metodologia está sendo seguida
- Reset do projeto com fundamentação adequada

---

## ?? Checklist Rápida (Sprint Review)

Ao final de cada sprint, validar:
- [ ] **Discovery**: Requisitos claros? Problema validado?
- [ ] **Artifacts**: Atualizados e completos?
- [ ] **Quality Gates**: Todos passaram?
- [ ] **Retrospective**: Executada com ações rastreadas?
- [ ] **Collaboration**: Agentes trabalharam juntos?
- [ ] **Memory**: Aprendizados documentados?

**Se qualquer item falhar**: Adicionar ação corretiva na retrospective.

---

## ?? Integração com Metodologia Avanade

- **Frequência**: Avaliar a cada sprint review (quinzenal)
- **Owner**: Supervisor ou Scrum Master
- **Output**: Compliance Report + Action Plan
- **Artifacts Relacionados**:
  - ${AVANADE_TASK_EDITORIAL_REVIEW_STRUCTURE}
  - ${AVANADE_TASK_ADVERSARIAL_REVIEW}
  - ${AVANADE_PARTY_MODE_GUIDE}
  - Todas as ${AVANADE_MEMORY_*} (memórias de agentes)
