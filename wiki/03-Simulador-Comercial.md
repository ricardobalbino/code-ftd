# Simulador Comercial — Especificação e MVP

> [← Voltar ao Início](./Home.md)

---

## 1. O Que É o Simulador Comercial?

O **Simulador Comercial** é a principal entrega do projeto — uma **interface de negociação consultiva** construída em Power Pages que substitui o fluxo atual de criação de propostas diretamente no CRM D365.

| Aspecto | Detalhe |
|---------|---------|
| **Tecnologia** | Power Pages (Single Page Application) |
| **Backend** | Dataverse Web API + Plugins C# + Azure Functions |
| **Autenticação** | Microsoft Entra ID (usuários internos com licença Enterprise) |
| **Deadline MVP** | 31 de Março de 2026 |
| **Responsável Negócio** | Majolie Galdino |
| **Responsável CX** | Oscar de Rooij |
| **Product Owner** | Kevellin |
| **Responsável Tecnologia** | Fernando Costa Filho |

---

## 2. Contexto: Por Que o Simulador?

### Situação Atual (AS-IS)
- Grande parte da negociação ocorre **fora dos sistemas oficiais** (planilhas, documentos paralelos)
- CRM recebe propostas já praticamente finalizadas — sem rastreabilidade do processo
- Consultor leva até **3 horas** para criar 1 proposta
  - Causa 1: Não cadastra conta antes — faz tudo junto
  - Causa 2: 200 produtos × 3 cliques = 600 cliques manuais
  - Causa 3: Lentidão de carregamento do CRM (forms, grids, dropdowns)
- Ausência de versionamento e comparação de propostas

### Situação Desejada (TO-BE)
O Simulador permite ao consultor **construir, simular, comparar e refinar propostas** de forma estruturada, enquanto o sistema lida com a complexidade de registrar tudo no CRM de forma automática.

**Resultados Esperados**:
- Criação de proposta em **< 15 minutos** (meta)
- Adição em lote: de 200 cliques para ~5 cliques
- 100% das propostas rastreadas no CRM
- Histórico completo de versões por proposta
- Relatórios de gestão para liderança comercial

---

## 3. Requisitos de Usabilidade

| Requisito | Descrição |
|-----------|-----------|
| **Mobile First** | Consultores em trânsito — celular é ferramenta principal |
| **Performance** | Campos dinâmicos com reação instantânea (sem roundtrips) |
| **Disponibilidade Offline** | Leitura limitada offline (regiões com conectividade ruim) |
| **Campos Dinâmicos** | Recálculo automático ao mudar qualquer parâmetro |

---

## 4. MVP — Deadline: 31 de Março de 2026

### Escopo do MVP (Time FTD desenvolvendo)

| Entrega | Descrição |
|---------|-----------|
| ✅ Botão no Spartan | Redireciona para Power Pages com contexto da oportunidade |
| ✅ Etapa 1: Dados da Proposta | Formulário com cliente, tipo, safra, alunado |
| ✅ Etapa 2: Produtos (adição individual) | Grid de produtos com totalizadores em real-time |
| ✅ Reorganização de campos CRM | Campos movidos/renomeados nas etapas do Spartan |

### Fora do MVP
- Adição em lote de produtos
- Cópia de proposta anterior (com código substituto + IPCA)
- Etapa 3 (Benefícios/Doações/Patrocínios)
- Etapa 4 (Configuração de Vendas/Canais)
- Etapa 5 (Matriz de Serviços)
- Motor de Aprovação integrado (eliminação do Vulcano)

---

## 5. As 6 Etapas do Simulador

### Etapa 1 — Contexto da Negociação (Dados da Proposta)

**Objetivo**: Vincular a proposta à conta, oportunidade e proposta anterior.

| Campo | Tipo | Observações |
|-------|------|-------------|
| Escola (Conta) | Lookup | Auto-preenche dados da conta |
| Oportunidade | Lookup | Vinculada à conta |
| Proposta Anterior | Lookup | Para aditivos/revisões |
| Tipo (Contrato/Aditivo) | OptionSet | Define template de contrato |
| Safra | Integer | Ano letivo (ex: 2026) |
| Vigência (início/fim) | Date | Período do contrato |
| Responsável Assinatura | Lookup → Contact | Rep. Legal (obrigatório para contrato) |
| Alunado por Série | Grid | Quantidade de alunos por série (impacta cálculos) |
| Modelos de Venda | MultiSelect | Canais ativos desta proposta |
| ID Proposta (metadado) | Auto | Gerado automaticamente |
| ID Revisão (metadado) | Auto | Inicia em 0 |

---

### Etapa 2 — Produtos e Serviços

**Objetivo**: Grid principal de produtos da proposta com totalizadores em tempo real.

**Funcionalidades**:
- Adição individual de produtos (MVP)
- Adição em lote por linha de negócio × coleção × série (pós-MVP)
- Filtros dinâmicos client-side: linha de negócio, coleção, série
- Totalizadores em real-time (sem F5): valor total, receita líquida, royalty
- Edição inline: desconto%, majoração%, quantidade de alunos

**Campos por linha de produto**:

| Campo | Descrição |
|-------|-----------|
| Produto | Nome + código |
| Linha de Negócio | Sistema de Ensino, Didático, Bilíngue, etc. |
| Série | EI, EFAI, EFAF, EM |
| Qtd. Alunos | Preenchido do alunado ou editável manualmente |
| Preço Tabela | Vindo da lista de preços da safra |
| Desconto % | Editável pelo consultor (regras de alçada aplicam) |
| Majoração % | Editável (apenas linhas que permitem) |
| Preço Escola | Calculado: tabela × (1 - desconto%) |
| Preço Família | Calculado: escola × (1 + majoração%) |
| Royalty % | Diferença entre preço escola e família |

**Totalizadores**:
- Valor Bruto Total
- Total de Descontos
- Receita Líquida
- Total Royalties
- Impacto Adiantamento

**Regras de Linhas de Negócio**:

| Linha | Adição em Lote | Permite Majoração |
|-------|----------------|-------------------|
| Sistema de Ensino | ✅ Sim | ✅ Sim |
| Didático | ✅ Sim | ❌ Não |
| Bilíngue | ✅ Sim | ✅ Sim |
| Espanhol | ✅ Sim | ✅ Sim |
| Literatura | ❌ Não (individual) | Verificar |
| APE | ✅ Sim | Verificar |

---

### Etapa 3 — Benefícios, Doações, Patrocínios e Adiantamento

**Objetivo**: Registrar as condições comerciais além do preço de produto.

| Tipo | Descrição | Impacto na Alçada |
|------|-----------|-------------------|
| **Benefícios** | Cupons filhos de professores (20-40% desconto) | Sim |
| **Doações** | Materiais doados (90% ou 100% desconto) | Sim |
| **Patrocínios** | Investimentos em dinheiro na escola (laptops, fachada) | Sim (alta alçada) |
| **Adiantamento** | Antecipação do royalty (fluxo de caixa) | Sim (alta alçada) |

> **Fora do escopo do MVP** — Onda 1 pós-MVP (~Agosto 2026)

---

### Etapa 4 — Configuração de Vendas

**Objetivo**: Definir canais de venda e sub-listas de produtos por canal.

| Canal | Descrição |
|-------|-----------|
| **FTD com Você (Lumisfera)** | E-commerce: escola compartilha link, famílias compram |
| **Venda Direta** | Faturamento direto para escola (invoice) |
| **Frente de Loja / Balcão Escola** | Venda presencial |
| **Smart POS** | Terminal de pagamento em plantão de vendas |

**Mudança Conceitual**: Era 1 proposta por canal → agora 1 proposta com múltiplos canais. Sub-lista define quais produtos estão disponíveis em cada canal.

> **Fora do escopo do MVP** — Onda 1 pós-MVP (~Agosto 2026)

---

### Etapa 5 — Matriz de Serviços

**Objetivo**: Validar e gerar lista de serviços educacionais vinculados aos produtos contratados.

- Botão "Validar Serviços" (processamento: 10-15 min via Power Automate)
- Notificação Teams quando completo
- Sendo **inteiramente revisitada** — fora do escopo atual

---

### Etapa 6 — Análise e Aprovação

**Objetivo**: Dashboard de análise da proposta + submissão para aprovação.

| Componente | Descrição |
|-----------|-----------|
| Big Numbers | Totalizadores visuais (valor, desconto médio, receita líquida) |
| Comparativo | Proposta atual vs. proposta anterior (lado a lado) |
| Nível de Alçada | Visualização do nível de aprovação calculado (1-7) |
| Justificativa | Campo livre para o consultor explicar a proposta |
| Submissão | Botão para enviar para aprovação |

---

## 6. Status e Estágios da Proposta

### Status (controla o ciclo de vida do registro)

| Status | Significado |
|--------|-------------|
| **Aberto** | Consultor está ativamente negociando |
| **Ativo** | Representa a proposta vigente assinada pelo cliente |
| **Histórico** | Sobreposto por versão mais nova — arquivo de referência |

### Estágios (controla o progresso na jornada)

| Estágio | Significado |
|---------|-------------|
| **Rascunho** | Criada mas não submetida |
| **Em Negociação** | Com o cliente para avaliação |
| **Em Aprovação** | Submetida para aprovação interna |
| **Aprovada (sem assinatura)** | Aprovada internamente, aguardando assinatura |
| **Aprovada e assinada** | Contrato assinado via Adobe Sign |
| **Fechado Perdido** | Negociação encerrada sem sucesso |
| **Cancelado** | Proposta cancelada |

---

## 7. Funcionalidades Futuras (Pós-MVP)

| Funcionalidade | Valor | Complexidade |
|---------------|-------|--------------|
| Adição em lote | Alta (200 cliques → 5) | Média |
| Cópia de proposta anterior + IPCA | Alta | Média |
| Motor de aprovação D365 (elimina Vulcano) | Alta | Alta |
| Inteligência comercial (upsell/cross-sell) | Média | Alta |
| Comissionamento do consultor visível | Média | Alta |
| Tutorial de produto / base de conhecimento | Baixa | Baixa |
| IA: sugestões de proposta | Baixa (maturidade de dados) | Muito alta |

---

*Referências: [especificacao-simulador-notion.md](../docs/especificacao-simulador-notion.md) | [discovery-simulador-comercial.md](../docs/discovery/discovery-simulador-comercial.md)*
