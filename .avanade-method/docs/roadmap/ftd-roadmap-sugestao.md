# Roadmap — FTD CRM Modernização
## Status: 📋 SUGESTÃO (Aguardando Validação)
## Data: 17 Março 2026
## Fontes: Discovery (9-16/Mar), Especificação Simulador (Oscar), Fit-Gap Analysis

---

## Princípios de Organização
- **Sequenciamento por dependência**: dados mestres antes de funcionalidades que os consomem
- **Valor x Risco**: itens bloqueantes sobem de prioridade mesmo se "menor escopo"
- **Premissa confirmada**: todo entregável acompanhado de relatório/BI (16/Mar)

---

## FASE 0 — Fundação de Dados (Pré-requisito para tudo)
**Período**: Imediato → ~Abr/2026
**Tema**: "Sem dados limpos, nada funciona"

| # | Item | Tipo | Bloqueado por | Desbloqueia | Owner |
|---|------|------|---------------|-------------|-------|
| 0.1 | **Brainstorm TOTVS** — alinhar taxonomia produto ISA/TOTVS/CRM | Workshop | - | 0.2, 0.3, 0.4 | Júlio + Fernando + Mônica |
| 0.2 | **Normalização cadastro de produto** — metadados (customizado/prateleira, LN, série, componente, safra) | Data + Config | 0.1 | Simulador Etapa 2 | FTD CRM + ISA |
| 0.3 | **Consolidação tabelas de preço** (12→5 ou 1 com flags) | Config | 0.1 + Consultoria pricing | Simulador Etapa 1 | Oscar + Pricing ext. |
| 0.4 | **Modelagem variação de produto** (Ponto e Ponto, Trilhas) | Data | 0.1, 0.2 | Adição em lote | FTD CRM |
| 0.5 | **Limpeza campos Oportunidade ↔ Proposta** (remover duplicados vermelhos) | Config | - | Fluxo Aprovação, Etapa 6 | Oscar + Avanade |
| 0.6 | **Diagnóstico de contas** (101K, recorte 2025-2026, relatório quantitativo) | Data + BI | - | 0.9, 0.10 | Avanade + FTD |
| 0.7 | **Categorização de contas** — nova hierarquia 3 níveis (segmento/tipo/classificação) | Config | Oscar + Comercial | Relatórios, Simulador | Oscar |
| 0.8 | **Separação campo Mantenedora** (3 usos → 3 campos) | Config | 0.7 | Livreiro, Simulador Etapa 1 | Avanade |
| 0.9 | **Higienização CNPJ** (escolas com CNPJ de livraria) | Data | 0.6 | Faturamento correto | FTD + Avanade |
| 0.10 | **Criação massiva oportunidades 2025-2026** + vínculo a propostas | Pro-code | 0.5, 0.6 | Relatórios, Funil, Simulador | Avanade |

---

## FASE 1 — MVP Simulador + Fluxo de Aprovação
**Período**: Mar-Mai/2026
**Tema**: "Consultor começa a usar / Aprovação digital funciona"

| # | Item | Tipo | Bloqueado por | Desbloqueia | Owner |
|---|------|------|---------------|-------------|-------|
| 1.1 | **MVP Simulador** — adição individual de produtos (Power Pages) | Pro-code | 0.2 (parcial) | 1.3, 1.4 | FTD (Fernando + Kevellin) |
| 1.2 | **Motor de Aprovação** — sequência + regras de transição (sem alçada) | Low-code + Pro-code | 0.5 | 1.5 | Avanade + FTD |
| 1.3 | **Etapa 1: Contexto da Negociação** — formulário, modelos de venda, lista de preço base | Pro-code | 0.3, 0.8 | 1.4 | Avanade |
| 1.4 | **Etapa 6: Revisão e Compartilhamento** — big numbers, comparativo, justificativa | Pro-code | 0.5, 1.1 | 1.5 | Avanade |
| 1.5 | **Regras de alçada** (conectar motor a regras comerciais da Mônica) | Config | 1.2, Mônica | Eliminação Vulcano | Avanade + FTD |
| 1.6 | **Eliminação Vulcano** (aprovação duplicada) | Config | 1.5 | - | FTD |
| 1.7 | **Status/Estágio da Proposta** — simplificação 3→2 campos + funil analítico | Config | - | Kanban, Relatórios | Avanade |

---

## FASE 2 — Simulador Completo (Onda 1)
**Período**: Mai-Ago/2026
**Tema**: "Proposta em 15 minutos, não 3 horas"

| # | Item | Tipo | Bloqueado por | Desbloqueia | Owner |
|---|------|------|---------------|-------------|-------|
| 2.1 | **Etapa 2: Produtos e Serviços** — adição em lote, totalizadores, taxa de administração, alçadas | Pro-code | 0.2, 0.4, 1.1 | 2.2 | Avanade |
| 2.2 | **Etapa 3: Benefícios, Doações, Patrocínio, Adiantamento** | Pro-code | 2.1 | 2.3 | Avanade |
| 2.3 | **Etapa 4: Configuração de Vendas/Canais** — sublistas, kits por série, logística | Pro-code | 2.1 | 2.7 | Avanade |
| 2.4 | **Copiar proposta anterior** (código substituto + reajuste IPCA) | Pro-code | 1.1, 0.3 | Renovações | Avanade |
| 2.5 | **Versionamento de Propostas** — encadeamento, status Aberto/Ativo/Histórico | Pro-code | 1.7 | 2.6 | Avanade |
| 2.6 | **Navegação: Dashboard + Kanban + Busca + Notificações** | Pro-code | 2.5 | Adoção | Avanade |
| 2.7 | **Visualização de Proposta pelo Cliente** — link online, comentários, aprovação/recusa | Pro-code | 2.3, 2.5 | Ciclo completo | Avanade |

---

## FASE 3 — Migração Livreiro + Integrações
**Período**: Ago-Nov/2026
**Tema**: "Todos os canais no CRM, Pede Livreiro morre"

| # | Item | Tipo | Bloqueado por | Desbloqueia | Owner |
|---|------|------|---------------|-------------|-------|
| 3.1 | **Cadastro livreiro no CRM** (conta + carteirização) | Config + Data | 0.7, 0.8 | 3.2 | FTD + Avanade |
| 3.2 | **Proposta livreiro** (vendedor independente + parceiro comercial) | Pro-code | 3.1, 2.3 | 3.3 | Avanade |
| 3.3 | **Integração Lumisfera B2B Livreiros** | Pro-code | 3.2 | 3.4 | Avanade + Lumisfera |
| 3.4 | **Eliminação Pede Livreiro** | Migração | 3.3 | - | FTD |
| 3.5 | **Redesign integração ISA ↔ TOTVS ↔ CRM** (cadastro de produto unificado) | Pro-code | 0.1, 0.2 | Sustentação | Avanade |
| 3.6 | **Migração Adobe Sign** (nova tecnologia) | Config | - | Contratos digitais | FTD |

---

## FASE 4 — Gestão Data-Driven + Otimização
**Período**: Nov/2026-Fev/2027
**Tema**: "Decisões baseadas em dados, não feeling"

| # | Item | Tipo | Bloqueado por | Desbloqueia | Owner |
|---|------|------|---------------|-------------|-------|
| 4.1 | **Captura de dados de uso** (tempo de manuseio, timestamps estágio/status) | Pro-code | 2.5, 2.6 | 4.2 | Avanade |
| 4.2 | **Dashboards BI** — Growth, Cost Optimization, Effectiveness, Digital Enablement | BI | 4.1, 0.10 | - | Avanade + FTD |
| 4.3 | **Etapa 5: Matriz de Serviços** (revisão otimizada) | Config | 2.3 | - | FTD |
| 4.4 | **Modularização contratos Word** (50+ templates) | Config | Jurídico | Eficiência | FTD |
| 4.5 | **Security roles equalização** | Config | - | Governança | FTD CRM |
| 4.6 | **Pipeline DevOps** — deploy automático plugins para Dev, repo acoplado a PBIs | DevOps | - | Produtividade | FTD CRM |

---

## FASE 5 — Futuro / Inovação
**Período**: 2027+
**Tema**: "Inteligência sobre base madura"

| # | Item | Tipo | Owner |
|---|------|------|-------|
| 5.1 | Comissionamento no simulador | Pro-code | TBD |
| 5.2 | Inteligência comercial (upsell, cross-sell, pacotes) | IA/ML | TBD |
| 5.3 | IA em propostas (recomendações, predição de aprovação) | IA/ML | TBD |
| 5.4 | Offline completo no simulador (PWA/service workers) | Pro-code | TBD |
| 5.5 | Plano de ação Dataverse storage (archive, elastic tables) | Infra | TBD |

---

## Diagrama de Dependências (Simplificado)

```
FASE 0 (Fundação)           FASE 1 (MVP+Aprovação)       FASE 2 (Simulador)          FASE 3 (Livreiro)
─────────────────           ──────────────────────       ─────────────────           ──────────────

0.1 Brainstorm TOTVS ─┬──► 0.2 Normalizar Produto ───► 1.1 MVP Simulador ────────► 2.1 Etapa 2 Lote
                      ├──► 0.3 Tabelas Preço ─────────► 1.3 Etapa 1 Contexto       2.2 Etapa 3 Benefícios
                      └──► 0.4 Variações Produto ──────────────────────────────────► 2.1

0.5 Limpar Campos ────────► 1.2 Motor Aprovação ──────► 1.5 Alçadas ──────────────► 1.6 Matar Vulcano
                    ──────► 1.4 Etapa 6 Revisão

0.6 Diagnóstico Contas ──► 0.9 CNPJ ──────────────────────────────────────────────────────────────
                    ──────► 0.10 Criar Oportunidades ─────────────────────────────► 4.2 BI

0.7 Categorização ────────► 0.8 Mantenedora ──────────────────────────────────────► 3.1 Livreiro CRM
                                                                                    3.2 Proposta Livreiro
                                                                                    3.3 Lumisfera B2B
                                                                                    3.4 Matar Pede Livreiro
```

---

## Resumo por Números

| Fase | Qtd Itens | Período | Tipo Predominante |
|------|-----------|---------|-------------------|
| 0 - Fundação | 10 | Imediato → Abr/2026 | Data + Config |
| 1 - MVP + Aprovação | 7 | Mar-Mai/2026 | Pro-code + Config |
| 2 - Simulador Completo | 7 | Mai-Ago/2026 | Pro-code |
| 3 - Livreiro + Integrações | 6 | Ago-Nov/2026 | Pro-code + Migração |
| 4 - Data-Driven | 6 | Nov/2026-Fev/2027 | BI + Config + DevOps |
| 5 - Futuro | 5 | 2027+ | IA/ML + Infra |
| **Total** | **41 itens** | | |

---

## Riscos e Dependências Externas

| Risco | Impacto | Mitigação |
|-------|---------|-----------|
| Consultoria pricing atrasa (esperada final mar/2026) | Bloqueia 0.3, atrasa Fase 1 | Avançar com modelo 5 tabelas provisório |
| Mônica não define alçadas a tempo | Bloqueia 1.5, mas motor (1.2) avança independente | Entregar motor sem alçadas, plugar depois |
| TOTVS não permite alterar tabelas-core | Bloqueia normalização ISA/TOTVS/CRM | Criar camada de tradução no pipeline ETL |
| Dataverse storage atinge limite | Bloqueia novas features | Elastic tables ou archive para dados históricos |
| Capacidade FTD CRM (5 pessoas para sustentação + projeto) | Atrasa tudo | Priorizar rigorosamente, Avanade absorve mais |

---

## Próximos Passos

1. **Validação com Oscar/Danilo** — prioridades e sequenciamento fazem sentido?
2. **Decomposição em Épicos** (Paula - PO) — cada item do roadmap vira épico com acceptance criteria
3. **Sprint Planning** (Roberto - SM) — Fase 0 e Fase 1 cabem nas primeiras sprints?
4. **Validação Arquitetural** (Wilson - Architect) — dependências técnicas estão corretas?
5. **PRD Formal** (João - PM) — formalizar roadmap aprovado como PRD do projeto
