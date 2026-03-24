# Integrações — FTD Educação

> [← Voltar ao Início](./Home.md)

**Referência**: `docs/arquitetura/d365-integration-landscape.md`

---

## 1. Mapa Geral de Integrações

```
                    ┌──────────────────────────────┐
                    │     Dynamics 365 CE          │
                    │     (Dataverse)              │
                    └──────────┬───────────────────┘
                               │
          ┌────────────────────┼────────────────────┐
          ▼                    ▼                     ▼
   ┌──────────────┐   ┌──────────────────┐   ┌────────────────┐
   │ TOTVS/Datasul│   │   ISA (Produtos) │   │  Adobe Sign    │
   │    (ERP)     │   │  (Catálogo SKU)  │   │  (Contratos)   │
   └──────────────┘   └──────────────────┘   └────────────────┘
          │
   ┌──────────────┐
   │  Lumisfera   │
   │ (E-commerce) │
   └──────────────┘
```

---

## 2. TOTVS / Datasul (ERP)

### Descrição
Sistema ERP corporativo da FTD — fonte de verdade para faturamento, estoque e financeiro.

### Fluxo de Integração
| Direção | Dados | Frequência |
|---------|-------|-----------|
| TOTVS → CRM | Contas (código ERP, dados cadastrais) | ETL 1x/dia às 6h |
| CRM → TOTVS | Pedidos confirmados, contratos assinados | Após aprovação Adobe Sign |

### Campos Sincronizados
| Campo CRM | Campo TOTVS | Observação |
|-----------|-------------|------------|
| `accountnumber` | Código Cliente | Sync automático 1x/dia |
| `ftd_cnpj` | CNPJ | Campo chave para matching |
| `name` | Razão Social | Da Receita Federal |

### Problemas Conhecidos
- ⚠️ **Cadastro duplo**: criar em AMBOS os sistemas e aguardar sincronização
- ⚠️ **Não existe owner da informação**: cada área cadastra onde quiser
- ⚠️ **Alinhamento CNPJ**: gap analysis CRM × TOTVS necessário
- ⚠️ **Marcio Jovanello**: "CRM deveria ser fonte de verdade para contas"

### Arquitetura de Integração
```
TOTVS ──[Azure Service Bus]──▶ Azure Function ──▶ Dataverse
CRM   ──[Azure Service Bus]──▶ Azure Function ──▶ TOTVS
```

- **Responsável**: Thiago Veiga (arquiteto de integrações — time separado)
- **CRM não expõe APIs**: consome endpoints do time Veiga
- **Batch projects**: atualizações em massa executadas em VM solicitada ao time de infra

---

## 3. ISA (Sistema de Cadastro de Produto)

### Descrição
Sistema legado de cadastro de produtos da FTD — fonte de verdade para família de produto.

### Fluxo de Integração
```
ISA ──▶ TOTVS ──▶ CRM D365 (Produto / Product catalog)
```

### Problema de Desalinhamento Taxonômico
| Sistema | Conceito | Exemplo |
|---------|---------|---------|
| **ISA** | Família SKU | Família A |
| **TOTVS** | Família Comercial | Fam. Comercial B |
| **CRM** | Linha de Negócio | Sistema de Ensino |

> ⚠️ Os 3 sistemas usam taxonomias diferentes para o mesmo conceito — causa a poluição da tabela de produtos (1.283 registros vs. 15 esperados).

### Roadmap
- Integração a ser **redesenhada** junto com novo cadastro de produto (Onda 1)
- Resultado mandatório para funcionamento do Simulador Comercial

---

## 4. Adobe Acrobat Sign

### Descrição
Plataforma de assinatura digital de contratos.

### Fluxo de Integração
```
ftd_proposta (Aprovada) → Plugin/Flow → Adobe Sign (Agreement) → Escola assina → CRM atualizado
```

| Campo CRM | Uso |
|-----------|-----|
| `ftd_contrato_adobe_id` | ID do agreement criado no Adobe Sign |
| Contact.emailaddress1 | Email do signatário (Rep. Legal) |

### Status
- Plugin "Adobe Agreement" instalado no CRM
- ⚠️ **Em vias de mudar de tecnologia** (Adobe Sign como parceiro está sendo descontinuado)
- 50+ templates Word/PDF (processo de geração ainda manual)

---

## 5. Lumisfera (Adobe Commerce)

### Descrição
E-commerce da FTD — plataforma para famílias comprarem materiais escolares.

### 4 Frentes do Lumisfera

| Frente | Modelo | Integração CRM |
|--------|--------|----------------|
| **Lumisfera Aberto** | B2C pura (Amazon model) | Não passa pelo CRM |
| **Lumisfera Vende Escola** | B2C vinculada à escola | Alinhada com Proposta CRM ("FTD com Você") |
| **Lumisfera B2B Funcionário** | Venda interna | Parcial |
| **Lumisfera B2B Livreiros** | **NOVO** — substitui Pede Livreiro | Livreiros terão Conta e Proposta no CRM |

### Integração com Proposta
- Sub-lista de produtos por canal: quais produtos disponíveis no Lumisfera para esta proposta
- Sincronização de sublista CRM → Lumisfera após proposta ativada

---

## 6. Azure Service Bus

### Descrição
Infraestrutura de mensageria para desacoplamento entre D365, TOTVS e Azure Functions.

### Uso no Projeto

| Producer | Consumer | Mensagem |
|----------|----------|---------|
| Plugin D365 | Azure Function | Proposta com >50 produtos para cálculo |
| Azure Function | TOTVS | Pedido confirmado após aprovação |
| TOTVS | Azure Function | Atualização de conta/produto |

### Por Que Service Bus?
- Plugins D365 têm timeout de 2 min → grandes volumes precisam de processamento assíncrono
- Garante entrega mesmo se TOTVS estiver indisponível momentaneamente
- Retry policy nativa do Service Bus

---

## 7. Resumo de Todas as Integrações

| Sistema | Direção | Frequência | Mecanismo | Status |
|---------|---------|-----------|-----------|--------|
| TOTVS → CRM | Inbound | 1x/dia (6h) | Azure Function + Service Bus | ✅ Ativo |
| CRM → TOTVS | Outbound | Evento-driven | Azure Function + Service Bus | ✅ Ativo |
| ISA → TOTVS → CRM | Inbound | Periódico | ETL | ⚠️ Em revisão |
| CRM → Adobe Sign | Outbound | Evento-driven | Plugin "Adobe Agreement" | ✅ Ativo (mudança tecnológica planejada) |
| Adobe Sign → CRM | Inbound | Webhook | Power Automate | ✅ Ativo |
| CRM → Lumisfera | Outbound | Evento-driven | Power Automate | ✅ Ativo |
| Simulador → Dataverse | Bidirecional | Real-time | Dataverse Web API | 🚧 Em desenvolvimento |

---

*Referências: [docs/arquitetura/d365-integration-landscape.md](../docs/arquitetura/d365-integration-landscape.md) | [ftd-knowledge-base.md](../docs/ftd-knowledge-base.md)*
