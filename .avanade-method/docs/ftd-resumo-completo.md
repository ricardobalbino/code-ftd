# Resumo Completo: FTD Educação
## Gerado em: 16 Março 2026

---

## A Empresa
- **FTD Educação S/A** — parte do **Grupo Marista**, +120 anos de história
- Sede em **São Paulo/SP**, **28 filiais** de distribuição pelo Brasil
- Impacta **~1,5 milhão de estudantes**
- Produz e distribui **livros e materiais didáticos** para escolas públicas e privadas
- **Pico sazonal**: Nov-Jan (adoção escolar, ~5.000 contratos/dia)

---

## Plataforma Tecnológica Atual
- **CRM**: Dynamics 365 CE Online (Sales + Customer Service)
- **5 Apps D365**: Spartan (vendas), PNLD (governo), Hub SAC (atendimento), Adobe Sign (contratos), Área do Cliente (portal escolas)
- **9 Solutions** segmentadas por tipo de componente (Data Model, Client Extensions, Plugins, Site Map, etc.)
- **5 Ambientes**: Dev (plugin manual), OAT (pipeline), Prod (pipeline + GMUD), QA e RC (criados mas não configurados)
- **Repo**: "FTD Dynamics" no Azure DevOps (branch `dev` = source of truth)
- **Stack**: C# Plugins, JS Web Resources, Power Automate, Azure Functions, Power Pages, TypeScript (PCF)
- **Monitoramento**: Datadog (APM) + Grafana (dashboards) + Application Insights
- **ERP**: TOTVS/Datasul (ETL bidirecional, contas sync 1x/dia às 6h)

---

## Jornada Comercial Completa

```
Conta → Contato (Rep. Legal) → Oportunidade → Produtos → Proposta (6 etapas) 
→ Aprovação (4 alçadas) → Contrato → Adobe Sign → TOTVS
```

**6 Linhas de Negócio**: Sistema de Ensino, Didático, Bilíngue, Espanhol, Literatura, APE — cada uma com regras próprias de majoração e adição em lote.

**4 Canais de Venda**: FTD com Você/Lumisfera (e-commerce principal), Venda Direta, Frente de Loja/Balcão, Smart POS.

**Tipos de Produto**: Prateleira, Customizado Compartilhado, Customizado, Personalizado, Grade (ensino médio).

**Condições Comerciais**: Desconto, Majoração/Markup, Taxa de Administração, Royalty, Adiantamento, Benefícios (cupons filhos de professores), Doações, Patrocínios.

**4 Alçadas de Aprovação**: (1) Consultor auto-aprova → (2) + Coordenador/Gerente → (3) + Diretor Adjunto → (4) + Diretor Comercial (Quintela). Regras baseadas em % royalty, % adiantamento, % patrocínio, taxa admin, parcelamento.

---

## 11 Pain Points Identificados (por severidade)

| # | Problema | Severidade |
|---|---------|-----------|
| 1 | Consultor leva até **3 HORAS** para criar 1 proposta (200 produtos × 3 cliques) | 🔴 Crítico |
| 2 | **Dataverse em nível CRÍTICO** de armazenamento (já passou do limite) | 🔴 Crítico |
| 3 | Tabela de produtos poluída (**1.283 linhas** onde deveria ser 15) | 🔴 Crítico |
| 4 | **12+ tabelas de preço** (workaround para visibilidade, não pricing) | 🟠 Alto |
| 5 | Campos duplicados Oportunidade ↔ Proposta (dados desatualizados) | 🟠 Alto |
| 6 | **50+ templates Word** para contratos (manutenção insustentável) | 🟠 Alto |
| 7 | Vulcano: aprovação externa duplicada (será eliminada) | 🟡 Médio |
| 8 | Security roles não equalizadas entre ambientes | 🟡 Médio |
| 9 | Cadastro duplo CRM/TOTVS sem owner da informação | 🟡 Médio |
| 10 | Repositório desacoplado dos PBIs | 🟡 Médio |
| 11 | Plugin deploy manual para Dev | 🟢 Baixo |

---

## Simulador Comercial — O Grande Projeto

### Decisão Arquitetural
Power Pages (frontend) + CRM (motor/backend), autenticação Entra ID (sem custo extra).

### Conceito
Interface de **negociação consultiva** posicionada como camada anterior e complementar ao CRM. O consultor constrói, simula, compara e refina propostas enquanto o simulador lida com a complexidade de registrar tudo no CRM.

### Arquitetura Técnica
SPA, cache heavy no client-side, cálculos real-time em JavaScript/PowerFX no frontend, validação backend via Azure Functions, débounce (≤50 produtos → plugin síncrono, >50 → Azure Function).

### Requisitos Críticos
Mobile First, performance instantânea, campos dinâmicos reativos, disponibilidade offline (leitura).

### 6 Etapas do Simulador

1. **Contexto da Negociação** — Conta, Oportunidade, tipo (Contrato/Aditivo), safra, alunado, destinatários
2. **Produtos e Serviços** — Adição individual e em lote, filtros por LN/Coleção/Série, totalizadores real-time. **Mudança conceitual**: 1 Proposta → 1 Lista de Produtos → N Modelos de Venda
3. **Benefícios, Doações, Patrocínios, Adiantamento** — Grid por tipo/série, impacto na receita em tempo real
4. **Configuração de Vendas** — Canais, logística, sublistas de produtos por modelo de venda, Kits por Série
5. **Matriz de Serviços** — Em revisão (menos prioritário)
6. **Revisão & Compartilhamento** — Big numbers, comparativo vs proposta anterior, justificativa para aprovação, config de contrato

### Versionamento de Propostas
Encadeamento bidirecional de propostas (anterior ↔ posterior), apenas 1 Ativa por vez por Oportunidade.
- **Status**: Aberto / Ativo / Histórico
- **Estágio**: Rascunho → Avaliação Cliente → Em Aprovação → Aprovado não assinado → Aprovado e assinado

### Navegação
Dashboard com métricas, Kanban por estágio, busca (razão social, CNPJ, contato), sistema de notificações real-time (recusa, aprovação, assinatura).

### Visualização do Cliente
Link online, comentários por seção, aprovar/recusar com 1 clique.

### Base Estruturante (9 cadastros que precisam limpeza)
LN/Coleções, Séries, Produtos, Listas de Preços, Estabelecimentos, Contatos, Escolas/Mantenedoras, Oportunidades.

### 20 Métricas de Sucesso

| Categoria | Qtd | Exemplos |
|-----------|-----|----------|
| **Growth** | 8 | Tempo médio até assinatura, taxa de avanço entre estágios, nº médio de versões por contrato, taxa de recusa por motivo, acurácia de forecast |
| **Cost Optimization** | 4 | Tempo de elaboração de proposta, horas economizadas por proposta, taxa de retrabalho, custo por proposta processada |
| **Organizational Effectiveness** | 3 | SLA de aprovação por nível, throughput do time, WIP por consultor |
| **Digital Enablement** | 5 | Tempo de carregamento de telas (p95), tempo de recálculo (p95 ms), taxa de sucesso de integrações, latência de sincronização, uso de funcionalidades chave |

### Fora do Escopo
- Geração de contratos ou aditivos
- Execução de pagamentos

---

## Integrações

| Sistema | Padrão | Status | Issues |
|---------|--------|--------|--------|
| TOTVS/Datasul | ETLs bidirecionais + Azure Functions | Ativo | Cadastro duplo, CNPJ desalinhado |
| ISA | ISA↔TOTVS↔CRM | Ativo | A redesenhar completamente |
| Adobe Sign | Plugin CRM | Ativo | Migrando de tecnologia |
| Data Lake | ETLs + Events | Ativo | - |
| Área do Cliente | Canvas App (squad separada) | Ativo | Tabelas compartilhadas |
| Vulcano | Manual (PDF) | Ativo | **Será eliminado** |

---

## Equipes

### FTD
| Pessoa | Papel |
|--------|-------|
| Julio Cesar | Tech Lead CRM (sustentação + melhorias) |
| Fernando | Dev (Power Pages, Power Automate, Azure Functions) |
| Kevellin | UX/Dev (Power Pages, protótipos) |
| Oscar de Rooij | PO/Business Analyst (especificações) |
| Eduardo | Functional Consultant (demo, treinamento) |
| Thiago Veiga | Arquiteto de integrações (time separado) |
| Giselle | Infraestrutura (abertura de GMUDs) |
| Karina | Apoio em higienização de dados |
| Claudia | Comercial (desde época do Danilo) |

### Avanade
| Pessoa | Papel |
|--------|-------|
| Danilo Macedo | Tech Lead (conhece FTD desde 2022) |
| João Carlos Figueirôa | Arquiteto |
| Camila Moraes | Analyst/PM |
| Rodrigo Silva | Architect |
| Marcio Jovanello | Manager |
| Fabiana Bacini | Scrum Master / PM |
| Tercio | Gestão de acessos |

### Cerimônias
| Dia | Reunião | Participantes |
|-----|---------|---------------|
| Segunda | Squad trabalho | Dev + UX + Oscar + Kevellin + Fabi |
| Terça | Negócio | Área de negócio |
| Quinta | Tecnologia | Arquitetos + Oscar + Medella + Jovanello |

---

## Roadmap (PBS - 11 Ondas)

| Fase | Deadline | Responsável | Escopo |
|------|----------|-------------|--------|
| MVP | 31/Mar/2026 | FTD | Adição individual de produtos (Power Pages) |
| Habilitadores | Pré-MVP | FTD+Avanade | 7 itens de base (infra, cadastros, integrações) |
| Simulador Faxina | Pós-MVP | Avanade | 6 itens de limpeza de dados |
| Simulador Features | Pós-MVP | Avanade | 4 features principais |
| Onda 1 pós-MVP | ~Ago/2026 | Avanade | Lote, copiar proposta, benefícios, aprovação, faxina |
| Onda 2 Funil | TBD | Avanade | 6 itens de funil de vendas |
| Produtos em Lote | TBD | Avanade | 2 itens |
| TBD | TBD | Avanade | 6 itens a definir |
| Pós-Venda | TBD | Avanade | Comissionamento, inteligência comercial (4 itens) |
| PNLD | TBD | Avanade | 4 itens setor público |
| Jornada | TBD | Avanade | 2 itens de jornada do cliente |

---

## Glossário

| Termo | Significado |
|-------|-------------|
| **Safra** | Ano letivo/comercial (ex: Safra 26 = 2026) |
| **Lumisfera** | E-commerce FTD (plataforma para famílias comprarem) |
| **FTD com Você** | Nome comercial do modelo Lumisfera |
| **Spartan** | App principal de vendas no D365 |
| **Anja** | Pessoa que cadastra proposta no lugar do consultor |
| **Estabelecimento** | Filial FTD no CRM (28 no Brasil) |
| **Mantenedora** | Rede/grupo que mantém escolas (ex: Marista) |
| **Royalty/Markup** | % que escola adiciona ao preço negociado (fica com escola) |
| **Grade** | Produto customizado para ensino médio de escola específica |
| **Alçada** | Nível de aprovação necessário para proposta |
| **GMUD** | Gestão de Mudança - processo de deploy para produção |
| **SMAX** | Sistema de abertura de GMUDs |
| **Vulcano** | Ferramenta externa de aprovação (será eliminada) |
| **ISA** | Sistema de cadastro de produto |
| **PNLD** | Programa Nacional do Livro Didático (governo) |
| **IPCA** | Índice de reajuste anual dos preços |
| **Hunter** | Consultor que prospecta novas contas |
| **Farmer** | Consultor que mantém contas existentes |
| **Smart POS** | Terminal de pagamento para plantão de vendas |
| **Código Substituto** | Link entre produto antigo e novo (entre safras) |

---

## Fontes
- 3 transcrições de onboarding (9, 10, 12 Março 2026)
- Especificação Simulador Comercial - Notion (Oscar, 501 linhas)
- PBS Transformação CX - Miro Board (62 itens, 11 ondas)
- Configuração D365 (`d365-config.yaml`)
- Discovery parcial (`ftd-discovery.md`)
