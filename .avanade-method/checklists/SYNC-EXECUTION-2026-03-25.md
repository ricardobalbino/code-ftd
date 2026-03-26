# 🚀 SINCRONIZAÇÃO AVANADE METHOD → avanade.ai
**Data**: 2026-03-25  
**Versão base**: v2.1 (AVANADE_INSTALLATION_CHECKLIST2.md)  
**Nova versão alvo**: v2.2  
**Gerado por**: Supervisor Agent — Análise Completa do Workspace

---

## 📊 ANÁLISE DO AMBIENTE LOCAL

### Resumo do Inventário (25/03/2026)

| Categoria | Qtd Local | Qtd Checklist v2.1 | Delta (NOVO) |
|-----------|------------|---------------------|--------------|
| Agentes `.github/agents/` (`.agent.md`) | 10 | 10 | 0 |
| Agent Customize `.avanade-method/agents/` | 10 | 10 | 0 |
| Base `_base/` | 3 | 3 | 0 |
| Workflows `workflows/` | 29 | 9 | **+20** |
| Tasks `tasks/` | 27 | 22 | **+5** |
| Checklists `checklists/` | 9 | 5 | **+4** |
| Templates `templates/` | 13 | 8 | **+5** |
| Memory `memory/` | 10 | 10 | 0 |
| Standards `standards/` | 6 | 5 | **+1** |
| Guides `guides/` | 2 | 1 | **+1** |
| Personas `personas/` | 9 | 0 | **+9 (NOVO)** |
| Agent Prompts `prompts/` | 11 | 0 | **+11 (NOVO)** |
| Configs `configs/` | 8 | 0 | **+8 (NOVO)** |
| `.github/prompts/` | 4 | 4 | 0 |
| **TOTAL** | **151** | **87** | **+64** |

---

## 📋 INVENTÁRIO COMPLETO PARA SINCRONIZAÇÃO

### GRUPO 1: AGENTES COPILOT (`.github/agents/`) — UPDATE
> Verificar se existem no servidor, atualizar conteúdo completo

| Arquivo Local | Artifact Key | Ação |
|---------------|-------------|------|
| `.github/agents/avanade-supervisor.agent.md` | `AVANADE_SUPERVISOR_PROMPT_MD` | SEARCH → UPDATE/CREATE |
| `.github/agents/joao-pm.agent.md` | `AVANADE_PM_JOAO_PROMPT_MD` | SEARCH → UPDATE/CREATE |
| `.github/agents/wilson-architect.agent.md` | `AVANADE_ARCHITECT_WILSON_PROMPT_MD` | SEARCH → UPDATE/CREATE |
| `.github/agents/maria-analyst.agent.md` | `AVANADE_ANALYST_MARIA_PROMPT_MD` | SEARCH → UPDATE/CREATE |
| `.github/agents/roberto-sm.agent.md` | `AVANADE_SM_PROMPT_MD` | SEARCH → UPDATE/CREATE |
| `.github/agents/carla-qa.agent.md` | `AVANADE_QA_CARLA_PROMPT_MD` | SEARCH → UPDATE/CREATE |
| `.github/agents/tiago-dev.agent.md` | `AVANADE_DEVELOPER_TIAGO_PROMPT_MD` | SEARCH → UPDATE/CREATE |
| `.github/agents/paula-po.agent.md` | `AVANADE_PO_PROMPT_MD` | SEARCH → UPDATE/CREATE |
| `.github/agents/sofia-ux.agent.md` | `AVANADE_UX_EXPERT_PROMPT_MD` | SEARCH → UPDATE/CREATE |
| `.github/agents/paige-tech-writer.agent.md` | `AVANADE_TECH_WRITER_PAIGE_PROMPT_MD` | SEARCH → UPDATE/CREATE |

### GRUPO 2: AGENT CUSTOMIZE YAML — UPDATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/agents/supervisor.customize.yaml` | `AVANADE_AGENT_SUPERVISOR_CUSTOMIZE_YAML` |
| `.avanade-method/agents/joao-pm.customize.yaml` | `AVANADE_AGENT_JOAO_PM_CUSTOMIZE_YAML` |
| `.avanade-method/agents/wilson-architect.customize.yaml` | `AVANADE_AGENT_WILSON_ARCHITECT_CUSTOMIZE_YAML` |
| `.avanade-method/agents/maria-analyst.customize.yaml` | `AVANADE_AGENT_MARIA_ANALYST_CUSTOMIZE_YAML` |
| `.avanade-method/agents/roberto-sm.customize.yaml` | `AVANADE_AGENT_ROBERTO_SM_CUSTOMIZE_YAML` |
| `.avanade-method/agents/carla-qa.customize.yaml` | `AVANADE_AGENT_CARLA_QA_CUSTOMIZE_YAML` |
| `.avanade-method/agents/tiago-dev.customize.yaml` | `AVANADE_AGENT_TIAGO_DEV_CUSTOMIZE_YAML` |
| `.avanade-method/agents/paula-po.customize.yaml` | `AVANADE_AGENT_PAULA_PO_CUSTOMIZE_YAML` |
| `.avanade-method/agents/sofia-ux.customize.yaml` | `AVANADE_AGENT_SOFIA_UX_CUSTOMIZE_YAML` |
| `.avanade-method/agents/paige-tech-writer.customize.yaml` | `AVANADE_AGENT_PAIGE_TECH_WRITER_CUSTOMIZE_YAML` |

### GRUPO 3: BASE SYSTEM — UPDATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/_base/avanade-master.md` | `AVANADE_BASE_MASTER_AGENT_MD` |
| `.avanade-method/_base/config.yaml` | `AVANADE_BASE_CONFIG_YAML` |
| `.avanade-method/_base/INHERITANCE-ARCHITECTURE.md` | `AVANADE_BASE_INHERITANCE_ARCHITECTURE_MD` |

### GRUPO 4: WORKFLOWS PADRÃO — UPDATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/workflows/create-brief.workflow.md` | `AVANADE_WORKFLOW_GUIDE_CREATE_BRIEF` |
| `.avanade-method/workflows/create-prd.workflow.md` | `AVANADE_WORKFLOW_GUIDE_CREATE_PRD` |
| `.avanade-method/workflows/create-architecture.workflow.md` | `AVANADE_WORKFLOW_GUIDE_CREATE_ARCHITECTURE` |
| `.avanade-method/workflows/create-ux.workflow.md` | `AVANADE_WORKFLOW_GUIDE_CREATE_UX` |
| `.avanade-method/workflows/create-epics-stories.workflow.md` | `AVANADE_WORKFLOW_GUIDE_CREATE_EPICS_STORIES` |
| `.avanade-method/workflows/sprint-planning.workflow.md` | `AVANADE_WORKFLOW_GUIDE_SPRINT_PLANNING` |
| `.avanade-method/workflows/code-review.workflow.md` | `AVANADE_WORKFLOW_GUIDE_CODE_REVIEW` |
| `.avanade-method/workflows/quick-dev.workflow.md` | `AVANADE_WORKFLOW_GUIDE_QUICK_DEV` |
| `.avanade-method/workflows/workflow-manifest.md` | `AVANADE_WORKFLOW_MANIFEST` |

### GRUPO 5: WORKFLOWS FTD/D365 ⭐ NOVO — CREATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/workflows/d365-discovery.workflow.md` | `AVANADE_WORKFLOW_D365_DISCOVERY` |
| `.avanade-method/workflows/d365-integration.workflow.md` | `AVANADE_WORKFLOW_D365_INTEGRATION` |
| `.avanade-method/workflows/d365-plugin-dev.workflow.md` | `AVANADE_WORKFLOW_D365_PLUGIN_DEV` |
| `.avanade-method/workflows/create-ftd-user-story.md` | `AVANADE_WORKFLOW_CREATE_FTD_USER_STORY` |
| `.avanade-method/workflows/process-transcription.md` | `AVANADE_WORKFLOW_PROCESS_TRANSCRIPTION` |
| `.avanade-method/workflows/validate-ftd-compliance.md` | `AVANADE_WORKFLOW_VALIDATE_FTD_COMPLIANCE` |
| `.avanade-method/workflows/WF-001-full-inception.yaml` | `AVANADE_WORKFLOW_WF001_FULL_INCEPTION` |
| `.avanade-method/workflows/WF-002-feature-development.yaml` | `AVANADE_WORKFLOW_WF002_FEATURE_DEVELOPMENT` |
| `.avanade-method/workflows/WF-003-enterprise-delivery.yaml` | `AVANADE_WORKFLOW_WF003_ENTERPRISE_DELIVERY` |
| `.avanade-method/workflows/WF-004-brownfield-fullstack.yaml` | `AVANADE_WORKFLOW_WF004_BROWNFIELD_FULLSTACK` |
| `.avanade-method/workflows/WF-005-brownfield-service.yaml` | `AVANADE_WORKFLOW_WF005_BROWNFIELD_SERVICE` |
| `.avanade-method/workflows/WF-006-brownfield-ui.yaml` | `AVANADE_WORKFLOW_WF006_BROWNFIELD_UI` |
| `.avanade-method/workflows/WF-007-greenfield-fullstack.yaml` | `AVANADE_WORKFLOW_WF007_GREENFIELD_FULLSTACK` |
| `.avanade-method/workflows/WF-008-greenfield-service.yaml` | `AVANADE_WORKFLOW_WF008_GREENFIELD_SERVICE` |
| `.avanade-method/workflows/WF-009-greenfield-ui.yaml` | `AVANADE_WORKFLOW_WF009_GREENFIELD_UI` |
| `.avanade-method/workflows/WF-04-processamento-documental.md` | `AVANADE_WORKFLOW_WF04_PROCESSAMENTO_DOCUMENTAL` |
| `.avanade-method/workflows/WF-05-health-check.md` | `AVANADE_WORKFLOW_WF05_HEALTH_CHECK` |
| `.avanade-method/workflows/WF-06-gerador-stories.md` | `AVANADE_WORKFLOW_WF06_GERADOR_STORIES` |
| `.avanade-method/workflows/WF-07-conformidade-bca.md` | `AVANADE_WORKFLOW_WF07_CONFORMIDADE_BCA` |
| `.avanade-method/workflows/WF-08-autodocumentacao-readme.md` | `AVANADE_WORKFLOW_WF08_AUTODOCUMENTACAO` |

### GRUPO 6: TASKS PADRÃO — UPDATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/tasks/advanced-elicitation.task.md` | `AVANADE_TASK_ADVANCED_ELICITATION` |
| `.avanade-method/tasks/architecture-quality.task.md` | `AVANADE_TASK_ARCHITECTURE_QUALITY` |
| `.avanade-method/tasks/story-readiness.task.md` | `AVANADE_TASK_STORY_READINESS` |
| `.avanade-method/tasks/retrospective-facilitation.task.md` | `AVANADE_TASK_RETROSPECTIVE_FACILITATION` |
| `.avanade-method/tasks/invest-validation.task.md` | `AVANADE_TASK_INVEST_VALIDATION` |
| `.avanade-method/tasks/clean-code.task.md` | `AVANADE_TASK_CLEAN_CODE` |
| `.avanade-method/tasks/debugging-guide.task.md` | `AVANADE_TASK_DEBUGGING_GUIDE` |
| `.avanade-method/tasks/code-review.task.md` | `AVANADE_TASK_CODE_REVIEW` |
| `.avanade-method/tasks/test-coverage.task.md` | `AVANADE_TASK_TEST_COVERAGE` |
| `.avanade-method/tasks/boundary-value-analysis.task.md` | `AVANADE_TASK_BOUNDARY_VALUE_ANALYSIS` |
| `.avanade-method/tasks/adversarial-review.task.md` | `AVANADE_TASK_ADVERSARIAL_REVIEW` |
| `.avanade-method/tasks/ux-checklist.task.md` | `AVANADE_TASK_UX_CHECKLIST` |
| `.avanade-method/tasks/usability-heuristics.task.md` | `AVANADE_TASK_USABILITY_HEURISTICS` |
| `.avanade-method/tasks/accessibility-wcag.task.md` | `AVANADE_TASK_ACCESSIBILITY_WCAG` |
| `.avanade-method/tasks/value-validation.task.md` | `AVANADE_TASK_VALUE_VALIDATION` |
| `.avanade-method/tasks/rice-prioritization.task.md` | `AVANADE_TASK_RICE_PRIORITIZATION` |
| `.avanade-method/tasks/methodology-compliance.task.md` | `AVANADE_TASK_METHODOLOGY_COMPLIANCE` |
| `.avanade-method/tasks/doc-accessibility.task.md` | `AVANADE_TASK_DOC_ACCESSIBILITY` |
| `.avanade-method/tasks/technical-accuracy.task.md` | `AVANADE_TASK_TECHNICAL_ACCURACY` |
| `.avanade-method/tasks/editorial-review-prose.task.md` | `AVANADE_TASK_EDITORIAL_REVIEW_PROSE` |
| `.avanade-method/tasks/editorial-review-structure.task.md` | `AVANADE_TASK_EDITORIAL_REVIEW_STRUCTURE` |
| `.avanade-method/tasks/create-next-story.task.md` | `AVANADE_CREATE_NEXT_STORY_TASK_MD` |

### GRUPO 7: TASKS FTD/D365 ⭐ NOVO — CREATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/tasks/d365-fit-gap.task.md` | `AVANADE_TASK_D365_FIT_GAP` |
| `.avanade-method/tasks/d365-plugin-review.task.md` | `AVANADE_TASK_D365_PLUGIN_REVIEW` |
| `.avanade-method/tasks/d365-solution-transport.task.md` | `AVANADE_TASK_D365_SOLUTION_TRANSPORT` |
| `.avanade-method/tasks/create-deep-research-prompt.task.md` | `AVANADE_TASK_CREATE_DEEP_RESEARCH_PROMPT` |
| `.avanade-method/tasks/generate-ai-frontend-prompt.task.md` | `AVANADE_TASK_GENERATE_AI_FRONTEND_PROMPT` |

### GRUPO 8: CHECKLISTS PADRÃO — UPDATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/checklists/pm-checklist.md` | `AVANADE_PM_CHECKLIST_MD` |
| `.avanade-method/checklists/architect-checklist.md` | `AVANADE_ARCHITECT_CHECKLIST_MD` |
| `.avanade-method/checklists/po-master-checklist.md` | `AVANADE_PO_MASTER_CHECKLIST_MD` |
| `.avanade-method/checklists/story-dod-checklist.md` | `AVANADE_STORY_DOD_CHECKLIST_MD` |
| `.avanade-method/checklists/installation-checklist.md` | `AVANADE_INSTALLATION_CHECKLIST` |

### GRUPO 9: CHECKLISTS FTD/D365 ⭐ NOVO — CREATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/checklists/d365-code-review-checklist.md` | `AVANADE_CHECKLIST_D365_CODE_REVIEW` |
| `.avanade-method/checklists/d365-solution-checklist.md` | `AVANADE_CHECKLIST_D365_SOLUTION` |
| `.avanade-method/checklists/artifact-sync-workflow.md` | `AVANADE_CHECKLIST_ARTIFACT_SYNC_WORKFLOW` |
| `.avanade-method/checklists/AVANADE_INSTALLATION_CHECKLIST2.md` | `AVANADE_INSTALLATION_CHECKLIST_V2_1` |

### GRUPO 10: TEMPLATES PADRÃO — UPDATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/templates/prd-template.yaml` | `AVANADE_PRD_TEMPLATE_YAML` |
| `.avanade-method/templates/architecture-template.md` | `AVANADE_ARCHITECTURE_TEMPLATE` |
| `.avanade-method/templates/adr-template.md` | `AVANADE_ADR_TEMPLATE` |
| `.avanade-method/templates/test-plan-template.md` | `AVANADE_TEST_PLAN_TEMPLATE` |
| `.avanade-method/templates/wireframe-template.md` | `AVANADE_WIREFRAME_TEMPLATE` |
| `.avanade-method/templates/backlog-template.yaml` | `AVANADE_BACKLOG_TEMPLATE_YAML` |
| `.avanade-method/templates/story-template.yaml` | `AVANADE_STORY_TEMPLATE_YAML` |
| `.avanade-method/templates/discovery-template.yaml` | `AVANADE_DISCOVERY_TEMPLATE_YAML` |
| `.avanade-method/templates/frontend-spec-template.yaml` | `AVANADE_FRONT_END_SPEC_TEMPLATE_YAML` |

### GRUPO 11: TEMPLATES FTD/D365 ⭐ NOVO — CREATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/templates/d365-architecture-template.md` | `AVANADE_TEMPLATE_D365_ARCHITECTURE` |
| `.avanade-method/templates/d365-discovery-template.md` | `AVANADE_TEMPLATE_D365_DISCOVERY` |
| `.avanade-method/templates/d365-prd-template.md` | `AVANADE_TEMPLATE_D365_PRD` |
| `.avanade-method/templates/d365-story-template.md` | `AVANADE_TEMPLATE_D365_STORY` |

### GRUPO 12: MEMORY (10 agentes) — UPDATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/memory/supervisor-memory.md` | `AVANADE_MEMORY_SUPERVISOR` |
| `.avanade-method/memory/maria-analyst-memory.md` | `AVANADE_MEMORY_ANALYST_MARIA` |
| `.avanade-method/memory/wilson-architect-memory.md` | `AVANADE_MEMORY_ARCHITECT_WILSON` |
| `.avanade-method/memory/joao-pm-memory.md` | `AVANADE_MEMORY_PM_JOAO` |
| `.avanade-method/memory/paula-po-memory.md` | `AVANADE_MEMORY_PO_PAULA` |
| `.avanade-method/memory/roberto-sm-memory.md` | `AVANADE_MEMORY_SM_ROBERTO` |
| `.avanade-method/memory/carla-qa-memory.md` | `AVANADE_MEMORY_QA_CARLA` |
| `.avanade-method/memory/tiago-dev-memory.md` | `AVANADE_MEMORY_DEV_TIAGO` |
| `.avanade-method/memory/sofia-ux-memory.md` | `AVANADE_MEMORY_UX_SOFIA` |
| `.avanade-method/memory/paige-tech-writer-memory.md` | `AVANADE_MEMORY_TECH_WRITER_PAIGE` |

### GRUPO 13: STANDARDS PADRÃO — UPDATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/standards/doc-standards.md` | `AVANADE_DOC_STANDARDS_MD` |
| `.avanade-method/standards/commonmark-template.md` | `AVANADE_COMMONMARK_TEMPLATE_MD` |
| `.avanade-method/standards/explanation-template.md` | `AVANADE_EXPLANATION_TEMPLATE_MD` |
| `.avanade-method/standards/fluent-design-guidelines.md` | `AVANADE_FLUENT_DESIGN_GUIDELINES` |
| `.avanade-method/standards/mermaid-library.md` | `AVANADE_MERMAID_LIBRARY_MD` |

### GRUPO 14: STANDARDS FTD ⭐ NOVO — CREATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/standards/d365-development-standards.md` | `AVANADE_STANDARDS_D365_DEVELOPMENT` |

### GRUPO 15: GUIDES — UPDATE/CREATE
| Arquivo Local | Artifact Key | Ação |
|---------------|-------------|------|
| `.avanade-method/guides/party-mode-guide.md` | `AVANADE_PARTY_MODE_GUIDE` | UPDATE |
| `.avanade-method/guides/manual-contextualizacao-bmad.md` | `AVANADE_GUIDE_MANUAL_CONTEXTUALIZACAO_BMAD` | ⭐ CREATE |

### GRUPO 16: PERSONAS ⭐ NOVO COMPLETO — CREATE (9 arquivos)
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/personas/joao-pm.yaml` | `AVANADE_PERSONA_JOAO_PM` |
| `.avanade-method/personas/wilson-architect.yaml` | `AVANADE_PERSONA_WILSON_ARCHITECT` |
| `.avanade-method/personas/maria-analyst.yaml` | `AVANADE_PERSONA_MARIA_ANALYST` |
| `.avanade-method/personas/roberto-sm.yaml` | `AVANADE_PERSONA_ROBERTO_SM` |
| `.avanade-method/personas/carla-qa.yaml` | `AVANADE_PERSONA_CARLA_QA` |
| `.avanade-method/personas/tiago-dev.yaml` | `AVANADE_PERSONA_TIAGO_DEV` |
| `.avanade-method/personas/paula-po.yaml` | `AVANADE_PERSONA_PAULA_PO` |
| `.avanade-method/personas/sofia-ux.yaml` | `AVANADE_PERSONA_SOFIA_UX` |
| `.avanade-method/personas/paige-tech-writer.yaml` | `AVANADE_PERSONA_PAIGE_TECH_WRITER` |

### GRUPO 17: AGENT PROMPTS `.avanade-method/prompts/` ⭐ NOVO — CREATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/prompts/supervisor.prompt.md` | `AVANADE_PROMPT_SUPERVISOR` |
| `.avanade-method/prompts/joao-pm.prompt.md` | `AVANADE_PROMPT_JOAO_PM` |
| `.avanade-method/prompts/wilson-architect.prompt.md` | `AVANADE_PROMPT_WILSON_ARCHITECT` |
| `.avanade-method/prompts/maria-analyst.prompt.md` | `AVANADE_PROMPT_MARIA_ANALYST` |
| `.avanade-method/prompts/roberto-sm.prompt.md` | `AVANADE_PROMPT_ROBERTO_SM` |
| `.avanade-method/prompts/carla-qa.prompt.md` | `AVANADE_PROMPT_CARLA_QA` |
| `.avanade-method/prompts/tiago-dev.prompt.md` | `AVANADE_PROMPT_TIAGO_DEV` |
| `.avanade-method/prompts/paula-po.prompt.md` | `AVANADE_PROMPT_PAULA_PO` |
| `.avanade-method/prompts/sofia-ux.prompt.md` | `AVANADE_PROMPT_SOFIA_UX` |
| `.avanade-method/prompts/paige-tech-writer.prompt.md` | `AVANADE_PROMPT_PAIGE_TECH_WRITER` |
| `.avanade-method/prompts/master-full.prompt.md` | `AVANADE_PROMPT_MASTER_FULL` |
| `.avanade-method/prompts/vscode-config.prompt.md` | `AVANADE_PROMPT_VSCODE_CONFIG` |
| `.avanade-method/prompts/builders/builder-agent.prompt.md` | `AVANADE_PROMPT_BUILDER_AGENT` |
| `.avanade-method/prompts/builders/builder-task.prompt.md` | `AVANADE_PROMPT_BUILDER_TASK` |
| `.avanade-method/prompts/builders/builder-workflow.prompt.md` | `AVANADE_PROMPT_BUILDER_WORKFLOW` |

### GRUPO 18: CONFIGS ⭐ NOVO — CREATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.avanade-method/configs/core-config.yaml` | `AVANADE_CONFIG_CORE` |
| `.avanade-method/configs/base-config.yaml` | `AVANADE_CONFIG_BASE` |
| `.avanade-method/configs/d365-config.yaml` | `AVANADE_CONFIG_D365` |
| `.avanade-method/configs/d365-context.yaml` | `AVANADE_CONFIG_D365_CONTEXT` |
| `.avanade-method/configs/cockpit-agents-complete-config.yaml` | `AVANADE_CONFIG_COCKPIT_AGENTS` |
| `.avanade-method/configs/builder-module-config.yaml` | `AVANADE_CONFIG_BUILDER_MODULE` |

### GRUPO 19: GITHUB PROMPTS — UPDATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.github/prompts/prd-creation.prompt.md` | `AVANADE_GITHUB_PROMPT_PRD_CREATION` |
| `.github/prompts/adr-template.prompt.md` | `AVANADE_GITHUB_PROMPT_ADR_TEMPLATE` |
| `.github/prompts/test-plan.prompt.md` | `AVANADE_GITHUB_PROMPT_TEST_PLAN` |
| `.github/prompts/wireframe.prompt.md` | `AVANADE_GITHUB_PROMPT_WIREFRAME` |

### GRUPO 20: COPILOT INSTRUCTIONS — UPDATE
| Arquivo Local | Artifact Key |
|---------------|-------------|
| `.github/copilot-instructions.md` | `AVANADE_COPILOT_INSTRUCTIONS` |

---

## ⚠️ DIAGNÓSTICO: POR QUE AS MCP TOOLS NÃO ESTÃO DISPONÍVEIS NO MODO AGENTE

As tools `mcp_avanade-metho_*` são providas pelo servidor **HTTP MCP** configurado em:
```json
C:\Users\r.a.mendes.da.silva\AppData\Roaming\Code\User\mcp.json
→ "avanade-method": { "url": "https://mcp.br.avanade.ai/mcp" }
```

**Causa**: Em sessões de agente (`runSubagent`), os tools MCP de servidores HTTP externos NÃO são disponibilizados pelo runtime. Os MCP tools externos estão disponíveis apenas na **janela de chat principal do VS Code Copilot**.

**Solução**: Usar o prompt abaixo diretamente no chat principal do VS Code.

---

## 🚀 PROMPT DE EXECUÇÃO — COPIE E USE NO CHAT PRINCIPAL DO VSCODE

> **IMPORTANTE**: Cole este prompt no chat do VS Code Copilot (não no Agente), onde os MCP tools `mcp_avanade-metho_*` estão disponíveis.

```markdown
# SINCRONIZAÇÃO AVANADE METHOD → avanade.ai — v2.2
**Data**: 2026-03-25
**Versão base**: v2.1
**Nova versão**: v2.2

## ⚠️ REGRAS CRÍTICAS — CONFIRMAR ANTES DE EXECUTAR

1. ✅ `accessibility: "Namespace"` (NUNCA "NAMESPACE_USER")
2. ✅ `type: "string"` (para YAML e MD)
3. ✅ `promote_to_production: true`
4. ✅ **Conteúdo COMPLETO e IDÊNTICO** — NUNCA resumir

---

## PASSO 1: BUSCA EM BATCH (verificar o que já existe)

Use `mcp_avanade-metho_search_artifacts` para cada grupo abaixo e registre:
- ✅ ENCONTRADO → UPDATE (anotar ID)
- ❌ NÃO ENCONTRADO → CREATE

**Buscar (padrão de busca)**:
- "AVANADE_SUPERVISOR"
- "AVANADE_PM_JOAO"
- "AVANADE_ARCHITECT_WILSON"
- "AVANADE_ANALYST_MARIA"
- "AVANADE_SM_PROMPT"
- "AVANADE_QA_CARLA"
- "AVANADE_DEVELOPER_TIAGO"
- "AVANADE_PO_PROMPT"
- "AVANADE_UX_EXPERT"
- "AVANADE_TECH_WRITER_PAIGE"
- "AVANADE_AGENT_SUPERVISOR"
- "AVANADE_AGENT_JOAO"
- "AVANADE_AGENT_WILSON"
- "AVANADE_AGENT_MARIA"
- "AVANADE_AGENT_ROBERTO"
- "AVANADE_AGENT_CARLA"
- "AVANADE_AGENT_TIAGO"
- "AVANADE_AGENT_PAULA"
- "AVANADE_AGENT_SOFIA"
- "AVANADE_AGENT_PAIGE"
- "AVANADE_BASE_MASTER"
- "AVANADE_BASE_CONFIG"
- "AVANADE_BASE_INHERITANCE"
- "AVANADE_WORKFLOW_GUIDE"
- "AVANADE_WORKFLOW_D365"
- "AVANADE_WORKFLOW_WF"
- "AVANADE_TASK_"
- "AVANADE_PM_CHECKLIST"
- "AVANADE_ARCHITECT_CHECKLIST"
- "AVANADE_PO_MASTER"
- "AVANADE_STORY_DOD"
- "AVANADE_INSTALLATION_CHECKLIST"
- "AVANADE_CHECKLIST_D365"
- "AVANADE_PRD_TEMPLATE"
- "AVANADE_ARCHITECTURE_TEMPLATE"
- "AVANADE_ADR_TEMPLATE"
- "AVANADE_TEST_PLAN"
- "AVANADE_WIREFRAME"
- "AVANADE_BACKLOG_TEMPLATE"
- "AVANADE_STORY_TEMPLATE"
- "AVANADE_DISCOVERY_TEMPLATE"
- "AVANADE_FRONT_END_SPEC"
- "AVANADE_TEMPLATE_D365"
- "AVANADE_MEMORY_"
- "AVANADE_DOC_STANDARDS"
- "AVANADE_COMMONMARK"
- "AVANADE_EXPLANATION_TEMPLATE"
- "AVANADE_FLUENT_DESIGN"
- "AVANADE_MERMAID_LIBRARY"
- "AVANADE_STANDARDS_D365"
- "AVANADE_PARTY_MODE"
- "AVANADE_GUIDE_"
- "AVANADE_PERSONA_"
- "AVANADE_PROMPT_"
- "AVANADE_CONFIG_"
- "AVANADE_GITHUB_PROMPT"
- "AVANADE_COPILOT_INSTRUCTIONS"

---

## PASSO 2: SINCRONIZAR EM ORDEM DE PRIORIDADE

### 2.1 – Agentes (GRUPO 1-2): UPDATE ou CREATE

Para cada arquivo, execute:
1. `read_file(filePath: "{caminho_completo}", startLine: 1, endLine: 9999)`
2. Se ID encontrado: `mcp_avanade-metho_update_artifact(id: "{id}", value: "{conteúdo_completo}", promote_to_production: true)`
3. Se não encontrado: `mcp_avanade-metho_create_artifact(key: "{ARTIFACT_KEY}", value: "{conteúdo_completo}", type: "string", accessibility: "Namespace", promote_to_production: true)`

**Arquivos:**
- `.github/agents/avanade-supervisor.agent.md` → `AVANADE_SUPERVISOR_PROMPT_MD`
- `.github/agents/joao-pm.agent.md` → `AVANADE_PM_JOAO_PROMPT_MD`
- `.github/agents/wilson-architect.agent.md` → `AVANADE_ARCHITECT_WILSON_PROMPT_MD`
- `.github/agents/maria-analyst.agent.md` → `AVANADE_ANALYST_MARIA_PROMPT_MD`
- `.github/agents/roberto-sm.agent.md` → `AVANADE_SM_PROMPT_MD`
- `.github/agents/carla-qa.agent.md` → `AVANADE_QA_CARLA_PROMPT_MD`
- `.github/agents/tiago-dev.agent.md` → `AVANADE_DEVELOPER_TIAGO_PROMPT_MD`
- `.github/agents/paula-po.agent.md` → `AVANADE_PO_PROMPT_MD`
- `.github/agents/sofia-ux.agent.md` → `AVANADE_UX_EXPERT_PROMPT_MD`
- `.github/agents/paige-tech-writer.agent.md` → `AVANADE_TECH_WRITER_PAIGE_PROMPT_MD`
- `.avanade-method/agents/supervisor.customize.yaml` → `AVANADE_AGENT_SUPERVISOR_CUSTOMIZE_YAML`
- `.avanade-method/agents/joao-pm.customize.yaml` → `AVANADE_AGENT_JOAO_PM_CUSTOMIZE_YAML`
- `.avanade-method/agents/wilson-architect.customize.yaml` → `AVANADE_AGENT_WILSON_ARCHITECT_CUSTOMIZE_YAML`
- `.avanade-method/agents/maria-analyst.customize.yaml` → `AVANADE_AGENT_MARIA_ANALYST_CUSTOMIZE_YAML`
- `.avanade-method/agents/roberto-sm.customize.yaml` → `AVANADE_AGENT_ROBERTO_SM_CUSTOMIZE_YAML`
- `.avanade-method/agents/carla-qa.customize.yaml` → `AVANADE_AGENT_CARLA_QA_CUSTOMIZE_YAML`
- `.avanade-method/agents/tiago-dev.customize.yaml` → `AVANADE_AGENT_TIAGO_DEV_CUSTOMIZE_YAML`
- `.avanade-method/agents/paula-po.customize.yaml` → `AVANADE_AGENT_PAULA_PO_CUSTOMIZE_YAML`
- `.avanade-method/agents/sofia-ux.customize.yaml` → `AVANADE_AGENT_SOFIA_UX_CUSTOMIZE_YAML`
- `.avanade-method/agents/paige-tech-writer.customize.yaml` → `AVANADE_AGENT_PAIGE_TECH_WRITER_CUSTOMIZE_YAML`

### 2.2 – Base System (GRUPO 3): UPDATE
- `.avanade-method/_base/avanade-master.md` → `AVANADE_BASE_MASTER_AGENT_MD`
- `.avanade-method/_base/config.yaml` → `AVANADE_BASE_CONFIG_YAML`
- `.avanade-method/_base/INHERITANCE-ARCHITECTURE.md` → `AVANADE_BASE_INHERITANCE_ARCHITECTURE_MD`

### 2.3 – Workflows Padrão (GRUPO 4): UPDATE
- `.avanade-method/workflows/create-brief.workflow.md` → `AVANADE_WORKFLOW_GUIDE_CREATE_BRIEF`
- `.avanade-method/workflows/create-prd.workflow.md` → `AVANADE_WORKFLOW_GUIDE_CREATE_PRD`
- `.avanade-method/workflows/create-architecture.workflow.md` → `AVANADE_WORKFLOW_GUIDE_CREATE_ARCHITECTURE`
- `.avanade-method/workflows/create-ux.workflow.md` → `AVANADE_WORKFLOW_GUIDE_CREATE_UX`
- `.avanade-method/workflows/create-epics-stories.workflow.md` → `AVANADE_WORKFLOW_GUIDE_CREATE_EPICS_STORIES`
- `.avanade-method/workflows/sprint-planning.workflow.md` → `AVANADE_WORKFLOW_GUIDE_SPRINT_PLANNING`
- `.avanade-method/workflows/code-review.workflow.md` → `AVANADE_WORKFLOW_GUIDE_CODE_REVIEW`
- `.avanade-method/workflows/quick-dev.workflow.md` → `AVANADE_WORKFLOW_GUIDE_QUICK_DEV`
- `.avanade-method/workflows/workflow-manifest.md` → `AVANADE_WORKFLOW_MANIFEST`

### 2.4 – Workflows FTD/D365 ⭐ NOVO (GRUPO 5): CREATE
- `.avanade-method/workflows/d365-discovery.workflow.md` → `AVANADE_WORKFLOW_D365_DISCOVERY`
- `.avanade-method/workflows/d365-integration.workflow.md` → `AVANADE_WORKFLOW_D365_INTEGRATION`
- `.avanade-method/workflows/d365-plugin-dev.workflow.md` → `AVANADE_WORKFLOW_D365_PLUGIN_DEV`
- `.avanade-method/workflows/create-ftd-user-story.md` → `AVANADE_WORKFLOW_CREATE_FTD_USER_STORY`
- `.avanade-method/workflows/process-transcription.md` → `AVANADE_WORKFLOW_PROCESS_TRANSCRIPTION`
- `.avanade-method/workflows/validate-ftd-compliance.md` → `AVANADE_WORKFLOW_VALIDATE_FTD_COMPLIANCE`
- `.avanade-method/workflows/WF-001-full-inception.yaml` → `AVANADE_WORKFLOW_WF001_FULL_INCEPTION`
- `.avanade-method/workflows/WF-002-feature-development.yaml` → `AVANADE_WORKFLOW_WF002_FEATURE_DEVELOPMENT`
- `.avanade-method/workflows/WF-003-enterprise-delivery.yaml` → `AVANADE_WORKFLOW_WF003_ENTERPRISE_DELIVERY`
- `.avanade-method/workflows/WF-004-brownfield-fullstack.yaml` → `AVANADE_WORKFLOW_WF004_BROWNFIELD_FULLSTACK`
- `.avanade-method/workflows/WF-005-brownfield-service.yaml` → `AVANADE_WORKFLOW_WF005_BROWNFIELD_SERVICE`
- `.avanade-method/workflows/WF-006-brownfield-ui.yaml` → `AVANADE_WORKFLOW_WF006_BROWNFIELD_UI`
- `.avanade-method/workflows/WF-007-greenfield-fullstack.yaml` → `AVANADE_WORKFLOW_WF007_GREENFIELD_FULLSTACK`
- `.avanade-method/workflows/WF-008-greenfield-service.yaml` → `AVANADE_WORKFLOW_WF008_GREENFIELD_SERVICE`
- `.avanade-method/workflows/WF-009-greenfield-ui.yaml` → `AVANADE_WORKFLOW_WF009_GREENFIELD_UI`
- `.avanade-method/workflows/WF-04-processamento-documental.md` → `AVANADE_WORKFLOW_WF04_PROCESSAMENTO_DOCUMENTAL`
- `.avanade-method/workflows/WF-05-health-check.md` → `AVANADE_WORKFLOW_WF05_HEALTH_CHECK`
- `.avanade-method/workflows/WF-06-gerador-stories.md` → `AVANADE_WORKFLOW_WF06_GERADOR_STORIES`
- `.avanade-method/workflows/WF-07-conformidade-bca.md` → `AVANADE_WORKFLOW_WF07_CONFORMIDADE_BCA`
- `.avanade-method/workflows/WF-08-autodocumentacao-readme.md` → `AVANADE_WORKFLOW_WF08_AUTODOCUMENTACAO`

### 2.5 – Tasks (GRUPOS 6-7): UPDATE + CREATE
[Todos 27 arquivos listados nas tabelas do inventário acima]

### 2.6 – Checklists (GRUPOS 8-9): UPDATE + CREATE
[Todos 9 arquivos listados nas tabelas do inventário acima]

### 2.7 – Templates (GRUPOS 10-11): UPDATE + CREATE
[Todos 13 arquivos listados nas tabelas do inventário acima]

### 2.8 – Memory, Standards, Guides (GRUPOS 12-15): UPDATE + CREATE
[Todos arquivos listados nas tabelas do inventário acima]

### 2.9 – Personas ⭐ NOVA CATEGORIA (GRUPO 16): CREATE
[9 arquivos .avanade-method/personas/*.yaml]

### 2.10 – Agent Prompts ⭐ NOVA CATEGORIA (GRUPO 17): CREATE
[15 arquivos .avanade-method/prompts/**]

### 2.11 – Configs ⭐ NOVA CATEGORIA (GRUPO 18): CREATE
[6 arquivos .avanade-method/configs/*.yaml]

### 2.12 – GitHub Prompts + Instructions (GRUPOS 19-20): UPDATE
[4 prompts + copilot-instructions.md]

---

## PASSO 3: ATUALIZAR CHECKLIST PARA v2.2

1. Ler `.avanade-method/checklists/AVANADE_INSTALLATION_CHECKLIST2.md` completo
2. Criar `AVANADE_INSTALLATION_CHECKLIST_v2.2.md` com:
   - Header atualizado: versão v2.2, data 2026-03-25
   - Novidades: FTD/D365 específicos, Personas, Prompts, Configs
   - Adicionar TODOS os novos arquivos com ⭐ NEW
   - Atualizar tabela de resumo (151 arquivos totais)
   - Adicionar CHANGELOG v2.2
3. Sincronizar novo checklist: UPDATE em `AVANADE_INSTALLATION_CHECKLIST`

---

## PASSO 4: VALIDAÇÃO FINAL

Confirmar para TODOS os artefatos:
- ✅ `success: true`
- ✅ `production_version_id` presente  
- ✅ `accessibility: "Namespace"`
- ✅ Conteúdo idêntico ao arquivo local (mesmo nº de linhas)

**Relatório esperado**:
```
✅ ~87 artefatos ATUALIZADOS (UPDATE)
✅ ~64 artefatos CRIADOS (CREATE)
✅ 1 checklist atualizado para v2.2
Total: ~152 artefatos sincronizados
```
```

---

## 🔧 CONFIGURAÇÃO MCP (Verificar antes de executar)

**Arquivo**: `C:\Users\r.a.mendes.da.silva\AppData\Roaming\Code\User\mcp.json`
```json
{
  "servers": {
    "avanade-method": {
      "url": "https://mcp.br.avanade.ai/mcp",
      "type": "http"
    }
  }
}
```

**Status**: Configurado ✅  
**Tools disponíveis no chat principal**: `mcp_avanade-metho_search_artifacts`, `mcp_avanade-metho_create_artifact`, `mcp_avanade-metho_update_artifact`

---

*Gerado pelo Supervisor Agent — Análise de workspace em 2026-03-25*
