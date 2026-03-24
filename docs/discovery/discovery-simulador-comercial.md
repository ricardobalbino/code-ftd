# Discovery — Simulador Comercial

**Projeto**: FTD Educação — Transformação CX  
**Data**: 18/Mar/2026  
**Fontes**: Especificação Notion (Oscar de Rooij), transcrições onboarding 09-10/Mar, refinamento 16-17/Mar  
**Spec completa**: `docs/especificacao-simulador-notion.md`

---

## 1. Visão Geral

O **Simulador Comercial** é a nova interface de negociação consultiva da FTD, posicionada como camada anterior e complementar ao CRM (D365 CE). Será construído em **Power Pages** com backend Dataverse/CRM.

**Pain point central**: Consultor leva até **3 horas para criar 1 proposta** (200 produtos × 3 cliques cada) usando CRM nativo + Excel paralelo.

**Objetivo**: Permitir que o consultor construa, simule, compare e refine propostas comerciais de forma estruturada, enquanto o simulador lida com a complexidade de registrar tudo no CRM.

---

## 2. Contexto AS-IS

- Negociação ocorre **fora dos sistemas oficiais** (planilhas, WhatsApp, reuniões informais)
- CRM recebe propostas **já praticamente finalizadas** — processo executado fora dele
- Ausência de **versionamento** e **comparação** estruturada de propostas
- Cálculos só acontecem no final — consultor fica "às cegas" durante montagem
- ~404 consultores, muitos detratores do CRM atual
- ~50 templates de documentos (contratos, aditivos) geridos manualmente em Word/PDF

### Workarounds atuais
- Excel paralelo para simulação de propostas
- Comunicação por WhatsApp/e-mail com PDFs/PPTs não padronizados
- Processo de contrato: baixar 2-3 templates Word, copiar/colar, ajustar, salvar PDF

---

## 3. Escopo Funcional (6 Etapas + Base Estruturante)

### Base Estruturante
| Cadastro | Status | Ação necessária |
|----------|--------|-----------------|
| **Linhas de Negócio** | ~15 no CRM, desalinhadas ISA/TOTVS | Revisão completa + sincronização 3 sistemas |
| **Séries** | Inconsistentes, valores duplicados | Limpeza de base ou criação campo novo |
| **Produtos** | 1.283 registros (deviam ser ~15-17 prateleira) | Limpeza + flag customizado/prateleira + variações |
| **Listas de Preço** | 12+ tabelas (deviam ser ~5) | Simplificação radical + esperar consultoria pricing |
| **Estabelecimentos (Filiais)** | Existentes, qualidade duvidosa | Revisão para tabela preço regional + aprovação |
| **Contatos** | Todos consultores veem todos contatos | Necessário segmentação acesso |
| **Escolas/Mantenedoras** | 101.000 contas, campo mantenedora com 3 usos | Higienização massiva + correção campo duplo |
| **Oportunidades** | Desvinculadas de propostas | Criação massiva 2025/2026 + simplificação campos |

### Etapa 1: Contexto da Negociação
- Vínculo Conta → Oportunidade → Proposta anterior
- Tipo: Contrato ou Aditivo (campo novo, não existe hoje)
- Ano-safra, alunado, modelos de venda, lista de preços base
- Formulário com dados CRM pré-preenchidos

### Etapa 2: Produtos e Serviços (MVP — em desenvolvimento)
- Adição em **lote** por Linha de Negócio × Coleção × Séries
- Adição **individual** com filtros rápidos + cache front-end
- Recálculo **instantâneo** de totalizadores (JavaScript front-end)
- Perspectiva FTD e Perspectiva Escola lado a lado
- Taxa de Administração e Alçadas de Aprovação visíveis em tempo real
- **Mudança estrutural**: 1 Proposta → 1 Lista Produtos → N Modelos de Venda (antes era 1:1:1)

### Etapa 3: Benefícios, Doações, Patrocínio e Adiantamento
- Responsabilidade **Avanade** (fora do MVP FTD)
- Grid de benefícios por série (cupons 20%-40% filhos de professores)
- Grid de patrocínios (investimento escola: tecnologia, infraestrutura)
- Adiantamento (% sobre royalties estimados)
- Totalizadores atualizados em tempo real com impacto em Receita Líquida

### Etapa 4: Configuração de Vendas (Modelos de Venda)
- Configurações logísticas por modelo (FTD Com Você, Lumisfera, Presencial, etc.)
- Sublistas de produtos por canal (B2C, B2B Escola, B2B Livreiro, Serviços)
- Kits por série (agrupamento obrigatório de produtos)
- Nome customizado de séries por escola

### Etapa 5: Matriz de Serviços
- Não será especificado neste momento (menos prioritário)
- CRM já gera documento automaticamente — será otimizado depois

### Etapa 6: Revisão e Compartilhamento
- Big Numbers para aprovadores (resumo financeiro consolidado)
- Comparativo com proposta anterior
- Justificativa do consultor para fluxo de aprovação
- Regras de alçada visíveis e dinâmicas
- Configuração do contrato/aditivo (não impacta aprovação)

---

## 4. Funcionalidades Transversais

### Versionamento de Propostas
- Encadeamento: Proposta 1.0 → 1.1 → 1.2 → ... → 2.0 (novo ano-safra)
- Apenas 1 proposta pode ter Status = Ativa por oportunidade
- Link bidirecional: anterior ⟷ posterior
- "Copiar proposta anterior" para renovação anual (com reajuste IPCA + substituição produtos)

### Navegação (Dashboard, Kanban, Busca, Notificações)
- Dashboard: propostas pendentes, aprovadas, recusadas
- Kanban por Estágio de Proposta com filtros avançados
- Busca por razão social, nome fantasia, CNPJ, contato
- Notificações: recusa, aprovação parcial/total, assinatura (Email + Teams)

### Visualização pelo Cliente
- Link de visualização online (Power Pages — Área do Cliente)
- Cliente pode comentar por seção, aprovar ou recusar
- Navegação entre versões disponível para o cliente
- Destinatários definidos na Etapa 1

### Gestão Data-Driven
- Funil de oportunidades (por tipo: novo, ampliação, renovação)
- Histórico comercial por cliente e por linha de negócio
- Relatório de customizações (equipe PCP/Demandas)

### Captura de Dados
- Tempo de manuseio de propostas (acumulado por sessão)
- Timestamps de cada mudança de Estágio/Status

---

## 5. Status e Estágios (Modelo-Alvo)

### Status da Proposta
| Status | Significado |
|--------|------------|
| **Aberto** | Consultor está trabalhando (negociação/aprovação em andamento) |
| **Ativo** | Vigente — contrato assinado, proposta em vigor |
| **Histórico** | Arquivada — substituída por versão mais nova |

### Estágio da Proposta
| Estágio | Quando |
|---------|--------|
| Rascunho | Consultor montando proposta |
| Em negociação | Proposta enviada ao cliente |
| Em aprovação | Fluxo interno de alçadas |
| Aprovada sem assinatura | Todas alçadas aprovaram, contrato pendente |
| Aprovada e assinada | Adobe Sign concluído |

> **IMPORTANTE**: Estes status/estágios NÃO existem hoje no CRM — precisam ser criados.  
> Hoje existem 3 campos diferentes (confusos): Status, Razão do Status, outro campo.

---

## 6. Requisitos Não-Funcionais

| Requisito | Detalhe |
|-----------|---------|
| **Mobile First** | Consultores em trânsito, celular é ferramenta de trabalho |
| **Performance** | Cálculos instantâneos no front-end (JavaScript/cache), validação back-end |
| **Campos Dinâmicos** | Reação instantânea a mudanças de parâmetros |
| **Offline (limitado)** | Navegação de leitura em regiões sem conexão |
| **Tecnologia** | Power Pages (front-end) + Dataverse (backend) + Azure Functions (cálculos complexos) |

---

## 7. Integrações

| Sistema | Direção | Frequência | Dados |
|---------|---------|------------|-------|
| **CRM D365 CE** | ↔ Bidirecional | Real-time | Contas, Oportunidades, Propostas, Pedidos, Produtos |
| **TOTVS/Datasul** | ← Leitura | ETL 1x/dia (~6h) | Cadastro produtos, código ERP, preços |
| **ISA** | ← Leitura | Batch | Família de Produto, catálogo |
| **Adobe Sign** | → Escrita | Evento | Envio contrato, recebimento assinatura |
| **Lumisfera** | ↔ Bidirecional | Configuração | Sublistas produtos por canal B2C/B2B |

---

## 8. Dependências Críticas

1. **Cadastro de Produtos revisado** — prerequisito para adição em lote funcionar
2. **Tabela de Preços simplificada** — bloqueado até resultado consultoria pricing (fim Mar/2026)
3. **Categorização de Contas** — campo tipo_instituicao mistura 3 conceitos
4. **Higienização de base** (101.000 contas) — recorte 2025+2026 como primeiro bloco
5. **Security roles equalizadas** — ambientes não sincronizados, perfis duplicados
6. **Dataverse storage** — nível crítico já ultrapassado

---

## 9. Riscos Identificados

| Risco | Impacto | Mitigação |
|-------|---------|-----------|
| Cadastro de produtos não limpo a tempo | Adição em lote não funciona | Recorte: começar com produtos prateleira limpos |
| Consultoria pricing muda regras | Motor de alçadas precisa ser refeito | Desenvolver motor flexível/parametrizável |
| Desalinhamento ISA/TOTVS/CRM | BIs e relatórios incorretos | Frente de trabalho paralela com Julio/Fernando/TOTVS |
| Resistência dos 404 consultores | Baixa adoção | Validação UX com 4 grupos + rollout gradual por filial |
| Dataverse storage crítico | Novas tabelas bloqueadas | Escalar com arquiteto infra imediatamente |

---

## 10. Métricas de Sucesso

### Growth
- Tempo médio total até assinatura (dias)
- Tempo médio de resposta consultor/cliente (horas)
- Taxa de avanço entre estágios (% por etapa)
- Número médio de versões por contrato ganho
- Taxa de recusa por motivo (% por categoria)

### Cost Optimization
- Tempo de elaboração de proposta: baseline 3h → meta < 30min
- Horas economizadas por proposta (convertível em R$)
- Taxa de retrabalho (% propostas retornadas para correção)

### Organizational Effectiveness
- SLA de aprovação interna por nível
- Throughput do time (propostas/semana por consultor/filial)
- WIP por consultor (sobrecarga)

### Digital Enablement
- Tempo de carregamento telas críticas (p95 em segundos)
- Tempo de recálculo instantâneo (p95 em ms)
- Taxa de sucesso integrações CRM (%)
- Uso de funcionalidades chave (% adoção)

---

## 11. Fora de Escopo
- Geração de contratos ou aditivos (automação futura)
- Execução de pagamentos
- Aprovação de pagamento Adiantamento/Patrocínio/Royalties (Vulcano)
- IA comercial (recomendações, pacotes pré-montados) — visão de futuro
