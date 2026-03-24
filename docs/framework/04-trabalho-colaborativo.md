# Trabalho Colaborativo - Múltiplos Desenvolvedores no Mesmo Projeto

> 📋 **Documento**: 04 - Trabalho Colaborativo  
> 🎯 **Objetivo**: Definir governança para múltiplos devs trabalhando simultaneamente  
> 👥 **Público**: Todos os desenvolvedores e tech leads

---

## 🎯 Objetivo

Quando **3+ desenvolvedores** trabalham no mesmo projeto Dynamics 365, é **crítico** ter governança clara para evitar:

- ❌ Sobrescrever mudanças uns dos outros
- ❌ Conflitos de solution export/import
- ❌ Plugins duplicados ou configurações divergentes
- ❌ Early Bound classes desatualizadas
- ❌ Builds quebrados por falta de sincronização

Este documento define **regras obrigatórias** para trabalho em equipe.

---

## 🏗️ Arquitetura de Colaboração

```
┌─────────────────────────────────────────────────────────────┐
│                    AMBIENTE DEV (Shared)                    │
│  - Metadata compartilhada (entidades, campos, forms)       │
│  - Solutions unmanaged                                      │
│  - Usado para development e sync                            │
└─────────────────────────────────────────────────────────────┘
                             ▲
                             │ Import/Export Solutions
                             │
┌──────────────┬──────────────┼──────────────┬────────────────┐
│   Dev 1      │   Dev 2      │   Dev 3      │   Dev N        │
│              │              │              │                │
│ Feature      │ Feature      │ Feature      │ Feature        │
│ Branch       │ Branch       │ Branch       │ Branch         │
│ feat/login   │ feat/api     │ feat/flow    │ feat/report    │
│              │              │              │                │
│ Local Code   │ Local Code   │ Local Code   │ Local Code     │
│ (Plugins)    │ (Plugins)    │ (Forms)      │ (Views)        │
└──────────────┴──────────────┴──────────────┴────────────────┘
                             │
                             ▼
                    Git Repository (Azure DevOps)
                    - develop (integration)
                    - main (production)
```

---

## 📋 Regras de Ouro (OBRIGATÓRIAS)

### Regra #1: Uma Feature = Uma Branch
```powershell
# ✅ CERTO
git checkout -b feat/add-approval-level-6-7

# ❌ ERRADO - trabalhar direto na develop
git checkout develop
# ... fazendo mudanças ...
```

**Por quê**: Isolamento garante que suas mudanças não impactam outros devs até o merge.

---

### Regra #2: Pull Diário da Develop
```powershell
# TODO INÍCIO DE DIA, antes de começar a trabalhar:
git checkout develop
git pull origin develop

# Depois merge na sua branch
git checkout feat/sua-feature
git merge develop
```

**Por quê**: Mantém sua branch sincronizada e evita conflitos gigantes no futuro.

---

### Regra #3: Comunicação Antes de Mexer em Metadata Compartilhada

**Metadata compartilhada** = mudanças que afetam todos:
- Criar/alterar **entidades** (tables)
- Criar/alterar **campos** (columns)
- Criar/alterar **relationships**
- Alterar **security roles**

**PROTOCOLO OBRIGATÓRIO**:
1. Avisar no Teams/Slack: "Vou criar campo X na entidade Y"
2. Aguardar confirmação do time: "Ok, pode ir"
3. Fazer a mudança no ambiente DEV
4. Exportar solution IMEDIATAMENTE
5. Commitar no Git
6. Avisar no Teams: "Campo X criado, façam pull!"

**Por quê**: Se 2 devs criarem o mesmo campo ao mesmo tempo, há risco de conflito irrecuperável.

---

### Regra #4: Solutions - Export Frequente

**QUANDO EXPORTAR**:
- ✅ Após criar/alterar metadata (campo, entidade, form)
- ✅ Antes de fazer commit
- ✅ Ao final do dia (se fez mudanças)
- ✅ Antes de PR (Pull Request)

**COMO EXPORTAR**:
```powershell
# Via pac CLI (recomendado - gera unpacked automaticamente)
pac solution export --name FTDCore --path ./solutions/FTDCore.zip --managed false

# Unpack para XML (versionável no Git)
pac solution unpack --zipfile ./solutions/FTDCore.zip --folder ./src/Solutions/FTDCore --processCanvasApps

# Commit
git add src/Solutions/FTDCore/
git commit -m "feat: add field ftd_approval_level_6"
```

**Por quê**: Solutions unpacked no Git = versionamento correto de metadata.

---

### Regra #5: Plugins - Namespace Único por Dev (Durante Development)

Ao desenvolver **novo plugin**, use prefixo temporário:

```csharp
// ✅ Durante desenvolvimento na branch
namespace FTD.Plugins.Dev.DaniloM
{
    [PluginRegistration("Create", "quote", ...)]
    public class CalculateApprovalLevel : Plugin<Quote>
    {
        // ...
    }
}

// ✅ Após merge para develop - remover prefixo .Dev.DaniloM
namespace FTD.Plugins
{
    // ...
}
```

**Por quê**: Evita que plugins com mesmo nome sejam registrados duplicados no ambiente DEV compartilhado.

---

### Regra #6: Early Bound - Gerar Apenas Quando Necesário

**NÃO GERE** early bound classes toda hora. **Gere apenas quando**:
- Você criou uma **nova entidade**
- Você adicionou **novos campos** que precisará usar no código
- Alguém avisou no Teams que criou metadata nova

**COMO GERAR**:
```powershell
# Via pac CLI
pac modelbuilder build --outdirectory "src/Backend/EarlyBound"

# Commit APENAS se adicionou novos arquivos
git add src/Backend/EarlyBound/NewEntity.cs
git commit -m "chore: add early bound for newentity"
```

**Por quê**: Early bound gera arquivos enormes. Se cada dev gera a cada mudança, haverá conflitos constantes.

---

### Regra #7: Build Local ANTES de Commit

```powershell
# SEMPRE antes de fazer commit/push:
dotnet build

# Se passar ✅ → pode commitar
# Se falhar ❌ → NÃO COMMITЕ ATÉ CORRIGIR
```

**Por quê**: Build quebrado no repositório causa bloqueio para TODOS os devs.

---

## 🔄 Workflow Dia-a-Dia (Exemplo Prático)

### Segunda-feira 9h - Início do Dia
```powershell
# 1. Atualizar develop
git checkout develop
git pull origin develop

# 2. Atualizar minha branch
git checkout feat/approval-level-6
git merge develop

# 3. Se houver conflitos - resolver agora
# (ver doc 06-resolucao-conflitos.md)

# 4. Importar solution atualizada no ambiente DEV local
pac solution import --path ./solutions/FTDCore.zip
```

### Durante o Dia - Desenvolvendo
```powershell
# Fazer mudanças no código (plugins, forms, etc.)

# Testar localmente

# Se criou/alterou metadata:
# 1. Avisar no Teams
# 2. Fazer mudança no DEV
# 3. Exportar solution
pac solution export --name FTDCore --path ./solutions/FTDCore.zip
pac solution unpack --zipfile ./solutions/FTDCore.zip --folder ./src/Solutions/FTDCore

# 4. Commitar
git add .
git commit -m "feat: add approval level 6 field"
git push origin feat/approval-level-6
```

### Final do Dia 18h - Checklist
```bash
# ✅ Fiz pull da develop hoje?
# ✅ Exportei solutions se mexi em metadata?
# ✅ Build local passou?
# ✅ Committei meu trabalho?
# ✅ Fiz push para o remote?
# ✅ Avisei no Teams sobre mudanças críticas?
```

---

## 🚨 Cenários Problemáticos e Como Evitar

### Cenário 1: Dois Devs Criando o Mesmo Campo

**Problema**: Dev A cria `ftd_approval_level`. Dev B também cria `ftd_approval_level`.  
**Resultado**: Conflito na solution, ambiente DEV corrompido.

**Solução**:
1. **Antes de criar** campo/entidade, avisar no Teams
2. Tech Lead mantém uma **planilha de alocação** de metadata
3. Se houver colisão, último dev **renomeia** seu campo

---

### Cenário 2: Solution Import Falha Após Pull

**Problema**: Fiz pull, tentei importar solution, erro de dependência.  
**Causa**: Someone criou uma relationship que depende de campo que você não tem.

**Solução**:
```powershell
# 1. Puxar TODAS as dependências
git pull origin develop

# 2. Importar solution base PRIMEIRO
pac solution import --path ./solutions/FTDCore_Dependencies.zip

# 3. Depois importar a principal
pac solution import --path ./solutions/FTDCore.zip

# 4. Se ainda falhar - pedir ajuda no Teams (pode ser bug de metadata)
```

---

### Cenário 3: Plugin Registrado Duplicado

**Problema**: 3 devs registraram o mesmo plugin, agora executa 3x.  
**Causa**: Não usaram namespace único durante dev.

**Solução**:
1. Tech Lead **desregistra** todos os plugins de DEV
2. Cada dev **re-registra** apenas os seus com namespace `Dev.[Nome]`
3. Ao fazer PR, namespace é removido

---

## 👥 Papéis e Responsabilidades

### Tech Lead
- ✅ Revisar PRs antes de merge
- ✅ Gerenciar conflitos complexos
- ✅ Manter planilha de alocação de metadata
- ✅ Fazer "cleanup" semanal do ambiente DEV (remover lixo)
- ✅ Validar que padrões BCA estão sendo seguidos

### Desenvolvedores
- ✅ Seguir TODAS as regras deste documento
- ✅ Comunicar mudanças em metadata compartilhada
- ✅ Resolver conflitos simples sozinho
- ✅ Pedir ajuda quando não souber
- ✅ Atualizar documentação se descobrir algo novo

### DevOps / Architect
- ✅ Configurar pipelines de CI/CD
- ✅ Monitorar quality gates
- ✅ Aprovar deploys para OAT/PROD
- ✅ Manter ambientes saudáveis

---

## 📊 Ferramentas de Coordenação

### 1. Planilha de Alocação (Excel/SharePoint)
```
| Dev      | Entidade/Área     | Campos       | Status      |
|----------|------------------|--------------|-------------|
| Danilo   | Quote            | ftd_level_6  | In Progress |
| Julio    | Account          | ftd_segment  | Done        |
| Fernando | Custom API       | N/A          | In Progress |
```

### 2. Teams Channel - #dev-sync
- Avisos de criação de metadata
- Alertas de breaks
- Dúvidas rápidas

### 3. Azure DevOps - Work Items
- Cada feature = 1 Work Item
- Work Item linkado ao PR
- Review obrigatório

---

## ✅ Checklist de Onboarding (Novo Dev)

Quando um novo desenvolvedor entrar no projeto:

- [ ] Leu documentos 01-Setup, 04-Colaborativo, 07-Workflow
- [ ] Configurou ambiente local (tools, conexões)
- [ ] Adicionado ao grupo Azure DevOps
- [ ] Adicionado ao Teams channel #dev-sync
- [ ] Recebeu credenciais D365 DEV
- [ ] Fez primeiro PR de teste (dummy commit)
- [ ] Entende branching strategy
- [ ] Sabe como pedir ajuda

---

## 📚 Próximos Passos

Após entender colaboração:

1. ✅ Leia [Sincronização de Artefatos](./05-sincronizacao-artefatos.md)
2. ✅ Leia [Resolução de Conflitos](./06-resolucao-conflitos.md)
3. ✅ Leia [Workflow de Desenvolvimento](./07-workflow-desenvolvimento.md)
4. ✅ Pratique com uma feature simples

---

**📝 Última atualização**: 20/Mar/2026 | **Versão**: 1.0 (Draft)
