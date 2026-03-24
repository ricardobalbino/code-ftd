# Workflow de Desenvolvimento - Do Código ao Deploy

> 📋 **Documento**: 07 - Workflow de Desenvolvimento  
> 🎯 **Objetivo**: Definir o fluxo completo desde pegar uma tarefa até deploy em produção  
> 📊 **Ciclo**: Feature → Development → Review → Merge → Deploy

---

## 🔄 Visão Geral do Workflow

```
┌─────────────────────────────────────────────────────────────────────┐
│ 1. PLANEJAMENTO                                                     │
│    Azure DevOps → Work Item criado → Dev assignado                 │
└─────────────────────────────────────────────────────────────────────┘
                             ▼
┌─────────────────────────────────────────────────────────────────────┐
│ 2. DEVELOPMENT (Branch feat/*)                                      │
│    ├── Criar branch                                                 │
│    ├── Desenvolver (código + metadata)                              │
│    ├── Testar localmente                                            │
│    └── Commitar frequentemente                                      │
└─────────────────────────────────────────────────────────────────────┘
                             ▼
┌─────────────────────────────────────────────────────────────────────┐
│ 3. CODE REVIEW (Pull Request)                                       │
│    ├── Criar PR para develop                                        │
│    ├── Automated checks (build, tests)                              │
│    ├── Peer review (1+ aprovação)                                   │
│    └── Tech Lead review (arquitetura)                               │
└─────────────────────────────────────────────────────────────────────┘
                             ▼
┌─────────────────────────────────────────────────────────────────────┐
│ 4. INTEGRATION (Branch develop)                                     │
│    ├── Merge PR                                                     │
│    ├── CI Pipeline (build + quality gates)                          │
│    └── Deploy para DEV (automated)                                  │
└─────────────────────────────────────────────────────────────────────┘
                             ▼
┌─────────────────────────────────────────────────────────────────────┐
│ 5. QA (Ambiente OAT)                                                │
│    ├── Deploy manual para OAT                                       │
│    ├── Testes funcionais                                            │
│    ├── Testes de regressão                                          │
│    └── Aprovação QA                                                 │
└─────────────────────────────────────────────────────────────────────┘
                             ▼
┌─────────────────────────────────────────────────────────────────────┐
│ 6. PRODUCTION (Branch main)                                         │
│    ├── Merge develop → main                                         │
│    ├── Deploy MANUAL para PROD (+ GMUD)                             │
│    ├── Smoke tests                                                  │
│    └── Monitor por 24h                                              │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 1️⃣ Fase: Planejamento

### Entrada
- Work Item criado no Azure DevOps (User Story, Bug, Task)
- Requisitos claros (Acceptance Criteria definidos)
- Prioridade e sprint definidos

### Ações do Desenvolvedor
```powershell
# 1. Abrir Work Item no Azure DevOps
# Ler AC (Acceptance Criteria) completos
# Esclarecer dúvidas com PO/Tech Lead

# 2. Estimar esforço (se não estimado)
# Planning Poker / Story Points

# 3. Mover para "In Progress" no board
# Work Item Status: New → Active
```

### Saída
- Dev entende o que precisa fazer
- Work Item em estado "Active"
- Sprint backlog atualizado

---

## 2️⃣ Fase: Development

### Entrada
- Work Item assignado e ativo
- Ambiente local funcionando

### Ações do Desenvolvedor

#### 📌 Passo 1: Criar Branch
```powershell
# Naming convention: feat/[work-item-id]-[descrição-curta]
git checkout develop
git pull origin develop
git checkout -b feat/1234-add-approval-level-6

# Exemplo de nomes:
# feat/1234-add-approval-level-6       # nova feature
# fix/1235-fix-null-reference          # bugfix
# chore/1236-update-early-bound        # tarefa técnica
```

#### 📌 Passo 2: Desenvolver

**Backend (Plugins, Custom APIs)**:
```csharp
// 1. Criar classe do plugin
namespace FTD.Plugins
{
    [PluginRegistration("Create", "quote", 
        ExecutionStage.PostOperation, ExecutionMode.Synchronous)]
    public class CalculateApprovalLevel : Plugin<Quote>
    {
        protected override void ExecuteLogic()
        {
            // Implementar lógica
        }
    }
}

// 2. Escrever unit tests
[TestClass]
public class CalculateApprovalLevelTests
{
    [TestMethod]
    public void ShouldCalculateLevel6ForRoyalties500K()
    {
        // Arrange, Act, Assert
    }
}

// 3. Build local
dotnet build

// 4. Rodar testes
dotnet test
```

**Frontend (TypeScript, Forms)**:
```typescript
// 1. Criar arquivo de script
// src/Frontend/Forms/quote.main.ts

export function onLoad(executionContext: Xrm.Events.EventContext) {
    const formContext = executionContext.getFormContext();
    // Implementar lógica
}

// 2. Build
npm run build

// 3. Deploy (via solution)
```

**Metadata (Entidades, Campos, Forms)**:
```powershell
# 1. Fazer mudanças no ambiente DEV via UI
# 2. Exportar solution
pac solution export --name FTDCore --path .\src\Solutions\FTDCore.zip

# 3. Unpack para XML
pac solution unpack --zipfile .\src\Solutions\FTDCore.zip `
    --folder .\src\Solutions\FTDCore --processCanvasApps
```

#### 📌 Passo 3: Testar Localmente
```powershell
# 1. Deploy plugin para ambiente DEV pessoal
spkl /deploy

# 2. Importar solution
pac solution import --path .\src\Solutions\FTDCore.zip

# 3. Testar no CRM manualmente
# - Criar registro
# - Verificar plugin executou
# - Validar resultado

# 4. Rodar testes automatizados
dotnet test --collect:"XPlat Code Coverage"
```

#### 📌 Passo 4: Commitar
```powershell
# Commits pequenos e frequentes (várias vezes ao dia)
git add .
git commit -m "feat(quote): add approval level 6 calculation #1234"

# Convention: tipo(escopo): mensagem #work-item
# Tipos: feat, fix, chore, docs, test, refactor
```

### Saída
- Feature implementada e testada localmente
- Código committado em branch
- Testes passando

---

## 3️⃣ Fase: Code Review (Pull Request)

### Entrada
- Branch feat/* com código completo e testado
- Ready para revisão

### Ações do Desenvolvedor

#### 📌 Passo 1: Criar Pull Request
```powershell
# 1. Push da branch
git push origin feat/1234-add-approval-level-6

# 2. Criar PR no Azure DevOps
# - Title: "[#1234] Add approval level 6 calculation"
# - Description: Preencher template (ver abaixo)
# - Link Work Item #1234
# - Assign reviewers (Tech Lead + 1 peer)
```

**Template de PR**:
```markdown
## 📋 Descrição
Implementação dos níveis 6 e 7 de aprovação para propostas com alto valor.

## 🔗 Work Item
Closes #1234

## ✅ Checklist
- [x] Código implementado
- [x] Unit tests escritos (coverage >80%)
- [x] Testado localmente
- [x] Solution exportada (se alterou metadata)
- [x] Documentação atualizada
- [x] Segue padrões Avanade BCA

## 🧪 Como Testar
1. Criar proposta com Royalties = R$ 600.000
2. Avançar estágio para "Em Aprovação"
3. Validar campo `ftd_nivel_alcada` = 6

## 📸 Screenshots (se aplicável)
[Anexar prints de telas/forms alterados]

## ⚠️ Breaking Changes
Nenhum

## 📝 Notas Adicionais
Plugin utiliza hierarquia de Região existente, zero mudanças de schema.
```

#### 📌 Passo 2: Aguardar Review

**Automated Checks** (CI Pipeline):
- ✅ Build passa
- ✅ Testes unitários passam
- ✅ Code coverage > 80%
- ✅ SonarQube quality gate passed
- ✅ Nenhuma vulnerabilidade crítica

**Manual Review** (Peers + Tech Lead):
- ✅ Código segue padrões BCA
- ✅ Lógica está correta
- ✅ Testes são abrangentes
- ✅ Documentação adequada
- ✅ Sem code smells óbvios

#### 📌 Passo 3: Resolver Comentários
```powershell
# Se reviewer pediu mudanças:
# 1. Fazer ajustes
# 2. Commitar
git add .
git commit -m "refactor: apply code review feedback"
git push origin feat/1234-add-approval-level-6

# 3. Responder comentários no PR
# 4. Marcar como "Resolved" quando corrigir
```

### Saída
- PR aprovado por 2+ reviewers
- Automated checks ✅
- Ready para merge

---

## 4️⃣ Fase: Integration (Merge)

### Entrada
- PR aprovado e checks passando

### Ações do Tech Lead (ou Dev se permitido)

```powershell
# 1. Fazer merge do PR
# Azure DevOps → Complete PR → Squash merge (preferido)

# 2. CI Pipeline automaticamente:
#    - Build develop branch
#    - Run tests
#    - Deploy para DEV (se configurado)

# 3. Validar que deploy foi bem-sucedido
# Verificar logs de pipeline
```

### Saída
- Código merged na `develop`
- Deploy automático para DEV
- Work Item atualizado (In Review)

---

## 5️⃣ Fase: QA Testing

### Entrada
- Feature deployada em DEV
- Ready para testes mais amplos

### Ações do QA Engineer

```powershell
# 1. Deploy para OAT (manual, via pipeline)
# Azure DevOps → Pipelines → Deploy-to-OAT → Run

# 2. Executar test cases
# - Testes funcionais (conforme AC)
# - Testes de regressão (features antigas não quebraram)
# - Testes exploratórios

# 3. Reportar bugs (se houver)
# Criar Work Items de Bug linkados à feature

# 4. Aprovar (se tudo OK)
# Work Item Status: In Review → Resolved
```

### Saída
- Feature validada em OAT
- Bugs corrigidos (se houver)
- Aprovação QA documentada

---

## 6️⃣ Fase: Production Deploy

### Entrada
- Feature aprovada em OAT
- Sprint encerrada (ou hotfix aprovado)

### Ações do Tech Lead + DevOps

```powershell
# 1. Criar PR: develop → main
git checkout main
git pull origin main
git merge develop  # Localmente primeiro

# 2. Testar merge localmente
dotnet build
dotnet test

# 3. Crear PR no Azure DevOps
# Título: "Release v1.2.0 - Sprint 10"
# Listar todas as features incluídas

# 4. Aprovar PR (requer aprovação especial)
# Tech Lead + Manager/PO

# 5. DEPLOY PARA PROD (Pipeline MANUAL)
# Azure DevOps → Pipelines → Deploy-to-PROD → Run
# Requer GMUD aprovada

# 6. Executar smoke tests em PROD
# - Login funciona?
# - Features críticas OK?
# - Sem erros no console?

# 7. Monitor por 24h
# - Application Insights
# - Logs de erros
# - Feedback de usuários
```

### Saída
- Feature em PRODUÇÃO
- Work Items fechados (Closed)
- Release notes atualizadas

---

## 📊 Tempos Típicos

| Fase | Tempo Médio | Depende de |
|------|------------|------------|
| Planejamento | 15-30 min | Clareza dos requisitos |
| Development | 0.5-5 dias | Complexidade da feature |
| Code Review | 2-4 horas | Disponibilidade reviewers |
| Integration | 5-15 min | CI pipeline speed |
| QA Testing | 0.5-2 dias | Abrangência dos testes |
| Production Deploy | 1-2 horas | Complexidade + GMUD |

**Total**: 2-10 dias (feature simples a complexa)

---

## ✅ Checklists por Fase

### Developer Checklist (Antes de Criar PR)
- [ ] Código implementado conforme AC
- [ ] Unit tests escritos (coverage > 80%)
- [ ] Testado localmente no ambiente DEV
- [ ] Build local passou (`dotnet build`)
- [ ] Testes locais passaram (`dotnet test`)
- [ ] Solution exportada (se alterou metadata)
- [ ] Código segue padrões Avanade BCA
- [ ] Sem credenciais hardcoded
- [ ] Documentação atualizada (README, comentários)
- [ ] Commits seguem convention
- [ ] Branch está sincronizada com develop

### Reviewer Checklist (Durante Code Review)
- [ ] Código segue padrões BCA (Plugin<T>, DI, etc.)
- [ ] Lógica de negócio está correta
- [ ] Testes são abrangentes e relevantes
- [ ] Não há code smells óbvios
- [ ] Não há vulnerabilidades de segurança
- [ ] Performance parece adequada
- [ ] Documentação está clara
- [ ] Metadata changes fazem sentido

### Tech Lead Checklist (Antes de Merge)
- [ ] 2+ aprovações de reviewers
- [ ] Automated checks passando
- [ ] Arquitetura está consistente
- [ ] Não há breaking changes não documentados
- [ ] Pronto para deploy

---

## 🚨 Cenários Especiais

### Hotfix (Produção Quebrada)
```
main → hotfix/critical-bug → (fix) → main + develop
                                        |
                                        └─ Deploy PROD imediato
```

### Feature Flag (Feature Incompleta)
```
feat/* → develop (com flag OFF) → main → PROD (flag OFF)
                  |
                  └─ Liga flag gradualmente (5% → 50% → 100%)
```

---

## 📚 Próximos Passos

1. ✅ Leia [Branching Strategy](./08-branching-strategy.md)
2. ✅ Leia [Code Review Guidelines](./09-code-review.md)
3. ✅ Leia [CI/CD Pipelines](./10-pipelines-cicd.md)

---

**📝 Última atualização**: 20/Mar/2026 | **Versão**: 1.0 (Draft)
