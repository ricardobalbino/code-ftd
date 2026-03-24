---

## ?? O que é este Artefato?

Este template fornece uma estrutura padronizada para documentar decisões arquiteturais importantes. ADRs são documentos imutáveis que registram:
- **O que** foi decidido
- **Por que** essa decisão foi tomada
- **Quando** a decisão foi feita
- **Quais alternativas** foram consideradas
- **Quais trade-offs** foram aceitos

---

## ?? Quando Usar

Use este template quando:
- ? Tomar uma decisão arquitetural significativa (tecnologia, padrão, infraestrutura)
- ? Escolher entre múltiplas opções técnicas com trade-offs claros
- ? Definir padrões que impactam múltiplos componentes/times
- ? Documentar escolhas que afetam NFRs (performance, segurança, escalabilidade)
- ? Reverter ou substituir uma decisão arquitetural anterior

**Não use** para decisões triviais ou de implementação de baixo nível.

---

## ?? Estrutura do Template

### ADR-[NÚMERO]: [TÍTULO DA DECISÃO]

```markdown
# ADR-[NÚMERO]: [Título Conciso da Decisão]

**Status**: [Proposto | Aceito | Rejeitado | Depreciado | Substituído por ADR-XXX]  
**Data**: YYYY-MM-DD  
**Autor(es)**: [Nome(s) do(s) Arquiteto(s)]  
**Stakeholders**: [Times/pessoas impactados]  
**Decisor**: [Quem aprovou]

---

## ?? Contexto

[Descreva o contexto que motivou esta decisão. O que está acontecendo? Por que precisamos decidir agora?]

**Fatores contextuais**:
- Requisitos de negócio relevantes
- Constraints técnicos existentes
- Deadlines ou dependências
- Estado atual da arquitetura

---

## ?? Decisão

**Decisão**: [Uma frase clara do que foi decidido]

[Descreva a decisão em detalhes: o que será implementado, como funcionará, quais tecnologias/padrões serão usados]

---

## ?? Alternativas Consideradas

### Opção 1: [Nome da Opção]
**Descrição**: [Como funcionaria]
**Prós**:
- ? [Vantagem 1]
- ? [Vantagem 2]

**Contras**:
- ? [Desvantagem 1]
- ? [Desvantagem 2]

---

### Opção 2: [Nome da Opção]
**Descrição**: [Como funcionaria]
**Prós**:
- ? [Vantagem 1]
- ? [Vantagem 2]

**Contras**:
- ? [Desvantagem 1]
- ? [Desvantagem 2]

---

### Opção 3: [Nome da Opção] (Escolhida)
**Descrição**: [Como funcionaria]
**Prós**:
- ? [Vantagem 1]
- ? [Vantagem 2]

**Contras**:
- ? [Desvantagem 1]
- ? [Desvantagem 2]

---

## ?? Análise de Trade-offs

| Critério | Opção 1 | Opção 2 | Opção 3 (Escolhida) |
|----------|---------|---------|---------------------|
| Performance | ??? | ?? | ???? |
| Scalability | ?? | ???? | ??? |
| Cost | ?????? | ?? | ???? |
| Complexity | ???? | ???????? | ?????? |
| Time to Market | ???? | ???????? | ?????? |
| Maintainability | ??? | ?? | ???? |

---

## ?? Justificativa

[Explique POR QUE a opção escolhida é a melhor dado o contexto. Quais fatores foram mais importantes?]

**Fatores decisivos**:
1. [Razão principal #1]
2. [Razão principal #2]
3. [Razão principal #3]

---

## ?? Consequências

### Positivas
- ? [Benefício esperado 1]
- ? [Benefício esperado 2]
- ? [Benefício esperado 3]

### Negativas (Trade-offs Aceitos)
- ?? [Trade-off 1 - como mitigar]
- ?? [Trade-off 2 - como mitigar]

### Riscos
- ?? **[Risco 1]**: [Como mitigar]
- ?? **[Risco 2]**: [Como mitigar]

---

## ??? Implementação

### Componentes Impactados
- [Componente/serviço 1]
- [Componente/serviço 2]

### Mudanças Necessárias
1. [Mudança técnica 1]
2. [Mudança técnica 2]
3. [Mudança de processo/documentação]

### Esforço Estimado
- **Desenvolvimento**: [X days/sprints]
- **Testing**: [X days]
- **Deployment**: [X days]

---

## ?? Checklist de Validação

Antes de aprovar este ADR, verificar:
- [ ] Contexto claramente explicado
- [ ] Pelo menos 2-3 alternativas consideradas
- [ ] Trade-offs documentados e aceitos
- [ ] Consequências (positivas e negativas) identificadas
- [ ] Riscos mapeados com planos de mitigação
- [ ] Stakeholders consultados
- [ ] Alinhamento com NFRs do projeto
- [ ] Impacto em custo avaliado
- [ ] Compatibilidade com arquitetura existente verificada

---

## ?? Referências

- [Link para requisitos relevantes]
- [Link para diagramas de arquitetura]
- [Documentação técnica de tecnologias escolhidas]
- [Benchmarks ou proofs-of-concept]
- [ADRs relacionados]

---

## ?? Notas Adicionais

[Qualquer informação adicional relevante, como limitações conhecidas, planos futuros de revisão, etc.]
```

---

## ?? Exemplos de ADRs

### Exemplo 1: ADR-001 - Escolha de Database

```markdown
# ADR-001: Uso de PostgreSQL como Database Principal

**Status**: Aceito  
**Data**: 2025-01-15  
**Autor(es)**: Wilson Santos (Arquiteto)  
**Stakeholders**: Time de Backend, Time de DevOps  
**Decisor**: Wilson Santos

---

## ?? Contexto

O sistema de gerenciamento de pedidos precisa de um banco de dados relacional para armazenar:
- Cadastro de clientes e produtos
- Histórico de pedidos e transações
- Relacionamentos complexos entre entidades

**Fatores contextuais**:
- Volume esperado: 1M de transações/mês
- Necessidade de ACID compliance
- Time já tem experiência com SQL
- Budget limitado (preferência por open-source)

---

## ?? Decisão

**Decisão**: Utilizaremos PostgreSQL 15+ como database principal do sistema.

Implementaremos PostgreSQL com:
- Connection pooling (PgBouncer)
- Read replicas para queries de leitura intensiva
- Backup automatizado diário
- Partitioning para tabelas de histórico

---

## ?? Alternativas Consideradas

### Opção 1: MySQL 8.0
**Prós**:
- ? Time tem mais experiência
- ? Comunidade maior

**Contras**:
- ? JSON support inferior ao PostgreSQL
- ? Window functions menos robustas

---

### Opção 2: SQL Server
**Prós**:
- ? Integração nativa com stack Microsoft
- ? Ferramentas empresariais robustas

**Contras**:
- ? Custo de licenciamento alto
- ? Lock-in com Microsoft

---

### Opção 3: PostgreSQL (Escolhida)
**Prós**:
- ? Open-source (zero custo de licença)
- ? JSON/JSONB support excelente
- ? Window functions completas
- ? Extensões (PostGIS, pg_stat_statements)

**Contras**:
- ? Curva de aprendizado para alguns membros do time
- ? Menos ferramentas GUI comparado a MySQL

---

## ?? Justificativa

PostgreSQL foi escolhido por:
1. **Custo**: Open-source elimina licenciamento
2. **Features**: JSON support será crítico para integração com APIs externas
3. **Performance**: Benchmarks mostraram 20% melhor performance em queries complexas
4. **Comunidade**: Ativa e em crescimento, com bom suporte

---

## ?? Consequências

### Positivas
- ? Economia de $15k/ano em licenças
- ? Flexibilidade com JSON data
- ? Performance otimizada para analytics

### Negativas (Trade-offs Aceitos)
- ?? 2 membros do time precisarão de 1 semana de treinamento

### Riscos
- ?? **Expertise inicial limitada**: Mitigado com treinamento e documentação interna
```

---

### Exemplo 2: ADR-002 - Microservices vs Monolith

```markdown
# ADR-002: Arquitetura Monolítica Modular para MVP

**Status**: Aceito  
**Data**: 2025-02-01  
**Decisão**: Iniciaremos com monolito modular, com plano de evolução para microservices.

**Justificativa**:
- Time pequeno (5 devs)
- Time-to-market crítico (4 meses)
- Domínio ainda sendo descoberto
- Infraestrutura de microservices adiciona complexidade desnecessária no MVP

**Plano de Evolução**:
Após 6 meses de produção, reavaliaremos para potencial migração de módulos específicos (pagamentos, notificações) para microservices baseado em:
- Volume de tráfego por módulo
- Necessidade de escala independente
- Maturidade do domínio
```

---

## ? Boas Práticas

### Numeração
- Use numeração sequencial: ADR-001, ADR-002, etc.
- Mantenha registro em `docs/adr/` no repositório
- Crie índice em `docs/adr/README.md`

### Escrita
- ? Seja conciso mas completo
- ? Use linguagem objetiva (evite jargão excessivo)
- ? Documente o CONTEXTO (não só a decisão)
- ? Sempre liste alternativas consideradas
- ? Seja honesto sobre trade-offs

### Processo
- ? ADRs são **imutáveis** - não delete ou edite após aprovação
- ? Se decisão mudar, crie novo ADR e marque o antigo como "Substituído por ADR-XXX"
- ? Revise ADRs em arquiteture review meetings
- ? Use pull requests para revisar ADRs antes de aprovar

---

## ?? Integração com Outros Artefatos

- **${AVANADE_ARCHITECTURE_TEMPLATE}**: ADRs detalham decisões mencionadas na arquitetura
- **${AVANADE_MEMORY_ARCHITECT_WILSON}**: ADRs alimentam a memória com decisões históricas
- **${AVANADE_TASK_ARCHITECTURE_QUALITY}**: ADRs são validados com este checklist

---

## ?? Referências

- [Documenting Architecture Decisions - Michael Nygard](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions)
- [ADR GitHub Organization](https://adr.github.io/)
- [When to Use ADRs](https://www.thoughtworks.com/radar/techniques/lightweight-architecture-decision-records)

---
