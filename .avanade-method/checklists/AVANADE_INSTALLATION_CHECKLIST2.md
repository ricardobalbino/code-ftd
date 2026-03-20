# ✅ Avanade Method v2 - Installation Checklist

**Versão**: 2.1  
**Data**: 2026-02-06  
**Status**: READY FOR DEPLOYMENT  
**Novidades**: Sistema de Herança (_base/ + agents/)

---

## 🎯 Objetivo

Este checklist garante que todos os arquivos do **Avanade Method v2** foram instalados corretamente na estrutura adequada. Use esta lista para validar instalações em novos ambientes.

---

## 📁 ESTRUTURA COMPLETA DE DIRETÓRIOS

```
📁 [WORKSPACE ROOT]/
│
├── 📁 .github/                             # GitHub Copilot Configuration (RAIZ)
│   │
│   ├── 📁 agents/                          # Agentes Copilot - VS Code Copilot (NOVO padrão)
│   │   ├── avanade-supervisor.chatmode.md  (generated from AVANADE_SUPERVISOR_PROMPT_MD)
│   │   ├── joao-pm.chatmode.md             (generated from AVANADE_PM_JOAO_PROMPT_MD)
│   │   ├── wilson-architect.chatmode.md    (generated from AVANADE_ARCHITECT_WILSON_PROMPT_MD)
│   │   ├── maria-analyst.chatmode.md       (generated from AVANADE_ANALYST_MARIA_PROMPT_MD)
│   │   ├── roberto-sm.chatmode.md          (generated from AVANADE_SM_PROMPT_MD)
│   │   ├── carla-qa.chatmode.md            (generated from AVANADE_QA_CARLA_PROMPT_MD)
│   │   ├── tiago-dev.chatmode.md           (generated from AVANADE_DEVELOPER_TIAGO_PROMPT_MD)
│   │   ├── paula-po.chatmode.md            (generated from AVANADE_PO_PROMPT_MD)
│   │   ├── sofia-ux.chatmode.md            (generated from AVANADE_UX_EXPERT_PROMPT_MD)
│   │   └── paige-tech-writer.chatmode.md   (generated from AVANADE_TECH_WRITER_PAIGE_PROMPT_MD)
│   │
│   ├── 📁 prompts/                         # Prompt Files (Templates) - VS Code Copilot
│   │   ├── prd-creation.prompt.md          (generated from AVANADE_PRD_TEMPLATE_YAML)
│   │   ├── adr-template.prompt.md          (generated from AVANADE_ADR_TEMPLATE)
│   │   ├── test-plan.prompt.md             (generated from AVANADE_TEST_PLAN_TEMPLATE)
│   │   └── wireframe.prompt.md             (generated from AVANADE_WIREFRAME_TEMPLATE)
│   │
│   └── copilot-instructions.md             # Global Copilot Instructions
│
├── 📁 .avanade-method/                     # Avanade Method Resources
│   │
│   ├── 📁 _base/                           # Base Templates & Inheritance System (NEW)
│   │   ├── avanade-master.md               (generated from AVANADE_BASE_MASTER_AGENT_MD)
│   │   ├── config.yaml                     (generated from AVANADE_BASE_CONFIG_YAML)
│   │   └── INHERITANCE-ARCHITECTURE.md     (generated from AVANADE_BASE_INHERITANCE_ARCHITECTURE_MD)
│   │
│   ├── 📁 agents/                          # Agent Customization Files (NEW)
│   │   ├── supervisor.customize.yaml       (generated from AVANADE_AGENT_SUPERVISOR_CUSTOMIZE_YAML)
│   │   ├── joao-pm.customize.yaml          (generated from AVANADE_AGENT_JOAO_PM_CUSTOMIZE_YAML)
│   │   ├── wilson-architect.customize.yaml (generated from AVANADE_AGENT_WILSON_ARCHITECT_CUSTOMIZE_YAML)
│   │   ├── maria-analyst.customize.yaml    (generated from AVANADE_AGENT_MARIA_ANALYST_CUSTOMIZE_YAML)
│   │   ├── roberto-sm.customize.yaml       (generated from AVANADE_AGENT_ROBERTO_SM_CUSTOMIZE_YAML)
│   │   ├── carla-qa.customize.yaml         (generated from AVANADE_AGENT_CARLA_QA_CUSTOMIZE_YAML)
│   │   ├── tiago-dev.customize.yaml        (generated from AVANADE_AGENT_TIAGO_DEV_CUSTOMIZE_YAML)
│   │   ├── paula-po.customize.yaml         (generated from AVANADE_AGENT_PAULA_PO_CUSTOMIZE_YAML)
│   │   ├── sofia-ux.customize.yaml         (generated from AVANADE_AGENT_SOFIA_UX_CUSTOMIZE_YAML)
│   │   └── paige-tech-writer.customize.yaml (generated from AVANADE_AGENT_PAIGE_TECH_WRITER_CUSTOMIZE_YAML)
│   │
│   ├── 📁 workflows/                       # Workflow Guides
│   │   ├── create-brief.workflow.md        (generated from AVANADE_WORKFLOW_GUIDE_CREATE_BRIEF)
│   │   ├── create-prd.workflow.md          (generated from AVANADE_WORKFLOW_GUIDE_CREATE_PRD)
│   │   ├── create-architecture.workflow.md (generated from AVANADE_WORKFLOW_GUIDE_CREATE_ARCHITECTURE)
│   │   ├── create-ux.workflow.md           (generated from AVANADE_WORKFLOW_GUIDE_CREATE_UX)
│   │   ├── create-epics-stories.workflow.md(generated from AVANADE_WORKFLOW_GUIDE_CREATE_EPICS_STORIES)
│   │   ├── sprint-planning.workflow.md     (generated from AVANADE_WORKFLOW_GUIDE_SPRINT_PLANNING)
│   │   ├── code-review.workflow.md         (generated from AVANADE_WORKFLOW_GUIDE_CODE_REVIEW)
│   │   ├── quick-dev.workflow.md           (generated from AVANADE_WORKFLOW_GUIDE_QUICK_DEV)
│   │   └── workflow-manifest.md            (generated from AVANADE_WORKFLOW_MANIFEST)
│   │
│   ├── 📁 tasks/                           # Task Definitions
│   │   ├── advanced-elicitation.task.md    (generated from AVANADE_TASK_ADVANCED_ELICITATION)
│   │   ├── architecture-quality.task.md    (generated from AVANADE_TASK_ARCHITECTURE_QUALITY)
│   │   ├── story-readiness.task.md         (generated from AVANADE_TASK_STORY_READINESS)
│   │   ├── retrospective-facilitation.task.md (generated from AVANADE_TASK_RETROSPECTIVE_FACILITATION)
│   │   ├── invest-validation.task.md       (generated from AVANADE_TASK_INVEST_VALIDATION)
│   │   ├── clean-code.task.md              (generated from AVANADE_TASK_CLEAN_CODE)
│   │   ├── debugging-guide.task.md         (generated from AVANADE_TASK_DEBUGGING_GUIDE)
│   │   ├── code-review.task.md             (generated from AVANADE_TASK_CODE_REVIEW)
│   │   ├── test-coverage.task.md           (generated from AVANADE_TASK_TEST_COVERAGE)
│   │   ├── boundary-value-analysis.task.md (generated from AVANADE_TASK_BOUNDARY_VALUE_ANALYSIS)
│   │   ├── adversarial-review.task.md      (generated from AVANADE_TASK_ADVERSARIAL_REVIEW)
│   │   ├── ux-checklist.task.md            (generated from AVANADE_TASK_UX_CHECKLIST)
│   │   ├── usability-heuristics.task.md    (generated from AVANADE_TASK_USABILITY_HEURISTICS)
│   │   ├── accessibility-wcag.task.md      (generated from AVANADE_TASK_ACCESSIBILITY_WCAG)
│   │   ├── value-validation.task.md        (generated from AVANADE_TASK_VALUE_VALIDATION)
│   │   ├── rice-prioritization.task.md     (generated from AVANADE_TASK_RICE_PRIORITIZATION)
│   │   ├── methodology-compliance.task.md  (generated from AVANADE_TASK_METHODOLOGY_COMPLIANCE)
│   │   ├── doc-accessibility.task.md       (generated from AVANADE_TASK_DOC_ACCESSIBILITY)
│   │   ├── technical-accuracy.task.md      (generated from AVANADE_TASK_TECHNICAL_ACCURACY)
│   │   ├── editorial-review-prose.task.md  (generated from AVANADE_TASK_EDITORIAL_REVIEW_PROSE)
│   │   ├── editorial-review-structure.task.md (generated from AVANADE_TASK_EDITORIAL_REVIEW_STRUCTURE)
│   │   └── create-next-story.task.md       (generated from AVANADE_CREATE_NEXT_STORY_TASK_MD)
│   │
│   ├── 📁 checklists/                      # Checklists
│   │   ├── pm-checklist.md                 (generated from AVANADE_PM_CHECKLIST_MD)
│   │   ├── architect-checklist.md          (generated from AVANADE_ARCHITECT_CHECKLIST_MD)
│   │   ├── po-master-checklist.md          (generated from AVANADE_PO_MASTER_CHECKLIST_MD)
│   │   ├── story-dod-checklist.md          (generated from AVANADE_STORY_DOD_CHECKLIST_MD)
│   │   └── installation-checklist.md       (generated from AVANADE_INSTALLATION_CHECKLIST)
│   │
│   ├── 📁 templates/                       # Project Templates
│   │   ├── prd-template.yaml               (generated from AVANADE_PRD_TEMPLATE_YAML)
│   │   ├── architecture-template.md        (generated from AVANADE_ARCHITECTURE_TEMPLATE)
│   │   ├── adr-template.md                 (generated from AVANADE_ADR_TEMPLATE)
│   │   ├── test-plan-template.md           (generated from AVANADE_TEST_PLAN_TEMPLATE)
│   │   ├── wireframe-template.md           (generated from AVANADE_WIREFRAME_TEMPLATE)
│   │   ├── backlog-template.yaml           (generated from AVANADE_BACKLOG_TEMPLATE_YAML)
│   │   ├── story-template.yaml             (generated from AVANADE_STORY_TEMPLATE_YAML)
│   │   ├── discovery-template.yaml         (generated from AVANADE_DISCOVERY_TEMPLATE_YAML)
│   │   └── frontend-spec-template.yaml     (generated from AVANADE_FRONT_END_SPEC_TEMPLATE_YAML)
│   │
│   ├── 📁 memory/                          # Agent Memory System
│   │   ├── supervisor-memory.md            (generated from AVANADE_MEMORY_SUPERVISOR)
│   │   ├── maria-analyst-memory.md         (generated from AVANADE_MEMORY_ANALYST_MARIA)
│   │   ├── wilson-architect-memory.md      (generated from AVANADE_MEMORY_ARCHITECT_WILSON)
│   │   ├── joao-pm-memory.md               (generated from AVANADE_MEMORY_PM_JOAO)
│   │   ├── paula-po-memory.md              (generated from AVANADE_MEMORY_PO_PAULA)
│   │   ├── roberto-sm-memory.md            (generated from AVANADE_MEMORY_SM_ROBERTO)
│   │   ├── carla-qa-memory.md              (generated from AVANADE_MEMORY_QA_CARLA)
│   │   ├── tiago-dev-memory.md             (generated from AVANADE_MEMORY_DEV_TIAGO)
│   │   ├── sofia-ux-memory.md              (generated from AVANADE_MEMORY_UX_SOFIA)
│   │   └── paige-tech-writer-memory.md     (generated from AVANADE_MEMORY_TECH_WRITER_PAIGE)
│   │
│   ├── 📁 standards/                       # Documentation Standards
│   │   ├── doc-standards.md                (generated from AVANADE_DOC_STANDARDS_MD)
│   │   ├── commonmark-template.md          (generated from AVANADE_COMMONMARK_TEMPLATE_MD)
│   │   ├── explanation-template.md         (generated from AVANADE_EXPLANATION_TEMPLATE_MD)
│   │   ├── fluent-design-guidelines.md     (generated from AVANADE_FLUENT_DESIGN_GUIDELINES)
│   │   └── mermaid-library.md              (generated from AVANADE_MERMAID_LIBRARY_MD)
│   │
│   ├── 📁 guides/                          # Special Guides
│   │   └── party-mode-guide.md             (generated from AVANADE_PARTY_MODE_GUIDE)
│   │
│   
│   
│
└── 📁 .vscode/
    └── settings.json
```

---

## 📊 RESUMO DA INSTALAÇÃO

| Categoria | Quantidade | Status |
|-----------|------------|--------|
| **Agentes Copilot (.github/agents/)** | 10 | ⬜ |
| **Prompt Files** | 4 | ⬜ |
| **Base & Inheritance (_base/)** | 3 | ⬜ |
| **Agent Customization (agents/)** | 10 | ⬜ |
| **Workflows** | 9 | ⬜ |
| **Tasks** | 22 | ⬜ |
| **Checklists** | 5 | ⬜ |
| **Templates** | 9 | ⬜ |
| **Memory System** | 10 | ⬜ |
| **Documentation** | 6 | ⬜ |
| **Standards** | 5 | ⬜ |
| **Root Files** | 2 | ⬜ |
| **Configurações** | 3 | ⬜ |
| **Agent Updates** | 10 | ⬜ |
| **MCP Config** | 1 | ⬜ |
| **TOTAL** | **109 arquivos** | ⬜ |

---

## ✅ PARTE 1: CONFIGURAÇÃO BASE (47 arquivos)

### 🔧 1.1 Configuração MCP

**Arquivo**: `%APPDATA%/Code/User/mcp.json`

```
⬜ mcp.json configurado com:
   ⬜ utility-agent-id (supervisor)
   ⬜ x-cockpit-team-member-ids (8 agent IDs)
   ⬜ URL MCP Server: https://mcp-agent.br.avanade.ai/mcp
```

**IDs dos Agentes Esperados**:
```
⬜ Supervisor: a1b3c1a8-3c6b-4cd4-8aa9-ed2c51903e8e
⬜ Maria (Analyst): [confirmar ID]
⬜ João (PM): [confirmar ID]
⬜ Wilson (Architect): [confirmar ID]
⬜ Paula (PO): [confirmar ID]
⬜ Roberto (SM): [confirmar ID]
⬜ Carla (QA): [confirmar ID]
⬜ Tiago (Dev): [confirmar ID]
⬜ Sofia (UX): [confirmar ID]
⬜ Paige (Tech Writer): [confirmar ID]
```

---

### 🧠 1.2 Agentes Copilot - `.github/agents/` (10 arquivos)

| # | Arquivo | Agente | Verificado |
|---|---------|--------|------------|
| 1 | `avanade-supervisor.chatmode.md` | Supervisor - Metacognitive Master | ⬜ |
| 2 | `joao-pm.chatmode.md` | João - Product Manager | ⬜ |
| 3 | `wilson-architect.chatmode.md` | Wilson - Architect | ⬜ |
| 4 | `maria-analyst.chatmode.md` | Maria - Business Analyst | ⬜ |
| 5 | `roberto-sm.chatmode.md` | Roberto - Scrum Master | ⬜ |
| 6 | `carla-qa.chatmode.md` | Carla - QA Engineer | ⬜ |
| 7 | `tiago-dev.chatmode.md` | Tiago - Developer | ⬜ |
| 8 | `paula-po.chatmode.md` | Paula - Product Owner | ⬜ |
| 9 | `sofia-ux.chatmode.md` | Sofia - UX Designer | ⬜ |
| 10 | `paige-tech-writer.chatmode.md` | Paige - Tech Writer | ⬜ |

**Validação**:
```powershell
# Comando para verificar (PowerShell)
Get-ChildItem ".github/agents/*.chatmode.md" | Measure-Object
# Esperado: Count = 10
```

---

### 📝 1.3 Prompt Files - `.github/prompts/` (4 arquivos)

| # | Arquivo | Propósito | Verificado |
|---|---------|-----------|------------|
| 1 | `prd-creation.prompt.md` | Product Requirements Document | ⬜ |
| 2 | `adr-template.prompt.md` | Architecture Decision Records | ⬜ |
| 3 | `test-plan.prompt.md` | Test Plans | ⬜ |
| 4 | `wireframe.prompt.md` | Wireframe Structure | ⬜ |

**Validação**:
```powershell
Get-ChildItem ".github/prompts/*.prompt.md" | Measure-Object
# Esperado: Count = 4
```

---

### 🔄 1.4 Workflows YAML - `.avanade-method/workflows/` (9 arquivos)

| # | Arquivo | Nome do Workflow | Tipo | Verificado |
|---|---------|------------------|------|------------|
| 1 | `WF-001-full-inception.yaml` | Full Project Inception | Generic | ⬜ |
| 2 | `WF-002-feature-development.yaml` | Feature Development | Generic | ⬜ |
| 3 | `WF-003-enterprise-delivery.yaml` | Enterprise Delivery | Enterprise | ⬜ |
| 4 | `WF-004-brownfield-fullstack.yaml` | Brownfield Full-Stack | Brownfield | ⬜ |
| 5 | `WF-005-brownfield-service.yaml` | Brownfield Service | Brownfield | ⬜ |
| 6 | `WF-006-brownfield-ui.yaml` | Brownfield UI | Brownfield | ⬜ |
| 7 | `WF-007-greenfield-fullstack.yaml` | Greenfield Full-Stack | Greenfield | ⬜ |
| 8 | `WF-008-greenfield-service.yaml` | Greenfield Service | Greenfield | ⬜ |
| 9 | `WF-009-greenfield-ui.yaml` | Greenfield UI | Greenfield | ⬜ |

**Cobertura de Cenários**:
- ⬜ Brownfield: 3 workflows (Full-Stack, Service, UI)
- ⬜ Greenfield: 3 workflows (Full-Stack, Service, UI)
- ⬜ Enterprise: 1 workflow
- ⬜ Generic: 2 workflows (Inception, Feature)

---

### 👥 1.5 Persona Configs - `.avanade-method/personas/` (8 arquivos)

| # | Arquivo | Persona | Role | Verificado |
|---|---------|---------|------|------------|
| 1 | `paula-po.yaml` | Paula | Product Owner | ⬜ |
| 2 | `wilson-architect.yaml` | Wilson | Architect | ⬜ |
| 3 | `joao-pm.yaml` | João | Product Manager | ⬜ |
| 4 | `roberto-sm.yaml` | Roberto | Scrum Master | ⬜ |
| 5 | `tiago-dev.yaml` | Tiago | Developer | ⬜ |
| 6 | `maria-analyst.yaml` | Maria | Business Analyst | ⬜ |
| 7 | `sofia-ux.yaml` | Sofia | UX Designer | ⬜ |
| 8 | `carla-qa.yaml` | Carla | QA Engineer | ⬜ |

---

### ⚙️ 1.6 Configurações Centrais (3 arquivos)

| # | Arquivo | Propósito | Verificado |
|---|---------|-----------|------------|
| 1 | `.github/copilot-instructions.md` | GitHub Copilot Global | ⬜ |
| 2 | `.vscode/settings.json` | VSCode Settings | ⬜ |
| 3 | `MCP.json` | MCP Server Config | ⬜ |

---

### 🧬 1.7 Base & Inheritance System - `.avanade-method/_base/` (3 arquivos) **[NEW]**

| # | Arquivo | Artifact Key | Propósito | Verificado |
|---|---------|--------------|-----------|------------|
| 1 | `avanade-master.md` | `AVANADE_BASE_MASTER_AGENT_MD` | Template base para todos agentes | ⬜ |
| 2 | `config.yaml` | `AVANADE_BASE_CONFIG_YAML` | Configuração do sistema de herança | ⬜ |
| 3 | `INHERITANCE-ARCHITECTURE.md` | `AVANADE_BASE_INHERITANCE_ARCHITECTURE_MD` | Documentação da arquitetura | ⬜ |

**Validação**:
```powershell
Get-ChildItem ".avanade-method/_base/*" | Measure-Object
# Esperado: Count = 3
```

---

### 🎭 1.8 Agent Customization - `.avanade-method/agents/` (10 arquivos) **[NEW]**

| # | Arquivo | Artifact Key | Agente | Verificado |
|---|---------|--------------|--------|------------|
| 1 | `supervisor.customize.yaml` | `AVANADE_AGENT_SUPERVISOR_CUSTOMIZE_YAML` | Supervisor | ⬜ |
| 2 | `joao-pm.customize.yaml` | `AVANADE_AGENT_JOAO_PM_CUSTOMIZE_YAML` | João PM | ⬜ |
| 3 | `wilson-architect.customize.yaml` | `AVANADE_AGENT_WILSON_ARCHITECT_CUSTOMIZE_YAML` | Wilson Architect | ⬜ |
| 4 | `maria-analyst.customize.yaml` | `AVANADE_AGENT_MARIA_ANALYST_CUSTOMIZE_YAML` | Maria Analyst | ⬜ |
| 5 | `roberto-sm.customize.yaml` | `AVANADE_AGENT_ROBERTO_SM_CUSTOMIZE_YAML` | Roberto SM | ⬜ |
| 6 | `carla-qa.customize.yaml` | `AVANADE_AGENT_CARLA_QA_CUSTOMIZE_YAML` | Carla QA | ⬜ |
| 7 | `tiago-dev.customize.yaml` | `AVANADE_AGENT_TIAGO_DEV_CUSTOMIZE_YAML` | Tiago Dev | ⬜ |
| 8 | `paula-po.customize.yaml` | `AVANADE_AGENT_PAULA_PO_CUSTOMIZE_YAML` | Paula PO | ⬜ |
| 9 | `sofia-ux.customize.yaml` | `AVANADE_AGENT_SOFIA_UX_CUSTOMIZE_YAML` | Sofia UX | ⬜ |
| 10 | `paige-tech-writer.customize.yaml` | `AVANADE_AGENT_PAIGE_TECH_WRITER_CUSTOMIZE_YAML` | Paige Tech Writer | ⬜ |

**Validação**:
```powershell
Get-ChildItem ".avanade-method/agents/*.customize.yaml" | Measure-Object
# Esperado: Count = 10
```

**Arquitetura de Herança**:
```
┌─────────────────────────────────────┐
│     avanade-master.md (Base)        │
│   Persona, Menu, Rules, Greeting    │
└──────────────┬──────────────────────┘
               │ inherits
               ▼
┌─────────────────────────────────────┐
│   {agent}.customize.yaml            │
│   Overrides específicos do agente   │
└──────────────┬──────────────────────┘
               │ generates
               ▼
┌─────────────────────────────────────┐
│   {agent}.chatmode.md               │
│   Arquivo final para VS Code        │
└─────────────────────────────────────┘
```

---

## ✅ PARTE 2: ARTEFATOS AVANADE METHOD V2 (61 arquivos)

### 📚 2.1 Workflow Guides (9 arquivos)

**Localização**: `_avanade-output/implementation-artifacts/artifacts/`

| # | Artefato | Workflow | Verificado |
|---|----------|----------|------------|
| 1 | `AVANADE_WORKFLOW_GUIDE_CREATE_PRD.md` | create-prd (tri-modal) | ⬜ |
| 2 | `AVANADE_WORKFLOW_GUIDE_CREATE_ARCHITECTURE.md` | create-architecture | ⬜ |
| 3 | `AVANADE_WORKFLOW_GUIDE_SPRINT_PLANNING.md` | sprint-planning | ⬜ |
| 4 | `AVANADE_WORKFLOW_GUIDE_QUICK_DEV.md` | quick-dev | ⬜ |
| 5 | `AVANADE_WORKFLOW_GUIDE_CREATE_BRIEF.md` | create-product-brief | ⬜ |
| 6 | `AVANADE_WORKFLOW_GUIDE_CREATE_UX.md` | create-ux-design | ⬜ |
| 7 | `AVANADE_WORKFLOW_GUIDE_CREATE_EPICS_STORIES.md` | create-epics-and-stories | ⬜ |
| 8 | `AVANADE_WORKFLOW_GUIDE_CODE_REVIEW.md` | code-review | ⬜ |
| 9 | `AVANADE_WORKFLOW_MANIFEST.md` | All 25 workflows catalog | ⬜ |

---

### ✅ 2.2 Task Artifacts (20 arquivos)

| # | Artefato | Agente Owner | Verificado |
|---|----------|--------------|------------|
| 1 | `AVANADE_TASK_ADVANCED_ELICITATION.md` | Maria (Analyst) | ⬜ |
| 2 | `AVANADE_TASK_ARCHITECTURE_QUALITY.md` | Wilson (Architect) | ⬜ |
| 3 | `AVANADE_TASK_STORY_READINESS.md` | Roberto (SM) | ⬜ |
| 4 | `AVANADE_TASK_RETROSPECTIVE_FACILITATION.md` | Roberto (SM) | ⬜ |
| 5 | `AVANADE_TASK_INVEST_VALIDATION.md` | Roberto (SM) | ⬜ |
| 6 | `AVANADE_TASK_CLEAN_CODE.md` | Tiago (Dev) | ⬜ |
| 7 | `AVANADE_TASK_DEBUGGING_GUIDE.md` | Tiago (Dev) | ⬜ |
| 8 | `AVANADE_TASK_CODE_REVIEW.md` | Tiago (Dev) | ⬜ |
| 9 | `AVANADE_TASK_TEST_COVERAGE.md` | Carla (QA) | ⬜ |
| 10 | `AVANADE_TASK_BOUNDARY_VALUE_ANALYSIS.md` | Carla (QA) | ⬜ |
| 11 | `AVANADE_TASK_ADVERSARIAL_REVIEW.md` | Carla (QA) | ⬜ |
| 12 | `AVANADE_TASK_UX_CHECKLIST.md` | Sofia (UX) | ⬜ |
| 13 | `AVANADE_TASK_USABILITY_HEURISTICS.md` | Sofia (UX) | ⬜ |
| 14 | `AVANADE_TASK_ACCESSIBILITY_WCAG.md` | Sofia (UX) | ⬜ |
| 15 | `AVANADE_TASK_VALUE_VALIDATION.md` | Paula (PO) | ⬜ |
| 16 | `AVANADE_TASK_RICE_PRIORITIZATION.md` | Paula (PO) | ⬜ |
| 17 | `AVANADE_TASK_METHODOLOGY_COMPLIANCE.md` | Supervisor | ⬜ |
| 18 | `AVANADE_TASK_DOC_ACCESSIBILITY.md` | Paige (Tech Writer) | ⬜ |
| 19 | `AVANADE_TASK_TECHNICAL_ACCURACY.md` | Paige (Tech Writer) | ⬜ |
| 20 | `AVANADE_TASK_EDITORIAL_REVIEW_PROSE.md` | Paige (Tech Writer) | ⬜ |
| 21 | `AVANADE_TASK_EDITORIAL_REVIEW_STRUCTURE.md` | Paige (Tech Writer) | ⬜ |
| 22 | `AVANADE_TASK_CREATE_NEXT_STORY.md` | Roberto (SM) | ⬜ |

---

### 🧠 2.3 Memory Artifacts (10 arquivos)

| # | Artefato | Agente | Verificado |
|---|----------|--------|------------|
| 1 | `AVANADE_MEMORY_SUPERVISOR.md` | Supervisor | ⬜ |
| 2 | `AVANADE_MEMORY_ANALYST_MARIA.md` | Maria | ⬜ |
| 3 | `AVANADE_MEMORY_ARCHITECT_WILSON.md` | Wilson | ⬜ |
| 4 | `AVANADE_MEMORY_PM_JOAO.md` | João | ⬜ |
| 5 | `AVANADE_MEMORY_PO_PAULA.md` | Paula | ⬜ |
| 6 | `AVANADE_MEMORY_SM_ROBERTO.md` | Roberto | ⬜ |
| 7 | `AVANADE_MEMORY_QA_CARLA.md` | Carla | ⬜ |
| 8 | `AVANADE_MEMORY_DEV_TIAGO.md` | Tiago | ⬜ |
| 9 | `AVANADE_MEMORY_UX_SOFIA.md` | Sofia | ⬜ |
| 10 | `AVANADE_MEMORY_TECH_WRITER_PAIGE.md` | Paige | ⬜ |

---

### 📋 2.4 Checklists (5 arquivos)

| # | Artefato | Propósito | Verificado |
|---|----------|-----------|------------|
| 1 | `AVANADE_PM_CHECKLIST_MD.md` | PM Validation | ⬜ |
| 2 | `AVANADE_ARCHITECT_CHECKLIST_MD.md` | Architect Validation | ⬜ |
| 3 | `AVANADE_PO_MASTER_CHECKLIST_MD.md` | PO Master Checklist | ⬜ |
| 4 | `AVANADE_STORY_DOD_CHECKLIST_MD.md` | Story Definition of Done | ⬜ |
| 5 | `AVANADE_INSTALLATION_CHECKLIST.md` | Installation Validation | ⬜ |

---

### 📋 2.5 Templates (9 arquivos)

| # | Artefato | Propósito | Verificado |
|---|----------|-----------|------------|
| 1 | `AVANADE_PRD_TEMPLATE_YAML.md` | PRD Structure | ⬜ |
| 2 | `AVANADE_ADR_TEMPLATE.md` | Architecture Decisions | ⬜ |
| 3 | `AVANADE_ARCHITECTURE_TEMPLATE.md` | Architecture Documentation | ⬜ |
| 4 | `AVANADE_TEST_PLAN_TEMPLATE.md` | Test Planning | ⬜ |
| 5 | `AVANADE_WIREFRAME_TEMPLATE.md` | Wireframe Structure | ⬜ |
| 6 | `AVANADE_BACKLOG_TEMPLATE_YAML.md` | Backlog Structure | ⬜ |
| 7 | `AVANADE_STORY_TEMPLATE_YAML.md` | User Story Template | ⬜ |
| 8 | `AVANADE_DISCOVERY_TEMPLATE_YAML.md` | Discovery Session | ⬜ |
| 9 | `AVANADE_FRONT_END_SPEC_TEMPLATE_YAML.md` | Front-End Specification | ⬜ |

---

### 📖 2.6 Documentation Artifacts (6 arquivos)

| # | Artefato | Propósito | Verificado |
|---|----------|-----------|------------|
| 1 | `AVANADE_DOC_STANDARDS_MD.md` | Documentation Standards | ⬜ |
| 2 | `AVANADE_COMMONMARK_TEMPLATE_MD.md` | Markdown Templates | ⬜ |
| 3 | `AVANADE_MERMAID_LIBRARY_MD.md` | Diagram Library | ⬜ |
| 4 | `AVANADE_EXPLANATION_TEMPLATE_MD.md` | Explanation Format | ⬜ |
| 5 | `AVANADE_FLUENT_DESIGN_GUIDELINES.md` | Microsoft Fluent UI Guidelines | ⬜ |
| 6 | `AVANADE_PARTY_MODE_GUIDE.md` | Party Mode Reference | ⬜ |

---

## ✅ PARTE 3: AGENT UPDATES (10 arquivos)

**Localização**: `_avanade-output/implementation-artifacts/agents/`

| # | Arquivo | Agente | Melhorias Aplicadas | Verificado |
|---|---------|--------|---------------------|------------|
| 1 | `SUPERVISOR-updates-COMPLETE.md` | Supervisor | Menu 28 ações, Party Mode, Agent Terraform | ⬜ |
| 2 | `MARIA-ANALYST-updates.md` | Maria | Advanced Elicitation, Discovery | ⬜ |
| 3 | `JOAO-PM-updates.md` | João | PRD Tri-modal, PM Checklist | ⬜ |
| 4 | `WILSON-ARCHITECT-updates.md` | Wilson | Architecture Quality, ADRs | ⬜ |
| 5 | `PAULA-PO-updates.md` | Paula | RICE, Value Validation | ⬜ |
| 6 | `ROBERTO-SM-updates.md` | Roberto | INVEST, Sprint Status | ⬜ |
| 7 | `CARLA-QA-updates.md` | Carla | Adversarial Review, Test Plans | ⬜ |
| 8 | `TIAGO-DEV-updates.md` | Tiago | Clean Code, TDD | ⬜ |
| 9 | `SOFIA-UX-updates.md` | Sofia | WCAG, Fluent Design | ⬜ |
| 10 | `PAIGE-TECH-WRITER-updates.md` | Paige | Doc Accessibility, Standards | ⬜ |

---

## 🧪 VALIDAÇÃO PÓS-INSTALAÇÃO

### Fase 1: Verificação de Estrutura (5 min)

```powershell
# Execute estes comandos para verificar a estrutura

# 1. Agentes Copilot
(Get-ChildItem ".github/agents/*.chatmode.md" -ErrorAction SilentlyContinue).Count -eq 10

# 2. Prompt Files
(Get-ChildItem ".github/prompts/*.prompt.md" -ErrorAction SilentlyContinue).Count -eq 4

# 3. Base & Inheritance System (NEW)
(Get-ChildItem ".avanade-method/_base/*" -ErrorAction SilentlyContinue).Count -eq 3

# 4. Agent Customization Files (NEW)
(Get-ChildItem ".avanade-method/agents/*.customize.yaml" -ErrorAction SilentlyContinue).Count -eq 10

# 5. Workflows
(Get-ChildItem ".avanade-method/workflows/*.workflow.md" -ErrorAction SilentlyContinue).Count -eq 9

# 6. Tasks
(Get-ChildItem ".avanade-method/tasks/*.task.md" -ErrorAction SilentlyContinue).Count -eq 22

# 7. Memory System
(Get-ChildItem ".avanade-method/memory/*-memory.md" -ErrorAction SilentlyContinue).Count -eq 10

# 8. Checklists
(Get-ChildItem ".avanade-method/checklists/*.md" -ErrorAction SilentlyContinue).Count -eq 5

# 9. Templates
(Get-ChildItem ".avanade-method/templates/*" -ErrorAction SilentlyContinue).Count -eq 9

# 10. Standards
(Get-ChildItem ".avanade-method/standards/*.md" -ErrorAction SilentlyContinue).Count -eq 5
```

### Fase 2: Teste de Agentes (10 min por agente)

**Para cada agente, execute no VSCode:**

| Agente | Comando de Teste | Esperado |
|--------|------------------|----------|
| Supervisor | `#avanade-method` | Menu de 28 ações | ⬜ |
| Maria | `@maria-analyst Olá` | Saudação + menu 8 ações | ⬜ |
| João | `@joao-pm Olá` | Saudação + menu 8 ações | ⬜ |
| Wilson | `@wilson-architect Olá` | Saudação + menu 8 ações | ⬜ |
| Paula | `@paula-po Olá` | Saudação + menu 8 ações | ⬜ |
| Roberto | `@roberto-sm Olá` | Saudação + menu 8 ações | ⬜ |
| Carla | `@carla-qa Olá` | Saudação + menu 8 ações | ⬜ |
| Tiago | `@tiago-dev Olá` | Saudação + menu 8 ações | ⬜ |
| Sofia | `@sofia-ux Olá` | Saudação + menu 8 ações | ⬜ |
| Paige | `@paige-tech-writer Olá` | Saudação + menu 8 ações | ⬜ |

### Fase 3: Teste de Workflows (15 min)

| Workflow | Comando | Validação | Verificado |
|----------|---------|-----------|------------|
| Create PRD | `4` no menu | Tri-modal (create/validate/edit) | ⬜ |
| Party Mode | `#party arquitetura` | 3-4 agentes simultâneos | ⬜ |
| Quick Dev | `16` no menu | Tech spec conversacional | ⬜ |
| Sprint Planning | `9` no menu | Gera sprint-status.yaml | ⬜ |

### Fase 4: Validação de Nomenclatura (5 min)

**Verificar que NÃO aparecem referências a "BMAD" (exceto em paths técnicos):**

```powershell
# Procurar por BMAD em artefatos (deve retornar 0 exceto paths)
Get-ChildItem "_avanade-output/implementation-artifacts" -Recurse -File | 
    Select-String -Pattern "BMAD" -CaseSensitive | 
    Where-Object { $_.Line -notmatch "_avanade-method|avanade-method-" }
```

**Termos corretos:**
- ⬜ "Avanade Method v2" (não "BMAD")
- ⬜ "Avanade Standard" (não "BMAD Standard")
- ⬜ "Avanade Workflow" (não "BMAD Workflow")

---

## 📋 CHECKLIST FINAL

### ✅ Instalação Base
- ⬜ 10/10 Agentes Copilot instalados (.github/agents/)
- ⬜ 4/4 Prompt Files instalados
- ⬜ 3/3 Base & Inheritance (_base/) instalados
- ⬜ 10/10 Agent Customization (agents/) instalados
- ⬜ 3/3 Configurações centrais
- ⬜ MCP configurado corretamente

### ✅ Artefatos Metodológicos
- ⬜ 9/9 Workflow Guides
- ⬜ 22/22 Task Artifacts
- ⬜ 10/10 Memory Artifacts
- ⬜ 5/5 Checklists
- ⬜ 9/9 Templates
- ⬜ 6/6 Documentation
- ⬜ 5/5 Standards
- ⬜ 2/2 Root Files (README, INSTALLATION-CHECKLIST)

### ✅ Agent Updates
- ⬜ 10/10 Agent update files aplicados

### ✅ Validação
- ⬜ Estrutura de pastas correta
- ⬜ Sistema de herança funcionando (_base + agents)
- ⬜ Todos agentes respondem
- ⬜ Workflows executam corretamente
- ⬜ Nomenclatura "Avanade Method" (não BMAD)

---

## 🔧 TROUBLESHOOTING

### Problema: Agente não responde

**Causa provável**: MCP não configurado

**Solução**:
1. Abrir `%APPDATA%/Code/User/mcp.json`
2. Verificar `utility-agent-id` e `x-cockpit-team-member-ids`
3. Reiniciar VSCode

### Problema: Agente Copilot não aparece

**Causa provável**: Arquivo .chatmode.md incorreto

**Solução**:
1. Verificar que arquivo está em `.github/agents/`
2. Verificar extensão `.chatmode.md`
3. Executar `Developer: Reload Window`

### Problema: Workflow não executa

**Causa provável**: Artefatos não encontrados

**Solução**:
1. Verificar que arquivos estão em `.avanade-method/workflows/`
2. Verificar nomenclatura `*.workflow.md`
3. Executar `/avanade-help` para verificar artefatos disponíveis

### Problema: Sistema de Herança não funciona **(NEW)**

**Causa provável**: Arquivos _base ou agents não instalados

**Solução**:
1. Verificar que `.avanade-method/_base/avanade-master.md` existe
2. Verificar que `.avanade-method/_base/config.yaml` existe
3. Verificar que todos 10 arquivos `.customize.yaml` existem em `.avanade-method/agents/`
4. Regenerar agentes em `.github/agents/` a partir dos customize.yaml

---

## 📊 STATUS DA INSTALAÇÃO

| Data | Instalador | Status | Observações |
|------|------------|--------|-------------|
| ____/__/____ | ____________ | ⬜ Pendente / ⬜ Completo | |

---

**Documento**: `${AVANADE_INSTALLATION_CHECKLIST}`  
**Versão**: 2.1  
**Última Atualização**: 2026-02-06  
**Owner**: Supervisor (Avanade Method v2)

