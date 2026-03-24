# Discovery & Análise - FTD Educação
## Status: 🟡 EM ANDAMENTO (Base de Transcrições)
## Última Atualização: 16 Março 2026
## Fontes: Onboarding (9-12/Mar), Refinamento Processo (16/Mar)

---

## 1. CONTEXTO DO CLIENTE

| Item | Detalhe |
|------|---------|
| Cliente | FTD Educação S/A |
| Grupo | Grupo Marista |
| Indústria | Educação / Editora / Distribuição de Livros Didáticos |
| Histórico | +120 anos de atuação |
| Sede | São Paulo, SP |
| Filiais | 28 (distribuição nacional) |
| Impacto | ~1.5 milhão de estudantes |
| Pico Sazonal | Nov-Jan (adoção escolar, ~5.000 contratos/dia) |

---

## 2. PLATAFORMA ATUAL

### CRM: Dynamics 365 CE Online
- **Apps**: Spartan (vendas), PNLD (público), Hub SAC (atendimento), Adobe Sign (contratos)
- **Ambientes**: Dev, OAT, Prod (QA e RC criados mas não configurados)
- **Solutions**: 9 segmentadas por tipo de componente
- **Pipeline**: Azure DevOps (plugins: manual para Dev, automático para OAT/Prod)
- **Repo**: FTD Dynamics (branch `dev` = source of truth)

### Stack Técnico
- C# Plugins (poucos, bug rate baixo após 1.5 anos de refactoring)
- JavaScript Web Resources (em refatoração)
- Power Automate (fluxo de aprovação sendo refatorado)
- Azure Functions (cálculos de proposta, renovação automática)
- Power Pages (Simulador Comercial - em desenvolvimento)
- TOTVS/Datasul (ERP), ISA (cadastro de produto)
- Datadog + Grafana + Application Insights (monitoring)
- Azure Key Vault (para Azure Functions)

---

## 3. PROCESSO COMERCIAL (As-Is Mapeado)

### Jornada Completa
```
Conta → Contato (Rep. Legal) → Oportunidade → Produtos → Proposta (6 etapas) → Aprovação (4 alçadas) → Contrato → Adobe Sign → TOTVS
```

### Etapas da Proposta
1. **Dados da Proposta**: cliente, sócio, destinatário, tipo, safra, vigência, alunado
2. **Produtos e Serviços**: grid, adição individual/lote, filtros, totalizadores
3. **Benefícios, Doações, Patrocínios, Adiantamento**: ⬜ A desenvolver (Avanade)
4. **Configuração de Vendas/Canais**: ⬜ A desenvolver (Avanade)
5. **Matriz de Serviços**: Em revisão
6. **Análise e Aprovação**: Big numbers, comparativo, alçadas visuais - ⬜ A desenvolver

### Linhas de Negócio
| Linha | Majoração | Lote | Obs |
|-------|-----------|------|-----|
| Sistema de Ensino | ✅ | ✅ | Coleções (Trilhas ~15 materiais/série). Inclui sub-linhas: Evolution (inglês), Grade (ensino médio). Trilhas tem variação maiúsculas/minúsculas (infantil) |
| Didático | ❌ | ✅ | Inclui sub-linhas: Didático Idiomas, Didático Vendo Escola — BI junta tudo num só, mas TOTVS separa |
| Bilíngue | ✅ | ✅ | - |
| Espanhol | ✅ | ✅ | - |
| Literatura | ❌ | ❌ | Produtos individuais (ex: Pequeno Príncipe) |
| APE | ❌ | ✅ | - |
| FC Zoom | N/A | N/A | Deveria estar dentro de Serviços Educacionais (erro de categorização) |
| Dicionário | N/A | N/A | Não deveria ser linha de negócio separada |
| Agenda | N/A | N/A | Não deveria ser linha de negócio separada |

### Canais de Venda
- FTD com Você (Lumisfera e-commerce) - **principal** (B2C: famílias compram na loja online da escola)
- Venda Direta (faturamento/invoice)
- Frente de Loja / Balcão Escola
- Smart POS (plantão de vendas presencial)
- **Mudança conceitual**: era 1 proposta/canal → 1 proposta multi-canal

### Ecossistema Lumisfera (Refinamento 16/Mar)
Lumisfera = plataforma Adobe Commerce com múltiplas "lojas":
- **Lumisfera Aberto** (e-commerce aberto, concorrente Amazon) — compras não passam pelo CRM, vão direto TOTVS
- **Lumisfera Vende Escola** (B2C) — escola orienta famílias a comprar na loja, vinculado a proposta CRM com modelo "FTD com Você"
- **Lumisfera B2B Livreiros** (novo, substituirá sistema "Pede Livreiro") — livreiro tem conta CRM, proposta CRM, compartilha com Lumisfera
- **Modelo cupom e-commerce**: consultor pode gerar cupom de desconto para escola comprar no Lumisfera Aberto (mesma proposta)

### Modelo de Venda: Livreiro (Parceiro Comercial)
Novo conceito detalhado no refinamento de 16/Mar:
- **Livreiro** = conta que atualmente NÃO é cadastrada no CRM (sistema separado "Pede Livreiro" → TOTVS)
- **Projeto deste semestre**: migrar para CRM, matar "Pede Livreiro"
- **Livreiro como vendedor independente**: proposta própria (só linhas de negócio permitidas, sem Sistema de Ensino)
- **Livreiro como parceiro comercial de escola**: escola fecha proposta com FTD, livreiro é o ponto de entrega/venda. A proposta é da escola (pode incluir Sistema de Ensino). São 2 propostas separadas.
- **Livreiro pode ser parceiro de múltiplas escolas** (sem limitação)
- **Landing page**: Lumisfera cria landing page com nome da escola e produtos com desconto para facilitar experiência da família

---

## 4. FIT-GAP ANALYSIS (Preliminar)

### 🟢 FIT (Funciona OOB)
- Gestão básica de contas e contatos
- Oportunidades (com ressignificação pendente)
- Model-driven Apps (Spartan, Hub SAC, PNLD)

### 🔵 CONFIG (Business Rules, Views, Forms)
- Reorganização de campos nas etapas da proposta
- Funil de oportunidade (melhoria de navegabilidade)
- Status/Stage da proposta (simplificação de 3→2 campos)

### 🟡 LOW-CODE (Power Automate, Canvas)
- Fluxo de aprovação multi-nível (refatoração do existente)
- Notificações Teams/Email para alçadas
- Matriz de serviços (revisão do processamento)

### 🟠 PRO-CODE (Plugins, PCF, Azure Functions, Power Pages)
- **Simulador Comercial** (Power Pages - desenvolvimento ativo)
- Cálculos de proposta (Azure Functions + plugin débounce)
- Adição de produtos em lote (Power Pages)
- Copiar proposta anterior (código substituto + reajuste IPCA)
- Benefícios/Doações/Patrocínios (grid interativo)
- Integração cadastro de produto (ISA ↔ TOTVS ↔ CRM)
- Higienização de base (CNPJ, produtos, tabela de preço)

### 🔴 GAP / A Definir
- Comissionamento do consultor no simulador
- Inteligência comercial (upsell, cross-sell, pacotes)
- Modularização de contratos Word (depende de revisão jurídica)
- IA em propostas (maturidade de dados insuficiente)

---

## 5. PAIN POINTS IDENTIFICADOS

| # | Pain Point | Severidade | Fonte |
|---|-----------|-----------|-------|
| 1 | Consultor leva até 3 HORAS para criar 1 proposta | 🔴 Crítico | Oscar (onboarding) |
| 2 | Dataverse em nível CRÍTICO de armazenamento | 🔴 Crítico | Julio (cenário atual) |
| 3 | Tabela de produtos poluída (1.283 vs 15) — sem metadados para filtrar, sem variação de produto, sem distinção customizado/prateleira | 🔴 Crítico | Oscar (onboarding + refinamento 16/Mar) |
| 4 | 12+ tabelas de preço (workaround de visualização, preço é único) → devem ser no máximo 5 | 🟠 Alto | Oscar (refinamento 16/Mar) |
| 5 | Campos duplicados Oportunidade ↔ Proposta | 🟠 Alto | Oscar (onboarding) |
| 6 | 50+ templates Word para contratos | 🟠 Alto | Oscar/Eduardo (usabilidade) |
| 7 | Vulcano: aprovação duplicada | 🟡 Médio | Oscar (onboarding) |
| 8 | Security roles não equalizadas | 🟡 Médio | Julio (cenário atual) |
| 9 | Cadastro duplo CRM/TOTVS sem owner | 🟡 Médio | Oscar/Jovanello (onboarding) |
| 10 | Repo desacoplado dos PBIs | 🟡 Médio | Julio (cenário atual) |
| 11 | Plugin deploy manual para Dev | 🟢 Baixo | Julio (cenário atual) |
| 12 | Categorização de contas errada (tipo_instituicao mistura conceitos: confessional, Marista, Rede S, KA) | 🟠 Alto | Oscar (refinamento 16/Mar) |
| 13 | Mantenedora: campo reaproveitado para parceiro comercial + outras funções (3 usos no mesmo campo) | 🟠 Alto | Oscar (refinamento 16/Mar) |
| 14 | Linha de negócio desalinhada entre ISA, TOTVS e CRM (3 taxonomias diferentes) | 🟠 Alto | Oscar (refinamento 16/Mar) |
| 15 | Livreiros não cadastrados no CRM (sistema separado "Pede Livreiro") | 🟡 Médio | Oscar (refinamento 16/Mar) |
| 16 | Escolas cadastradas com CNPJ de livraria (workaround para faturamento) | 🟡 Médio | Oscar (refinamento 16/Mar) |
| 17 | Variação de produto inexistente (ex: Ponto e Ponto 4 ou 8 correções, Trilhas maiúscula/minúscula) | 🟡 Médio | Oscar (refinamento 16/Mar) |
| 18 | Tipos de compromisso poluídos (WhatsApp, home office, revisão veículo — CRM usado como agenda pessoal) | 🟢 Baixo | Oscar (refinamento 16/Mar) |
| 19 | 101.000 contas de escolas no CRM, higienização necessária com recorte 2025-2026 | 🟠 Alto | Oscar/Julio (refinamento 16/Mar) |
| 20 | Oportunidades desvinculadas de propostas — precisam criação massiva + vínculo para relatórios 2025-2026 | 🟠 Alto | Oscar (refinamento 16/Mar) |

---

## 6. INTEGRAÇÕES

| Sistema | Tipo | Padrão | Status | Issues |
|---------|------|--------|--------|--------|
| TOTVS/Datasul | ERP | ETLs bidirecionais | Ativo | Cadastro duplo, CNPJ desalinhado |
| ISA | Cadastro Produto | ISA↔TOTVS↔CRM | Ativo | A redesenhar completamente. ISA "família de produto" ≠ CRM "linha de negócio" ≠ TOTVS "família comercial" (3 taxonomias) |
| Adobe Sign | Assinatura | Plugin CRM | Ativo | Migrando de tecnologia |
| Data Lake | Analytics/BI | ETLs + Events | Ativo | - |
| Área do Cliente | Portal Escolas | Canvas App (squad separada) | Ativo | Tabelas compartilhadas |
| Vulcano | Aprovação | Manual (PDF) | Ativo | **SERÁ ELIMINADO** |
| Pede Livreiro | Pedidos Livreiro | TOTVS direto | Ativo | **SERÁ ELIMINADO** → migrar para CRM + Lumisfera B2B |
| Lumisfera | E-commerce (Adobe Commerce) | 4 lojas | Ativo | Aberto, Vende Escola, B2B Funcionário, B2B Livreiros |

---

## 7. EQUIPES E GOVERNANÇA

### Time FTD CRM
| Pessoa | Papel |
|--------|-------|
| Julio Cesar | Tech Lead CRM (sustentação + melhorias) |
| Fernando | Dev (Power Pages, Power Automate, Azure Functions) |
| Kevellin | UX/Dev (Power Pages, protótipos) |
| Oscar de Rooij | PO/Business Analyst (especificações) |
| Eduardo | Functional Consultant (demo, treinamento) |

### Time Avanade
| Pessoa | Papel |
|--------|-------|
| Danilo Macedo | Tech Lead (conhece FTD desde 2022) |
| João Carlos | Arquiteto |
| Camila Moraes | Analyst/PM |
| Tercio Eduardo Freire Lins | Analyst/Consultant |
| Marcio Jovanello | Manager |

### Stakeholders Mencionados (Refinamento 16/Mar)
- **Marcel** (FTD): confirmou modelo de segurança (consultores veem só suas contas)
- **Mônica** (FTD Business): regras de alçada de aprovação (em revisão)
- **Júlio/Fernando** (FTD Tech): reunião de brainstorm TOTVS para definir tabelas-core
- **Consultoria externa de pricing**: resultado esperado final de março/2026

---

## 8. ROADMAP

| Fase | Deadline | Responsável | Escopo |
|------|----------|-------------|--------|
| MVP Simulador | 31/mar/2026 | FTD | Adição individual de produtos (Power Pages) |
| Fluxo de Aprovação | Próxima entrega | Avanade + FTD | Motor de aprovação (motor + sequência, independente de regras de alçada, Kevellin escrevendo stories) |
| Onda 1 pós-MVP | ~ago/2026 | **Avanade** | Lote, copiar proposta, benefícios, faxina dados, migração Pede Livreiro |
| Higienização + Categorização | Sem deadline definido | FTD + Avanade | 101K contas, recorte 2025-2026, tipo_instituição, CNPJ, deduplicação |
| Oportunidades 2025-2026 | TBD | Avanade | Algoritmo criação massiva + vínculo a propostas existentes |
| Pós-Venda | TBD | Avanade | Comissionamento, inteligência comercial |

> **NOTA**: Todo entregável deve vir acompanhado de relatório/BI de acompanhamento (premissa confirmada 16/Mar)

---

## 9. ESTRUTURA DE CADASTRO DE PRODUTO (Detalhamento 16/Mar)

### Problema Central
O consultor ao buscar produtos no CRM recebe **1.283 registros** quando deveria ver **~15** por linha de negócio. Causa raiz: faltam metadados para filtrar.

### Metadados Necessários (Não Existem Hoje)
- Produto **customizado** vs **prateleira** (flag)
- Linha de negócio (normalizada entre ISA/TOTVS/CRM)
- Série / segmento (infantil, fundamental, médio)
- Componente (livro do aluno, livro do professor, digital)
- Ano de vigência (safra)

### Variações de Produto (Não Modeladas)
- **Ponto e Ponto**: 4 correções ou 8 correções (créditos diferentes, mesmo produto base)
- **Trilhas**: versão maiúscula (infantil I) e minúscula (infantil II) — mesma coleção, público diferente
- **Customizado vs Prateleira**: escola pode pedir coleção montada sob medida ou padrão

### Desalinhamento Taxonômico (3 Sistemas)
| Conceito | ISA | TOTVS | CRM |
|----------|-----|-------|-----|
| Agrupamento de produto | Família de Produto | Família Comercial | Linha de Negócio |
| Nível de detalhe | Alto (1.283+ SKUs) | Médio (famílias comerciais) | Baixo (~9 linhas) |
| Quem mantém | Time ISA | Time TOTVS | FTD CRM |

> **Ação pendente**: Reunião de brainstorm com Júlio, Fernando e Mônica para alinhar estrutura e definir o que pode ser alterado nas tabelas-core do TOTVS (restrição: TOTVS não permite novos campos em tabelas-core facilmente).

---

## 10. TABELAS DE PREÇO (Detalhamento 16/Mar)

### Situação Atual
- **12+ tabelas de preço** existentes no CRM
- Na prática, o **preço é único** para todos os segmentos — as tabelas são workaround para **visualização** (ex: separar escola privada de pública)
- Consultoria externa de pricing contratada, resultado esperado **final de março/2026**

### Modelo-Alvo (Consenso Parcial)
- **Máximo 5 tabelas**: escola privado, livreiro privado, escola pública, e-commerce aberto, customizados
- **Ideal**: 1 tabela com coluna de flags (tipo_segmento + canal) para filtrar via views
- Necessário esperar resultado da consultoria de pricing para definir se haverá diferenciação real de preço

---

## 11. CATEGORIZAÇÃO DE CONTAS (Em Revisão - 16/Mar)

### Campo Problema: `tipo_de_instituicao`
Mistura 3 conceitos diferentes no mesmo nível:
- **Tipo organizacional**: confessional, laica, particular, pública
- **Rede/Marca**: Marista, Rede S, Eadverso
- **Classificação comercial**: KA (Key Account), KIA

### Modelo-Alvo (Em Discussão)
Hierarquia de 3 níveis:
1. **Segmento**: Privado / Público
2. **Tipo de Instituição**: Confessional, Laica, Rede (com sub-classificação)
3. **Classificação Comercial**: KA, Regional, etc.

### Campo `Mantenedora` (Reaproveitado)
Atualmente tem **3 usos simultâneos**:
1. Mantenedora real (grupo que mantém a escola)
2. Parceiro comercial (livreiro vinculado)
3. Outro uso legacy (não documentado)

> **Ação**: Oscar está revisando com equipe comercial. Sem deadline definido.

---

## 12. HIGIENIZAÇÃO DE CONTAS (Planejamento 16/Mar)

- **Volume**: ~101.000 contas de escola no CRM
- **Abordagem**: recorte de **2 anos** (2025-2026) para primeiro diagnóstico
- **Issues conhecidos**:
  - Escolas com CNPJ de livraria (workaround antigo de faturamento)
  - Contas sem atividade recente
  - Tipos de compromisso poluídos (WhatsApp, home office, revisão de veículo)
- **Premissa**: diagnóstico quantitativo com relatório antes de qualquer ação de limpeza

---

## 13. MODELO DE SEGURANÇA (Confirmado 16/Mar)

- **Consultores** veem **apenas suas próprias contas** (confirmado por Marcel)
- **Coordenadores/Gerentes** veem contas da **filial inteira**
- **Filtro**: por business unit (filial)
- **Carteirização**: conta atribuída a consultor implica vínculo exclusivo de visibilidade
- **Parceiro comercial/Livreiro**: carteirização TBD (depende da migração para CRM)

---

## 14. FLUXO DE APROVAÇÃO (Próxima Entrega Prioritária - 16/Mar)

- **Especificação pronta** (Oscar) — pode ser usada como insumo para stories
- **Kevellin escrevendo stories** com base na spec
- **Arquitetura**: Motor de fluxo (sequência de etapas + regras de transição) é **independente** das regras de alçada
- **Regras de alçada**: ainda em revisão pela área comercial (Mônica). Motor pode ser entregue antes.
- **Etapa 6 (Análise e Aprovação)**: depende de campos organizados na proposta (big numbers). Campos vermelhos = duplicados da proposta, serão removidos.
- **Refatoração**: eliminação de campos duplicados Oportunidade ↔ Proposta em andamento

---

## 15. OPORTUNIDADES E RELATÓRIOS (16/Mar)

- **Problema**: oportunidades não estão vinculadas a propostas (dados de 2024 e anteriores)
- **Necessidade**: algoritmo para **criação massiva** de oportunidades 2025/2026 vinculadas a propostas existentes
- **Motivação**: relatórios/BI dependem do vínculo oportunidade-proposta para funcionar
- **Campos vermelhos** na oportunidade = serão **removidos** (duplicatas da proposta)

---

## 16. 🔎 GAPS DE INFORMAÇÃO (A Investigar)

### Gaps Técnicos
- [ ] Lista exata de plugins existentes (entidades/eventos)
- [ ] Número e lista de PCF Controls
- [ ] Lista completa de Power Automate flows
- [ ] Lista de entidades customizadas (ftd_*)
- [ ] Detalhes do assessment de código legado
- [ ] Plano de ação para limite de armazenamento Dataverse

### Gaps de Negócio
- [ ] Especificação do Oscar (70 páginas) - acessar documento completo
- [x] ~~Modelo de segurança detalhado~~ → CONFIRMADO: consultores = só suas contas, coordenadores = filial (Marcel, 16/Mar)
- [ ] Regras exatas de alçada de aprovação (em revisão pela comercial — Mônica)
- [ ] Resultado da consultoria de pricing (esperado final mar/2026)
- [ ] Definição final de categorização de contas (tipo_instituição, hierarquia 3 níveis)
- [ ] Estrutura normalizada de produto (reunião brainstorm TOTVS pendente)
- [ ] Carteirização de livreiros no CRM (depende da migração Pede Livreiro)
- [ ] Requisitos LGPD específicos

### Próximos Discovery Sessions
- Brainstorm TOTVS (Júlio, Fernando, Mônica) — estrutura de produto + tabelas-core
- Spec de aprovação (Oscar) → stories via BMAD agents (teste piloto com Kevellin)
