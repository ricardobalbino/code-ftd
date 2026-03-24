# Sincronização de Artefatos - Manter Todos Atualizados

> 📋 **Documento**: 05 - Sincronização de Artefatos  
> 🎯 **Objetivo**: Garantir que todos os devs tenham as mesmas versões de código, metadata e configurações  
> ⏱️ **Frequência**: Diária (mínimo 1x/dia)

---

## 🎯 O Problema

Em projetos D365 com múltiplos desenvolvedores, há **3 tipos de artefatos** que precisam estar sincronizados:

1. **Código-fonte** (C#, TypeScript) → Versionado no Git ✅ Fácil
2. **Metadata CRM** (entidades, campos, forms) → Solutions exportadas ⚠️ Complexo
3. **Configurações** (plugin steps, workflows, flows) → Solutions + deployment ⚠️ Complexo

Se **qualquer um** estiver desatualizado = ambiente quebrado.

---

## 🔄 Fluxo de Sincronização Completo

```
┌─────────────────────────────────────────────────────────────────┐
│ REPOSITÓRIO GIT (Source of Truth)                              │
│                                                                 │
│ ├── src/Backend/                  (Código C#)                  │
│ ├── src/Frontend/                 (Código TS)                  │
│ ├── src/Solutions/FTDCore/        (Solution unpacked - XML)    │
│ └── src/Solutions/FTDCore.zip     (Solution packed - binário)  │
└─────────────────────────────────────────────────────────────────┘
                            ▲ Pull/Push
                            │
┌───────────────────────────┼─────────────────────────────────────┐
│         DESENVOLVEDOR     │                                     │
│                           │                                     │
│  1. git pull develop      │  ◄── Baixa código + solutions       │
│  2. import solution       │  ◄── Atualiza metadata local        │
│  3. build código          │  ◄── Compila plugins                │
│  4. deploy plugins        │  ◄── Registra no ambiente           │
└───────────────────────────────────────────────────────────────────┘
                            ▼ Import
┌───────────────────────────────────────────────────────────────────┐
│ AMBIENTE DEV (Dynamics 365)                                       │
│                                                                   │
│ - Metadata (entidades, campos, forms, views)                     │
│ - Plugin assemblies registrados                                  │
│ - Workflows/Flows ativos                                         │
└───────────────────────────────────────────────────────────────────┘
```

---

## 📋 Rotina Diária de Sincronização

### 🌅 Início do Dia (9h - ANTES de começar a trabalhar)

```powershell
# PASSO 1: Atualizar código e solutions do Git
cd C:\repos\[seu-projeto]
git checkout develop
git pull origin develop

# PASSO 2: Verificar o que mudou
git log --oneline --since="1 day ago"
# Ler commits - alguém mexeu em metadata?

# PASSO 3: Importar solution atualizada no ambiente DEV
$solutionPath = ".\src\Solutions\FTDCore.zip"

if (Test-Path $solutionPath) {
    Write-Host "Importando solution FTDCore..." -ForegroundColor Cyan
    pac solution import --path $solutionPath --force-overwrite
    
    if ($LASTEXITCODE -eq 0) {
        Write-Host "✅ Solution importada com sucesso!" -ForegroundColor Green
    } else {
        Write-Host "❌ Erro ao importar solution. Verifique dependências." -ForegroundColor Red
        # Ver seção Troubleshooting abaixo
    }
}

# PASSO 4: Compilar código
Write-Host "Compilando código..." -ForegroundColor Cyan
dotnet restore
dotnet build

if ($LASTEXITCODE -eq 0) {
    Write-Host "✅ Build bem-sucedido!" -ForegroundColor Green
} else {
    Write-Host "❌ Build falhou. Verifique erros de compilação." -ForegroundColor Red
    exit 1
}

# PASSO 5: (Opcional) Atualizar Early Bound se houve mudanças em metadata
# Verificar com o time se precisa
# pac modelbuilder build --outdirectory "src/Backend/EarlyBound"

Write-Host "`n✅ Sincronização completa! Você está pronto para começar." -ForegroundColor Green
```

**⏱️ Tempo**: 3-5 minutos  
**Frequência**: TODO DIA antes de começar

---

### 🔄 Durante o Dia (Quando alguém commitou)

**Cenário**: Você vê no Teams: *"@team acabei de commitar mudanças na entity Quote, façam pull!"*

```powershell
# 1. Salvar seu trabalho
git add .
git stash push -m "WIP: saving before sync"

# 2. Pull das mudanças
git checkout develop
git pull origin develop

# 3. Merge na sua branch
git checkout feat/sua-feature
git merge develop

# 4. Resolver conflitos se houver (ver doc 06)

# 5. Importar solution nova
pac solution import --path .\src\Solutions\FTDCore.zip --force-overwrite

# 6. Restaurar seu trabalho
git stash pop

# 7. Testar que tudo ainda funciona
dotnet build
```

**⏱️ Tempo**: 5-10 minutos  
**Frequência**: Sempre que alguém avisar de mudanças críticas

---

### 🌙 Final do Dia (18h - ANTES de ir embora)

```powershell
# 1. Exportar sua solution se fez mudanças em metadata
if ("Você criou/alterou campos/entidades/forms hoje?") {
    pac solution export --name FTDCore --path .\src\Solutions\FTDCore.zip --managed false
    pac solution unpack --zipfile .\src\Solutions\FTDCore.zip --folder .\src\Solutions\FTDCore --processCanvasApps
}

# 2. Commit + Push
git add .
git commit -m "feat: [descrição do que fez]"
git push origin feat/sua-feature

# 3. Avisar no Teams se fez mudanças críticas
# "Commitei mudanças em [entidade/campo]. Façam pull amanhã!"
```

**⏱️ Tempo**: 3-5 minutos  
**Frequência**: TODO DIA antes de sair

---

## 🎯 Artefatos que Precisam Sincronização

### 1. Código-fonte (.cs, .ts, .js)
**Sincronização**: Git (fácil ✅)

| O que | Como | Quando |
|-------|------|--------|
| Baixar | `git pull` | Início do dia |
| Subir | `git push` | Fim do dia / após feature |

---

### 2. Solutions (metadata CRM)
**Sincronização**: Git + Import (complexo ⚠️)

| O que | Como | Quando |
|-------|------|--------|
| Baixar | `git pull` → `pac solution import` | Início do dia |
| Subir | `pac solution export` → `git push` | Após alterar metadata |

**Componentes da Solution**:
- Entidades (tables)
- Campos (columns)
- Relationships
- Forms
- Views
- Dashboards
- Charts
- Business Rules
- Workflows (classic)
- Ribbon customizations

---

### 3. Plugin Assemblies
**Sincronização**: Build + Deploy (complexo ⚠️)

| O que | Como | Quando |
|-------|------|--------|
| Baixar | `git pull` → `dotnet build` | Início do dia |
| Subir | `dotnet build` → `spkl /deploy` | Após alterar código plugin |

**Atenção**: Plugin assemblies NÃO estão incluídos na solution export/import por padrão. Precisam de deploy separado via `spkl` ou Plugin Registration Tool.

---

### 4. Power Automate Flows / Classic Workflows
**Sincronização**: Solution export/import ✅

Flows **modernos** (Cloud Flows) são incluídos na solution automaticamente.  
Workflows **clássicos** (on-premise style) também.

**Atenção**: Flows podem ficar em estado **draft** após import. Precisa ativar manualmente.

---

### 5. Early Bound Classes
**Sincronização**: Geração sob demanda ⚠️

| O que | Como | Quando |
|-------|------|--------|
| Gerar | `pac modelbuilder build` | Quando metadata mudar |
| Commitar | `git add` + `git commit` | Após gerar (se mudou) |

**Regra de ouro**: NÃO gere todo dia. Gere apenas quando:
- Você criou entidade nova
- Você adicionou campo que vai usar
- Alguém avisou no Teams que criou metadata

---

## 🛠️ Scripts de Automação

### Script: `sync-from-develop.ps1`
Automatiza o processo de sincronização completo:

```powershell
# sync-from-develop.ps1
param(
    [switch]$ImportSolution,
    [switch]$UpdateEarlyBound
)

Write-Host "🔄 Iniciando sincronização..." -ForegroundColor Cyan

# 1. Stash trabalho atual
Write-Host "Salvando trabalho local..."
git stash push -m "Auto-stash before sync $(Get-Date -Format 'yyyy-MM-dd HH:mm')"

# 2. Pull da develop
Write-Host "Baixando atualizações..."
$currentBranch = git branch --show-current
git checkout develop
git pull origin develop

# 3. Merge na branch atual
git checkout $currentBranch
git merge develop

# 4. Importar solution se solicitado
if ($ImportSolution) {
    Write-Host "Importando solution..."
    pac solution import --path .\src\Solutions\FTDCore.zip --force-overwrite
}

# 5. Atualizar Early Bound se solicitado
if ($UpdateEarlyBound) {
    Write-Host "Atualizando Early Bound..."
    pac modelbuilder build --outdirectory "src/Backend/EarlyBound"
}

# 6. Build
Write-Host "Compilando..."
dotnet restore
dotnet build

# 7. Restaurar stash
Write-Host "Restaurando trabalho local..."
git stash pop

Write-Host "`n✅ Sincronização completa!" -ForegroundColor Green
```

**Uso**:
```powershell
# Sync básico (só código)
.\scripts\sync-from-develop.ps1

# Sync + importar solution
.\scripts\sync-from-develop.ps1 -ImportSolution

# Sync + solution + early bound
.\scripts\sync-from-develop.ps1 -ImportSolution -UpdateEarlyBound
```

---

## 🚨 Problemas Comuns e Soluções

### Problema 1: Solution Import Failed - Missing Dependency

**Erro**:
```
The import failed because the component [Component Name] (id=[guid]) 
could not be found.
```

**Causa**: Solution depende de outra solution que você não tem.

**Solução**:
```powershell
# 1. Identificar dependência
pac solution check --path .\src\Solutions\FTDCore.zip

# 2. Importar solution base primeiro
# (Tech Lead deve fornecer lista de dependências)
pac solution import --path .\src\Solutions\FTDCore_Base.zip
pac solution import --path .\src\Solutions\FTDCore.zip
```

---

### Problema 2: Build Failed - EarlyBound Class Missing

**Erro**:
```
Error CS0246: The type or namespace name 'NewEntity' could not be found
```

**Causa**: Alguém criou entidade nova, gerou early bound, você não tem.

**Solução**:
```powershell
# Opção 1: Gerar early bound você mesmo
pac modelbuilder build --outdirectory "src/Backend/EarlyBound"

# Opção 2: Perguntar no Teams quem commitou para gerar e commitar
```

---

### Problema 3: Plugin Não Executando Após Pull

**Causa**: Plugin assembly está desatualizado no ambiente DEV.

**Solução**:
```powershell
# Rebuild + redeploy
dotnet build
spkl /deploy

# Verificar no XrmToolBox → Plugin Registration Tool
# Se o assembly tem timestamp antigo = deploy não funcionou
```

---

### Problema 4: Merge Conflict em Solution XML

**Erro**: Git mostra conflito em arquivo `.xml` dentro de `src/Solutions/FTDCore/`.

**Solução**: Ver documento [06-resolucao-conflitos.md](./06-resolucao-conflitos.md).

---

## ✅ Checklist de Sincronização

Use este checklist toda manhã:

```markdown
## Checklist Diário de Sincronização

- [ ] `git pull origin develop` executado
- [ ] Revisei commits das últimas 24h
- [ ] Solution importada (se houve mudanças em metadata)
- [ ] `dotnet build` passou sem erros
- [ ] Early bound atualizado (se necessário)
- [ ] Plugins deployados (se alterei código)
- [ ] Testei que ambiente está funcional
```

---

## 📊 Indicadores de Saúde

### 🟢 Time Sincronizado (Saudável)
- ✅ Todos fazem pull diário
- ✅ Solutions são exportadas sempre que metadata muda
- ✅ Build local passa para todos
- ✅ Commits frequentes (várias vezes ao dia)

### 🟡 Time Parcialmente Sincronizado (Atenção)
- ⚠️ Alguns devs esquecem de fazer pull
- ⚠️ Solutions exportadas irregularmente
- ⚠️ Build falha ocasionalmente para alguns devs
- ⚠️ Commits no final do dia apenas

### 🔴 Time Dessincronizado (Crítico)
- ❌ Devs trabalham dias sem fazer pull
- ❌ Solutions nunca são exportadas
- ❌ Build quebrado persistentemente
- ❌ Commits semanais ou menos

**Se o time está 🟡 ou 🔴** → Urgente: reunião de alinhamento + revisão de processos.

---

## 📚 Próximos Passos

Após dominar sincronização:

1. ✅ Leia [Resolução de Conflitos](./06-resolucao-conflitos.md)
2. ✅ Leia [Workflow de Desenvolvimento](./07-workflow-desenvolvimento.md)
3. ✅ Configure alerts no Azure DevOps para commits

---

**📝 Última atualização**: 20/Mar/2026 | **Versão**: 1.0 (Draft)
