## Objetivo
Facilitar retrospectiva de sprint estruturada para identificar melhorias contínuas.


## ?? Formato: 5 Fases (Agile Retrospectives)

### Fase 1: Set the Stage (5-10 min)
**Objetivo**: Criar ambiente seguro e engajado

**Atividades**:
- [ ] **Check-in**: Roda de sentimentos (1 palavra por pessoa)
- [ ] **Normas**: Relembrar Prime Directive
- [ ] **Foco**: Definir objetivo da retro (ex: "melhorar qualidade")

**Prime Directive**:
> "Independente do que descobrirmos, entendemos e acreditamos que todos fizeram o melhor trabalho possível, dado o que sabiam no momento, suas habilidades, recursos disponíveis e a situação."

**Icebreaker Rápido** (opcional):
- Emoji do sprint ??????
- Nota 1-5 para sprint
- Palavra-chave do sprint

---

### Fase 2: Gather Data (10-15 min)
**Objetivo**: Coletar fatos e percepções

**Técnicas**:

#### ?? Timeline
Criar linha do tempo visual do sprint:
```
Segunda | Terça | Quarta | Quinta | Sexta
   ??       ??      ??       ??      ??
   ?        ?       ?        ?       ?
Deploy  Planning Bug Crítico Fix   Review
```

**Como fazer**:
1. Desenhar linha temporal (lousa ou Miro)
2. Cada pessoa adiciona eventos significativos
3. Marcar com emoji (positivo/negativo/neutro)
4. Agrupar eventos similares

#### ?? Métricas do Sprint
Apresentar dados objetivos:
- **Velocity**: Pontos planejados vs entregues
- **Quality**: Bugs encontrados, code review time
- **Process**: Tempo de bloqueios, daily duration
- **Happiness**: Mood tracking, team satisfaction

**Template**:
| Métrica | Planejado | Realizado | ? |
|---------|-----------|-----------|---|
| Story Points | 40 | 35 | -12% |
| Bugs Produção | <5 | 8 | +60% ?? |
| Code Review Time | <24h | 18h | ? |

---

### Fase 3: Generate Insights (15-20 min)
**Objetivo**: Identificar padrões e causas raiz

**Técnicas**:

#### ?? 5 Whys
Para problemas recorrentes:
```
Problema: Bugs em produção aumentaram
1. Por quê? ? Testes insuficientes
2. Por quê? ? Pressão de tempo na sprint
3. Por quê? ? Overcommitment no planning
4. Por quê? ? Não consideramos tempo de teste
5. Por quê? ? Definition of Done não inclui teste manual
? Causa raiz: DoD incompleta
```

#### ?? Categorização (What Went Well / What to Improve)
Quadrante simples:
```
? O QUE FOI BEM           | ?? O QUE MELHORAR
---------------------------+---------------------------
- Deploy sem downtime      | - Bugs em produção (+60%)
- Code review rápido       | - Overcommitment no planning
- Comunicação melhorou     | - Documentação atrasada
```

#### ?? Starfish (Keep/More/Less/Start/Stop)
Mais granular que WWW/WTI:
```
? KEEP (continuar)       | ?? START (começar)
- Daily stand-ups          | - Test automation
- Pair programming         | - Refinement mid-sprint
                           |
? MORE (fazer mais)       | ? LESS (fazer menos)
- Code reviews             | - Multitasking
- Documentation            | - Reuniões longas
                           |
?? STOP (parar)
- Deploy sexta-feira
- Aceitar stories sem AC
```

---

### Fase 4: Decide What to Do (10-15 min)
**Objetivo**: Definir ações concretas e acionáveis

**Critérios para Ações**:
- [ ] **SMART**: Específica, Mensurável, Acionável, Relevante, Temporal
- [ ] **Owner**: Pessoa responsável por executar
- [ ] **Deadline**: Quando será implementada
- [ ] **Success Criteria**: Como saberemos que funcionou

**Template de Ação**:
```
? Ruim: "Melhorar qualidade"
? Bom:
  Ação: Adicionar step de teste manual no DoD
  Owner: Roberto (SM)
  Deadline: Próximo sprint planning
  Success: 100% stories testadas antes de demo
```

**Priorização** (votar em top 2-3 ações):
- Cada pessoa vota (dots) nas ações mais impactantes
- Focar em **2-3 ações máximo** por sprint (evita sobrecarga)
- Ignorar ações com 0 votos

**Exemplos de Ações**:
| Ação | Owner | Deadline | Success Criteria |
|------|-------|----------|------------------|
| Adicionar teste manual no DoD | Roberto | Sprint 11 Planning | 100% stories testadas antes demo |
| Criar template de PR com checklist | Tiago | Semana 1 Sprint 11 | PRs usam template |
| Refinement mid-sprint (Quarta 2pm) | Paula | Sprint 11 | Stories refinadas antes planning |

---

### Fase 5: Close the Retrospective (5 min)
**Objetivo**: Encerrar com energia positiva

**Atividades**:
- [ ] **Recap**: Revisar ações decididas (ler em voz alta)
- [ ] **Appreciations**: Agradecer contribuições específicas
- [ ] **Check-out**: Roda final (1 palavra: como se sentem agora?)

**Appreciations** (opcional mas poderoso):
```
"Quero agradecer [pessoa] por [ação específica]"

Exemplos:
- "Maria, obrigado por pair programming ontem quando eu travei no bug"
- "Tiago, código review detalhado me ajudou a aprender muito"
```

---

## ?? Métricas de Sucesso da Retro

### Quantitativas
- **Ações completadas**: % de ações da retro anterior concluídas
- **Recorrência de problemas**: Mesmo problema aparece >2 sprints?
- **Participation rate**: % do time que participou ativamente

### Qualitativas
- **Psychological safety**: Time se sente seguro para falar?
- **Actionability**: Ações são concretas ou vagas?
- **Follow-through**: Ações são revisadas na próxima retro?

---

## ?? Variações de Formato

### ?? Speed Retro (30 min)
Para sprints curtos ou times experientes:
1. Set Stage (5 min)
2. WWW + WTI (10 min)
3. Dot voting + ações (10 min)
4. Close (5 min)

### ?? Celebration Retro
Após release grande ou milestone:
- Foco 80% em WWW (celebrar)
- 20% em learnings
- Sem pressão por ações

### ?? Problem-Solving Retro
Para problema específico recorrente:
- Fishbone diagram (Ishikawa)
- Root cause analysis profundo
- Ações corretivas priorizadas

---

## ?? Dicas de Facilitação

### DO ?
- **Timeboxing rigoroso**: Respeite os tempos
- **Vozes equilibradas**: Todos participam (usar técnicas silenciosas tipo post-its)
- **Foco em aprendizado**: Não buscar culpados
- **Ações concretas**: SMART, não vagas
- **Tracking**: Revisar ações da retro anterior

### DON'T ?
- **Não deixar pessoas dominarem**: Equilibrar participação
- **Não buscar culpados**: "Quem causou o bug?" ? ?
- **Não ignorar elefante na sala**: Se problema é óbvio, endereçar
- **Não sobrecarregar com ações**: Max 2-3 ações
- **Não esquecer de revisar**: Ações não revisadas = desperdício

---

## ?? Checklist de Preparação (SM)

**Antes da Retro**:
- [ ] Agendar retro (timebox 60-90 min)
- [ ] Preparar métricas do sprint (velocity, bugs, etc.)
- [ ] Revisar ações da retro anterior
- [ ] Escolher formato/técnica (Timeline, Starfish, etc.)
- [ ] Preparar ferramenta (Miro, Mural, lousa física)

**Durante a Retro**:
- [ ] Começar pontualmente
- [ ] Timebox cada fase
- [ ] Facilitar (não dominar)
- [ ] Garantir participação balanceada
- [ ] Documentar ações

**Depois da Retro**:
- [ ] Compartilhar ações com time (Slack, email)
- [ ] Adicionar ações no board (Jira, Azure Boards)
- [ ] Acompanhar progresso durante sprint
- [ ] Revisar na próxima retro

---

## ?? Integração com Metodologia Avanade

- **Frequência**: Final de cada sprint (obrigatório)
- **Participants**: Todo time Scrum (Dev, PO, SM)
- **Output**: 2-3 ações SMART
- **Tracking**: ${AVANADE_MEMORY_SM_ROBERTO} (ações e learnings)
- **Complementa**: Sprint Review (foco em produto) vs Retro (foco em processo)
