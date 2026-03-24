# Roadmap — FTD Educação CRM

> [← Voltar ao Início](./Home.md)

---

## 1. Visão Geral das Ondas

```
Mar 2026         Ago 2026           2027+
   │                 │                │
   ▼                 ▼                ▼
[MVP]          [Onda 1 Pós-MVP]   [Pós-Venda]
Time FTD       Avanade assume      Funcionalidades
desenvolve     desenvolvimento     avançadas
```

---

## 2. MVP — Deadline: 31 de Março de 2026

**Responsável**: Time FTD (Fernando, Kevellin)  
**Escopo mínimo validado com negócio**

### Entregas Confirmadas

| Entrega | Status | Descrição |
|---------|--------|-----------|
| Botão Spartan → Simulador | 🚧 Em dev | Redirect com contexto da oportunidade |
| Etapa 1: Dados da Proposta | 🚧 Em dev | Formulário completo: cliente, safra, alunado |
| Etapa 2: Grid de Produtos | 🚧 Em dev | Adição individual + totalizadores real-time |
| Reorganização de campos CRM | 🚧 Em dev | Simplificação das telas do Spartan |

### Fora do MVP (Confirmado)
- Adição em lote de produtos
- Etapa 3 (Benefícios/Doações/Patrocínios)
- Etapa 4 (Configuração de Canais de Venda)
- Motor de Aprovação integrado (elimina Vulcano)
- Cópia de proposta anterior + IPCA

---

## 3. Onda 1 Pós-MVP — Estimativa: ~Agosto 2026

**Responsável**: **Avanade assume o desenvolvimento**

### Funcionalidades Planejadas

| Prioridade | Funcionalidade | Valor de Negócio |
|-----------|---------------|-----------------|
| 🔴 Alta | **Adição em lote de produtos** | 200 cliques → ~5 cliques |
| 🔴 Alta | **Cópia de proposta anterior** | Com código substituto + reajuste IPCA |
| 🔴 Alta | **Motor de Aprovação D365** | Eliminar Vulcano (ferramenta externa) |
| 🔴 Alta | **Etapa 3: Benefícios/Doações/Patrocínios** | Completar simulador |
| 🔴 Alta | **Etapa 4: Configuração de Canais** | Sublistas por canal |
| 🟡 Média | **Revisão do cadastro de produtos** | Resolver poluição da tabela |
| 🟡 Média | **Higienização de base** | 101.000 contas + oportunidades desvinculadas |
| 🟡 Média | **Integração ISA ↔ TOTVS ↔ CRM** | Redesenho da sincronização de produtos |
| 🟡 Média | **Livreiro no CRM** | Migrar Pede Livreiro → D365 (Conta + Proposta) |

### Faxina Técnica (Onda 1)

| Item | Descrição |
|------|-----------|
| Canais de venda | Revisão completa dos modelos de venda |
| Cadastro de produto | Limpeza + flag Prateleira vs. Customizado |
| Tabela de preço | Revisão estrutural (atualmente 12+ tabelas mal usadas) |
| Contrato | Modularização de templates (50+ Word/PDF → sistema estruturado) |
| Security roles | Equalização entre ambientes Dev/OAT/PROD |
| Power Automate flows | Refatoração do fluxo de aprovação (muito extenso) |
| JavaScript legado | Migração para TypeScript/PCF (assessment já feito) |

---

## 4. Pós-Venda — 2027+

| Funcionalidade | Valor | Complexidade |
|---------------|-------|--------------|
| **Comissionamento do consultor** | Alta — consultor vê comissão em real-time | Alta |
| **Inteligência Comercial** | Upsell/cross-sell baseado em histórico | Alta |
| **Relatórios de Gestão** | Dashboards para liderança (funil, performance, filial) | Média |
| **IA em Propostas** | Sugestões automáticas de produtos | Muito alta (maturidade de dados baixa) |
| **Pacotes Pré-prontos** | Templates de proposta por perfil de escola | Média |
| **Tutorial de Produto** | Base de conhecimento integrada | Baixa |

---

## 5. Débitos Técnicos Críticos (Prioridade Imediata)

| Débito | Risco | Solução |
|--------|-------|---------|
| ⚠️ Dataverse em nível crítico de storage | **CRÍTICO** — risco de paralisação | Higienização urgente (arquivamento/purge) |
| ⚠️ Tabela de produtos poluída | Alto — impede funcionamento do Simulador | Flag Prateleira vs. Customizado + limpeza |
| ⚠️ Desalinhamento taxonômico ISA/TOTVS/CRM | Alto — dados inconsistentes | Redesenho da integração Onda 1 |
| ⚠️ 101.000 contas sem limpeza | Alto — dados inconsistentes | Higienização com apoio da equipe comercial |
| ⚠️ Oportunidades desvinculadas de propostas | Médio — bloqueia BI | Algoritmo de criação massiva |
| ⚠️ Contrato manual (50+ templates) | Médio — retrabalho | Modularização com jurídico Onda 1 |
| ⚠️ Alçadas mal parametrizadas | Médio — trava propostas no nível 4 | Revisão com Mônica (Comercial) |

---

## 6. Marcos de Decisão

| Marco | Data | Decisão |
|-------|------|---------|
| MVP Go-Live | 31/Mar/2026 | Simulador em produção (adição individual) |
| Revisão de Alçadas | Abr/2026 | Mônica finaliza novas regras |
| Kick-off Onda 1 Avanade | ~Mai/2026 | Avanade assume desenvolvimento |
| Eliminação do Vulcano | ~Set/2026 | Motor de aprovação D365 live |
| Integração ISA/TOTVS/CRM | ~Set/2026 | Sincronização redesenhada |

---

*Referências: [ftd-knowledge-base.md](../.avanade-method/docs/ftd-knowledge-base.md) | [especificacao-simulador-notion.md](../.avanade-method/docs/especificacao-simulador-notion.md)*
