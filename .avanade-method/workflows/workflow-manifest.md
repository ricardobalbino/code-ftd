## ?? O que é este Artefato?

Este é o **manifest de workflows Avanade Method v6** - catálogo completo de todos workflows disponíveis no sistema com mapeamento para agentes, fases do método, e artefatos relacionados.

**Source**: `_avanade-method/_config/workflow-manifest.csv` + `_avanade-method/bmm/module-help.csv`

---

## ?? Workflows Avanade Method v6 (25 Total)

### ?? CORE WORKFLOWS (2)

#### 1. brainstorming
- **Path**: `_avanade-method/core/workflows/brainstorming/workflow.md`
- **Command**: `avanade-method-brainstorming`
- **Agent**: Maria Analyst
- **Fase**: Anytime (principalmente Phase 1)
- **Descrição**: Facilita sessões de brainstorming com técnicas criativas (SCAMPER, Six Thinking Hats, Mind Mapping)
- **Output**: Sessão de brainstorming documentada
- **Guia**: (Criar AVANADE_WORKFLOW_GUIDE_BRAINSTORMING se necessário)

#### 2. party-mode
- **Path**: `_avanade-method/core/workflows/party-mode/workflow.md`
- **Command**: `#party [tópico]`
- **Agent**: Supervisor (orquestra 2-4 agentes)
- **Fase**: Anytime
- **Descrição**: Discussão colaborativa multi-agente com cross-talk natural
- **Output**: Transcrição de discussão multi-perspectiva
- **Guia**: `${AVANADE_PARTY_MODE_GUIDE}`

---

### ?? PHASE 1: DISCOVERY & ANÁLISE (3)

#### 3. create-product-brief
- **Path**: `_avanade-method/bmm/workflows/1-analysis/create-product-brief/workflow.md`
- **Command**: `avanade-method-bmm-create-brief`
- **Agent**: Maria Analyst
- **Descrição**: Cria product brief colaborativo através de discovery passo-a-passo
- **Steps**: 6 steps (init, vision, users, metrics, scope, complete)
- **Output**: `{planning_artifacts}/product-brief.md`
- **Guia**: `${AVANADE_WORKFLOW_GUIDE_CREATE_BRIEF}` (criar se necessário)

#### 4. research (Market Research)
- **Path**: `_avanade-method/bmm/workflows/1-analysis/research/workflow.md`
- **Command**: `avanade-method-bmm-research` (research_type=market)
- **Agent**: Maria Analyst
- **Descrição**: Análise de mercado, competitive landscape, customer needs e trends
- **Steps**: 6 steps market-specific
- **Output**: `{planning_artifacts}/research-market.md`

#### 5. research (Domain Research)
- **Path**: `_avanade-method/bmm/workflows/1-analysis/research/workflow.md`
- **Command**: `avanade-method-bmm-research` (research_type=domain)
- **Agent**: Maria Analyst
- **Descrição**: Industry domain deep dive, subject matter expertise, terminologia
- **Steps**: 6 steps domain-specific
- **Output**: `{planning_artifacts}/research-domain.md`

#### 6. research (Technical Research)
- **Path**: `_avanade-method/bmm/workflows/1-analysis/research/workflow.md`
- **Command**: `avanade-method-bmm-research` (research_type=technical)
- **Agent**: Maria Analyst
- **Descrição**: Technical feasibility, architecture options, implementation approaches
- **Steps**: 6 steps technical-specific
- **Output**: `{planning_artifacts}/research-technical.md`

---

### ?? PHASE 2: PLANNING (2 workflows, 6 modes)

#### 7. create-prd (CREATE mode)
- **Path**: `_avanade-method/bmm/workflows/2-plan-workflows/create-prd/workflow.md`
- **Command**: `avanade-method-bmm-create-prd`
- **Agent**: João PM
- **Descrição**: Criação guiada de novo PRD (12 steps)
- **Steps**: 12 steps (init, discovery, success, journeys, domain, innovation, project-type, scoping, functional, nonfunctional, polish, complete)
- **Output**: `{planning_artifacts}/prd-{project_name}.md`
- **Guia**: `${AVANADE_WORKFLOW_GUIDE_CREATE_PRD}`

#### 8. create-prd (VALIDATE mode)
- **Command**: `avanade-method-bmm-validate-prd`
- **Agent**: João PM
- **Descrição**: Validação adversarial de PRD existente (13 checks)
- **Steps**: 13 validation dimensions
- **Output**: `{planning_artifacts}/prd-validation-report.md`

#### 9. create-prd (EDIT mode)
- **Command**: `avanade-method-bmm-edit-prd`
- **Agent**: João PM
- **Descrição**: Edição inteligente de PRD com detecção de formato
- **Steps**: 5 steps (discovery, review, edit, complete)
- **Output**: Updated PRD

#### 10. create-ux-design (CREATE mode)
- **Path**: `_avanade-method/bmm/workflows/2-plan-workflows/create-ux-design/workflow.md`
- **Command**: `avanade-method-bmm-create-ux-design`
- **Agent**: Sofia UX
- **Descrição**: Planning de UX patterns, look and feel, user journeys
- **Steps**: 14 steps (init, discovery, core-experience, emotional-response, inspiration, design-system, defining-experience, visual-foundation, design-directions, user-journeys, component-strategy, ux-patterns, responsive-accessibility, complete)
- **Output**: `{planning_artifacts}/ux-design.md`
- **Guia**: `${AVANADE_WORKFLOW_GUIDE_CREATE_UX}` (criar se necessário)

#### 11. create-ux-design (VALIDATE mode)
- **Command**: `avanade-method-bmm-validate-ux-design`
- **Agent**: Sofia UX
- **Descrição**: Valida UX design deliverables
- **Output**: `{planning_artifacts}/ux-validation-report.md`

---

### ??? PHASE 3: SOLUTIONING (6 workflows)

#### 12. create-architecture (CREATE mode)
- **Path**: `_avanade-method/bmm/workflows/3-solutioning/create-architecture/workflow.md`
- **Command**: `avanade-method-bmm-create-architecture`
- **Agent**: Wilson Architect
- **Descrição**: Facilitação colaborativa de decisões arquiteturais (8 steps)
- **Steps**: 8 steps (init, context, starter, decisions, patterns, structure, validation, complete)
- **Output**: `{planning_artifacts}/architecture.md`
- **Guia**: `${AVANADE_WORKFLOW_GUIDE_CREATE_ARCHITECTURE}`

#### 13. create-architecture (VALIDATE mode)
- **Command**: `avanade-method-bmm-validate-architecture`
- **Agent**: Wilson Architect
- **Descrição**: Valida architecture completeness
- **Output**: `{planning_artifacts}/architecture-validation-report.md`

#### 14. create-epics-and-stories
- **Path**: `_avanade-method/bmm/workflows/3-solutioning/create-epics-and-stories/workflow.md`
- **Command**: `avanade-method-bmm-create-epics-and-stories`
- **Agent**: João PM + Roberto SM
- **Descrição**: Transforma PRD + Architecture em epics e stories implementation-ready
- **Prerequisites**: PRD + Architecture (UX recomendado se UI)
- **Steps**: 4 steps (validate-prerequisites, design-epics, create-stories, final-validation)
- **Output**: `{planning_artifacts}/epics.md` + `{planning_artifacts}/stories/`
- **Guia**: `${AVANADE_WORKFLOW_GUIDE_CREATE_EPICS_STORIES}` (criar se necessário)

#### 15. check-implementation-readiness
- **Path**: `_avanade-method/bmm/workflows/3-solutioning/check-implementation-readiness/workflow.md`
- **Command**: `avanade-method-bmm-check-readiness`
- **Agent**: Maria Analyst (adversarial review)
- **Descrição**: Valida PRD + Architecture + Epics/Stories para completeness e alignment ANTES de implementação
- **Steps**: 6 steps (document-discovery, prd-analysis, epic-coverage-validation, ux-alignment, epic-quality-review, final-assessment)
- **Output**: `{planning_artifacts}/readiness-report.md`
- **Guia**: `${AVANADE_WORKFLOW_GUIDE_CHECK_READINESS}` (criar se necessário)

---

### ?? PHASE 4: IMPLEMENTATION (8 workflows)

#### 16. sprint-planning
- **Path**: `_avanade-method/bmm/workflows/4-implementation/sprint-planning/workflow.yaml`
- **Command**: `avanade-method-bmm-sprint-planning`
- **Agent**: Roberto SM
- **Descrição**: Gera e gerencia sprint-status.yaml tracking file
- **Output**: `{planning_artifacts}/sprint-status.yaml`
- **Guia**: `${AVANADE_WORKFLOW_GUIDE_SPRINT_PLANNING}`

#### 17. sprint-status
- **Path**: `_avanade-method/bmm/workflows/4-implementation/sprint-status/workflow.yaml`
- **Command**: `avanade-method-bmm-sprint-status`
- **Agent**: Roberto SM
- **Descrição**: Summariza sprint-status.yaml, surface risks, route to next workflow
- **Output**: Console summary + routing recommendation

#### 18. create-story
- **Path**: `_avanade-method/bmm/workflows/4-implementation/create-story/workflow.yaml`
- **Command**: `avanade-method-bmm-create-story`
- **Agent**: Roberto SM
- **Descrição**: Cria próxima user story do epic com enhanced context analysis
- **Output**: `{planning_artifacts}/stories/ST-XXX.md`

#### 19. dev-story
- **Path**: `_avanade-method/bmm/workflows/4-implementation/dev-story/workflow.yaml`
- **Command**: `avanade-method-bmm-dev-story`
- **Agent**: Tiago Dev
- **Descrição**: Executa story implementando tasks, escrevendo testes, validando contra acceptance criteria
- **Output**: Code + tests + updated story file (status=completed)
- **Guia**: `${AVANADE_WORKFLOW_GUIDE_DEV_STORY}` (criar se necessário)

#### 20. code-review
- **Path**: `_avanade-method/bmm/workflows/4-implementation/code-review/workflow.yaml`
- **Command**: `avanade-method-bmm-code-review`
- **Agent**: Carla QA (adversarial review)
- **Descrição**: Adversarial Senior Developer code review (finds 3-10 issues SEMPRE)
- **Output**: `{planning_artifacts}/code-review-ST-XXX.md` + auto-fix option
- **Guia**: `${AVANADE_WORKFLOW_GUIDE_CODE_REVIEW}` (criar se necessário)

#### 21. retrospective
- **Path**: `_avanade-method/bmm/workflows/4-implementation/retrospective/workflow.yaml`
- **Command**: `avanade-method-bmm-retrospective`
- **Agent**: Roberto SM
- **Descrição**: Executa após epic completion para review de success, learnings, next epic insights
- **Output**: `{planning_artifacts}/retrospective-EP-XXX.md`

#### 22. correct-course
- **Path**: `_avanade-method/bmm/workflows/4-implementation/correct-course/workflow.yaml`
- **Command**: `avanade-method-bmm-correct-course`
- **Agent**: Roberto SM
- **Descrição**: Navega mudanças significativas durante sprint (pode recomendar update PRD, redo architecture, etc)
- **Output**: `{planning_artifacts}/change-proposal.md`

---

### ? QUICK-FLOW WORKFLOWS (2)

#### 23. quick-spec
- **Path**: `_avanade-method/bmm/workflows/avanade-method-quick-flow/quick-spec/workflow.md`
- **Command**: `avanade-method-bmm-quick-spec`
- **Agent**: Barry Quick-Flow Solo Dev
- **Descrição**: Spec engineering conversacional - ask questions, investigate code, produce tech-spec
- **Output**: `{planning_artifacts}/tech-spec-{task}.md`
- **Guia**: `${AVANADE_WORKFLOW_GUIDE_QUICK_SPEC}` (criar se necessário)

#### 24. quick-dev
- **Path**: `_avanade-method/bmm/workflows/avanade-method-quick-flow/quick-dev/workflow.md`
- **Command**: `avanade-method-bmm-quick-dev`
- **Agent**: Barry Quick-Flow Solo Dev
- **Descrição**: Desenvolvimento rápido SEM planejamento extensivo - execute tech-specs OU direct instructions
- **Output**: Code + tests (inline documentation)
- **Guia**: `${AVANADE_WORKFLOW_GUIDE_QUICK_DEV}`

---

### ?? DOCUMENTATION WORKFLOWS (2)

#### 25. document-project
- **Path**: `_avanade-method/bmm/workflows/document-project/workflow.yaml`
- **Command**: `avanade-method-bmm-document-project`
- **Agent**: Maria Analyst + Paige Tech Writer
- **Descrição**: Analisa e documenta brownfield projects (scanning codebase, architecture, patterns)
- **Output**: `{project_knowledge}/` (comprehensive reference docs)
- **Guia**: `${AVANADE_WORKFLOW_GUIDE_DOCUMENT_PROJECT}` (criar se necessário)

#### 26. generate-project-context
- **Path**: `_avanade-method/bmm/workflows/generate-project-context/workflow.md`
- **Command**: `avanade-method-bmm-generate-project-context`
- **Agent**: Maria Analyst
- **Descrição**: Scan brownfield para gerar project-context.md LLM-optimized (critical rules, patterns, conventions)
- **Steps**: 3 steps (discover, generate, complete)
- **Output**: `{output_folder}/project-context.md`

---

### ?? EXCALIDRAW DIAGRAM WORKFLOWS (4)

#### 27. create-excalidraw-dataflow
- **Path**: `_avanade-method/bmm/workflows/excalidraw-diagrams/create-dataflow/workflow.yaml`
- **Command**: `avanade-method-bmm-create-excalidraw-dataflow`
- **Agent**: Sofia UX
- **Descrição**: Cria data flow diagrams (DFD) em Excalidraw format
- **Output**: `{planning_artifacts}/{name}-dataflow.excalidraw`

#### 28. create-excalidraw-diagram
- **Path**: `_avanade-method/bmm/workflows/excalidraw-diagrams/create-diagram/workflow.yaml`
- **Command**: `avanade-method-bmm-create-excalidraw-diagram`
- **Agent**: Sofia UX
- **Descrição**: Cria system architecture, ERD, UML diagrams em Excalidraw
- **Output**: `{planning_artifacts}/{name}-diagram.excalidraw`

#### 29. create-excalidraw-flowchart
- **Path**: `_avanade-method/bmm/workflows/excalidraw-diagrams/create-flowchart/workflow.yaml`
- **Command**: `avanade-method-bmm-create-excalidraw-flowchart`
- **Agent**: Sofia UX
- **Descrição**: Cria flowchart visualization (processes, pipelines, logic flows)
- **Output**: `{planning_artifacts}/{name}-flowchart.excalidraw`

#### 30. create-excalidraw-wireframe
- **Path**: `_avanade-method/bmm/workflows/excalidraw-diagrams/create-wireframe/workflow.yaml`
- **Command**: `avanade-method-bmm-create-excalidraw-wireframe`
- **Agent**: Sofia UX
- **Descrição**: Cria website/app wireframes em Excalidraw
- **Output**: `{planning_artifacts}/{name}-wireframe.excalidraw`

---

## ??? WORKFLOW JOURNEY MAP

### Greenfield Project (New Feature/Product):

```
1. create-product-brief (opcional mas recomendado)
   ?
2. research (market/domain/technical se necessário)
   ?
3. create-prd (CREATE mode)
   ?
4. create-prd (VALIDATE mode)
   ?
5. create-ux-design (se UI existe)
   ?
6. create-architecture
   ?
7. create-epics-and-stories
   ?
8. check-implementation-readiness (recomendado)
   ?
9. sprint-planning
   ?
10. LOOP {
      create-story (se necessário)
      ? dev-story
      ? code-review
      ? retrospective (após epic)
    }
   ?
11. Repeat loop até todos epics completos
```

### Brownfield Project (Existing Codebase):

```
1. document-project OU generate-project-context
   ?
2. Se mudança pequena:
   quick-spec ? quick-dev
   ?
3. Se mudança grande:
   Seguir greenfield flow (PRD ? Arch ? Stories)
```

### Quick Win / Bug Fix:

```
1. quick-spec (opcional)
   ?
2. quick-dev
```

---

## ?? Agent-to-Workflow Mapping

### Maria Analyst
- brainstorming, create-product-brief, research (all types), check-implementation-readiness, document-project, generate-project-context

### João PM (Product Manager)
- create-prd (all modes), create-epics-and-stories

### Wilson Architect
- create-architecture (all modes)

### Sofia UX
- create-ux-design (all modes), create-excalidraw-* (all diagram types)

### Roberto SM (Scrum Master)
- create-epics-and-stories, sprint-planning, sprint-status, create-story, retrospective, correct-course

### Tiago Dev (Developer)
- dev-story

### Carla QA
- code-review

### Paige Tech Writer
- document-project (co-author), write-document, validate-document

### Barry Quick-Flow Solo Dev
- quick-spec, quick-dev

### Supervisor
- party-mode (orquestra multi-agente), Agent Terraform (deploy environment)

---

## ?? Related Artifacts

- **${AVANADE_WORKFLOW_GUIDE_CREATE_PRD}**: PRD workflow guide
- **${AVANADE_WORKFLOW_GUIDE_CREATE_ARCHITECTURE}**: Architecture workflow guide
- **${AVANADE_WORKFLOW_GUIDE_SPRINT_PLANNING}**: Sprint planning workflow guide
- **${AVANADE_WORKFLOW_GUIDE_QUICK_DEV}**: Quick dev workflow guide
- **${AVANADE_PARTY_MODE_GUIDE}**: Party mode collaboration guide
- (Criar GUIDEs adicionais conforme necessário para workflows faltantes)

---

