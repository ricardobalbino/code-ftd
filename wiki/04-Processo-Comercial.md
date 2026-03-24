# Processo Comercial — Jornada Completa

> [← Voltar ao Início](./Home.md)

---

## 1. Fluxo Principal

```
Conta → Contato (Rep. Legal) → Oportunidade → Produtos → Proposta → Aprovação → Contrato → Adobe Sign → TOTVS
```

---

## 2. Criação de Conta

### Quem pode criar contas?
- Consultores, anjas, coordenadores e gerentes: ✅ Podem criar
- Consultores PNLD: ❌ **Não podem criar** — apenas administram/compartilham contas existentes

### Campos obrigatórios
| Campo | Validação |
|-------|-----------|
| Nome (da Receita Federal) | Obrigatório |
| CNPJ | Obrigatório + validação de duplicidade → **BLOQUEIO** se duplicado |
| CEP | Auto-preenche endereço |
| Tipo de Instituição | Obrigatório |

### Validações Automáticas (Plugins)
| Validação | Comportamento |
|-----------|---------------|
| CNPJ duplicado | **BLOQUEIO** — `InvalidPluginExecutionException` (Pre-Validation Stage 10) |
| Código MEC duplicado | **ALERTA** — aviso, mas permite salvar |
| Campos obrigatórios vazios | Impede salvar |

### Campos em Revisão
- `ftd_tipo_instituicao`: mistura 3 conceitos distintos → sendo segregado em 3 campos:
  - `ftd_segmento`: L1 — Privado / Público
  - `ftd_tipo_organizacional`: L2 — Confessional, Laica, Rede, Parceiro, KA
  - `ftd_classificacao_comercial`: L3 — KA, Regional, Especial, Regular

### Propriedade e Carteirização
- `ownerid` = consultor responsável (carteirização manual)
- Sem round-robin ou balanceamento automático
- Desligamento: transferência manual da carteira pelo gerente

---

## 3. Contatos

- **Representante Legal**: OBRIGATÓRIO para criar proposta (assina o contrato)
- CPF obrigatório quando `ftd_is_representante_legal = true`
- Vinculado à conta via `parentcustomerid`
- Campo **Decisor** (Perfil) → usado para definir destinatários da proposta no Simulador

---

## 4. Oportunidade

### Propósito
Pipeline de vendas — vínculo entre conta e proposta.

### Regra de Negócio
> Uma conta pode ter **apenas 1 oportunidade aberta por ano-safra**.

### Campos-Chave

| Campo | Tipo | Observação |
|-------|------|------------|
| `ftd_modelo_venda` | OptionSet | Canal principal de venda |
| `ftd_safra` | Integer | Ano letivo (ex: 2026) |
| `ftd_tipo_contrato` | OptionSet | **A CRIAR**: Contrato Novo / Aditivo |
| `ftd_classificacao_negociacao` | OptionSet | **A CRIAR**: escola / livreiro / família |
| `ftd_taxa_admin` | Decimal | Taxa de administração |
| `ftd_vigencia_inicio/fim` | Date | Período da vigência |

### Problemas Conhecidos
- ⚠️ 101.000+ oportunidades de 2024 **desvinculadas** de propostas → algoritmo de criação massiva necessário
- ⚠️ Muitos campos duplicados entre Oportunidade e Proposta → levantamento por Oscar ("campos vermelhos")
- Oportunidade será **ressignificada** (simplificada) no novo modelo

---

## 5. Proposta Comercial

### Ciclo de Vida
```
Rascunho → Em Negociação → Em Aprovação → Aprovada s/ assinatura → Aprovada e assinada
```

### Versionamento
- Revisão inicia em `ID Revisão = 0`
- Revisões podem chegar a **27-28** (negociação intensa)
- Apenas 1 proposta por oportunidade pode ter Status = **Ativo**
- Revisão anterior recebe Status = **Histórico**
- Link bidirecional: `ftd_proposta_anterior_id ↔ ftd_proposta_posterior_id`

### DOR PRINCIPAL
> Consultor leva até **3 horas** para criar 1 proposta  
> → **Solução**: Simulador Comercial (Power Pages)

---

## 6. Produtos e Linhas de Negócio

### Linhas de Negócio Ativas

| Linha | Adição em Lote | Permite Majoração | Observação |
|-------|----------------|-------------------|------------|
| Sistema de Ensino | ✅ | ✅ | Principal linha |
| Didático | ✅ | ❌ | Não permite markup |
| Bilíngue | ✅ | ✅ | |
| Espanhol | ✅ | ✅ | |
| Literatura | ❌ | — | Produtos individuais (ex: Pequeno Príncipe) |
| APE | ✅ | — | |

### Tipos de Produto

| Tipo | Descrição |
|------|-----------|
| **Prateleira** | Produto padrão do catálogo |
| **Customizado Compartilhado** | Alteração leve (tirar capítulo) — qualquer escola pode usar |
| **Customizado Personalizado** | Alteração pesada (capa, mascote) — vinculado a escola/mantenedora específica |
| **Grade** | Produto específico para ensino médio, por escola |
| **Código Substituto** | Versionamento entre safras: produto antigo → novo |

### Problema Crítico de Produtos
> Tabela poluída: filtro que deveria retornar **15 linhas** retorna **1.283**  
> Causa: falta de flag distinguindo Produto Prateleira vs. Customizado  
> Solução: revisão de cadastro + limpeza de base (Onda 1)

---

## 7. Condições Comerciais

| Condição | Descrição | Afeta Alçada? |
|---------|-----------|---------------|
| **Desconto** | % sobre preço de tabela (por produto) | Sim |
| **Majoração/Markup** | Preço acima da tabela (escola define preço família) | Sim |
| **Taxa de Administração** | Condição comercial | Sim |
| **Royalty** | Diferença entre preço negociado e preço família (acumula, pago mai/ago) | Sim |
| **Adiantamento** | Antecipação do royalty (fluxo de caixa) | Sim (alta alçada) |
| **Benefícios** | Cupons filhos de professores (40% desconto) | Sim |
| **Doações** | 90% ou 100% desconto | Sim |
| **Patrocínios** | Investimentos em dinheiro na escola | Sim (alta alçada) |
| **Parcelamento na loja** | Afeta nível de alçada | Sim |

---

## 8. Fluxo de Aprovação (Alçadas)

### Os 7 Níveis de Alçada

| Nível | Aprovador | Critério (Cumulativo) |
|-------|-----------|----------------------|
| 1 | Consultor | Auto-aprovação (dentro da política) |
| 2 | Coordenador | Descontos nível 1 |
| 3 | Gerente Filial | Descontos nível 2 / Estornos |
| 4 | Gerente Regional | Descontos nível 3 |
| 5 | Diretor Adjunto (DG) | Patrocínios / Adiantamentos altos |
| 6 | Diretor Superintendente | Casos críticos / Royalties |
| 7 | Presidente | Alçadas máximas (Grupo Marista) |

### Regras de Resubmissão
- Se totalizadores de preço **não mudaram**: pula níveis e vai direto para quem recusou
- Regras de alçada sendo revisadas pela Mônica (Comercial)
- **Problema atual**: Muitas propostas travam no Nível 4 por regras mal parametrizadas

### Ferramentas de Aprovação
- **Atual**: Vulcano (ferramenta externa) — **SERÁ ELIMINADO**
- **Novo**: Power Automate Approvals (nativo D365) — Motor independente das regras de alçada

---

## 9. Contrato e Assinatura Digital

### Fluxo de Contrato
```
Proposta Aprovada → Geração do Contrato → Download Templates Word/PDF → Cópia/Cola → Adobe Sign → Assinatura
```

**Problema**: Processo MANUAL — consultor faz download de vários templates, copia/cola no Word, salva PDF.

### Tipos de Contrato
- Contrato Novo
- Aditivo de Encerramento
- Aditivo de Renovação
- Outros tipos de aditivo

### Volume de Templates
- **50+ templates** Word/PDF para diferentes tipos de contrato/aditivo
- Modularização do contrato: precisa revisão com jurídico (roadmap)

### Adobe Sign
- Plugin "Adobe Agreement" no CRM
- Em vias de **mudar de tecnologia** (Adobe Sign está sendo descontinuado como parceiro)
- Assinatura é digital com rastreabilidade do contrato

---

## 10. Canais de Venda (Modelos de Venda)

| Canal | Descrição |
|-------|-----------|
| **FTD com Você (Lumisfera)** | E-commerce principal — escola compartilha link, famílias compram |
| **Venda Direta** | Faturamento direto para escola (invoice) |
| **Frente de Loja / Balcão Escola** | Venda presencial na escola |
| **Smart POS** | Terminal de pagamento para plantão de vendas |

**Mudança Arquitetural**:  
Era: 1 proposta por canal → **Novo**: 1 proposta com múltiplos canais + sub-lista de produtos por canal

---

## 11. Consultores: Hunter vs. Farmer

| Perfil | Função |
|--------|--------|
| **Hunter** | Prospecta novas contas (escola nunca atendida) |
| **Farmer** | Mantém carteira de contas existentes (renovações, expansão) |
| **Anja** | Pessoa que cadastra proposta no lugar do consultor (apoio administrativo) |

---

*Referências: [ftd-knowledge-base.md](../.avanade-method/docs/ftd-knowledge-base.md) | [discovery-refinamento-processo-16mar.md](../docs/discovery/discovery-refinamento-processo-16mar.md)*
