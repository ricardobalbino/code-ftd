# Modelo de Dados — Dataverse FTD

> [← Voltar ao Início](./Home.md)

**Status**: EM REVISÃO — modelo-alvo (inclui entidades a criar)  
**Referência**: `docs/arquitetura/d365-data-model.md`

---

## 1. Visão Geral

O Dataverse da FTD combina **entidades padrão D365 customizadas** com **entidades customizadas `ftd_*`**.

**Jornada de dados**:
```
Account → Contact → Opportunity → ftd_proposta → ftd_produto_proposta
                                               ↓
                                         Order → Invoice → TOTVS
```

### Prefixos de Customização
| Prefixo | Significado |
|---------|-------------|
| `ftd_` | Campo/entidade customizado FTD |
| (sem prefixo) | Campo padrão D365 |
| `ftd_proposta` | Entidade customizada (substitui/estende Quote padrão) |

---

## 2. Entidades Padrão D365 (Customizadas)

### 2.1 Account (Conta)

**Propósito**: Escola, mantenedora, livreiro ou governo  
**Volume**: ~101.000 registros (higienização necessária)

| Campo | Tipo | Observações |
|-------|------|-------------|
| `name` | Text | Nome da Receita Federal (obrigatório) |
| `accountnumber` | Text | Código ERP (TOTVS, sync 1x/dia às 6h) |
| `ftd_cnpj` | Text | CNPJ — duplicidade **BLOQUEIA** |
| `ftd_codigo_mec` | Text | Código MEC — duplicidade gera **ALERTA** |
| `ftd_tipo_instituicao` | OptionSet | ⚠️ EM REVISÃO: mistura 3 conceitos |
| `ftd_segmento` | OptionSet | **L1**: Privado / Público |
| `ftd_tipo_organizacional` | OptionSet | **L2**: Confessional, Laica, Rede, Parceiro, KA |
| `ftd_classificacao_comercial` | OptionSet | **L3**: KA, Regional, Especial, Regular |
| `ftd_mantenedora_id` | Lookup → Account | ⚠️ EM REVISÃO: campo com 3 usos simultâneos |
| `ftd_parceiro_comercial_id` | Lookup → Account | **A CRIAR**: livreiro vinculado à escola |
| `ftd_num_alunos` | Integer | Impacta cálculos de proposta (padrão=1) |
| `ftd_estabelecimento_id` | Lookup → ftd_estabelecimento | Filial FTD responsável (28 filiais) |
| `ownerid` | Lookup → User | Consultor responsável (carteirização manual) |

**Plugins Ativos**:
- `ftd_cnpj` único: Pre-Validation Stage 10 → `InvalidPluginExecutionException` se duplicado
- Código MEC: Pre-Validation → `SetFieldNotification` se duplicado (aviso, não bloqueio)

---

### 2.2 Contact (Contato)

**Propósito**: Representante Legal da escola (assina contratos)

| Campo | Tipo | Observações |
|-------|------|-------------|
| `parentcustomerid` | Lookup → Account | Escola vinculada |
| `ftd_cpf` | Text | **Obrigatório** quando é Representante Legal |
| `ftd_is_representante_legal` | Boolean | Flag identificador |
| `emailaddress1` | Email | Para Adobe Sign |

---

### 2.3 Opportunity (Oportunidade)

**Propósito**: Pipeline de vendas — vínculo entre conta e proposta  
**Problema**: 101.000+ oportunidades 2024 **desvinculadas** de propostas

| Campo | Tipo | Observações |
|-------|------|-------------|
| `parentaccountid` | Lookup → Account | Escola |
| `parentcontactid` | Lookup → Contact | Rep. Legal |
| `ftd_modelo_venda` | OptionSet | Canal principal |
| `ftd_safra` | Integer | Ano letivo/comercial |
| `ftd_tipo_contrato` | OptionSet | **A CRIAR**: Contrato Novo / Aditivo |
| `ftd_classificacao_negociacao` | OptionSet | **A CRIAR**: escola / livreiro / família |
| `ftd_taxa_admin` | Decimal | Taxa de administração (afeta alçada) |

---

### 2.4 ftd_proposta (Proposta Comercial)

**Propósito**: Entidade central do Simulador — substitui/estende Quote padrão

| Campo | Tipo | Observações |
|-------|------|-------------|
| `ftd_oportunidade_id` | Lookup → Opportunity | Vínculo obrigatório |
| `ftd_proposta_anterior_id` | Lookup → ftd_proposta | Versionamento (predecessor) |
| `ftd_proposta_posterior_id` | Lookup → ftd_proposta | Versionamento (sucessor) |
| `ftd_revisao` | Integer | Número da revisão (0 = inicial, até 27-28) |
| `ftd_status_proposta` | OptionSet | **A CRIAR**: Aberto / Ativo / Histórico |
| `ftd_estagio_proposta` | OptionSet | **A CRIAR**: Rascunho → Aprovada e assinada |
| `ftd_safra` | Integer | Ano-safra |
| `ftd_tipo` | OptionSet | **A CRIAR**: Contrato Novo / Aditivo |
| `ftd_alunado_total` | Integer | Total de alunos |
| `ftd_taxa_admin` | Decimal | Taxa de administração |
| `ftd_valor_total` | Currency | Valor total da proposta |
| `ftd_valor_receita_liquida` | Currency | Após descontos e benefícios |
| `ftd_nivel_alcada` | Integer | Nível de aprovação calculado (1-7) |
| `ftd_justificativa_consultor` | Multiline Text | Para aprovadores |
| `ftd_contrato_adobe_id` | Text | ID do agreement no Adobe Sign |

**Regras de Versionamento**:
- Apenas 1 proposta por Oportunidade pode ter Status = `Ativo`
- Revisão anterior → Status = `Histórico`
- Link bidirecional obrigatório: anterior ↔ posterior

---

## 3. Entidades Customizadas FTD

### 3.1 ftd_produto_proposta (Produto da Proposta)

**Propósito**: Linha de produto dentro da proposta (junction entre proposta e produto)  
**Relação**: N:1 → ftd_proposta | N:1 → Product (catálogo)

| Campo | Tipo | Observações |
|-------|------|-------------|
| `ftd_proposta_id` | Lookup → ftd_proposta | Proposta pai |
| `ftd_produto_id` | Lookup → Product | Produto do catálogo |
| `ftd_linha_negocio` | OptionSet | Sistema de Ensino, Didático, Bilíngue, etc. |
| `ftd_serie_id` | Lookup → ftd_serie | Série do aluno |
| `ftd_quantidade_alunos` | Integer | Para este produto |
| `ftd_preco_tabela` | Currency | Da lista de preços |
| `ftd_desconto_pct` | Decimal | % de desconto |
| `ftd_majoracao_pct` | Decimal | % de majoração (markup escola) |
| `ftd_preco_escola` | Currency | Calculado: tabela × (1 - desconto%) |
| `ftd_preco_familia` | Currency | Calculado: escola × (1 + majoração%) |
| `ftd_royalty_pct` | Decimal | Diferença preço escola vs. família |
| `ftd_tipo_produto` | OptionSet | Prateleira / Customizado / Personalizado / Grade |
| `ftd_canal_ids` | Multiselect | Canais onde este produto está disponível |

---

### 3.2 ftd_beneficio (Benefício da Proposta)

**Propósito**: Cupons para filhos de professores

| Campo | Tipo | Observações |
|-------|------|-------------|
| `ftd_proposta_id` | Lookup → ftd_proposta | Proposta pai |
| `ftd_serie_id` | Lookup → ftd_serie | Série vinculada |
| `ftd_desconto_pct` | Decimal | 20% ou 40% |
| `ftd_quantidade_beneficiarios` | Integer | Número de beneficiários |
| `ftd_impacto_receita` | Currency | Impacto calculado na receita |

---

### 3.3 ftd_aprovacao (Registro de Aprovação)

**Propósito**: Histórico de cada etapa do fluxo de aprovação

| Campo | Tipo | Observações |
|-------|------|-------------|
| `ftd_proposta_id` | Lookup → ftd_proposta | Proposta vinculada |
| `ftd_nivel` | Integer | Nível de alçada (1-7) |
| `ftd_aprovador_id` | Lookup → User | Quem aprovou/recusou |
| `ftd_status_aprovacao` | OptionSet | Aprovado / Recusado / Pendente |
| `ftd_data_decisao` | DateTime | Quando foi decidido |
| `ftd_justificativa` | Text | Motivo da recusa |

---

### 3.4 ftd_estabelecimento (Filial FTD)

**Propósito**: Representação das 28 filiais FTD no Brasil

| Campo | Tipo | Observações |
|-------|------|-------------|
| `name` | Text | Nome da filial |
| `ftd_regiao` | OptionSet | Região geográfica |
| `ftd_coordenador_id` | Lookup → User | Coordenador da filial |
| `ftd_lista_preco_id` | Lookup → PriceLevel | Lista de preços regional |

---

## 4. Problemas Conhecidos no Modelo de Dados

| Problema | Impacto | Solução |
|---------|---------|---------|
| `ftd_tipo_instituicao` mistura conceitos | Filtros incorretos de conta | Criar 3 campos separados |
| `ftd_mantenedora_id` com 3 usos | Confusão arquitetural | Decisão de arquitetura + limpeza |
| 12+ listas de preço | Usadas para resolver visibilidade, não pricing real | Revisão completa Onda 1 |
| Dataverse em nível crítico de storage | Risco de paralisação | Higienização urgente |
| Tabela de produtos poluída (1.283 vs. 15 esperados) | Seleção de produtos difícil | Flag Prateleira vs. Customizado |
| Séries inconsistentes no cadastro de produtos | Filtros quebram | Correção de base + padronização |

---

## 5. Diagrama de Relacionamentos (Simplificado)

```
User (Consultor)
    │
    │ ownerid
    ▼
Account (Escola)
    │
    │ parentaccountid
    ▼
Opportunity (Oportunidade)
    │
    │ ftd_oportunidade_id
    ▼
ftd_proposta (Proposta)
    │
    ├──▶ ftd_produto_proposta (N produtos)
    │        └──▶ Product (catálogo)
    │
    ├──▶ ftd_beneficio (benefícios/doações)
    │
    └──▶ ftd_aprovacao (histórico de aprovação)
             └──▶ User (aprovador)
```

---

*Referências: [docs/arquitetura/d365-data-model.md](../docs/arquitetura/d365-data-model.md)*
