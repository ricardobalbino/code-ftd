---

## 📋 Objetivo

Guia para criar a próxima story de um Epic ou Sprint. Processo estruturado para garantir que novas stories são bem formadas, priorizadas e prontas para desenvolvimento.

---

## 🔄 Workflow: Create Next Story

### Step 1: Identificar Contexto

```markdown
## Story Context Gathering

### Epic/Feature Context
- **Epic ID**: [ID do Epic pai]
- **Epic Goal**: [Objetivo do Epic]
- **Stories já completadas**: [Lista]
- **Stories em progresso**: [Lista]
- **Stories restantes**: [Lista]

### Sprint Context
- **Sprint atual**: [Número]
- **Dias restantes**: [N]
- **Capacity disponível**: [Story points]
- **Stories comprometidas**: [Lista]
```

### Step 2: Selecionar Próxima Story

**Critérios de Seleção (em ordem)**:

1. **Prioridade de negócio**
   - Must Have > Should Have > Could Have

2. **Dependências técnicas**
   - Desbloqueadas primeiro
   - Foundation antes de features

3. **Value/Effort ratio**
   - High value / Low effort = first
   - Usar RICE se necessário

4. **Capacity fit**
   - Cabe no sprint atual?
   - Developer disponível?

### Step 3: Refinar Story

**Discovery Questions**:

```markdown
1. **Objetivo**: O que o usuário quer alcançar?
2. **Contexto**: Quando/onde isso acontece?
3. **Valor**: Por que isso é importante?
4. **Critérios**: Como saberemos que está pronto?
5. **Edge cases**: O que pode dar errado?
6. **Dependencies**: Precisa de algo pronto antes?
```

### Step 4: Escrever Story

Use ${AVANADE_STORY_TEMPLATE}:

```yaml
## Story Statement
as_a: "[Tipo de usuário definido]"
i_want: "[Ação específica]"
so_that: "[Benefício mensurável]"

## Acceptance Criteria
- given: "[Contexto]"
  when: "[Ação]"
  then: "[Resultado verificável]"
```

### Step 5: Validar INVEST

- [ ] **I**ndependent - Pode ser feita sozinha?
- [ ] **N**egotiable - Detalhes discutíveis?
- [ ] **V**aluable - Valor claro?
- [ ] **E**stimable - Time pode estimar?
- [ ] **S**mall - Cabe em 1 sprint?
- [ ] **T**estable - Critérios verificáveis?

### Step 6: Estimar

**Planning Poker ou T-Shirt**:

| Points | Complexidade |
|--------|--------------|
| 1-2 | Trivial/Simple |
| 3-5 | Small/Medium |
| 8 | Complex |
| 13+ | Dividir! |

### Step 7: Acceptance Criteria Review

Garantir que cada AC:
- [ ] É verificável (pass/fail claro)
- [ ] Não é muito granular
- [ ] Não é duplicado
- [ ] Cobre happy path + errors

### Step 8: Ready for Sprint

**Ready Checklist**:
- [ ] Story statement completo
- [ ] Acceptance criteria definidos (3-7)
- [ ] Story points estimados
- [ ] Dependencies identificadas
- [ ] UI/UX assets disponíveis (se aplicável)
- [ ] DoD viável
- [ ] PO aprovou

---

## 📝 Quick Story Creation Template

```markdown
# [EPIC-XXX] Story Title

**Sprint**: [Number/Backlog]
**Points**: [Estimate]
**Priority**: [Must/Should/Could]

## Story
Como [tipo usuário],
Quero [ação],
Para que [benefício].

## Acceptance Criteria
1. ✅ Dado [contexto], quando [ação], então [resultado]
2. ✅ Dado [contexto], quando [ação], então [resultado]
3. ⚠️ Edge case: [descrição]

## Technical Notes
- Approach: [Brief]
- Components: [List]
- Risks: [If any]

## DoD
- [ ] AC atendidos
- [ ] Tests escritos
- [ ] Code review
- [ ] PO validou
```

---

## 🔄 Story Flow States

```
[Draft] → [Ready] → [In Progress] → [Review] → [Done]
   ↓                      ↓
[Blocked]            [Needs Info]
   ↓                      ↓
   └──────── [Backlog] ←──┘
```

---

## ⚠️ Anti-Patterns a Evitar

| Anti-Pattern | Problema | Solução |
|--------------|----------|---------|
| Story gigante | Não cabe no sprint | Dividir em stories menores |
| Story técnica pura | Sem valor de usuário | Conectar a benefício |
| Story vaga | Não estimável | Adicionar detalhes |
| Dependency chain | Bloqueia outras | Reordenar ou paralelizar |
| Gold plating | Scope creep | Focar no essencial |

---

## 🔗 Relacionamentos

- **Input**: ${AVANADE_STORY_TEMPLATE}
- **Validação**: ${AVANADE_TASK_INVEST_VALIDATION}, ${AVANADE_TASK_STORY_READINESS}
- **DoD**: ${AVANADE_STORY_DOD_CHECKLIST}
- **Usado por**: ${AVANADE_MEMORY_PO_PAULA}, ${AVANADE_MEMORY_SM_ROBERTO}