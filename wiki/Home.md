# Wiki — FTD Educação | Dynamics 365 CE

> **Projeto de Modernização CRM** — Avanade × FTD Educação S/A (Grupo Marista)

Bem-vindo à wiki do projeto de **Transformação CX da FTD Educação**. Esta wiki consolida toda a documentação técnica, de negócio e metodológica do projeto de modernização do CRM Dynamics 365.

---

## 📋 Índice

| Seção | Descrição |
|-------|-----------|
| [Sobre o Projeto](./01-Sobre-o-Projeto.md) | Contexto, objetivos, cliente e ecossistema digital |
| [Arquitetura de Solução](./02-Arquitetura.md) | Diagramas C4, containers, componentes e decisões arquiteturais |
| [Simulador Comercial](./03-Simulador-Comercial.md) | Especificação completa, MVP e as 6 etapas do simulador |
| [Processo Comercial](./04-Processo-Comercial.md) | Jornada completa: conta → contrato → Adobe Sign → TOTVS |
| [Modelo de Dados](./05-Modelo-de-Dados.md) | Entidades Dataverse, campos customizados `ftd_*` e relacionamentos |
| [Integrações](./06-Integracoes.md) | TOTVS/Datasul, ISA, Adobe Sign, Lumisfera, Azure Service Bus |
| [Padrões de Desenvolvimento](./07-Padroes-de-Desenvolvimento.md) | BCA v3: C# Plugins, TypeScript, testes, pipelines CI/CD |
| [Ambientes e Infraestrutura](./08-Ambientes-e-Infraestrutura.md) | D365 envs, Azure DevOps, Azure Key Vault, deploys |
| [Times e Stakeholders](./09-Times-e-Stakeholders.md) | Equipes FTD e Avanade, papéis e cerimônias |
| [Roadmap](./10-Roadmap.md) | MVP, Onda 1, entregas futuras e priorização |
| [Glossário](./11-Glossario.md) | Termos específicos do domínio FTD |

---

## 🎯 Contexto Rápido

| Item | Detalhe |
|------|---------|
| **Cliente** | FTD Educação S/A (Grupo Marista) |
| **Tipo** | Brownfield — D365 CE Online em produção há 1,5+ anos |
| **Plataforma** | D365 CE (Sales + CS), Power Pages, Azure Functions |
| **Principal Entrega** | Simulador Comercial (Power Pages) |
| **Deadline MVP** | 31 de Março de 2026 |
| **Pain Point Central** | Consultor leva até **3 horas** para criar 1 proposta |
| **Meta** | Reduzir criação de proposta para **< 15 minutos** |
| **Metodologia** | Avanade BCA v3 (Best Practice Accelerator) |

---

## 🏗️ Arquitetura em Uma Linha

```
Power Pages (Simulador) → Dataverse Web API → D365 CE → Azure Functions → TOTVS/ISA/Adobe Sign
```

---

## 🔗 Documentação Relacionada

- **Especificação Completa do Simulador**: `.avanade-method/docs/especificacao-simulador-notion.md`
- **Knowledge Base FTD**: `.avanade-method/docs/ftd-knowledge-base.md`
- **BCA Guidelines**: `.github/instructions/avanade-bca-guidelines.instructions.md`
- **D365 Config**: `.avanade-method/configs/d365-config.yaml`
- **Docs Arquitetura**: `docs/arquitetura/`
- **Discovery**: `docs/discovery/`
- **PRDs**: `docs/prd/`

---

*Última atualização: Março 2026 | Versão: 1.0*
