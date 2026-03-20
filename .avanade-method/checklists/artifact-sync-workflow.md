# 🔄 Avanade Method - Artifact Synchronization Workflow

**Versão**: 1.0  
**Data**: 2026-03-19  
**Propósito**: Workflow padronizado para sincronizar artefatos Avanade Method entre ambiente local (VSCode) e remoto (avanade.ai)

---

## 🎯 Objetivo

Este guia documenta o processo completo para:
1. Criar/atualizar artefatos localmente no VSCode
2. Sincronizar com o servidor MCP remoto (avanade.ai)
3. Atualizar o installation checklist
4. Manter consistência entre ambientes

**Princípio Fundamental**: 
**Ambiente Remoto (avanade.ai) = Espelho IDÊNTICO do Ambiente Local (VSCode)**

- Todo arquivo local deve ter conteúdo IDÊNTICO no servidor remoto
- Sem resumos, sem cortes, sem modificações
- Sincronização 1:1 (byte a byte)
- Garante que toda equipe tem acesso ao mesmo conteúdo completo

---

## 📋 Pré-requisitos

**Ferramentas necessárias**:
- ✅ VSCode com GitHub Copilot
- ✅ MCP Server configurado (`mcp.json`)
- ✅ Acesso ao servidor: `https://mcp-agent.br.avanade.ai/mcp`
- ✅ Arquivo `AVANADE_INSTALLATION_CHECKLIST.md` (versão atual)

**MCP Tools obrigatórias**:
- `mcp_avanade-metho_search_artifacts` - Buscar artefatos existentes
- `mcp_avanade-metho_create_artifact` - Criar novos artefatos
- `mcp_avanade-metho_update_artifact` - Atualizar artefatos existentes
- `read_file` - Ler conteúdo completo dos arquivos locais

---

## 🔐 REGRAS CRÍTICAS (OBRIGATÓRIAS)

### ⚠️ REGRA #1: Accessibility Level
```yaml
❌ ERRADO: accessibility: "NAMESPACE_USER"
✅ CORRETO: accessibility: "Namespace"
```
**Todos os artefatos DEVEM usar `"Namespace"` (camelCase) para acesso team-wide.**

### ⚠️ REGRA #2: Conteúdo Completo e Idêntico
```yaml
❌ ERRADO: Resumir, truncar ou modificar conteúdo
✅ CORRETO: Usar conteúdo COMPLETO E IDÊNTICO do arquivo local
```
**CRÍTICO - Sincronização 1:1**:
- ❌ NUNCA resumir artefatos
- ❌ NUNCA cortar partes do conteúdo
- ❌ NUNCA modificar o conteúdo original
- ✅ SEMPRE ler o arquivo local COMPLETO
- ✅ SEMPRE enviar o conteúdo INTEGRAL
- ✅ **Conteúdo remoto = Conteúdo local (idênticos, byte a byte)**

**Por que isso é crítico?**
- O ambiente remoto (avanade.ai) deve ser espelho fiel do ambiente local (VSCode)
- Equipe precisa ter acesso ao conteúdo COMPLETO dos artefatos
- Resumos causam perda de informação e inconsistências
- Chatmode.md com ~9-10KB é esperado, não ~1-2KB

### ⚠️ REGRA #3: Promote to Production
```yaml
✅ SEMPRE: promote_to_production: true
```
**Todos os artefatos devem ser promovidos para produção imediatamente.**

### ⚠️ REGRA #4: Artifact Type
```yaml
✅ SEMPRE: type: "string"
```
**Tanto arquivos YAML quanto Markdown usam `type: "string"` (não existe type: "YAML").**

---

## 🚀 WORKFLOW COMPLETO

### **FASE 1: Preparação (5 min)**

#### 1.1 Obter Versão Atual do Checklist

**Prompt para Copilot**:
```
Por favor, leia o arquivo AVANADE_INSTALLATION_CHECKLIST.md mais recente 
disponível localmente ou busque no servidor remoto se necessário.

Identifique:
- Versão atual (ex: v2.1, v2.2)
- Número total de agentes
- Número total de arquivos
```

**Saída esperada**: 
- Versão identificada: `v2.X`
- Total de agentes: `N`
- Total de arquivos: `M`

---

### **FASE 2: Criação/Atualização Local (15-30 min)**

#### 2.1 Criar ou Atualizar Arquivos Localmente

**Para NOVOS agentes/componentes**:
```
Estrutura obrigatória:
├── .github/agents/
│   └── {nome-agente}.chatmode.md    (Definição completa do agente Copilot)
│
└── .avanade-method/agents/
    └── {nome-agente}.customize.yaml (Customizações YAML do agente)
```

**Para ATUALIZAÇÕES de agentes existentes**:
```
Apenas modificar os arquivos existentes:
- .avanade-method/agents/{agente}.customize.yaml
- .github/agents/{agente}.chatmode.md (se necessário)
```

#### 2.2 Validar Sintaticamente os Arquivos

**Validação YAML**:
```powershell
# Testar se YAML é válido
Get-Content ".avanade-method/agents/{agente}.customize.yaml" | ConvertFrom-Yaml
```

**Validação Markdown**:
```powershell
# Verificar que arquivo não está vazio e tem estrutura XML (para chatmodes)
$content = Get-Content ".github/agents/{agente}.chatmode.md" -Raw
$content.Length -gt 1000  # Deve ter conteúdo substancial
```

---

### **FASE 3: Sincronização Remota (20-40 min)**

#### 3.1 Buscar Artefatos Existentes no Servidor

**Prompt para Copilot**:
```
Busque no servidor MCP os seguintes artefatos usando mcp_avanade-metho_search_artifacts:

Para NOVOS agentes (exemplo: "lucas-security"):
1. AVANADE_AGENT_LUCAS_SECURITY_CUSTOMIZE_YAML
2. AVANADE_SECURITY_LUCAS_PROMPT_MD

Para agentes EXISTENTES (exemplo: "carla-qa"):
1. AVANADE_AGENT_CARLA_QA_CUSTOMIZE_YAML
2. AVANADE_QA_CARLA_PROMPT_MD

Documente os IDs encontrados para UPDATE ou confirme que não existem para CREATE.
```

**Resultado esperado**:
```yaml
Artefatos encontrados (UPDATE):
- AVANADE_AGENT_CARLA_QA_CUSTOMIZE_YAML: ID dea05a3e-xxxx
- AVANADE_QA_CARLA_PROMPT_MD: ID 404c0825-xxxx

Artefatos NÃO encontrados (CREATE):
- AVANADE_AGENT_LUCAS_SECURITY_CUSTOMIZE_YAML
- AVANADE_SECURITY_LUCAS_PROMPT_MD
```

---

#### 3.2 Sincronizar Artefatos (UPDATE)

**Para artefatos que JÁ EXISTEM no servidor:**

**Prompt para Copilot**:
```
Para cada artefato EXISTENTE:

1. Leia o conteúdo COMPLETO do arquivo local usando read_file
2. Execute mcp_avanade-metho_update_artifact com:
   - id: "{ID_DO_ARTEFATO}" (obtido na busca)
   - value: "{CONTEÚDO_COMPLETO_DO_ARQUIVO}"
   - promote_to_production: true

Exemplo para carla-qa.customize.yaml:
- Arquivo local: .avanade-method/agents/carla-qa.customize.yaml
- Artifact ID: dea05a3e-xxxx (já existe)
- Ler arquivo completo (todas as linhas)
- Update com conteúdo completo + promote to production

CRÍTICO - CONTEÚDO IDÊNTICO:
- ❌ NÃO resumir o conteúdo
- ❌ NÃO cortar partes
- ❌ NÃO modificar nada
- ✅ Use o arquivo local INTEIRO
- ✅ Conteúdo remoto = Conteúdo local (byte a byte)
- ✅ Mesmo número de linhas, mesmo tamanho em KB
```

**Template do comando**:
```javascript
mcp_avanade-metho_update_artifact(
  id: "{artifact_id}",
  value: "{conteúdo_completo_lido_do_arquivo_local}",
  promote_to_production: true
)
```

---

#### 3.3 Criar Novos Artefatos (CREATE)

**Para artefatos que NÃO EXISTEM no servidor:**

**Prompt para Copilot**:
```
Para cada artefato NOVO:

1. Leia o conteúdo COMPLETO do arquivo local usando read_file
2. Execute mcp_avanade-metho_create_artifact com:
   - key: "{ARTIFACT_KEY}" (padrão de nomenclatura do checklist)
   - value: "{CONTEÚDO_COMPLETO_DO_ARQUIVO}"
   - type: "string"
   - accessibility: "Namespace" (camelCase!)
   - promote_to_production: true

Exemplo para lucas-security.customize.yaml:
- Arquivo local: .avanade-method/agents/lucas-security.customize.yaml
- Artifact Key: AVANADE_AGENT_LUCAS_SECURITY_CUSTOMIZE_YAML
- Type: "string"
- Accessibility: "Namespace" (⚠️ ATENÇÃO: camelCase!)
- Ler arquivo completo (todas as linhas)
- Create com conteúdo completo + promote to production

CRÍTICO - CONTEÚDO IDÊNTICO:
- ❌ Accessibility DEVE ser "Namespace" (não "NAMESPACE_USER")
- ❌ NÃO resumir o conteúdo
- ❌ NÃO cortar partes
- ❌ NÃO modificar nada
- ✅ Use o arquivo local INTEIRO
- ✅ Conteúdo remoto deve ser IDÊNTICO ao local
- ✅ Verifique: mesmo tamanho em KB e número de linhas
```

**Template do comando**:
```javascript
mcp_avanade-metho_create_artifact(
  key: "{ARTIFACT_KEY}",
  value: "{conteúdo_completo_lido_do_arquivo_local}",
  type: "string",
  accessibility: "Namespace",  // ⚠️ CamelCase!
  promote_to_production: true
)
```

---

#### 3.4 Verificar Sucessos

**Prompt para Copilot**:
```
Após todas as operações de CREATE/UPDATE:

1. Liste todos os artefatos sincronizados
2. Confirme IDs retornados (para CREATE) ou production_version_id (para UPDATE)
3. Valide que TODOS retornaram success: true
4. Documente qualquer falha para correção

Formato esperado:
✅ AVANADE_AGENT_LUCAS_SECURITY_CUSTOMIZE_YAML: Created (ID: 2c7a4d43-xxxx)
✅ AVANADE_SECURITY_LUCAS_PROMPT_MD: Created (ID: 60be4a21-xxxx, prod: 2bee494b-xxxx)
✅ AVANADE_AGENT_CARLA_QA_CUSTOMIZE_YAML: Updated (prod v2: abc123...)
```

---

### **FASE 4: Atualização do Checklist (10-15 min)**

#### 4.1 Atualizar Checklist Localmente

**Prompt para Copilot**:
```
Atualizar o arquivo AVANADE_INSTALLATION_CHECKLIST.md (versão atual) para nova versão:

REGRAS CRÍTICAS:
1. ❌ NÃO RESUMIR o checklist
2. ✅ Fazer CÓPIA COMPLETA da versão atual
3. ✅ Adicionar APENAS os novos arquivos/agentes
4. ✅ Incrementar número da versão (ex: v2.1 → v2.2)
5. ✅ Atualizar contagens:
   - Total de agentes
   - Total de arquivos
   - Total por categoria

SEÇÕES A ATUALIZAR:
1. Cabeçalho: versão, data, novidades
2. Estrutura de Diretórios: adicionar novos arquivos com ⭐ NEW
3. Resumo da Instalação: atualizar contagens
4. Seção 1.2 (Agentes Copilot): adicionar linhas para novos agentes
5. Seção 1.8 (Agent Customization): adicionar linhas para novos agents
6. Seção 1.1 (MCP Config): adicionar novos agent IDs
7. Fase 2 (Testes): adicionar comandos de teste para novos agentes
8. Validação: atualizar comandos PowerShell (ex: Count -eq 13)
9. CHANGELOG: documentar mudanças

EXEMPLO (v2.1 → v2.2 com 3 novos agentes):
- Versão: 2.1 → 2.2
- Agentes: 10 → 13 (+3)
- Arquivos: 109 → 115 (+6)
- Novidades: "3 Novos Agentes Especializados (Lucas Security, Pedro DevOps, Ana Data)"

Mantenha TODA a estrutura original. Apenas incremente o necessário.
```

**Resultado esperado**:
- Novo arquivo: `AVANADE_INSTALLATION_CHECKLIST{versão}.md`
- Estrutura completa mantida
- Novos componentes adicionados com marcador ⭐ NEW
- Contagens atualizadas
- CHANGELOG documentado

---

#### 4.2 Sincronizar Checklist com Servidor Remoto

**Prompt para Copilot**:
```
Sincronizar o AVANADE_INSTALLATION_CHECKLIST atualizado com o servidor:

1. Buscar artefato existente:
   - Key: AVANADE_INSTALLATION_CHECKLIST
   - Obter ID atual

2. Ler o arquivo LOCAL completo:
   - Arquivo: AVANADE_INSTALLATION_CHECKLIST{versão}.md
   - Ler TODAS as linhas (não resumir)

3. Executar UPDATE:
   mcp_avanade-metho_update_artifact(
     id: "{id_do_checklist}",
     value: "{conteúdo_completo_do_checklist_atualizado}",
     promote_to_production: true
   )

4. Verificar sucesso e production_version_id retornado

CRÍTICO: O checklist DEVE ter conteúdo completo (tipicamente 500-800+ linhas).
Se o upload tiver menos de 300 linhas, algo está errado - NÃO prosseguir.
```

---

### **FASE 5: Validação Final (10 min)**

#### 5.1 Checklist de Validação

**Prompt para Copilot**:
```
Executar validação completa da sincronização:

VALIDAÇÃO DE ARTEFATOS:
✅ Todos os artefatos retornaram success: true?
✅ Todos possuem production_version_id?
✅ Accessibility de todos é "Namespace"?
✅ Nenhum artefato foi resumido/truncado?

VALIDAÇÃO DO CHECKLIST:
✅ Versão incrementada corretamente?
✅ Contagens de arquivos/agentes atualizadas?
✅ Novos componentes marcados com ⭐ NEW?
✅ CHANGELOG completo?
✅ Estrutura completa mantida (não resumida)?

VALIDAÇÃO REMOTA:
✅ Checklist atualizado no servidor com success: true?
✅ Production version ID retornado?

Se QUALQUER item falhar, corrigir antes de considerar concluído.
```

#### 5.2 Documentar Resultados

**Criar resumo final**:
```markdown
## Sincronização Concluída - v{versão}

**Data**: {data}
**Versão anterior**: v{X}
**Nova versão**: v{Y}

### Artefatos Sincronizados:
- ✅ {N} artefatos atualizados (UPDATE)
- ✅ {M} artefatos criados (CREATE)
- ✅ 1 checklist atualizado

### Novos Componentes:
- {lista de novos agentes/componentes}

### IDs dos Novos Artefatos:
- {Artifact Key}: {ID}
- ...

### Production Version IDs:
- {Artifact}: {prod_version_id}
- ...

### Validação:
✅ Accessibility: Namespace
✅ Conteúdo: Completo
✅ Production: Promovido
✅ Checklist: Atualizado e completo
```

---

## 📋 TEMPLATE DE PROMPT REUTILIZÁVEL

Use este template para futuras sincronizações:

```markdown
# SINCRONIZAÇÃO AVANADE METHOD - v{nova_versão}

## CONTEXTO
Estou atualizando o Avanade Method de v{atual} para v{nova}.

Novos componentes a adicionar:
- {lista de novos agentes/componentes}

Componentes existentes a atualizar:
- {lista de componentes a atualizar}

## INSTRUÇÕES PARA COPILOT

### FASE 1: Leitura (OBRIGATÓRIA)
1. Leia o arquivo AVANADE_INSTALLATION_CHECKLIST.md atual
2. Identifique versão, total de agentes, total de arquivos
3. Confirme estrutura comigo antes de prosseguir

### FASE 2: Busca de Artefatos Existentes (OBRIGATÓRIA)
Use mcp_avanade-metho_search_artifacts para buscar:

**Para componentes NOVOS**:
{Liste os artifact keys esperados baseados no padrão}

**Para componentes EXISTENTES**:
{Liste os artifact keys a serem atualizados}

Documente quais existem (UPDATE) e quais não existem (CREATE).

### FASE 3: Sincronização Remota (CRÍTICO)

**REGRAS OBRIGATÓRIAS**:
- ✅ Accessibility: "Namespace" (camelCase)
- ✅ Conteúdo COMPLETO (read_file para ler arquivo inteiro)
- ✅ promote_to_production: true
- ✅ type: "string" (para YAML e MD)

**Para cada artefato existente (UPDATE)**:
1. Ler arquivo local completo com read_file
2. Executar update_artifact com ID + conteúdo completo
3. Verificar success: true

**Para cada artefato novo (CREATE)**:
1. Ler arquivo local completo com read_file
2. Executar create_artifact com:
   - key: {ARTIFACT_KEY}
   - value: {conteúdo completo}
   - type: "string"
   - accessibility: "Namespace"
   - promote_to_production: true
3. Verificar success: true e guardar ID

### FASE 4: Atualização do Checklist (CRÍTICO)

1. Criar nova versão do checklist:
   - ❌ NÃO RESUMIR
   - ✅ CÓPIA COMPLETA da versão atual
   - ✅ Adicionar novos componentes com ⭐ NEW
   - ✅ Atualizar contagens
   - ✅ Incrementar versão
   - ✅ Documentar CHANGELOG

2. Sincronizar checklist com servidor:
   - Buscar ID de AVANADE_INSTALLATION_CHECKLIST
   - Ler checklist completo (todas as linhas)
   - UPDATE com conteúdo completo
   - promote_to_production: true

### FASE 5: Validação (OBRIGATÓRIA)

Validar:
- ✅ Todos artefatos success: true
- ✅ Todos com accessibility "Namespace"
- ✅ Checklist atualizado no servidor
- ✅ Contagens corretas
- ✅ Estrutura completa mantida

Fornecer relatório final com IDs e production version IDs.
```

---

## 🔍 TROUBLESHOOTING

### Erro: "accessibility" validation failed

**Causa**: Usado `"NAMESPACE_USER"` ou `"NAMESPACE"` (uppercase)  
**Solução**: Usar `"Namespace"` (camelCase)

```yaml
❌ "NAMESPACE_USER" ou "NAMESPACE"
✅ "Namespace"
```

---

### Erro: Artefato criado mas conteúdo truncado

**Causa**: Conteúdo foi resumido em vez de usar arquivo completo  
**Solução**: 
1. Re-ler arquivo local com `read_file` (todas as linhas)
2. Executar `update_artifact` com conteúdo completo
3. Verificar que production_version_id mudou

```javascript
// ❌ ERRADO - resumido
value: "# Agent\nSummary of content..."

// ✅ CORRETO - completo
read_file(path, startLine: 1, endLine: 500)  // Ler tudo
value: {conteúdo_completo_retornado}
```

---

### Erro: Checklist com estrutura resumida

**Causa**: Novo checklist foi criado do zero em vez de copiar o anterior  
**Solução**:
1. Ler checklist v{anterior} COMPLETO
2. Fazer replace apenas das seções que mudam
3. Usar `replace_string_in_file` ou `multi_replace_string_in_file`
4. NÃO criar novo arquivo do zero

---

### Erro: Type "YAML" não aceito

**Causa**: Tentou usar `type: "YAML"` que não existe  
**Solução**: Usar `type: "string"` para TODOS os arquivos (YAML e MD)

```yaml
✅ type: "string"  # Para .yaml e .md
```

---

## 📊 HISTÓRICO DE VERSÕES

### v2.1 → v2.2 (2026-03-19)
- **Adicionado**: 3 novos agentes especializados
  - Lucas Security (Security Specialist - OWASP, STRIDE)
  - Pedro DevOps (DevOps Engineer - CI/CD, IaC)
  - Ana Data (Data Engineer - Synapse, Medallion Architecture)
- **Arquivos**: 109 → 115 (+6)
- **Agentes**: 10 → 13 (+3)
- **Lições aprendidas**:
  - Dois erros corrigidos: accessibility (NAMESPACE_USER → Namespace) e conteúdo (resumido → completo)
  - Checklist foi recriado resumido, depois corrigido com estrutura completa da v2.1

---

## 🎓 LIÇÕES APRENDIDAS

1. **NUNCA assumir valores de enums** - Sempre testar ou consultar documentação
   - `accessibility` usa camelCase: `"Namespace"` não `"NAMESPACE"`

2. **NUNCA resumir artefatos** - Conteúdo deve ser completo
   - Ler arquivo completo com `read_file`
   - Verificar tamanho antes do upload (chatmodes ~9-10KB)

3. **NUNCA resumir o checklist** - Fazer cópia completa + adições
   - Usar versão anterior como base
   - Adicionar apenas deltas necessários

4. **SEMPRE validar antes de prosseguir** - Confirmar success em cada etapa
   - Buscar artefatos antes de decidir CREATE vs UPDATE
   - Verificar IDs retornados
   - Confirmar production promotion

5. **Documentar tudo** - Próxima sincronização será mais fácil
   - Guardar artifact IDs
   - Documentar production version IDs
   - Manter histórico de mudanças

---

## 🤝 SUPORTE

**Dúvidas ou problemas**:
- Consulte este guide completo
- Revise as REGRAS CRÍTICAS no início
- Valide accessibility, type e promote_to_production

**Documentação MCP Server**:
- URL: https://mcp-agent.br.avanade.ai/mcp
- Tools: search_artifacts, create_artifact, update_artifact

---

**Documento**: `artifact-sync-workflow.md`  
**Versão**: 1.0  
**Criado**: 2026-03-19  
**Owner**: Avanade Method Team  
**Última Atualização**: 2026-03-19
