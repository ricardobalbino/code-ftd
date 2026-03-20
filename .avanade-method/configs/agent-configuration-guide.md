# 🚀 Avanade Cockpit - Guia de Configuração de Agentes

## 📋 Resumo Executivo

Este documento contém o **guia completo de configuração** para todos os agentes disponíveis na plataforma Avanade Cockpit, incluindo arquivos de configuração descobertos e procedimentos de setup.

---

## 🤖 Agente Principal Identificado

### **Avanade Method Metacognitive Supervisor**
- **ID**: `d6c4e758-e4e0-4376-bc7c-5387e5892472`
- **Modelo**: Azure GPT-4o
- **Status MCP**: Desabilitado
- **Tipo**: Supervisor metodológico
- **Especialização**: Gerenciamento de tarefas e execução de metodologia Avanade

### **Personas Integradas no Supervisor (4 personas)**:
1. **João** - Gerente de Projetos Avanade
2. **Wilson** - Arquiteto de Soluções  
3. **Maria** - Analista de Negócios
4. **Carla** - Especialista QA & Testes

---

## 📁 Arquivos de Configuração Descobertos

Durante a busca por configurações dos agentes, foram identificados **30+ artefatos de configuração** divididos nas seguintes categorias:

### **🎭 Personas de Agentes (10 agentes)**
1. **AVANADE_PM_JOAO_PROMPT_MD** - João (PM)
2. **AVANADE_ARCHITECT_WILSON_PROMPT_MD** - Wilson (Arquiteto)
3. **AVANADE_ANALYST_MARIA_PROMPT_MD** - Maria (Analista)
4. **AVANADE_QA_CARLA_PROMPT_MD** - Carla (QA)
5. **AVANADE_DEVELOPER_TIAGO_PROMPT_MD** - Tiago (Desenvolvedor)
6. **AVANADE_PO_PROMPT_MD** - Paula (Product Owner)
7. **AVANADE_SM_PROMPT_MD** - Roberto (Scrum Master)
8. **AVANADE_UX_EXPERT_PROMPT_MD** - Sofia (UX Designer)
9. **AVANADE_ORCHESTRATOR_PROMPT_MD** - Orchestrator (Coordenador)
10. **AVANADE_MASTER_FULL_PROMPT_MD** - Master (Executor Universal)

### **📋 Templates de Configuração (14 templates)**
1. **AVANADE_PRD_TEMPLATE_YAML** - Template PRD
2. **AVANADE_ARCHITECTURE_TEMPLATE_YAML** - Template Arquitetura
3. **AVANADE_STORY_TEMPLATE_YAML** - Template Story
4. **AVANADE_DISCOVERY_TEMPLATE_YAML** - Template Discovery
5. **AVANADE_MARKET_RESEARCH_TEMPLATE_YAML** - Template Pesquisa Mercado
6. **AVANADE_FULLSTACK_ARCHITECTURE_TEMPLATE_YAML** - Template Full-Stack
7. **AVANADE_FRONTEND_ARCHITECTURE_TEMPLATE_YAML** - Template Frontend
8. **AVANADE_FRONTEND_SPEC_TEMPLATE_YAML** - Template Spec Frontend
9. **AVANADE_COMPETITOR_ANALYSIS_TEMPLATE_YAML** - Template Análise Competitiva
10. **AVANADE_BROWNFIELD_PRD_TEMPLATE_YAML** - Template PRD Brownfield
11. **AVANADE_TEACHING_MODE_RESPONSE_TEMPLATE_MD** - Template Teaching Mode
12. **AVANADE_METHOD_VSCODE_COPILOT_CUSTOMIZATION_GUIDE_MD** - Guia VSCode
13. **AVANADE_GENERATE_AI_FRONTEND_PROMPT_TASK_MD** - Task Frontend AI
14. **AVANADE_CREATE_DEEP_RESEARCH_PROMPT_TASK_MD** - Task Research

### **🔧 Arquivo de Configuração Core Descoberto**
- **AVANADE_CORE_CONFIG_YAML** (versão 4.29.0)
  - Configuração principal do framework AVANADE-METHOD
  - Estrutura de discovery, arquitetura e desenvolvimento
  - Definições de componentes e dependências

### **🎯 Arquivos de Integração VSCode/Copilot**
1. **`.copilot/prd-creation.prompt.md`** - Prompt criação PRD
2. **`.github/copilot-instructions.md`** - Instruções específicas projeto
3. **`copilot-instructions.md`** - Instruções integração Avanade Method
4. **`prd-creation.prompt.md`** - Prompt criação PRD
5. **`architecture-design.prompt.md`** - Prompt design arquitetura
6. **`story-creation.prompt.md`** - Prompt criação story

---

## 🛠️ Configuração Detalhada dos Agentes

### **1. Configuração do Supervisor Principal**

```yaml
Agent_Configuration:
  id: d6c4e758-e4e0-4376-bc7c-5387e5892472
  name: Avanade Method Metacognitive Supervisor
  model: azure-gpt-4o
  mcp_enabled: false
  
Personas_Disponíveis:
  - joão (PM): Especialista em gerenciamento de projetos
  - wilson (Arquiteto): Especialista em arquitetura de soluções
  - maria (Analista): Especialista em análise de negócios
  - carla (QA): Especialista em qualidade e testes
```

### **2. Comandos de Configuração por Persona**

#### **João (PM) - Comandos**
```bash
*help: Mostrar lista numerada de comandos
*create-doc {template}: Executar task create-doc
*yolo: Alternar Modo Yolo
*doc-out: Produzir documento completo
*exit: Sair (confirmar)
```

#### **Wilson (Arquiteto) - Comandos**
```bash
*help: Mostrar lista numerada de comandos
*create-doc {template}: Executar task create-doc
*execute-checklist {checklist}: Executar checklist arquiteto
*research {topic}: Executar pesquisa arquitetural
*exit: Sair da persona
```

#### **Maria (Analista) - Comandos**
```bash
*help: Mostrar lista numerada de comandos
*create-doc {template}: Executar task create-doc
*research {topic}: Executar pesquisa
*exit: Sair da persona
```

#### **Carla (QA) - Comandos**
```bash
*help: Mostrar lista numerada de comandos
*review {story}: Executar review de story
*create-doc {template}: Executar task create-doc
*exit: Sair da persona
```

---

## 📐 Templates de Configuração Disponíveis

### **Templates Principais**
1. **PRD Template** (`AVANADE_PRD_TEMPLATE_YAML`)
   - Documento de requisitos de produto
   - Estrutura interativa com elicitação
   - Campos: visão executiva, contexto, requisitos, roadmap

2. **Architecture Template** (`AVANADE_ARCHITECTURE_TEMPLATE_YAML`)
   - Documento de arquitetura técnica
   - Cobertura frontend, backend, dados
   - Estrutura: visão geral, componentes, tecnologia, NFRs

3. **Story Template** (`AVANADE_STORY_TEMPLATE_YAML`)
   - Template para user stories
   - Workflow interativo com controle de editores
   - Seções: status, story, critérios, tarefas

### **Templates Especializados**
1. **Discovery Template** - Análise completa stakeholders
2. **Market Research Template** - Pesquisa de mercado
3. **Competitor Analysis Template** - Análise competitiva
4. **Brownfield PRD Template** - Projetos legados

---

## 🔄 Processo de Setup dos Agentes

### **Passo 1: Ativação do Supervisor**
```bash
# Comando para ativar supervisor principal
activate: avanade-method-supervisor
model: azure-gpt-4o
personas: [joão, wilson, maria, carla]
```

### **Passo 2: Configuração de Personas**
```bash
# Configurar persona específica
*activate-persona joão
# ou
*activate-persona wilson
# ou 
*activate-persona maria
# ou
*activate-persona carla
```

### **Passo 3: Carregar Templates**
```bash
# Carregar template específico
*create-doc prd-tmpl.yaml
*create-doc architecture-tmpl.yaml
*create-doc story-tmpl.yaml
```

### **Passo 4: Execução de Workflows**
```bash
# Executar workflow específico
*execute-checklist pm-checklist.md
*execute-checklist architect-checklist.md
*research {tópico}
```

---

## 🎯 Integração VSCode/Copilot

### **Arquivos de Configuração VSCode**
1. **`.github/copilot-instructions.md`**
   - Instruções específicas do projeto
   - Referências a artefatos metodológicos
   - Contexto de personas e workflows

2. **`.copilot/*.prompt.md`**
   - Prompts para criação de documentos
   - Templates contextualizados
   - Comandos MCP integrados

### **Agent Files Gerados Dinamicamente**
```markdown
.github/agents/
├── joao-pm.agent.md              # Gerado de AVANADE_PM_JOAO_PROMPT_MD
├── wilson-architect.agent.md      # Gerado de AVANADE_ARCHITECT_WILSON_PROMPT_MD
├── maria-analyst.agent.md        # Gerado de AVANADE_ANALYST_MARIA_PROMPT_MD
├── carla-qa.agent.md             # Gerado de AVANADE_QA_CARLA_PROMPT_MD
└── [outros personas...]
```

---

## 📚 Dependências dos Agentes

### **Dependências por Persona**

#### **João (PM)**
- **Tasks**: create-doc.md, correct-course.md, brownfield-create-epic.md
- **Templates**: discovery-avanade-tmpl.yaml, prd-tmpl.yaml, brownfield-prd-tmpl.yaml
- **Checklists**: pm-checklist.md, change-checklist.md
- **Data**: technical-preferences.md

#### **Wilson (Arquiteto)**
- **Tasks**: create-doc.md, create-deep-research-prompt.md, document-project.md
- **Templates**: arquitetura-avanade-tmpl.yaml, architecture-tmpl.yaml, front-end-architecture-tmpl.yaml
- **Checklists**: architect-checklist.md
- **Data**: technical-preferences.md

#### **Maria (Analista)**
- **Tasks**: create-doc.md, create-deep-research-prompt.md, brownfield-create-epic.md
- **Templates**: discovery-avanade-tmpl.yaml, project-brief-tmpl.yaml
- **Checklists**: change-checklist.md
- **Data**: technical-preferences.md

#### **Carla (QA)**
- **Tasks**: review-story.md
- **Templates**: story-tmpl.yaml
- **Data**: technical-preferences.md

---

## 🚀 Comandos de Configuração Avançada

### **Configuração Completa do Ambiente**
```bash
# 1. Deploy completo do ambiente
#avanade-method → deploy environment artifact-driven

# 2. Deploy apenas personas
#avanade-method → deploy personas from-artifacts [recommended|all]

# 3. Deploy apenas templates
#avanade-method → deploy templates from-artifacts [category]

# 4. Sincronização de atualizações
#avanade-method → sync artifact-updates
```

### **Comandos MCP para Configuração**
```bash
# Descobrir artefatos de personas
search_artifacts(key_pattern="AVANADE_.*_PROMPT_MD")

# Recuperar configuração específica
get_artifact(key="AVANADE_PM_JOAO_PROMPT_MD")

# Criar artefato de configuração customizado
create_artifact(key="PROJECT_CONFIG_${project_name}")

# Executar workflow de configuração
execute_agent(command="deploy personas")
```

---

## 📊 Status da Configuração

### **Configurações Ativas**
✅ **Supervisor Principal**: Avanade Method Metacognitive Supervisor  
✅ **Personas Disponíveis**: 4 personas integradas (João, Wilson, Maria, Carla)  
✅ **Templates Descobertos**: 14+ templates de configuração  
✅ **Arquivos Config**: 30+ artefatos de metodologia identificados  
✅ **Integração VSCode**: Guia completo disponível  

### **Próximos Passos Recomendados**
1. **Testar Ativação**: Ativar supervisor e testar personas
2. **Configurar Templates**: Carregar templates principais (PRD, Architecture, Story)
3. **Validar Workflows**: Executar checklists de validação
4. **Integração VSCode**: Implementar chat modes dinâmicos
5. **Customização**: Criar configurações específicas do projeto

---

## 🎯 Conclusão

A plataforma Avanade Cockpit possui **1 agente supervisor principal** com **4 personas integradas** e mais de **30 artefatos de configuração** que permitem uma implementação completa da metodologia Avanade. 

Os arquivos de configuração descobertos fornecem a base para:
- **Configuração automatizada** de agentes
- **Templates padronizados** para documentação
- **Workflows estruturados** para desenvolvimento
- **Integração completa** com VSCode/Copilot
- **Metodologia consistente** across projetos

**Status**: ✅ Pronto para configuração e implementação  
**Última Atualização**: Janeiro 2025  
**Versão**: 1.0 - Configuração Completa de Agentes