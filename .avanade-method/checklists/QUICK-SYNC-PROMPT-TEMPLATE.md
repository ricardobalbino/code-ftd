# 🚀 QUICK SYNC PROMPT - Template Reutilizável

**Use este template para sincronizar artefatos Avanade Method rapidamente**

---

## 📋 TEMPLATE - Copie e cole no Copilot

```markdown
# SINCRONIZAÇÃO AVANADE METHOD - v{NOVA_VERSÃO}

## 📦 COMPONENTES A SINCRONIZAR

**Novos componentes** (CREATE):
- [ ] {nome-agente-1}: .github/agents/{nome}.chatmode.md + .avanade-method/agents/{nome}.customize.yaml
- [ ] {nome-agente-2}: ...
- [ ] {nome-componente}: ...

**Componentes existentes** (UPDATE):
- [ ] {agente-existente}: atualizar .avanade-method/agents/{nome}.customize.yaml
- [ ] {componente-existente}: ...

**Versão do checklist base**: v{VERSÃO_ATUAL}  
**Nova versão do checklist**: v{NOVA_VERSÃO}

---

## 🎯 INSTRUÇÕES PARA COPILOT

### ⚠️ REGRAS CRÍTICAS (OBRIGATÓRIAS)

Antes de executar QUALQUER operação, confirme:
1. ✅ `accessibility: "Namespace"` (camelCase, NÃO "NAMESPACE_USER")
2. ✅ `type: "string"` (para YAML e MD)
3. ✅ `promote_to_production: true` (sempre)
4. ✅ **Conteúdo COMPLETO e IDÊNTICO** dos arquivos locais:
   - ❌ NÃO resumir
   - ❌ NÃO cortar partes
   - ❌ NÃO modificar
   - ✅ read_file do arquivo LOCAL inteiro
   - ✅ **Conteúdo remoto = Conteúdo local (idênticos)**

---

### PASSO 1: Leitura e Análise (5 min)

Por favor:
1. Leia o arquivo `AVANADE_INSTALLATION_CHECKLIST.md` (versão atual)
2. Identifique:
   - Versão atual: v_____
   - Total de agentes: _____
   - Total de arquivos: _____
3. Confirme comigo antes de prosseguir

---

### PASSO 2: Buscar Artefatos Existentes (5 min)

Use `mcp_avanade-metho_search_artifacts` para buscar:

**Novos componentes** (espera-se NÃO encontrar):
- `AVANADE_AGENT_{NOME}_CUSTOMIZE_YAML`
- `AVANADE_{ESPECIALIDADE}_{NOME}_PROMPT_MD`

**Componentes existentes** (espera-se encontrar):
- `AVANADE_AGENT_{NOME}_CUSTOMIZE_YAML`
- `AVANADE_{ESPECIALIDADE}_{NOME}_PROMPT_MD`

Documente:
- ✅ Encontrados (UPDATE): {artifact_key} → ID: {id}
- ❌ Não encontrados (CREATE): {artifact_key}

---

### PASSO 3: Sincronizar Artefatos (15-30 min)

**Para cada artefato EXISTENTE (UPDATE)**:

1. Ler arquivo local COMPLETO:
   ```
   read_file(
     filePath: ".avanade-method/agents/{nome}.customize.yaml",
     startLine: 1,
     endLine: 500  # Ajustar conforme necessário
   )
   ```

2. Executar UPDATE:
   ```
   mcp_avanade-metho_update_artifact(
     id: "{id_encontrado_na_busca}",
     value: "{conteúdo_completo_do_arquivo}",
     promote_to_production: true
   )
   ```
   **CRÍTICO**: `value` deve conter conteúdo IDÊNTICO ao arquivo local
   - Mesmo número de linhas
   - Mesmo tamanho em bytes
   - Sem resumos, cortes ou modificações

3. Verificar resposta: `success: true` e `production_version_id` retornado

---

**Para cada artefato NOVO (CREATE)**:

1. Ler arquivo local COMPLETO:
   ```
   read_file(
     filePath: ".github/agents/{nome}.chatmode.md",
     startLine: 1,
     endLine: 500  # Ajustar conforme necessário
   )
   ```

2. Executar CREATE:
   ```
   mcp_avanade-metho_create_artifact(
     key: "AVANADE_{PATTERN}_KEY",
     value: "{conteúdo_completo_do_arquivo}",
     type: "string",
     accessibility: "Namespace",  # ⚠️ CamelCase!
     promote_to_production: true
   )
   ```
   **CRÍTICO**: `value` deve conter conteúdo IDÊNTICO ao arquivo local
   - Para chatmode.md: espera-se ~9-10KB (~500 linhas)
   - Para customize.yaml: espera-se tamanho real do arquivo
   - Sem resumos, cortes ou modificações
   - **Ambiente remoto = Espelho do ambiente local**

3. Verificar resposta: `success: true` e guardar `id` retornado

---

### PASSO 4: Atualizar Checklist (10 min)

**CRÍTICO: NÃO RESUMIR O CHECKLIST**

1. Ler checklist atual COMPLETO (todas as linhas)

2. Criar nova versão fazendo:
   - ✅ CÓPIA COMPLETA da versão atual
   - ✅ Atualizar header (versão, data, novidades)
   - ✅ Adicionar novos arquivos na estrutura de diretórios com ⭐ NEW
   - ✅ Atualizar contagens:
     - Resumo da Instalação (tabela)
     - Seção 1.1 (MCP Config - agent IDs)
     - Seção 1.2 (Agentes Copilot - tabela)
     - Seção 1.8 (Agent Customization - tabela)
     - Validação (comandos PowerShell Count)
   - ✅ Adicionar seção CHANGELOG v{nova}

3. Salvar como: `AVANADE_INSTALLATION_CHECKLIST{nova_versão}.md`

4. Validar que novo checklist tem > 500 linhas (se < 300, algo está errado)

---

### PASSO 5: Sincronizar Checklist com Servidor (5 min)

1. Buscar artefato existente:
   ```
   mcp_avanade-metho_search_artifacts(
     key_pattern: "AVANADE_INSTALLATION_CHECKLIST"
   )
   ```

2. Ler checklist atualizado COMPLETO:
   ```
   read_file(
     filePath: "AVANADE_INSTALLATION_CHECKLIST{nova_versão}.md",
     startLine: 1,
     endLine: 1000  # Ajustar conforme necessário
   )
   ```

3. Executar UPDATE:
   ```
   mcp_avanade-metho_update_artifact(
     id: "{id_do_checklist}",
     value: "{conteúdo_completo_do_checklist}",
     promote_to_production: true
   )
   ```

4. Verificar: `success: true`

---

### PASSO 6: Validação Final (5 min)

Por favor, confirme:

**Artefatos**:
- ✅ Todos retornaram `success: true`?
- ✅ Todos têm `production_version_id`?
- ✅ Accessibility de todos é `"Namespace"`?
- ✅ Conteúdo é IDÊNTICO ao arquivo local?
  - Mesmo número de linhas?
  - Mesmo tamanho em KB?
  - Nenhum resumo, corte ou modificação?

**Checklist**:
- ✅ Versão incrementada?
- ✅ Contagens atualizadas?
- ✅ Novos componentes marcados ⭐ NEW?
- ✅ CHANGELOG completo?
- ✅ Estrutura completa mantida (> 500 linhas)?

**Servidor**:
- ✅ Checklist sincronizado com success?

Se TUDO ✅, forneça relatório final.

---

## 📊 RELATÓRIO FINAL

Após conclusão, forneça:

```markdown
## ✅ Sincronização Concluída - v{nova}

**Data**: {data}
**Versão anterior**: v{antiga}
**Nova versão**: v{nova}

### Resumo:
- ✅ {N} artefatos atualizados (UPDATE)
- ✅ {M} artefatos criados (CREATE)  
- ✅ 1 checklist atualizado

### Novos Componentes:
- {agente-1}: {descrição}
- {agente-2}: {descrição}

### Artifact IDs (Novos):
- AVANADE_AGENT_{NOME}_CUSTOMIZE_YAML: {id}
- AVANADE_{SPEC}_{NOME}_PROMPT_MD: {id}

### Production Version IDs:
- {Artefato}: {prod_version_id}
- AVANADE_INSTALLATION_CHECKLIST: {prod_version_id}

### Validação:
✅ Accessibility: Namespace
✅ Conteúdo: Completo (não resumido)
✅ Production: Todos promovidos
✅ Checklist: {linhas_totais} linhas (completo)
```

---

FIM DO PROMPT
```

---

## 💡 EXEMPLO DE USO COMPLETO

### Cenário: Adicionar 2 novos agentes na v2.3

```markdown
# SINCRONIZAÇÃO AVANADE METHOD - v2.3

## 📦 COMPONENTES A SINCRONIZAR

**Novos componentes** (CREATE):
- [x] bruno-architect-cloud: 
  - .github/agents/bruno-architect-cloud.chatmode.md
  - .avanade-method/agents/bruno-architect-cloud.customize.yaml
- [x] fernanda-compliance:
  - .github/agents/fernanda-compliance.chatmode.md
  - .avanade-method/agents/fernanda-compliance.customize.yaml

**Componentes existentes** (UPDATE):
- [x] supervisor: atualizar routing para bruno e fernanda
  - .avanade-method/agents/supervisor.customize.yaml

**Versão do checklist base**: v2.2  
**Nova versão do checklist**: v2.3

---

## 🎯 INSTRUÇÕES PARA COPILOT

### ⚠️ REGRAS CRÍTICAS (OBRIGATÓRIAS)

Antes de executar QUALQUER operação, confirme:
1. ✅ `accessibility: "Namespace"` (camelCase, NÃO "NAMESPACE_USER")
2. ✅ `type: "string"` (para YAML e MD)
3. ✅ `promote_to_production: true` (sempre)
4. ✅ **Conteúdo COMPLETO e IDÊNTICO** dos arquivos locais:
   - ❌ NÃO resumir
   - ❌ NÃO cortar partes
   - ❌ NÃO modificar
   - ✅ read_file do arquivo LOCAL inteiro
   - ✅ **Conteúdo remoto = Conteúdo local (idênticos)**

---

### PASSO 1: Leitura e Análise (5 min)

Por favor:
1. Leia o arquivo `AVANADE_INSTALLATION_CHECKLIST2.2.md`
2. Identifique:
   - Versão atual: v2.2
   - Total de agentes: 13
   - Total de arquivos: 115
3. Confirme comigo antes de prosseguir

[... resto do prompt seguindo o template ...]
```

---

## 🎓 DICAS DE USO

### ✅ Boas Práticas

1. **Sempre comece lendo o checklist atual**
   - Garante contexto completo
   - Evita erros de versionamento

2. **Busque artefatos antes de criar**
   - Evita duplicatas
   - Usa UPDATE quando apropriado

3. **Valide cada fase antes de prosseguir**
   - Confirme success: true
   - Verifique IDs retornados

4. **Documente tudo**
   - Mantenha registro de IDs
   - Atualize este template se necessário

---

### ❌ Erros Comuns a Evitar

1. **Não usar "NAMESPACE_USER"**
   - ✅ Use: `"Namespace"` (camelCase)

2. **Não resumir, cortar ou modificar conteúdo**
   - ✅ Leia arquivo local COMPLETO com read_file
   - ✅ Conteúdo remoto deve ser IDÊNTICO ao local
   - ✅ Mesmo número de linhas, mesmo tamanho em KB

3. **Não resumir checklist**
   - ✅ Copie estrutura completa da versão anterior
   - ✅ Checklist deve ter > 500 linhas

4. **Não esquecer promote_to_production**
   - ✅ Sempre: `promote_to_production: true`

---

## 🔗 Recursos Adicionais

- **Workflow Completo**: [artifact-sync-workflow.md](artifact-sync-workflow.md)
- **Troubleshooting**: Ver seção no workflow completo
- **Histórico de Versões**: Ver CHANGELOG no checklist

---

**Documento**: `QUICK-SYNC-PROMPT-TEMPLATE.md`  
**Versão**: 1.0  
**Criado**: 2026-03-19  
**Owner**: Avanade Method Team
