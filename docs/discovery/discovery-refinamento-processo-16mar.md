# Discovery — Refinamento de Processo Comercial
## Fonte: Transcrição "Negócio - Refinamento Processo" (16/Mar/2026)
## Participantes: Oscar de Rooij, Danilo Macedo, Camila Moraes, Kevellin dos Santos Corrado, Tercio Eduardo Freire Lins, João Carlos Figueirôa de Castilho, Julio Cesar Teixeira dos Santos, Fernando Mauricio da Costa Filho
## Data da Sessão: 16 de março de 2026 (2 sessões — tarde do dia 16 e continuação dia 17)

---

## 1. ESTRUTURA DE CADASTRO DE PRODUTO

### 1.1 Problema Central
- Ao buscar produtos no CRM para o simulador, **aparecem 1.283 registros** quando deveriam aparecer **~17 produtos de prateleira** por linha de negócio.
- Causa raiz: **ausência de metadados** suficientes para filtrar corretamente os produtos (não há flag customizado vs. prateleira, não há variação de produto, não há segmento/série).
- Produtos customizados (ex: "Colégio Sandra Maria") aparecem misturados com produtos padrão.
- **Único modo atual** de o consultor identificar o produto correto: a área interna envia o **código do produto por e-mail**, o consultor digita manualmente no CRM.

### 1.2 Linha de Negócio — Desalinhamento Taxonômico (3 Sistemas)
| Conceito | ISA | TOTVS | CRM |
|----------|-----|-------|-----|
| Agrupamento | Família de Produto | Família Comercial | Linha de Negócio |
| Divergência | Inclui Paradidático, Literatura | Família Comercial com "infinitas variações" | ~9 valores (Didático, Didático Idiomas, Didático Vendo Escola, FC Zoom, Evolution, etc.) |

- **Não há equivalência direta** entre os 3 sistemas.
- BI comercial, BI de produtos e BI da controladoria fazem **cada um sua leitura diferente** sobre os mesmos dados; nenhum alinhado com os outros.
- Anomalias: BI de vendas junta "Didático", "Didático Idiomas" e "Didático Vendo Escola" em um só; BI de produtos os separa; Controladoria faz outra coisa.
- **Evolution** deveria estar dentro de Sistema de Ensino (é inglês), mas está separado.
- **FC Zoom** deveria estar em Serviços Educacionais.
- **Dicionário** e **Agenda** não deveriam ser linhas de negócio.

### 1.3 Variações de Produto (Não Modeladas)
- **Ponto e Ponto**: plataforma digital vendida com créditos de correção de redação — variação de 4 ou 8 correções (preços diferentes, mesmo produto base).
- **Trilhas**: versão com letras maiúsculas (infantil I) e minúsculas (infantil II) — escola escolhe, mesma coleção.
- Não existe o conceito de **variação de produto** (como P/M/G no e-commerce) na estrutura atual.

### 1.4 Comportamento por Linha de Negócio
- **Sistema de Ensino**: permite majoração de preço (aumento sobre tabela).
- **Didático**: **não** permite majoração.
- **FTDSE** (variante): complexidade do novo ensino médio + campo "grade" (exclusivo para este modelo, criado como solução de contorno).

### 1.5 Status e Ações
- Frente de trabalho iniciada para revisar cadastro de produto e estrutura de campos.
- **Reunião de brainstorm** agendada (quinta-feira da semana de 16/Mar) com Julio, Fernando (CRM) e Mônica (TOTVS) para alinhar estrutura e definir soluções com base em restrições (tabela-core do TOTVS não pode ser editada → necessidade de tabela derivada).
- **Trabalho andando devagar** — Oscar reportou necessidade de ajuda para escalar. Impacta todos os BIs da empresa, divergência de visões entre áreas.
- Equipe Avanade (Danilo, Camila, Tercio) será incluída na reunião.

---

## 2. TABELAS DE PREÇO

### 2.1 Situação Atual
- **12+ tabelas de preço** no CRM; na prática, o **preço é único** (as tabelas existem apenas para **visualização/segmentação** de quais produtos ficam disponíveis para quais contextos).
- Consultor precisa **saber em qual tabela de preço** está o produto que quer vender (análogo a ter múltiplos catálogos impressos).
- Produto pode aparecer em mais de uma tabela por **erro operacional** (não é para acontecer).

### 2.2 Modelo-Alvo (Consenso Parcial)
No máximo **5 tabelas** legítimas:
1. Venda para **escola do mercado privado**
2. Venda para **livreiro do mercado privado**
3. Venda para **escola pública**
4. **E-commerce aberto** (não passa pelo CRM, vai Lumisfera → TOTVS direto)
5. **Customizados** (produtos vinculados a escola ou mantenedora específica)

### 2.3 Lógica de Recorte para o Simulador
- Tabela de preço (ano-safra) = produtos que são **vendáveis** este ano (subconjunto de todos os cadastrados).
- Para a proposta do cliente: recorte adicional por **mercado** (privado/público) e **tipo de cliente** (escola/livreiro).
- **Decisão técnica**: pode ser 1 tabela com colunas de flags, ou múltiplas tabelas — o que for mais fácil de desenvolver.

### 2.4 Pricing — Consultoria Externa
- Consultoria contratada atuando em **definição de pricing** (rentabilidade do cliente, rentabilidade de produto).
- **Resultado esperado**: final de março/2026.
- Impacto: pode haver **diferenciação real de preço** por segmento, região, tipo de escola (premium, triple-A), canal de venda — isso ainda é desconhecido.
- Até que o resultado chegue, o motor de regras de alçada **não será alterado**.

---

## 3. CATEGORIZAÇÃO DE CONTAS

### 3.1 Campo Problema: `tipo_de_instituicao`
Mistura **3 conceitos diferentes** no mesmo nível:
- **Tipo organizacional**: confessional, laica
- **Rede/Marca específica**: Marista (que é confessional), Rede S
- **Key Account**: grandes redes tratadas como KA

Exemplo: filtrar "confessionais" exige incluir "confessional" **e** "Marista" separadamente.

### 3.2 Modelo Proposto (Em Discussão)
Hierarquia de **3 níveis**:
1. **Nível 1**: Segmento (Privado / Público)
2. **Nível 2**: Tipo de Instituição (Confessional, Laica, Rede, etc.)
3. **Nível 3**: Classificação Comercial / KA

- Oscar recebeu primeira versão desta categorização, mas ainda com dúvidas e inconsistências.

### 3.3 Campo `Mantenedora` (Reaproveitado)
Atualmente tem **3 usos simultâneos** no mesmo campo:
1. **Mantenedora real** (grupo que mantém a escola)
2. **Parceiro comercial** (livreiro vinculado a uma proposta da escola)
3. Outro uso legacy

- Problemas: parceiro comercial não faz sentido dentro de "mantenedora"; campos da tela de mantenedora não se aplicam a parceiro comercial.

---

## 4. CONTAS E HIGIENIZAÇÃO

### 4.1 Volume e Estado
- **101.000 contas de escola** cadastradas no CRM.
- Problemas: nomes não batem (com/sem til, abreviaturas), campos desalinhados entre CRM e TOTVS, duplicidade de CNPJ (proteção contra duplicidade só veio nos últimos 1-2 anos), escolas cadastradas com **CNPJ de livraria** (workaround para faturamento).

### 4.2 Abordagem Planejada
- **Recorte de 2 anos** (safra 2025 + 2026) como primeiro bloco de trabalho.
- Diagnóstico: exportar dados CRM + TOTVS, comparar campo a campo, identificar percentual de sincronização.
- **Equipe comercial** precisa acompanhar para definir regras (ex: qual sistema é master por campo).
- Estratégia para duplicatas: considerar arquivar as não sincronizadas com TOTVS (dado que CRM nunca foi utilizado adequadamente para relacionamento/inteligência).

### 4.3 Tipos de Compromisso (Poluídos)
- Exemplo de registros: WhatsApp, prestação de contas, deslocamento, home office, revisão de veículo.
- CRM usado como **agenda pessoal** do consultor — polui dados para inteligência.
- Alternativa: usar Outlook para agendas pessoais; Dynamics tem ferramentas de check-in com geolocalização.

### 4.4 Responsáveis e Timeline
- Julio (FTD) está conduzindo análise inicial; passará para Avanade quando começarem ações de extração e correção massiva.
- **Sem deadline definido** para higienização — reportado como sem cronograma.

---

## 5. MODELO DE SEGURANÇA

| Perfil | Acesso |
|--------|--------|
| **Consultor** | Apenas suas próprias contas (confirmado por Marcel) |
| **Coordenador/Gerente de filial** | Todas as contas da filial |
| **Contatos** | Todos os consultores veem todos os contatos (problema: acesso a dados pessoais de diretores de escolas que não são seus clientes) |
| **Livreiro** | Carteirização TBD (ainda não cadastrados no CRM) |

---

## 6. ECOSSISTEMA LUMISFERA (E-COMMERCE)

### 6.1 Lojas
| Loja | Tipo | Relação com CRM |
|------|------|-----------------|
| **Lumisfera Aberto** | E-commerce aberto (tipo Amazon) | Compras **não** passam pelo CRM (direto → TOTVS) |
| **Lumisfera Vende Escola** (B2C) | Escola orienta famílias a comprar na loja | Vinculado a proposta CRM, modelo "FTD com Você" |
| **Lumisfera B2B Funcionário** | Funcionário da escola compra | Proposta CRM |
| **Lumisfera B2B Livreiros** | Substituirá "Pede Livreiro" | Projeto deste semestre |

### 6.2 Modelo de Cupom E-commerce
- Consultor visita escola, escola não fecha → consultor gera **cupom de desconto** (ex: 30%) para a escola comprar no Lumisfera Aberto.
- Lumisfera cria **landing page** com nome da escola e produtos com desconto.
- Tudo na mesma proposta (não é proposta separada).

---

## 7. MODELO DE VENDA: LIVREIRO (PARCEIRO COMERCIAL)

### 7.1 Situação Atual
- Livreiros **não** são cadastrados no CRM (sistema separado "Pede Livreiro" → TOTVS).
- **Projeto deste semestre**: migrar para CRM, eliminar "Pede Livreiro".

### 7.2 Cenários de Venda do Livreiro
1. **Livreiro como vendedor independente**: proposta própria no CRM, apenas linhas de negócio permitidas (sem Sistema de Ensino).
2. **Livreiro como parceiro comercial de escola**: escola fecha proposta com FTD, livreiro é ponto de entrega/venda. Proposta é da **escola** (pode incluir Sistema de Ensino). São **2 propostas separadas**.
3. **Livreiro pode ser parceiro de múltiplas escolas** (sem limitação).
4. Informações da proposta CRM são compartilhadas com **Lumisfera B2B** para livreiros.

### 7.3 Tipo de Cliente (Não Existe Hoje)
- Campo `tipo_de_cliente` (escola vs. livreiro) **não existe** no CRM.
- Hoje, a distinção é inferida apenas pelo **modelo de venda** na proposta (misturado com todos os outros).
- Com tipificação adequada: proposta filtraria modelos de venda aplicáveis, experiência mais fluida.

---

## 8. FLUXO DE APROVAÇÃO DE PROPOSTAS (Especificação Detalhada)

### 8.1 Dores Identificadas
1. Processos em **múltiplos sistemas** (CRM + TOTVS + Vulcano) → ineficiência e erros.
2. **Duplicidade de aprovações** entre Vulcano e CRM → retrabalho.
3. Validação com informações **fragmentadas** (CRM + simulador Excel anexado).
4. Aprovadores têm **dificuldade** em visualizar indicadores críticos rapidamente.
5. Tempo de aprovação **longo**; consultor sem visibilidade de onde a proposta está parada.
6. Regras de alçada dependem de **conhecimento tácito** ou consulta manual.

### 8.2 Objetivo
- Aprovação em **único sistema** (eliminar Vulcano).
- Redução do tempo de trâmite.
- **Painel unificado** (Etapa 6 no simulador/Power Pages) para gestores aprovarem com um clique.
- Política comercial **visível** para o consultor durante a construção da proposta.
- Rastreabilidade completa.

### 8.3 Alçadas de Aprovação (7 Níveis)

| Nível | Aprovadores (cumulativo) |
|-------|--------------------------|
| 1 | Consultor (auto-aprovação) |
| 2 | Nível 1 + Coordenador |
| 3 | Nível 2 + Gerente |
| 4 | Nível 3 + Diretor Adjunto + Diretor Comercial (Quintela) |
| 5 | Nível 4 + Diretor-Geral (Alisson) |
| 6 | Nível 5 + Superintendência |
| 7 | Nível 6 + Presidência |

- **Adicionais** (após nível 4): Controladoria e Financeiro participam (não necessariamente como aprovadores, mas para acompanhar/capturar informações).
- **No CRM hoje**: implementado até nível 4 (Quintela). Níveis 5-7 precisam ser adicionados.
- Coordenador e Gerente são definidos pela **região da conta**. Diretor Adjunto para cima é **pessoa única** (não varia por região).

### 8.4 Regras de Alçada (Motor Existente — Não Será Alterado)

#### Regras Percentuais
| Regra | Condição | Nível |
|-------|----------|-------|
| **Royalty** | ≤ 50% | 2 |
| | > 50% e < 70% | 3 |
| | ≥ 70% | 4 |
| | (e valores mais altos → 5, 6, 7) | 5-7 |
| **Adiantamento** | < 50% | 4 |
| | ≥ 50% | 4 |
| **Patrocínio** | Não tem | 1 |
| | < 3% | 2 |
| | ≥ 3% | 3 |
| **Taxa de Administração** | < 5% | 4 |
| | ≥ 5% e < 20% | 1 |
| | ≥ 20% | 2 |

#### Regras de Valor Bruto
| Regra | Condição | Nível |
|-------|----------|-------|
| **Adiantamento (valor absoluto)** | ≤ R$ 100.000 | 4 |
| | > R$ 100.000 | 5 |
| **Royalty (valor absoluto)** | Não tem | 1 |
| | (escala crescente → 2,3,4,5,6,7) | 2-7 |

#### Regras de Parcelamento
| Condição | Nível |
|----------|-------|
| Até 6 parcelas (sem juros) | 1 |
| ≥ 7 parcelas (sem juros) | 4 |

- **O nível mais alto** entre todas as regras analisadas prevalece.
- **Regras de desconto de produto**: motor complexo já existente, não especificado, não será alterado.
- **Existência de contrato**: campo para marcar que entidade pública ou tipo específico não assina contrato (já em desenvolvimento).

### 8.5 Implementação Técnica Atual
- **Plugin** roda na ativação da proposta → analisa informações → define alçada por regra → campo único com nível máximo.
- **Power Automate (Flow)** dispara após ativação → consulta nível → busca aprovadores (cadastrados por região na entidade de região) → envia notificação → sequencial (primeiro aprova nível N, depois verifica se há N+1).
- **Incerteza**: regras podem estar hardcoded no plugin ou parametrizadas em tabela — **a confirmar com Julio/Fernando**.
- Notificações: Teams (confirmado), e-mail (a confirmar).

### 8.6 Fluxo de Aprovação — Comportamento Detalhado

#### Início
1. Cliente aprova proposta (hoje: WhatsApp para consultor; futuro: aprovação no portal).
2. Status da proposta: **aberto** (mantém). Estágio: **negociação → em aprovação**.
3. Oportunidade sincroniza: etapa também vai para "em aprovação".
4. **Oportunidade nunca volta atrás** (diferente da proposta que pode ser revisada).

#### Tramitação
- Aprovação sequencial e cumulativa (N1 → N2 → N3...).
- Aprovadores notificados por **e-mail e Teams** com link direto para o simulador (**Etapa 6** — tela visual com big numbers para decisão).
- **Recusa**: exige preenchimento de **razão da recusa** (guardada, apresentada ao consultor).

#### Recusa e Resubmissão — Regra de Pulo
- **Versão 1** de qualquer proposta/aditivo: **sempre** passa por todas as alçadas.
- **Versão 2+** após recusa: se os **totalizadores não foram impactados** (royalty, adiantamento, descontos), a proposta pode **pular** diretamente para a alçada que recusou (reaproveitando aprovações anteriores).
- **Justificativa**: mudanças que não impactam totalizadores são operacionais (ex: modalidade de entrega, configuração de loja).
- Se **totalizadores foram impactados**: volta ao início das aprovações.
- Se nova versão impactar totalizadores: **precisa nova aprovação do cliente** antes de reentrar no fluxo de aprovação interno.
- Aprovações reaproveitadas devem ser **visualmente indicadas** na interface (ex: "aprovação capturada na versão anterior").

#### Finalização
1. Última aprovação → estágio: **aprovada sem assinatura**.
2. Equipe administrativa confecciona contrato manualmente (automação futura).
3. Contrato anexado → processo de assinatura Adobe Sign pelo CRM (já existente).
4. Assinatura recebida → estágio: **aprovada e assinada** (automático via integração existente).
5. Status: **ativo**. Proposta anterior: status **histórico**.

### 8.7 Status e Estágios da Proposta (Modelo-Alvo)

| Status | Significado |
|--------|-------------|
| **Aberto** | Consultor trabalhando (negociação, aprovação) |
| **Ativo** | Vigente (contrato assinado) |
| **Histórico** | Arquivada (substituída por versão mais nova) |

| Estágio | Quando |
|---------|--------|
| Em negociação | Consultor + cliente batendo/voltando |
| Em aprovação | Fluxo interno de alçadas |
| Aprovada sem assinatura | Todas as alçadas aprovaram, contrato pendente |
| Aprovada e assinada | Adobe Sign concluído |

> **NOTA**: Estes status e estágios **não existem** no CRM hoje. Precisam ser criados.

---

## 9. CADEIA DE PROPOSTAS (Encadeamento e Vínculo)

### 9.1 Conceito
- Propostas contam a **história** do que foi negociado com o cliente.
- Cada nova versão (recusa/aditivo) é vinculada à anterior via **campo de relacionamento**.
- Vínculo também existe entre propostas de **anos-safra diferentes** (cópia interanual).

### 9.2 Nomenclatura de Versões
- **Dentro do ano-safra**: 1.0 → 1.1 → 1.2 → ... (versões por recusa/ajuste).
- **Novo ano-safra**: 2.0 (cópia da última aprovada do ano anterior, ex: 1.4).
- Versão 1 de qualquer aditivo **sempre** passa por todas as aprovações.

### 9.3 Funcionalidade "Copiar Proposta Anterior"
- **Não existe hoje** (a ser desenvolvida).
- Para renovação anual: copia proposta vigente do ano anterior.
- Aplica: reajuste de preço por linha de negócio (IPCA), substitui produtos descontinuados por código substituto, remove produtos que saíram da tabela.
- Modal exibe: linha de negócio + percentual de reajuste definido pela FTD.

### 9.4 Aditivo
- **Aditivo** = quando alteração é feita sobre contrato já assinado (ativo).
- É criado como nova proposta, vem **preenchida** com dados da proposta anterior.
- Distinção entre contrato (cliente novo) e aditivo (renovação/alteração) será via campo na **Etapa 1** (dados da proposta) — **não existe hoje**, será implementado no simulador.
- Quando aditivo é aprovado e assinado: aditivo → **ativo**, anterior → **histórico**.

---

## 10. PEDIDO

### 10.1 Conceito
- Pedido = **cópia** da proposta ativa (snapshot do que foi contratado).
- É a **ponte** entre equipe comercial e equipe de operações/Customer Success.
- Permite acompanhar histórico exato do que cada cliente contratou.
- Base futura para **faturas** (royalties, adiantamento, patrocínio).

### 10.2 Escopo Atual (Decisão da Sessão)
- **Apenas criar** o pedido como cópia da proposta (sem ações adicionais).
- Pedido deve ser **travado** (não permitir alteração após criação).
- Não será trabalhado fluxo pós-venda, status de pedido, faturamento — fica para fase futura.
- Entidade de pedido já existe no CRM (126 registros antigos), mas não é utilizada há tempos.
- Regra: pedido **só pode ser criado** a partir de uma proposta.

### 10.3 Pós-Venda (Futuro)
- Pedido será base para: planejamento de implantação, setup Lumisfera, setup Iônica, previsão logística, impressão/entrega de livros, pagamento de royalties, pagamento de adiantamento, pagamento de patrocínio.
- **Hub do SAC** (Dynamics Customer Service) é usado pelo CRC — já operacional.

---

## 11. OPORTUNIDADES

### 11.1 Situação Atual
- Oportunidades **desvinculadas** de propostas (dados de 2024 e anteriores).
- Maioria dos campos será **removida** (vermelho no mapeamento) — duplicados da proposta.
- Business Process Flow existente **sem travas** obrigatórias (nenhum campo obrigatório por estágio).

### 11.2 Ações Necessárias
- **Criação massiva** de oportunidades 2025/2026 + vínculo com propostas existentes (algoritmo).
- Simplificação radical dos campos.
- Oportunidade **acompanha** o status da proposta (mas nunca regride).
- Uma oportunidade pode ter **2-3 propostas** (por modelo de venda) — temporário até futura unificação (1 proposta = N modelos de venda).

---

## 12. ETAPA 6 — ANÁLISE E APROVAÇÃO (Big Numbers)

### 12.1 Propósito
- Tela unificada no simulador (Power Pages) com **todas as informações** necessárias para o aprovador tomar decisão rapidamente.
- Elimina necessidade de caçar dados em múltiplos sistemas.

### 12.2 Informações Previstas
- Análise de **condições comerciais** por linha de negócio (preenchimento dinâmico).
- Diferença da tabela por linha de negócio.
- Royalties, adiantamento, patrocínios, descontos.
- Comparativos com propostas anteriores.
- **Regras de alçada** visíveis e dinâmicas (barra de ajuste mostra regra ativando/desligando em tempo real).
- Informações dos totalizadores que determinam o nível de aprovação necessário.

### 12.3 Campos e Dados
- Campos necessários **já existem** na proposta (campo de royalty, adiantamento, etc.).
- Problema: **valores** dos campos (ex: linha de negócio) estão incorretos/desalinhados.
- Estratégia: desenvolver a tela com os campos existentes; quando a revisão de categorização for concluída, os valores se ajustam automaticamente.
- **Patrocínio**: hoje é campo numérico simples; futuro será grid tipificado (mas desenvolvimento pode iniciar com campo numérico).

---

## 13. MANUTENÇÃO DA POLÍTICA COMERCIAL

- Necessidade de entender se regras estão **hardcoded no plugin** ou **parametrizadas em tabela**.
- Manutenção de aprovadores (férias, substituição, saída) precisa ser viável sem desenvolvimento.
- Motor atual **não será alterado** até chegarem novas regras da consultoria (pricing + rentabilidade).
- **Quando novas regras chegarem**: avaliar se motor atende; possibilidade de **refazer o motor** se regras forem completamente diferentes.
- Expectativa de que novas regras precisarão de **flexibilidade** para ajustes frequentes (área comercial ainda não madura; piloto → iterações).

---

## 14. NOVOS USUÁRIOS NO CRM

- Além de aprovadores, **usuários que hoje acessam o Vulcano** para visualizar informações de proposta precisam ter acesso ao CRM.
- Perfis: Controladoria, Financeiro, outros — acesso **somente leitura** para acompanhamento.
- Necessário: mapeamento completo de quem são esses usuários e quais informações precisam.
- Objetivo final: relatórios que atendam cada área, eliminando necessidade de "pegar dados da proposta no meio do fluxo".

---

## 15. DECISÕES E ENCAMINHAMENTOS DA SESSÃO

| # | Decisão/Encaminhamento | Responsável | Status |
|---|------------------------|-------------|--------|
| 1 | Reunião brainstorm TOTVS (estrutura de produto + tabelas-core) na quinta | Oscar + Julio + Fernando + Mônica + Avanade | Agendada |
| 2 | Avanade (Danilo, Camila, Tercio) incluídos na reunião TOTVS | Oscar | Feito |
| 3 | **Aprovação de proposta** como próxima entrega prioritária | Consenso | Confirmado |
| 4 | Piloto de geração de **user stories via agentes de IA** com especificação de aprovação | Danilo + Kevellin | A fazer |
| 5 | Oscar marcará no documento o que **já existe** vs. o que é **novo** (verde = existente) | Oscar | Em andamento |
| 6 | Verificar se regras de alçada estão hardcoded ou parametrizadas | Avanade (aprofundamento técnico com Julio/Fernando) | Pendente |
| 7 | Pedido: criar como cópia travada da proposta, sem fluxo posterior | Consenso | Confirmado |
| 8 | Especificação de aprovação será compartilhada com Avanade para análise | Oscar/Kevellin | Feito (link enviado) |
| 9 | Cronograma será definido em sessão seguinte | Oscar + Danilo | Pendente |
| 10 | Conversa sobre categorização de contas com equipe comercial (amanhã 8h) | Oscar | Agendada |

---

## 16. ROADMAP / PRIORIZAÇÃO (Confirmada na Sessão)

| Prioridade | Item | Dependência | Status |
|-----------|------|-------------|--------|
| **1** | Fluxo de aprovação de propostas | Nenhuma (regras atuais, independente de novas alçadas) | Especificação pronta |
| **2** | Revisão de status/estágios de proposta e oportunidade | Pré-requisito para aprovação | A desenvolver |
| **3** | Étapa 6 — Painel de big numbers no simulador | Campos existem (mesmo com valores incorretos) | A desenvolver |
| **4** | Pedido (cópia travada da proposta) | Fluxo de aprovação concluído | A desenvolver |
| **5** | Estrutura de cadastro de produto | Reunião TOTVS + alinhamento 3 taxonomias | Em Discovery |
| **6** | Tabela de preço unificada | Resultado consultoria pricing | Bloqueado (fim mar/2026) |
| **7** | Categorização de contas | Definição comercial em andamento | Em Discovery |
| **8** | Higienização de contas | Categorização definida | Planejamento inicial |
| **9** | Oportunidades massivas 2025-2026 | Simplificação de campos | A planejar |
| **10** | Pós-venda / faturas | Pedido implementado | Futuro |

---

## 17. PREMISSAS CONFIRMADAS

1. **Todo entregável** deve vir acompanhado de **relatório/BI** de acompanhamento — premissa inegociável.
2. Objetivo da Onda 1 pós-MVP: entregar pela primeira vez na FTD um CRM capaz de acompanhar **propostas, oportunidades e contas** com relatórios de gestão para 2025-2026.
3. Motor de aprovação pode ser entregue **antes** da definição das novas regras de alçada — motor e regras são independentes.
4. Desenvolvimento iterativo: entregar com dados/campos existentes (mesmo imperfeitos); ajustar quando revisões forem concluídas.
5. Agentes de IA serão testados como ferramenta para agilizar escrita de user stories (piloto com especificação de aprovação).

---

## 18. GAPS DE INFORMAÇÃO (A Investigar)

| # | Gap | Ação |
|---|-----|------|
| 1 | Regras de alçada hardcoded vs. parametrizadas | Aprofundamento técnico com Julio/Fernando |
| 2 | Mapa completo de usuários do Vulcano (quem precisa acessar CRM) | Oscar mapear |
| 3 | Carteirização de livreiros (propriedade exclusiva ou não) | Definir com área comercial |
| 4 | Resultado da consultoria de pricing | Esperado final mar/2026 |
| 5 | Novas regras de alçada da consultoria de rentabilidade | Sem previsão |
| 6 | Funcionalidade existente de copiar proposta (além da recusa) | Confirmar com Julio/Kevellin |
| 7 | Campos do pedido (depará com proposta) | Verificar entidade existente |
| 8 | Métricas de engajamento do CRM (Henrique/Marcio Analytics) | Oscar verificar com Medella |
| 9 | Dados pós-venda: ferramenta usada pelo CRC | Confirmado: Hub do SAC (Dynamics Customer Service) |
| 10 | Notificação por e-mail (além de Teams) | Confirmar canal atual |
