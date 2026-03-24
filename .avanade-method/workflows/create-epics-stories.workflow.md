## ?? O que é este Workflow?

O **create-epics-and-stories** é o workflow Avanade Method v6 que transforma **planning artifacts** (PRD + Architecture + UX) em **implementation artifacts** (Epics + User Stories). É a **ponte crítica entre Phase 2-Planning e Phase 4-Implementation**.

**Filosofia**: "Stories são contratos entre business e dev - cada story deve ser independently deliverable com acceptance criteria testáveis"

---

## ?? Quando Usar?

### ? Use create-epics-and-stories quando:
- **PRD criado e validado** (requirements claros)
- **Architecture criada** (decisões técnicas tomadas)
- **Ready para implementation** (time pronto para começar sprint)
- **Backlog precisa ser populado** com implementation-ready stories
- **Epic breakdown** necessário (epic grande demais para 1 sprint)

### ? NÃO use quando:
- **PRD não existe** (crie PRD primeiro - `create-prd`)
- **Architecture não existe** (crie Architecture primeiro - `create-architecture`)
- **Quick win** (<1 dia trabalho, use `quick-dev` direto)
- **Stories já existem** e estão validadas (pode ir direto para `sprint-planning`)

---

## ?? Prerequisites (CRITICAL)

### Obrigatórios (Workflow falhará sem):
1. **PRD** (`prd-{project}.md`) - OBRIGATÓRIO
   - Functional requirements claros
   - Success criteria mensuráveis
   - User journeys documentadas

2. **Architecture** (`architecture.md`) - OBRIGATÓRIO
   - Decisões arquiteturais tomadas
   - Tech stack definido
   - Component structure clara

### Recomendados (Melhoram quality):
3. **UX Design** (`ux-design.md`) - RECOMENDADO se projeto tem UI
   - User journeys mapeadas
   - Component strategy definida
   - UX patterns documentados

### Workflow de Validação:
- **check-implementation-readiness** ? Valida PRD + Architecture + Stories ANTES de implementation

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

## ?? Workflow Process (4 Steps)

### STEP 1: validate-prerequisites
**Objetivo**: Garantir que PRD + Architecture existem e são completos
**Validações**:
```yaml
PRD Validation:
  - PRD file existe? ? {planning_artifacts}/prd-{project}.md
  - Functional requirements presentes?
  - Success criteria mensuráveis?
  - User journeys documentadas?

Architecture Validation:
  - Architecture file existe? ? {planning_artifacts}/architecture.md
  - Tech stack definido?
  - Component structure clara?
  - Dependencies identificadas?

UX Validation (se UI project):
  - UX design existe? ? {planning_artifacts}/ux-design.md
  - User journeys mapeadas?
  - Components planejados?
```

**Decisão**:
- ? Se PRD + Architecture completos ? Proceed to STEP 2
- ? Se gaps críticos ? **STOP** e instruir criar/validar PRD ou Architecture primeiro
- ?? Se UX faltando mas UI-heavy ? **WARN** mas permitir continuar

**Output**: Validation report (pass/fail)

---

### STEP 2: design-epics (Create Epic Structure)
**Objetivo**: Organizar work em epics lógicos alinhados com PRD
**Filosofia**: "Epics organizam por USER VALUE, não por technical layers"

#### Epic Design Principles:

**? GOOD Epic Grouping**:
- **By User Value**: "Export Automation", "Report Scheduling", "Email Delivery"
- **By User Journey**: Cada epic = 1 major user journey do PRD
- **By Business Capability**: "Self-Service Analytics", "Data Integration"

**? BAD Epic Grouping** (Evitar):
- **By Technical Layer**: "Backend APIs", "Frontend UI", "Database" (não entrega valor isoladamente)
- **By Team**: "Team A work", "Team B work" (organização interna, não valor)
- **Too Granular**: "Button component", "Input validation" (isso são tasks, não epics)

#### Epic Creation Process:

**Descoberta Guiada**:
```yaml
Perguntas Facilitadoras:
1. "Quais são as capabilities principais do PRD?"
   ? Funcional requirements agrupados por capability
2. "Quais user journeys críticas?"
   ? Top 3-5 journeys do PRD
3. "Como agrupar work para entregar valor incrementalmente?"
   ? Each epic deve deliverar value standalone
4. "Qual epic é MVP (must-have vs nice-to-have)?"
   ? Priority: P0 (MVP), P1 (important), P2 (nice-to-have)
```

**Epic Structure Template**:
```markdown
# Epic: [Nome - ex: "Export Automation"]

**ID**: EPIC-001  
**Priority**: P0 (MVP) | P1 | P2  
**Business Value**: [Por que este epic é valioso - ex: "Elimina 1.8h/dia de trabalho manual"]  
**User Segment**: [Quem se beneficia - ex: "Financial Analysts"]

## Description
[2-3 parágrafos sobre o que este epic entrega]

## Success Criteria
[Como sabemos que epic está completo]
- [ ] Criterion 1
- [ ] Criterion 2

## PRD Traceability
**Requirements Covered**:
- FR-001: Multi-format export
- FR-002: Automated scheduling
- NFR-001: <2s export generation

**User Journeys Covered**:
- Journey 1: "Export Monthly Report"

## Dependencies
- Epic: [Other epic if dependency]
- External: [External system/API if dependency]

## Estimation
- **Story Count**: ~8-12 stories (estimativa inicial)
- **Effort**: [Story points ou dev days - rough estimate]
- **Sprint Capacity**: 2-3 sprints (estimativa)

## Stories
[Lista de stories criadas no STEP 3]
```

**Example Epics**:
```markdown
EPIC-001: Export Automation (P0 - MVP)
+-- Business Value: Eliminar 1.8h/dia trabalho manual, reduzir 40% error rate
+-- Requirements: FR-001, FR-002, FR-003
+-- Estimated Stories: 10-12

EPIC-002: Report Scheduling (P0 - MVP)
+-- Business Value: Scheduled exports sem intervenção manual
+-- Requirements: FR-004, FR-005, NFR-002
+-- Estimated Stories: 6-8

EPIC-003: Email Delivery (P1 - Important)
+-- Business Value: Distribuição automática de reports
+-- Requirements: FR-006, FR-007
+-- Estimated Stories: 4-6

EPIC-004: Advanced Analytics (P2 - Nice-to-have)
+-- Business Value: Insights adicionais para power users
+-- Requirements: FR-008, FR-009
+-- Estimated Stories: 8-10
```

**Epic Prioritization**:
```yaml
P0 (MVP - Must Have):
  - EPIC-001: Export Automation
  - EPIC-002: Report Scheduling

P1 (Important - Should Have):
  - EPIC-003: Email Delivery

P2 (Nice-to-Have - Could Have):
  - EPIC-004: Advanced Analytics
```

**Output**: `{planning_artifacts}/epics.md` com 3-6 epics estruturados

---

### STEP 3: create-stories (Breakdown Epics into Stories)
**Objetivo**: Criar user stories implementation-ready para cada epic
**Filosofia**: "Story = Smallest deployable increment of user value"

#### Story Creation Principles (INVEST):

**I - Independent**: Story pode ser desenvolvida sem depender de outras  
**N - Negotiable**: Detalhes de implementação flexíveis (não spec detalhada)  
**V - Valuable**: Entrega valor para usuário (não "Create database table")  
**E - Estimable**: Team consegue estimar esforço (<8 story points ideal)  
**S - Small**: Completável em 1 sprint (idealmente 2-5 dias)  
**T - Testable**: Acceptance criteria claros e testáveis

#### Story Structure Template:

```markdown
# User Story: [ID - ex: ST-001]

**Epic**: EPIC-001 (Export Automation)  
**Priority**: P0 | P1 | P2  
**Status**: not-started | in-progress | in-review | completed | blocked

---

## User Story
**As a** [persona - ex: "Financial Analyst"]  
**I want** [capability - ex: "export monthly reports to Excel format"]  
**So that** [benefit - ex: "I can share data with stakeholders without manual copy/paste"]

---

## Description
[2-3 frases expandindo context e scope desta story]

---

## Acceptance Criteria (AC)

### AC1: [Descrição do critério]
**Given** [context/precondition]  
**When** [action/trigger]  
**Then** [expected result]

**Example**:
```gherkin
Given I am on the Export page
And I have selected ERP as data source
When I click "Export to Excel" button
Then export file is generated within 2 seconds
And file contains all selected data columns
And file is downloaded automatically
```

### AC2: [Próximo critério]
...

### AC3: Error Handling
**Given** [error scenario]  
**When** [trigger]  
**Then** [error handling behavior]

---

## Technical Notes (For Developers)

### Implementation Guidance
- Architecture reference: [Link to architecture section relevant]
- Components affected: [Ex: "ExportService, FileGenerationAPI"]
- Tech stack: [Ex: "C# backend, React frontend"]

### Dependencies
- Story: [Other story ID if dependency]
- API: [External API if needed]
- Component: [Shared component if dependency]

### Performance Requirements
- [Ex: "Export generation <2s for files up to 10MB"]
- [Ex: "Support up to 100k rows"]

### Security Considerations
- [Ex: "Validate user has permission for data source"]
- [Ex: "Sanitize file names to prevent injection"]

---

## Definition of Done (DoD)

- [ ] Code implemented and peer-reviewed
- [ ] Unit tests written (>80% coverage)
- [ ] Integration tests for AC scenarios
- [ ] Acceptance criteria validated
- [ ] Documentation updated (README, API docs)
- [ ] No critical bugs
- [ ] Code merged to main branch
- [ ] Deployed to staging environment

---

## Estimation
**Story Points**: [1, 2, 3, 5, 8, 13] - Planning poker estimate  
**Effort**: [Dev days - ex: "2-3 days"]

---

## PRD Traceability
**Requirements Implemented**:
- FR-001: Multi-format export capability
- NFR-001: <2s performance requirement

**User Journey**:
- Journey 1: "Export Monthly Report" - Step 2 (Choose format)

---

**Created**: [Date]  
**Assigned To**: [Developer name or "Unassigned"]  
**Sprint**: [Sprint number or "Backlog"]
```

#### Story Breakdown Strategy:

**Para cada Epic, criar stories seguindo SLICING PATTERNS**:

**Pattern 1: By User Flow Steps**
```
Epic: Export Automation
+-- ST-001: Select data source (ERP, CRM, Custom)
+-- ST-002: Choose export format (Excel, PDF, CSV)
+-- ST-003: Preview data before export
+-- ST-004: Execute export and download file
+-- ST-005: View export history
```

**Pattern 2: By Happy Path + Exceptions**
```
Epic: Export Automation
+-- ST-001: Export to Excel (happy path)
+-- ST-002: Handle export errors (network failure, timeout)
+-- ST-003: Handle large datasets (>100k rows pagination)
+-- ST-004: Handle permission errors (unauthorized data source)
```

**Pattern 3: By Component (se Architecture-driven)**
```
Epic: Export Automation
+-- ST-001: Backend - Export API endpoint
+-- ST-002: Backend - File generation service
+-- ST-003: Frontend - Export configuration UI
+-- ST-004: Integration - ERP data source connector
```

**Recomendação**: Combinar patterns - começar com User Flow, adicionar error handling como stories separadas.

#### Story Sizing Guidelines:

**Story Points Scale (Fibonacci)**:
- **1 point**: <1 dia (trivial change, config update)
- **2 points**: 1 dia (small feature, straightforward implementation)
- **3 points**: 2 dias (medium feature, some complexity)
- **5 points**: 3-4 dias (larger feature, multiple components)
- **8 points**: 5-8 dias (complex feature, dependencies)
- **13 points**: >1 semana (**RED FLAG - break down further!**)

**Story Splitting Rule**:
- Se story > 8 points ? **MUST SPLIT** into smaller stories
- Target: Most stories 2-5 points (sweet spot for velocity prediction)

**Output**: 
- `{planning_artifacts}/stories/` folder com individual story files
- Cada story: `ST-001.md`, `ST-002.md`, etc
- Total: ~30-50 stories para projeto médio

---

### STEP 4: final-validation (Validate Stories Against INVEST)
**Objetivo**: Review all stories para INVEST compliance e completeness
**Validação Checklist**:

```yaml
For Each Story:

1. INVEST Validation:
   - [ ] Independent? (Pode desenvolver standalone?)
   - [ ] Negotiable? (Implementation details flexíveis?)
   - [ ] Valuable? (Entrega valor user-facing?)
   - [ ] Estimable? (Team consegue estimar?)
   - [ ] Small? (2-8 story points, <1 sprint?)
   - [ ] Testable? (AC claros e mensuráveis?)

2. Completeness:
   - [ ] User story format correto? (As a/I want/So that)
   - [ ] 3+ Acceptance Criteria?
   - [ ] DoD checklist presente?
   - [ ] PRD traceability documentada?

3. Quality:
   - [ ] AC são testáveis? (Given/When/Then formato)
   - [ ] Technical notes suficientes para developers?
   - [ ] Dependencies identificadas?
   - [ ] Error handling considerado?

4. Priority & Scope:
   - [ ] Priority alinhada com epic priority?
   - [ ] MVP stories marcadas como P0?
   - [ ] Nice-to-have marcadas como P2?
```

**Ações de Validação**:
- **Se story falha INVEST** ? Refactor story (split ou merge)
- **Se AC vagos** ? Clarificar com examples concretos
- **Se dependencies não mapeadas** ? Documentar dependencies
- **Se story >8 points** ? **MUST SPLIT** into smaller stories

**Final Review Questions**:
```yaml
1. "Estas stories cobrem TODOS functional requirements do PRD?"
   ? Traceability check - cada FR deve ter story(s)

2. "Stories estão ordenadas por dependency?"
   ? Story A depende de Story B ? B deve vir antes

3. "MVP stories são sufficient para launch?"
   ? P0 stories entregam value mínimo viável?

4. "Team consegue começar sprint planning com estas stories?"
   ? Stories têm detail suficiente para estimation e tasking?
```

**Output Final**:
- `{planning_artifacts}/epics.md` - Epic structure finalizada
- `{planning_artifacts}/stories/ST-*.md` - 30-50 story files
- Stories INVEST-compliant e implementation-ready

**Next Steps Suggested**:
```yaml
Próximos Workflows:
1. check-implementation-readiness ? Valida PRD+Arch+Stories antes de começar
2. sprint-planning ? Gera sprint-status.yaml e popula backlog
3. create-story ? Cria próxima story se precisar adicionar mais
```

---

## ?? OUTPUT FORMAT

### Epic File Structure (`epics.md`):

```markdown
# Epics: [Project Name]

**Created**: [Date]  
**Authors**: João PM + Roberto SM  
**PRD**: prd-{project}.md  
**Architecture**: architecture.md  
**Total Epics**: 4

---

## Epic Summary

| ID | Name | Priority | Stories | Status | Sprint Target |
|----|------|----------|---------|--------|---------------|
| EPIC-001 | Export Automation | P0 | 12 | not-started | Sprint 1-2 |
| EPIC-002 | Report Scheduling | P0 | 8 | not-started | Sprint 2-3 |
| EPIC-003 | Email Delivery | P1 | 6 | not-started | Sprint 4 |
| EPIC-004 | Advanced Analytics | P2 | 10 | not-started | Future |

---

## EPIC-001: Export Automation

**Priority**: P0 (MVP)  
**Business Value**: Elimina 1.8h/dia trabalho manual por analyst, reduz 40% error rate  
**User Segment**: Financial Analysts

### Description
[Epic description...]

### Success Criteria
- [ ] Users can export to Excel, PDF, CSV formats
- [ ] Export generation <2s for files up to 10MB
- [ ] Error rate <5% (baseline: 40%)

### Stories (12 total)
1. ST-001: Select data source (ERP, CRM, Custom) - 3 points
2. ST-002: Choose export format (Excel, PDF, CSV) - 2 points
3. ST-003: Preview data before export - 5 points
...

### PRD Traceability
- FR-001: Multi-format export
- FR-002: Automated processing
- NFR-001: <2s performance

---

## EPIC-002: Report Scheduling
[Same structure...]
```

### Story File Structure (`stories/ST-001.md`):

```markdown
# ST-001: Select Data Source for Export

**Epic**: EPIC-001 (Export Automation)  
**Priority**: P0  
**Points**: 3  
**Status**: not-started

---

## User Story
As a Financial Analyst  
I want to select which data source to export from (ERP, CRM, or Custom Query)  
So that I can choose the right data for my monthly report

---

## Acceptance Criteria

### AC1: Data Source Selection
Given I am on the Export page
When I see the data source dropdown
Then I should see 3 options: "ERP", "CRM", "Custom Query"
And default selection is "ERP"

### AC2: ERP Selection
Given I select "ERP" as data source
When the selection is made
Then the system loads available ERP datasets
And displays them in a secondary dropdown

[More ACs...]

---

## Technical Notes
- Architecture ref: Section 4.2 "Data Source Connectors"
- Components: ExportService, DataSourceRegistry
- ERP connector uses SAP RFC API

---

## Definition of Done
- [ ] Code implemented
- [ ] Unit tests (>80% coverage)
- [ ] Integration test for AC1-AC3
- [ ] Peer review completed
- [ ] Deployed to staging

---

**Created**: 2025-02-03  
**Assigned**: Unassigned  
**Sprint**: Backlog
```

---

## ?? Integration Points

### Prerequisites (Critical):
1. **create-prd** ? PRD must exist
2. **create-architecture** ? Architecture must exist
3. **create-ux-design** (recommended if UI) ? UX patterns inform story breakdown

### Validation Workflow:
- **check-implementation-readiness** ? Run BEFORE implementation to validate PRD+Arch+Stories alignment

### Next Steps:
1. **sprint-planning** ? Generate `sprint-status.yaml` tracking
2. **create-story** ? Add new stories to epics as needed
3. **dev-story** ? Implement individual stories

### Artifacts Flow:
```
PRD + Architecture + UX Design
         ?
create-epics-and-stories
         ?
epics.md + stories/*.md
         ?
check-implementation-readiness (validation)
         ?
sprint-planning (populate sprint-status.yaml)
         ?
dev-story (implement)
```

---

## ? Best Practices

### DO:
- ? **Epic by user value** - "Export Automation" não "Backend APIs"
- ? **Story slicing vertical** - End-to-end slice com UI+backend+data
- ? **INVEST validation** - Every story MUST pass INVEST criteria
- ? **Testable AC** - Given/When/Then formato, concrete examples
- ? **Small stories** - 2-5 points sweet spot (1-3 dias)
- ? **PRD traceability** - Each FR must have story(s)
- ? **Error handling stories** - Não just happy path

### DON'T:
- ? **Epic by technical layer** - "Frontend", "Backend", "Database" não entrega valor
- ? **Story horizontal slicing** - "Create all database tables" não é deployable
- ? **Vague AC** - "System should work correctly" não é testável
- ? **Large stories** - >8 points = MUST SPLIT
- ? **Skip DoD** - Definition of Done é contract de quality
- ? **Ignore dependencies** - Não documentar dependencies = blocked sprints

---

## ?? Common Pitfalls

### Pitfall 1: **Epic by Technical Layer**
**Sintoma**: EPIC-001: "Frontend UI", EPIC-002: "Backend APIs", EPIC-003: "Database"  
**Problema**: Nenhum epic entrega valor standalone - precisa todos 3 para funcionar  
**Solução**: Epic by user capability - "Export Automation", "Report Scheduling" (cada entrega valor)

### Pitfall 2: **Horizontal Story Slicing**
**Sintoma**: ST-001: "Create all database tables", ST-002: "Build all API endpoints"  
**Problema**: Não deployable, não testable, não valuable  
**Solução**: Vertical slicing - ST-001: "Export to Excel" (UI + API + data, end-to-end)

### Pitfall 3: **Vague Acceptance Criteria**
**Sintoma**: AC1: "Export should work correctly", AC2: "User should be happy"  
**Problema**: Não testável, subjetivo, não define "done"  
**Solução**: Given/When/Then com examples concretos - AC1: "Given ERP selected, When click Export, Then Excel file downloads within 2s"

### Pitfall 4: **Stories Too Large (>8 points)**
**Sintoma**: ST-001: "Implement entire export system" - 21 points  
**Problema**: Não completável em 1 sprint, hard to estimate, risky  
**Solução**: MUST SPLIT - ST-001: "Select data source" (3pt), ST-002: "Choose format" (2pt), etc

### Pitfall 5: **Missing Error Handling**
**Sintoma**: All stories are happy path - nenhuma story sobre errors  
**Problema**: Production failures inevitáveis, não planned for  
**Solução**: Error handling stories - ST-005: "Handle export timeout", ST-006: "Handle permission errors"

---

## ?? Examples

### Example: Good Epic Structure

**GOOD** ?:
```markdown
## EPIC-001: Export Automation (P0)

**Business Value**: Elimina 1.8h/dia trabalho manual, $412k/ano savings  
**User Segment**: Financial Analysts (50-500 employee companies)

**Stories**:
1. ST-001: Select data source (ERP, CRM, Custom) - 3pt
2. ST-002: Choose export format (Excel, PDF, CSV) - 2pt
3. ST-003: Preview data before export - 5pt
4. ST-004: Execute export and download - 5pt
5. ST-005: Handle export timeout errors - 3pt
6. ST-006: Handle large datasets (>100k rows) - 5pt
7. ST-007: View export history - 3pt
8. ST-008: Re-run previous export - 2pt

**Total**: 28 story points (~2-3 sprints)
```

**BAD** ?:
```markdown
## EPIC-001: Backend Development (P0)

**Stories**:
1. ST-001: Create database schema - 5pt
2. ST-002: Build REST APIs - 13pt
3. ST-003: Implement business logic - 21pt

**Total**: 39 story points
```
**Por que BAD**: Epic é technical layer (não valor), stories são horizontal (não deployable), stories >8pt (too large)

---

### Example: Good User Story with AC

**GOOD** ?:
```markdown
# ST-001: Select Data Source for Export

## User Story
As a Financial Analyst  
I want to select which data source to export from  
So that I can choose ERP, CRM, or custom data for my monthly report

## Acceptance Criteria

### AC1: Data Source Dropdown Visible
Given I am on the Export page
When the page loads
Then I see a "Data Source" dropdown
And it shows 3 options: "ERP", "CRM", "Custom Query"
And "ERP" is selected by default

### AC2: ERP Selection Loads Datasets
Given I select "ERP" from dropdown
When selection is confirmed
Then system loads available ERP datasets within 1 second
And displays them in "Dataset" secondary dropdown
And loading spinner shows while loading

### AC3: Error Handling - ERP Unavailable
Given ERP service is down
When I select "ERP"
Then error message shows: "ERP temporarily unavailable. Try again later."
And I can select CRM or Custom Query instead

## DoD
- [ ] Unit tests for dropdown logic
- [ ] Integration test for ERP connector
- [ ] Error handling tested (mock ERP down)
- [ ] Accessibility: Keyboard navigable, screen reader labels

**Points**: 3  
**PRD Ref**: FR-001 (Data source selection)
```

**BAD** ?:
```markdown
# ST-001: Data Source Feature

## User Story
User can select data source

## Acceptance Criteria
- Works correctly
- No bugs

**Points**: 8
```
**Por que BAD**: Vago ("works correctly"), não testável, não tem Given/When/Then, missing error handling, too large (8pt sem details)

---

## ?? References

- **Avanade Method Workflow Path**: `_avanade-method/bmm/workflows/3-solutioning/create-epics-and-stories/`
- **Workflow Manifest Entry**: `workflow-manifest.csv` line 14
- **Command**: `avanade-method-bmm-create-epics-and-stories`
- **Owner Agents**: João PM + Roberto SM

**Related Artifacts**:
- ${AVANADE_PRD_TEMPLATE_YAML} - PRD structure reference
- ${AVANADE_TASK_STORY_READINESS} - Story quality checklist
- ${AVANADE_PM_CHECKLIST_MD} - PM validation checklist
- ${AVANADE_TASK_VALUE_VALIDATION} - Value validation framework

**Related Workflows**:
- `create-prd` ? Creates PRD (prerequisite)
- `create-architecture` ? Creates Architecture (prerequisite)
- `check-implementation-readiness` ? Validates PRD+Arch+Stories alignment
- `sprint-planning` ? Generates sprint-status.yaml from stories
- `create-story` ? Adds new story to epic
