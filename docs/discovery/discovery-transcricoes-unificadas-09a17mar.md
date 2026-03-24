# Transcrições Unificadas — Reuniões de Negócio FTD (09 a 17/Mar/2026)

**Projeto**: FTD Educação — Transformação CX  
**Data de compilação**: 18/Mar/2026  
**Total de reuniões**: 7 sessões + 1 documento PBS  
**Fontes**: `docs/transcricoes/` (7 arquivos ~587KB total)

---

## Índice Cronológico

| # | Data | Reunião | Participantes-chave | Duração est. |
|---|------|---------|---------------------|-------------|
| 1 | 09/Mar 12:03 | Onboarding Geral | Oscar, Kevellin, Danilo, Camila, João, Fabiana, Rodrigo | ~2h |
| 2 | 09/Mar 18:06 | Protótipo Simulador + Arquitetura Produtos | Oscar, Kevellin, Danilo, Camila, João, Fabiana, Rodrigo | ~1h |
| 3 | 10/Mar 13:05 | Cenário Atual / Onboarding Técnico | Julio, Fernando, Danilo, Camila, João, Kevellin | ~1h |
| 4 | 10/Mar 13:05 | Onboarding Técnico — Arquitetura, Pipeline, Power Pages | Julio, Fernando, Danilo, Camila, João | ~1h |
| 5 | 10/Mar 17:06 | Demo Fluxo Operacional CRM (Conta?Oportunidade?Proposta) | Eduardo, Camila, Danilo, Kevellin, Oscar, Tercio | ~1h |
| 6 | 10/Mar 17:06 | Usabilidade — Navegação CRM e Proposta | Eduardo, Camila, Danilo, Kevellin, João, Tercio | ~2h |
| 7 | 16/Mar 17:03 | Refinamento de Processo Comercial | Oscar, Danilo, Camila, Kevellin, Tercio, João, Julio, Fernando | ~3h+ |
| PBS | - | PBS Transformação CX (CSV) | - | - |

---

## Participantes Completos

### Equipe FTD
| Nome | Papel |
|------|-------|
| **Oscar de Rooij** | Responsável CX / Product Discovery |
| **Kevellin dos Santos Corrado** | Product Owner |
| **Julio Cesar Teixeira dos Santos** | Líder Técnico CRM |
| **Fernando Mauricio da Costa Filho** | Dev back-end / Power Pages |
| **Eduardo De Oliveira** | Analista CRM (demonstrações) |
| **Fabiana Bacini** | Participante (papel não identificado) |

### Equipe Avanade
| Nome | Papel |
|------|-------|
| **Danilo Macedo** | Arquiteto de Soluções |
| **Camila Moraes** | Project Manager |
| **João Carlos Figueirôa de Castilho** | Arquiteto |
| **Rodrigo Silva** | Consultor técnico |
| **Tercio Eduardo Freire Lins** | Consultor auxiliar |

---

## REUNIÃO 1: Onboarding Geral (09/Mar — 12:03)

**Arquivo fonte**: `docs/arquitetura/onboarding.txt` (122KB)

### Tópicos Principais
- Apresentação do protótipo final do Simulador Comercial em Power Pages (identidade visual FTD)
- Validação de UX com 4 grupos: liderança comercial, coordenadores filiais, "anjas", time UX FTD
- Arquitetura catálogo de produtos: 1.283 registros vs. ~15-17 esperados (prateleira)
- Problema: ausência de metadados para distinguir produto prateleira vs. customizado
- 12+ tabelas de preço (problema é visualização/segmentação, não preço diferente)
- Conceito de proprietário de conta (consultor) vs. equipes gestores
- Funcionalidade de cópia de proposta (hoje deficiente)
- Visão futura: IA comercial (recomendações, pacotes pré-montados, comissão projetada)

### Decisões
1. Etapa 3 (Benefícios, Patrocínios, Adiantamento) ? responsabilidade Avanade (fora MVP)
2. Arquitetura tabela de preços ? discussão com Julio, Fernando, Avanade
3. Verificação versionamento nativo proposta ? pauta reunião técnica
4. Apresentação técnica Power Pages ? agendada pós-entrega interna

### Dores Identificadas
- Consultor leva **3 horas para criar 1 proposta** (200 produtos × 3 cliques)
- Cálculos só no final — consultor "às cegas"
- **Excel paralelo** obrigatório como workaround
- Gestão de acesso confusa (proprietário conta, papéis secundários)
- Produtos poluídos: 1.283 para ~15-17 (customizados misturados)
- Mudanças sem aviso geram desconfiança (404 consultores, muitos detratores)
- Desligamento consultor ? sem transferência de contas em 15 dias

### Ações
- [ ] Oscar: Discutir arquitetura catálogo/tabelas preço com Julio/Fernando/Avanade
- [ ] Danilo/Julio: Verificar versionamento nativo proposta
- [ ] Oscar: Validar com gerentes se cópia de oportunidade é utilizada
- [ ] Oscar: Definir classificação "tipo de negociação" (escola/revendedor/família)

---

## REUNIÃO 2: Protótipo Simulador + Arquitetura Produtos (09/Mar — 18:06)

**Arquivo fonte**: `docs/arquitetura/Protótipo Final do Simulador Comercial e Arquitetura de Produtos.txt` (42KB)

### Tópicos Principais
- Demonstração completa do protótipo Power Pages com branding FTD
- Etapa 2 (Produtos e Serviços) em desenvolvimento: filtros rápidos + cache, adição múltipla sem fechar modal
- Resumo financeiro lado a lado: atualização real-time (JavaScript front-end)
- Agrupamento por nível educacional; terminologia revisada com equipe comercial
- Categorização de produtos não modelada:
  - Prateleira (comuns)
  - Customizados compartilhados (alteração leve, qualquer escola)
  - Customizados exclusivos (escola específica)
  - Personalizados exclusivos (mudança pesada, muda código)
- 3 categorias de venda não existem no CRM: escola, revendedor/livreiro, família
- Código "substituto" para versionamento entre safras
- Revisão de oportunidade: removendo campos duplicados com proposta
- Status proposta: 3 campos confusos atuais ? modelo-alvo 2 campos (Status + Estágio)

### Decisões
1. Confirmação Etapa 3 com Avanade
2. Discussão arquitetura tabela preços com equipe técnica
3. Sessão técnica verificação versionamento nativo
4. Eduardo + Oscar: alinhamento status/depara (11/03)

### Dores Identificadas
- ~50 templates documentos Word/PDF para contratos (manejo 100% manual)
- Campos proposta desorganizados: 3 campos para 2 conceitos
- Campos duplicados oportunidade ? proposta
- Fluxo contrato 100% manual (baixar, copiar/colar, ajustar, PDF)

### Ações
- [ ] Oscar: Revisar todos campos de oportunidade (sai/fica/entra)
- [ ] Oscar: Depara de todos estágios na nova estrutura
- [ ] Oscar: Revisão tabela preços com Julio/Fernando
- [ ] Oscar: Tipificação cliente (escola/revendedor/família)
- [ ] Kevellin: Compartilhar lista security roles com perfis
- [ ] Julio: Compartilhar tabela logs integração genérica

---

## REUNIÃO 3: Cenário Atual / Onboarding Técnico (10/Mar — 13:05)

**Arquivo fonte**: `docs/transcricoes/cenario atual.txt` (50KB)

### Tópicos Principais
- **Ambientes**: DEV, UAT, Produção (3 em pipeline); QA e RC (em configuração)
- **Soluções**: 9 separadas por componente (data model, client extension, plugins, site map, etc.)
- Deploy via pipeline Azure DevOps; Produção requer GMUD (SMax/Giselle)
- **Exceção**: deploy plugins para DEV é manual (pipeline não configurado)
- Repositório FTD Dynamics (Azure DevOps); branches dev + master; dev é referência
- Comunicação verbal para evitar sobrescrita (sem proteção automatizada)
- Repositório atual não no mesmo projeto dos PBIs ? sem link automático commits/features
- **Aplicativos CRM**: Spartan (vendas), PNLD (público), Hub SAC, Adobe Sign
- Power Automate: fluxo aprovação em refatoração (muito extenso)
- Usuário serviço "FTDMaxFlow" proprietário dos flows/conexões
- Tabela logs integração genérica (reutilizável)
- Integrações por time separado (Thiago Veiga, arquiteto)
- **Dataverse storage**: nível crítico já ultrapassado
- Débitos técnicos: refatoração Plugin/JS/Power Automate em andamento (não-críticos)
- Índice bugs: muito baixo (2 no mês anterior) após 1,5 ano reestruturação
- Documentação: limitada (não existe modelo ER, guia arquitetura, padrões formais)
- Prefixo: "ftd_" (sem convenção além disso)
- Security roles: problema reconhecido — ambientes não equalizados

### Decisões
1. Apresentação técnica Power Pages ? semana seguinte
2. Coexistência 2 times no pipeline ? sessão dedicada 11/03

### Dores Identificadas
- **Dataverse storage em nível crítico** — pode bloquear novas funcionalidades
- **Governança acesso desigualizada** entre ambientes
- **Modelo ER desatualizado/inexistente**
- **Falta documentação técnica** — conhecimento tácito
- **Pipeline concorrido** com outras áreas — fila
- **Branch protection não automatizada** — comunicação verbal = ponto falha
- **Dois times (FTD+Avanade)** no pipeline sem processo definido

### Ações
- [ ] Fernando/Kevellin: Apresentação técnica Power Pages para Avanade
- [ ] Julio: Compartilhar security roles + descrição perfis
- [ ] Julio/Fernando: Compartilhar tabela logs integração
- [ ] Camila/Danilo: Verificar plano ação storage Dataverse
- [ ] Danilo/Julio: Sessão coexistência pipeline (11/03)

---

## REUNIÃO 4: Onboarding Técnico — Arquitetura, Pipeline, Power Pages (10/Mar — 13:05)

**Arquivo fonte**: `docs/arquitetura/onboarding Tecnico - Arquitetura, Ambientes, Pipeline, Power Pages e Governança do CRM Dynamics.txt` (13KB)

> **Nota**: Esta reunião é continuação/complemento da Reunião 3 (mesmo horário). Conteúdo técnico detalhado sobre a mesma pauta, consolidado acima.

### Tópicos Adicionais
- Licenciamento Power Pages: validado com Microsoft, Entra ID para internos = sem custo adicional
- Azure Functions compartilhadas entre fluxo atual CRM e futuro Power Pages
- Squad muito pequena ? conhecimento tácito compartilhado via acompanhamento
- Azure Key Vault para Azure Functions; Power Automates usam variáveis ambiente

---

## REUNIÃO 5: Demo Fluxo Operacional CRM (10/Mar — 17:06)

**Arquivo fonte**: `Demonstração do Fluxo Operacional do CRM Dynamics (Conta ? Oportunidade ? Proposta).txt` (14KB)

### Tópicos Principais
- **Demo em PRODUÇÃO** — Eduardo criou conta/proposta teste que entrou em fluxo real
- Criação de Conta: campos obrigatórios (CNPJ, CEP, tipo instituição, código MEC)
- Validações: CNPJ duplicado bloqueado; MEC duplicado alerta
- Permissões: consultores, anjas, coordenadores, gerentes podem; PNLD não
- Cadastro Contato: representante legal obrigatório; CPF necessário
- Criação Oportunidade: modelo venda, entrega primária, taxa admin, safra, vigência
- Produtos na Oportunidade: obrigatório antes de criar proposta
- Adição individual + em lote (com filtros)
- Grid de indicação: escolas com histórico têm produtos sugeridos
- Criação Proposta: ID revisão 0 (primeira versão); 15+ campos obrigatórios
- Sincronização CRM ? TOTVS: 1x/dia (~6h)
- Análise de Vendas: descontos, quantidades por aluno, agrupamento por LN/nível
- Matriz de Serviços: 10-15 min processamento (não é obrigatório antes de ativar!)
- Fluxo Aprovação: sequencial, notificação Email+Teams, aprovador vê valores proposta
- Status: Rascunho, Ativo em aprovação, Ativo aprovado, Fechado perdido/cancelado, Contrato emitido
- Revisões: até 27-28 revisões registradas (negociação complexa)
- ~50 templates documentos Word/PDF para contratos

### Decisões
1. Campo "Aplicar desconto em lote" será removido (não funcional)
2. Eduardo + Oscar: alinhamento status/depara (11/03 manhã)

### Dores Identificadas
- Validação serviços **não obrigatória** antes de ativar proposta (gap)
- Matriz serviços leva **10-15 minutos** — impede real-time
- Fluxo aprovação **sequencial** — se aprovador ausente, congela
- Toda revisão volta do zero (sem reabsorção aprovações anteriores)
- Geração contrato 100% manual com ~50 templates
- Estrutura contrato não modularizável

### Ações
- [ ] Eduardo/Oscar: Depara status proposta (11/03)
- [ ] Equipe: Inclusão validação obrigatória "Validar Serviços"
- [ ] Kevellin: Remover campo "Aplicar desconto em lote"
- [ ] Oscar/Jurídico: Revisar/modularizar templates contrato

---

## REUNIÃO 6: Usabilidade — Navegação CRM e Proposta (10/Mar — 17:06)

**Arquivo fonte**: `docs/transcricoes/usabilidade.txt` (161KB)

> **Nota**: Continuação da Reunião 5, mesma sessão. Eduardo continuou demonstrando fluxo completo com foco em experiência de uso.

### Tópicos Adicionais
- Navegação entre abas da proposta (dados gerais, análise vendas, produtos)
- Experiência consultor: muitos cliques, formulários extensos, campos desnecessários
- Notificações: Teams (bot) + e-mail para fluxo aprovação
- Funcionalidade "Gerar Grade Mestre" (grade professor)
- Campo "Transferir Arquivos" — não utilizado atualmente
- Produtos Grade (personalização ensino médio) — sob demanda por escola

---

## REUNIÃO 7: Refinamento de Processo Comercial (16/Mar — 17:03)

**Arquivo fonte**: `docs/transcricoes/Negocio - Refinamento Processo.txt` (181KB)  
**Discovery detalhado**: `docs/discovery/discovery-refinamento-processo-16mar.md`

### Parte 1: Estrutura de Cadastro de Produto
- **Problema central**: 1.283 registros vs. ~15-17 prateleira por LN
- **Causa raiz**: sem metadados (flag customizado/prateleira, variação, segmento/série)
- **Desalinhamento taxonômico**: ISA (Família Produto) ? TOTVS (Família Comercial) ? CRM (Linha Negócio)
- Variações não modeladas: Ponto e Ponto (4 vs. 8 correções), Trilhas (maiúsculas vs. minúsculas)
- Comportamento diferente por LN: Sistema Ensino permite majoração; Didático não
- Reunião brainstorm agendada: Julio, Fernando, Mônica (TOTVS), Avanade

### Parte 2: Tabelas de Preço
- 12+ tabelas ? meta 5 máximo (escola privada, livreiro, público, e-commerce, customizados)
- Preço é **único** — tabelas existem para visualização/segmentação
- Consultoria pricing externa pode trazer diferenciação real (resultado fim Mar/2026)
- Até resultado chegar: motor de regras não será alterado

### Parte 3: Categorização de Contas
- Campo `tipo_de_instituicao` mistura 3 conceitos (organizacional, rede/marca, KA)
- Modelo proposto: hierarquia 3 níveis (Segmento ? Tipo ? Classificação)
- Campo Mantenedora tem 3 usos simultâneos (real, parceiro comercial, legacy)

### Parte 4: Higienização de Contas
- 101.000 contas com problemas (nomes, campos, duplicidade CNPJ)
- Abordagem: recorte 2 anos (2025+2026) como primeiro bloco
- CRM usado como agenda pessoal (WhatsApp, deslocamento, home office) ? polui dados

### Parte 5: Segurança
- Consultor: apenas suas contas
- Coordenador/Gerente: todas contas da filial
- **Contatos: TODOS consultores veem TODOS** (problema privacidade)
- Livreiros: carteirização TBD

### Parte 6: Ecossistema Lumisfera (E-commerce)
- Lumisfera Aberto (B2C puro, não passa CRM)
- Lumisfera Vende Escola (B2C orientado escola)
- Lumisfera B2B Funcionário + B2B Livreiros
- Modelo Cupom E-commerce (desconto pós-visita)

### Parte 7: Modelo Livreiro
- Livreiros **não cadastrados CRM hoje** ? sistema "Pede Livreiro" ? TOTVS
- Projeto deste semestre: migrar para CRM, eliminar "Pede Livreiro"
- 3 cenários: livreiro independente, parceiro escola (2 propostas), parceiro múltiplas escolas

### Parte 8: Fluxo de Aprovação (Especificação Detalhada)
- 7 Níveis de alçada (cumulativos); CRM atual só vai até nível 4
- 4 grupos de regras: percentuais, valores brutos, parcelamento, desconto/contrato
- Plugin + Power Automate existentes; regras possivelmente hardcoded
- Regra de "pulo" em resubmissão: se totalizadores não mudaram, pula para alçada que recusou
- Status/Estágios propostos: Aberto/Ativo/Histórico + Rascunho/Em negociação/Em aprovação/Aprovada sem assinatura/Aprovada e assinada

### Parte 9: Cadeia de Propostas
- Encadeamento: 1.0 ? 1.1 ? ... ? 2.0 (novo ano-safra)
- "Copiar proposta anterior" para renovação (reajuste IPCA + substituição produtos)
- Contrato vs. Aditivo: campo não existe hoje, será adicionado

### Parte 10: Pedido
- Cópia travada da proposta ativa (snapshot contratado)
- Ponte comercial ? operações/CS
- Base futura para faturas

### Parte 11: Oportunidades
- Maioria campos será removida (duplicados proposta)
- Criação massiva 2025/2026 + vínculo propostas existentes
- Uma oportunidade pode ter 2-3 propostas (temporário)

### Parte 12: Etapa 6 — Painel Big Numbers
- Condições comerciais por LN, royalties, adiantamento, patrocínios, comparativos
- Regras alçada visíveis e dinâmicas (barra de ajuste em tempo real)

### Parte 13: Manutenção Política Comercial
- A confirmar se regras hardcoded ou parametrizadas
- Motor não será alterado até consultoria pricing
- Flexibilidade necessária para ajustes frequentes

### Parte 14: Novos Usuários CRM
- Ex-usuários Vulcano precisam acesso CRM (somente leitura: Controladoria, Financeiro)

### Decisões
1. Fluxo aprovação como próxima entrega prioritária
2. Avanade em reunião brainstorm TOTVS
3. Pedido: cópia travada, sem fluxo posterior
4. Piloto geração user stories via agentes IA
5. Oscar marcará o que já existe vs. novo no documento

### Ações
- [ ] Oscar: Marcar no documento existente vs. novo (verde = existente)
- [ ] Avanade: Participar reunião brainstorm TOTVS (quinta)
- [ ] Investigar regras alçada: hardcoded ou parametrizadas
- [ ] Especificação aprovação compartilhada com Avanade para análise
- [ ] Oscar: Categorização contas com equipe comercial
- [ ] Danilo + Kevellin: Piloto user stories via agentes IA

---

## PBS — Transformação CX (Backlog de Produto)

**Arquivo fonte**: `docs/transcricoes/PBS - Transformação CX.csv` (4KB)

| Item PBS | Objetivo |
|----------|----------|
| Simulador de Juros (Royalties, Adiantamento, Parcelamentos) | Eliminar Excel, otimizar infos dentro do CRM |
| Simulador Comercial | Eliminar simulador Excel, ambiente integrado CRM |
| Estrutura de equipe PNLD | Eliminar Excel carteira, formalizar estrutura |
| Gabaritos comissionamento vendas públicas | Eliminar Vulcano/e-mail, comissão automática |
| Processos Royalties/Adiantamento/Patrocínio/Despesas | Eliminar Vulcano, fluxo aprovação novo (DG+Super+Presidente) |
| Customização e Personalização | Eliminar Forms/e-mail, fluxo solicitação?aprovação?produção |
| Rescisão Contratual | Cálculo multa + rastreamento na ferramenta |
| Remuneração Variável (Comissionamento) | Visualização impacto remuneração consultor |
| Livro Mestre — Pós-Adoção | Mapeamento livros entregues para margem rentabilidade |
| Automatização propostas blindagem contratos | Migração automática condições mínimas carteira |
| Livro Mestre — Divulgação | Produtos entregues como divulgação para rentabilidade |

---

## Consolidado: Dores Recorrentes (Cross-Reuniões)

| # | Dor | Recorrência | Severidade |
|---|-----|-------------|-----------|
| 1 | Excel/workarounds paralelos para simulação | TODAS reuniões | ?? Crítica |
| 2 | Dupla aprovação CRM + Vulcano | Reuniões 1, 5, 7 | ?? Crítica |
| 3 | Desalinhamento taxonômico ISA/TOTVS/CRM | Reuniões 1, 2, 7 | ?? Crítica |
| 4 | Cadastro produtos poluído (1.283 vs. ~15) | Reuniões 1, 2, 7 | ?? Alta |
| 5 | 12+ tabelas de preço (deviam ser ~5) | Reuniões 1, 7 | ?? Alta |
| 6 | 101.000 contas com problemas higienização | Reunião 7 | ?? Alta |
| 7 | Dataverse storage nível crítico | Reunião 3 | ?? Crítica |
| 8 | Falta documentação técnica (sem ER, sem padrões) | Reunião 3 | ?? Alta |
| 9 | Security roles não equalizadas entre ambientes | Reuniões 3, 7 | ?? Alta |
| 10 | Contrato 100% manual (~50 templates Word/PDF) | Reuniões 2, 5 | ?? Alta |
| 11 | Fluxo aprovação sequencial (congela se ausente) | Reunião 5 | ?? Alta |
| 12 | CRM usado como agenda pessoal (polui dados) | Reunião 7 | ?? Média |
| 13 | Campo tipo cliente não existe (escola/livreiro/família) | Reuniões 2, 7 | ?? Alta |
| 14 | Contatos visíveis para todos (problema privacidade) | Reunião 7 | ?? Alta |

---

## Consolidado: Decisões Tomadas (Cross-Reuniões)

| # | Decisão | Reunião | Impacto |
|---|---------|---------|---------|
| 1 | Etapa 3 (Benefícios/Patrocínio/Adiantamento) ? Avanade | 1 | Escopo Avanade |
| 2 | Fluxo aprovação = próxima entrega prioritária | 7 | Roadmap |
| 3 | Pedido = cópia travada proposta (sem fluxo posterior) | 7 | Escopo |
| 4 | Motor alçadas não será alterado até consultoria pricing | 7 | Arquitetura |
| 5 | Status/Estágios proposta: modelo novo 2 campos | 2, 5, 7 | Dataverse |
| 6 | Campo "Aplicar desconto em lote" removido | 5 | Limpeza |
| 7 | Piloto user stories via agentes IA | 7 | Processo |
| 8 | Avanade incluída em brainstorm TOTVS | 7 | Colaboração |

---

## Consolidado: Roadmap Confirmado

1. ? Aprovação de propostas (especificação pronta)
2. ? Status/Estágios proposta + oportunidade (prerequisito)
3. Etapa 6 — Painel big numbers
4. Pedido (cópia travada proposta)
5. Estrutura cadastro produto (reunião TOTVS + descoberta)
6. Tabela preço unificada (bloqueado até consultoria pricing fim-Mar)
7. Categorização contas (descoberta em andamento)
8. Higienização contas (pós-categorização)
9. Oportunidades massivas 2025-2026 (pós-simplificação)
10. Pós-venda / Faturas (futuro)

