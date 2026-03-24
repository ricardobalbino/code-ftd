# Sobre o Projeto — FTD Educação

> [← Voltar ao Início](./Home.md)

---

## 1. Quem é a FTD Educação?

A **FTD Educação S/A**, parte do **Grupo Marista**, é uma das maiores editoras e distribuidoras de materiais didáticos do Brasil.

| Dado | Valor |
|------|-------|
| **História** | +120 anos de atuação no Brasil |
| **Filiais** | 28 centros de distribuição nacionais |
| **Alcance** | ~1,5 milhão de estudantes impactados |
| **Pico Sazonal** | Novembro a Janeiro (adoção escolar): até **5.000 contratos/dia** |
| **Volume 2026** | Previsão de +500.000 pedidos de materiais didáticos e literatura |
| **Meta Financeira** | R$ 3 bilhões de faturamento até 2030 (estimativa 2025: R$ 1,5 bilhão) |

### Certificações e Reconhecimentos
- **Selo Top Employer 2025** — excelência em RH e gestão de pessoas
- **Top 50 Globais** — entre as maiores editoras mundiais (Publishers Weekly)
- **Compliance BNCC** — 100% dos materiais alinhados à Base Nacional Comum Curricular

---

## 2. O Projeto de Modernização CRM

### 2.1 Contexto
O projeto é uma modernização **brownfield** do CRM Dynamics 365 CE, já em produção há 1,5+ anos com 9 soluções customizadas e processos comerciais ativos.

### 2.2 Pain Point Central
O processo de criação de propostas comerciais é extremamente manual e lento:

| Problema | Causa | Impacto |
|---------|-------|---------|
| Consultor leva até **3 horas** por proposta | Propostas com até 200 produtos × 3 cliques cada | Baixa produtividade, gargalo comercial |
| Ferramentas externas paralelas (planilhas, Vulcano) | Ausência de interface adequada no CRM | Perda de rastreabilidade e dados históricos |
| Lentidão do CRM | Carregamento de forms, grids e dropdowns | Abandono do sistema |
| Tabela de produtos poluída | Filtro retorna 1.283 produtos (15 esperados) | Dificuldade em selecionar produtos corretos |

### 2.3 Objetivos Arquiteturais

1. Modernizar a jornada comercial completa via **Simulador Comercial** (Power Pages)
2. Eliminar ferramentas legadas externas (Vulcano, Pede Livreiro)
3. Estabelecer CRM D365 como **fonte de verdade** para contas
4. Racionalizar integrações com TOTVS, ISA, Lumisfera, Adobe Sign
5. Garantir escalabilidade para pico sazonal (5-10x volume normal)

---

## 3. Ecossistema Digital FTD

A FTD atua como ecossistema integrado de plataformas digitais:

| Plataforma | Descrição |
|-----------|-----------|
| **iônica** | Ambiente digital principal de aprendizagem |
| **Lumisfera** | E-commerce (plataforma para famílias comprarem material escolar) |
| **Pontue** | IA para correção de redações (adquirida pela FTD) |
| **Estuda.com** | Plataforma de avaliação e simulados (participação minoritária) |
| **Diário Escola** | Sistema de gestão e comunicação escolar (parceiro estratégico) |
| **Quantbot** | Robótica e programação gamificada (Azure/Microsoft) |

---

## 4. Aplicativos D365 em Uso

| Aplicativo | Área | Descrição |
|-----------|------|-----------|
| **Spartan** | Comercial | App principal de vendas (principal portal do consultor) |
| **PNLD** | Pública | Programa Nacional do Livro Didático |
| **Hub SAC** | CRC | Atendimentos ao cliente (Customer Service) |
| **Área do Cliente** | Online | Canvas App — escolas visualizam e aprovam propostas (squad separada) |
| **Adobe Sign** | Contratos | Assinatura digital de contratos (em vias de mudança de tecnologia) |

---

## 5. Stack Tecnológica

```
┌─────────────────────────────────────────────────────────────┐
│                    FRONTEND / PORTAIS                        │
│   Power Pages (Simulador)  │  Canvas App (Área do Cliente)  │
├─────────────────────────────────────────────────────────────┤
│              PLATAFORMA CRM CENTRAL                          │
│       Dynamics 365 CE Online (Sales + Customer Service)      │
│         Dataverse │ Power Automate │ PCF Controls             │
├─────────────────────────────────────────────────────────────┤
│                CAMADA DE SERVIÇOS AZURE                      │
│  Azure Functions │ Service Bus │ Event Grid │ Key Vault       │
│  Application Insights │ Datadog │ Grafana                    │
├─────────────────────────────────────────────────────────────┤
│                    INTEGRAÇÕES EXTERNAS                      │
│  TOTVS/Datasul (ERP) │ ISA (Produtos) │ Adobe Sign (Assin.)  │
│  Lumisfera (E-commerce) │ SMAX (GMUDs)                       │
└─────────────────────────────────────────────────────────────┘
```

| Camada | Tecnologia | Propósito |
|--------|-----------|-----------|
| **CRM** | D365 CE Online (Sales, CS) | Sistema central de gestão comercial |
| **Frontend Simulador** | Power Pages (SPA) | Interface de negociação consultiva |
| **Backend Logic** | C# Plugins (.NET) | Regras de negócio server-side |
| **Automação** | Power Automate Flows | Aprovações, notificações, sync |
| **Integrações Pesadas** | Azure Functions (.NET 8 Isolated) | ETL, cálculos >50 produtos |
| **Mensageria** | Azure Service Bus | Desacoplamento D365 ↔ TOTVS |
| **Eventos** | Azure Event Grid | Roteamento de eventos para Data Lake |
| **Segredos** | Azure Key Vault | Tokens, URLs, connection strings |
| **ERP** | TOTVS/Datasul | Faturamento, estoque, financeiro |
| **Catálogo** | ISA | Cadastro de produtos (família SKU) |
| **Assinatura** | Adobe Acrobat Sign | Contratos digitais |
| **E-commerce** | Lumisfera (Adobe Commerce) | FTD com Você, B2B Livreiros |
| **Observabilidade** | Datadog + Grafana + App Insights | Monitoramento e alertas |
| **CI/CD** | Azure DevOps Pipelines | Deploy e controle de versões |

---

## 6. Tipo de Projeto: Brownfield

Este projeto é **brownfield** — modificamos um ambiente D365 CE já em produção com:

- **9 soluções customizadas** (segmentadas por dependência)
- **Plugins C#** em produção há 1,5+ anos
- **Power Automate flows** existentes (alguns precisam refatoração)
- **Web Resources JavaScript** (legado, sendo migrados para TypeScript/PCF)
- **101.000+ contas** ativas (precisam higienização)
- Histórico de oportunidades e propostas sem vínculo entre si

### Princípio Brownfield
> Não "reescrever do zero" — evoluir o que existe, refatorar o que bloqueia, e construir o novo (Simulador) sobre a base existente.

---

*Referências: [README.md](../README.md) | [ftd-knowledge-base.md](../.avanade-method/docs/ftd-knowledge-base.md)*
