# Discovery — Aprovação de Propostas

**Projeto**: FTD Educação — Transformação CX  
**Data**: 18/Mar/2026  
**Fontes**: Especificação Notion (Oscar de Rooij), transcrições refinamento 16-17/Mar, onboarding 09-10/Mar  
**Spec completa**: `docs/especificacao-aprovacao-notion.md`

---

## 1. Visão Geral

A funcionalidade **Aprovação de Propostas** estende o motor de alçadas dinâmico existente no CRM (D365 CE) de 5 para 7 níveis, para automatizar 100% do fluxo de aprovações comerciais, **eliminando o sistema Vulcano** do processo.

**Contexto técnico**: O CRM já possui fluxo de aprovação funcional nos níveis 1-5. Esta entrega adiciona níveis 6 (Superintendência) e 7 (Presidência) ao fluxo existente, sem refatoração do motor atual.

**Pain points centrais**:
- Dupla aprovação (CRM + Vulcano) gerando retrabalho
- Aprovadores sem visão consolidada para tomada de decisão rápida
- Consultor sem visibilidade de onde a proposta está parada
- Regras de alçada dependem de conhecimento tácito

**Objetivo**: Orquestrar aprovações internas da FTD em um único sistema, com interface moderna (Power Pages/Simulador Comercial Etapa 6), mobile, com rastreabilidade completa.

---

## 2. Contexto AS-IS

O processo de aprovação é atualmente difuso e carente de governança sistêmica:

- **Vulcano**: sistema paralelo obrigatório para aprovações — será eliminado
- **CRM**: usado apenas para formalização tardia
- **Comunicação**: propostas tramitam por e-mail, WhatsApp, reuniões informais
- **Duplicidade**: mesma aprovação registrada em CRM E Vulcano
- **Informações fragmentadas**: aprovador precisa consultar CRM + Excel simulador anexado
- **Tempo**: aprovação longa, sem rastreabilidade entre sistemas

### 6 Dores Mapeadas
1. Processos em múltiplos sistemas (CRM, TOTVS, Vulcano) → ineficiência/erros
2. Duplicidade aprovações Vulcano ↔ CRM → retrabalho
3. Validação com informações fragmentadas (CRM parte + Excel parte)
4. Aprovadores sem visualização rápida de indicadores críticos (descontos, royalties, adiantamentos)
5. Tempo de aprovação longo + consultor sem visibilidade do paradeiro
6. Regras de alçada dependem de conhecimento tácito ou consulta manual

---

## 3. Alçadas de Aprovação (Extensão 1-5 para 7 Níveis)

| Nível | Aprovadores (cumulativo) | Status CRM |
|-------|-------------------------|------------|
| **1** | Consultor (auto-aprovação) | Existe |
| **2** | Nível 1 + Coordenador + Gerente de Filial | Existe |
| **3** | Nível 2 + Diretor Adjunto | Existe |
| **4** | Nível 3 + Diretor Comercial (Quintela) | Existe (atual teto) |
| **5** | Nível 4 + Diretor-Geral (Alisson) | **NOVO** — precisa criar |
| **6** | Nível 5 + Superintendência | **NOVO** — precisa criar |
| **7** | Nível 6 + Presidência | **NOVO** — precisa criar |

**📌 IMPORTANTE - Escopo de Implementação:**
- **Níveis 1-4**: Já existem e funcionam no CRM atual
- **Nível 5**: Existe no modelo de dados mas pode não estar sendo utilizado (validar)
- **Níveis 6-7**: **NOVOS** — precisam ser criados (dados mestres + aprovadores + ajustes no código)
- **Motor de cálculo**: Já funciona, apenas remover limitador se houver (validar código)
- **Fluxos Power Automate**: Já existem, validar se contemplam até nível 7

**Observações:**
- Coordenador/Gerente definidos pela **região da conta** (varia por filial)
- Diretor Adjunto+ é **pessoa fixa** (não varia por região)
- Controladoria e Financeiro acompanham a partir do Nível 4 (somente leitura, capturam dados)
- Novos usuários que hoje acessam Vulcano precisarão de acesso CRM (mapeamento pendente)

---

## 4. Política Comercial — Gatilhos de Alçada

### Regras Percentuais
| Indicador | Condição | Nível |
|-----------|----------|-------|
| **Royalties** | ≤ 50% | 2 |
| | > 50% e ≤ 70% | 3 |
| | > 70% | 4 |
| **Adiantamento** | < 50% | 4 |
| | ≥ 50% | 4 |
| **Patrocínio** | Sem patrocínio | 1 |
| | ≤ 3% | 2 |
| | > 3% | 3 |
| **Taxa Administração** | < 5% | 4 |
| | ≥ 5% e ≤ 20% | 1 |
| | > 20% | 2 |

### Regras de Valores Brutos
| Indicador | Condição | Nível |
|-----------|----------|-------|
| **Adiantamento** | Sem adiantamento | 1 |
| | ≤ R$ 100K | 4 |
| | > R$ 100K | 5 |
| **Royalties** | Sem royalties | 1 |
| | < R$ 50K | 2 |
| | ≥ R$ 50K | 4 |
| | ≥ R$ 250K | 5 |
| | ≥ R$ 500K | 6 |
| | ≥ R$ 1M | 7 |

### Regras de Parcelamento
| Condição | Nível |
|----------|-------|
| Até 6 parcelas | 1 |
| ≥ 7 parcelas | 4 |

> **Resultado**: O nível **mais alto** entre TODAS as análises define o nível de aprovação.

### Regras existentes (fora de escopo)
- Regra de desconto de produto — já implementada no CRM
- Regra de existência de contrato — já implementada no CRM

---

## 5. Implementação Técnica Atual

| Componente | Detalhe | Status |
|------------|---------|--------|
| **Plugin** | Roda na ativação da proposta → analisa infos → define nível por regra | Existente |
| **Power Automate** | Dispara após ativação → consulta nível → busca aprovadores → notificação sequencial | Existente (em refatoração) |
| **Regras** | Possivelmente hardcoded no plugin (a confirmar) ou em tabela | **GAP — precisa investigar** |
| **Notificações** | Teams (confirmado) + e-mail (a confirmar) | Parcialmente existente |
| **Motor** | Não será alterado até novas regras da consultoria pricing | Decisão 16/Mar |

**Incerteza crítica**: Regras hardcoded no plugin ou parametrizadas em tabela? → Impacta arquitetura e manutenção futura.

---

## 6. Fluxo de Aprovação — Comportamento Detalhado

### Início do Fluxo
1. Cliente aprova proposta (hoje: WhatsApp; futuro: portal Área do Cliente)
2. Status mantém `Aberta`; Estágio muda de `Em negociação` → `Em aprovação`
3. Oportunidade sincroniza: etapa também "Em aprovação" (nunca regride)
4. CRM calcula alçadas com base nas condições comerciais
5. Notifica primeiro aprovador via **Email + Teams** com link para Etapa 6 do Simulador

### Aprovação Sequencial
- Níveis são cumulativos (N1 → N2 → N3 → ...)
- Cada aprovação: registro `usuário + timestamp`
- Notificação de aprovação parcial enviada ao Consultor
- Última aprovação → Estágio: `Aprovada sem assinatura`

### Recusa e Resubmissão
1. Aprovador recusa com **justificativa obrigatória** (registrada)
2. Estágio → `Recusada`; Consultor notificado com razão + link
3. Consultor analisa e decide:
   - **Criar nova versão**: copia todos dados, incrementa ID revisão
   - **Encerrar negociação**: Status → Histórico, Oportunidade → Perda

### Regra de "Pulo" (Resubmissão Inteligente)
| Cenário | Comportamento |
|---------|---------------|
| Totalizadores **diferentes** da versão anterior | Nova aprovação desde o início (cliente precisa reaprovar antes) |
| Totalizadores **iguais** + Estágio anterior `Recusada` | Pula para alçada que recusou; anteriores marcadas como "aprovação na proposta anterior" |
| Totalizadores **iguais** + Estágio anterior `Assinatura recusada` | Todas aprovações herdadas; Consultor + Jurídico + Administrativo ajustam contrato |

**Totalizadores monitorados**: Receita Bruta, Receita após Benefícios e Patrocínios, Total Investido na Escola, Adiantamento, Parcelas.

### Perda de Oportunidade
Razões padronizadas:
- Preço / Condições Comerciais
- Logística
- Plataforma eCommerce
- Plataforma educacional
- Atendimento Pedagógico
- Qualidade dos materiais didáticos
- Escola não se adaptou ao material didático
- Mudança de mantenedora / decisão da rede
- Uso de material próprio
- Escola encerrou atividades
- Outros (especificar)

---

## 7. Pós-Aprovação — Assinatura

### Proposta aprovada internamente
1. Estágio: `Aprovada sem assinatura`
2. Equipe Administrativa Comercial **confecciona contrato manualmente**
3. Contrato anexado no CRM → enviado via **Adobe Sign** (integração existente)

### Assinatura Docusign/Adobe Sign
| Evento | Ação |
|--------|------|
| **Assinado** | Estágio → `Aprovada e assinada`; Status → `Ativa`; Proposta anterior → `Histórico`; Pedido criado (cópia travada) |
| **Recusado** | Estágio → `Assinatura recusada`; Consultor decide: encerrar ou criar nova versão |

### Pedido
- = cópia da proposta ativa (snapshot do que foi contratado)
- Ponte equipe comercial → operações/Customer Success
- **Travado** — sem alteração pós-criação
- Base futura para Faturas (royalties, adiantamento, patrocínio)

---

## 8. Notificações

| Evento | Destinatário | Canal | Conteúdo |
|--------|-------------|-------|----------|
| Proposta para aprovação | Aprovador da alçada em vigor | Email + Teams | Link Etapa 6 + ênfase aprovação pendente |
| Aprovação parcial | Consultor | Email + Teams | Quem aprovou + próximo aprovador |
| Proposta aprovada (final) | Consultor + Coordenador + Gerente | Email + Teams | Ênfase aprovação final |
| Proposta recusada | Consultor | Email + Teams | Quem recusou + razão + link proposta |
| Oportunidade perdida | Coordenador + Gerente | Email + Teams | Ênfase perda + razão |
| Proposta assinada | Todos envolvidos | Email + Teams | Celebrativo + link |
| Assinatura recusada | Consultor + Coordenador + Gerente | Email + Teams | Ênfase recusa assinatura |

---

## 9. Cadeia de Propostas (Versionamento)

```
Proposta 1.0 (recusada) — Safra 2024
  └→ Revisão 1.1 (recusada)
      └→ Revisão 1.2 (recusada)
          └→ Revisão 1.3 (recusada)
              └→ Revisão 1.4 (**aprovada e assinada**)
Proposta 2.0 (recusada) — Safra 2025 (vínculo com 1.4)
  └→ Revisão 2.1 (**aprovada e assinada**)
```

**Regras:**
- Versão 1 de qualquer proposta → **sempre** passa todas alçadas
- Versão 2+ após recusa → pode "pular" se totalizadores não mudaram
- Aditivo = alteração sobre contrato ativo; quando aprovado, aditivo → Ativo, anterior → Histórico
- Campo Contrato vs. Aditivo será adicionado (não existe hoje)

---

## 10. Manutenção da Política Comercial

- Motor atual **não será alterado** até chegarem novas regras (consultoria pricing — final Mar/2026)
- Necessidade de ajuste de valores sem desenvolvimento (interface admin ou ticket)
- Manutenção de aprovadores (férias, substituição, saída) precisa ser viável sem deploy
- Quando novas regras chegarem: avaliar se motor atende ou precisa ser **refeito**

---

## 11. Dependências

| Dependência | Status | Impacto |
|-------------|--------|---------|
| Simulador Comercial (Etapa 6) | Em desenvolvimento | Interface de aprovação vive dentro do Simulador |
| Status/Estágios da Proposta | Precisa criar | Prerequisito para todo o fluxo |
| Cadastro de aprovadores por região | Parcialmente existe | Níveis 5-7 precisam ser adicionados |
| Integração Adobe Sign/Docusign | Existente | Recebe status assinatura |
| Onboarding novos usuários CRM | Pendente levantamento | Quem usa Vulcano precisa migrar |

---

## 12. Métricas de Performance

| Pilar | Métrica | Atual | Meta |
|-------|---------|-------|------|
| Growth | Tempo médio total até assinatura | Não medido | - |
| Growth | Tempo médio aprovação propostas (fluxo interno) | Não medido | - |
| Growth | Tempo médio aprovação cada alçada | Não medido | - |
| Org. Effectiveness | Tempo para aprovador entender proposta | Alto (vários sistemas) | Minutos (Etapa 6) |

---

## 13. Fora de Escopo

- **Aprovação do Cliente**: jornada do cliente via link externo → Spec Simulador Comercial
- **Geração de Contratos (PDF)**: geração automática de documento jurídico → fase futura
- **Aprovação de pagamento Adiantamento/Patrocínio/Royalties**: continuam no Vulcano → outro momento

---

## 14. Gaps de Investigação

| # | Gap | Responsável | Prazo |
|---|-----|-------------|-------|
| 1 | Regras de alçada hardcoded ou parametrizadas no plugin? | Avanade + Julio/Fernando | Imediato |
| 2 | Novos usuários CRM (ex-Vulcano) — mapeamento completo | Oscar + Silvia Lima | A definir |
| 3 | Resultado consultoria pricing → pode mudar motor de regras | Equipe Comercial FTD | Fim Mar/2026 |
| 4 | Coexistência 2 times (FTD + Avanade) no pipeline deploy | Danilo + Julio | Em andamento |
| 5 | Security roles: equalizar entre ambientes | Julio | Roadmap |
