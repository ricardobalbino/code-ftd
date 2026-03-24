---
applyTo: "**"
---

# FTD Educação - Contexto Global

**Projeto**: FTD Educação S/A (Grupo Marista) — Modernização CRM
**Stack**: D365 CE Online (Sales, CS), Power Pages (Simulador), Azure Functions, TOTVS/Datasul, ISA, Adobe Sign
**Tipo**: Brownfield — customizações múltiplas em ambiente existente

## Docs Mandatórios (ler antes de qualquer tarefa)
- `docs/ftd-knowledge-base.md` — Knowledge base completo
- `docs/discovery/ftd-discovery.md` — Discovery com fit-gap
- `docs/especificacao-simulador-notion.md` — Spec do Simulador (501 linhas)
- `.avanade-method/configs/d365-config.yaml` — Config D365 com dados reais
- `.github/instructions/avanade-bca-guidelines.instructions.md` — Diretrizes BCA (padrões de dev Avanade)
- `docs/diretriz-avanade-inventory.md` — Inventário completo 197 docs BCA (referência detalhada)

## Contexto Chave
- **Simulador Comercial**: Power Pages + CRM backend, MVP deadline 31/Mar/2026
- **Pain point principal**: Consultor leva até 3 horas para criar 1 proposta (200 produtos × 3 cliques)
- **Integrações**: TOTVS/Datasul (ETL 1x/dia 6h), ISA (produtos), Adobe Sign (contratos)
- **Ambientes**: DEV, OAT (pipeline), PROD (pipeline + GMUD), QA e RC (não configurados)

## Padrões de Desenvolvimento Avanade (BCA v3)
- **Backend**: Plugin<T> → Service → Repository com DI (Unity). [PluginRegistration] attribute. Early Bound obrigatório.
- **Frontend**: TypeScript obrigatório. Contract/Controller pattern. Fluent Rules. Early Bound Forms.
- **Testing**: Shift-left. MSTest+NSubstitute (C#), Jest (TS). Coverage mandatória.
- **Pipelines**: 4 YAML — Guard (CI), Deploy Code, Deploy (MANUAL), Export-Unpack.
- **Fluxo**: Customization Master → unpack CLI → commit #workitem → PR → guard build → deploy manual.
- **Referência completa**: Ver `.github/instructions/avanade-bca-guidelines.instructions.md`
