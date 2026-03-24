# FTD Educação - Knowledge Base Completa
## Fonte: Transcrições de Onboarding (9-10-12 Março 2026)
### Participantes: Julio (Tech Lead FTD), Fernando (Dev FTD), Oscar (PO/Business Analyst FTD), Eduardo (Functional), Kevellin (UX/Dev), Danilo (Avanade), João Carlos (Avanade Architect), Camila (Avanade), Rodrigo (Avanade), Fabiana (Scrum), Marcio Jovanello (Avanade Manager)

---

## 1. AMBIENTES E INFRAESTRUTURA

### Ambientes D365
| Ambiente | Status | Pipeline |
|----------|--------|----------|
| **Dev** | ✅ Ativo | Plugin deploy MANUAL (pipeline não configurado) |
| **OAT (UAT)** | ✅ Ativo | Pipeline automático |
| **Produção** | ✅ Ativo | Pipeline + recurso Infra via SMAX |
| **QA** | 🔧 Criado, não configurado | Pendente no pipeline |
| **Release Candidate** | 🔧 Criado, não configurado | Pendente no pipeline |

### Pipeline (Azure DevOps)
- Releases dentro do projeto FTD no Azure DevOps
- Exige aprovador cadastrado no pipeline
- **Usuário de serviço**: "FTDMaxFlow" — proprietário de todos os Power Automate flows e conexões. Acesso restrito a Julio, Fernando e Thiago (sustentação).
- **Azure Key Vault**: usado para Azure Functions (tokens, URLs). Power Automates usam **variáveis de ambiente** (não Key Vault).
- **Batch projects**: atualizações em massa executadas em VM solicitada ao time de infra (não na máquina do dev).
- **Pipeline é concorrido com outras áreas** (pode haver fila)
- Subidas manuais: SOMENTE casos de urgência/hotfix com liberação específica
- Para Produção: abertura de GMUD no SMAX (Giselle monta a estrutura)
- Releases empacotam patches das solutions

### Repositório
- **Nome**: FTD Dynamics (dentro do org FTD Educação)
- **Branches**: `dev` (source of truth) e `master`
- **Padrão**: Feature branches a partir de `dev`
- **PROBLEMA**: Repo NÃO está no mesmo projeto dos PBIs → não consegue vincular commits a features
- **Migração em andamento**: novo org para unificar repo + PBIs
- Comunicação manual entre devs para evitar sobrescrever assemblies
- Code review é feito junto com o dev, acompanhamento direto

### Solutions (9 soluções segmentadas)
- Numeradas com ordem de dependência (deploy sequencial)
- Segmentação por tipo de componente:
  - Data Model
  - Client Extensions
  - Plugins
  - Site Map
  - (+ outras ~5)
- Trabalham com **patches**

---

## 2. APLICATIVOS D365

| Aplicativo | Área | Descrição |
|-----------|------|-----------|
| **PNLD** | Pública | Programa Nacional do Livro Didático |
| **Hub SAC** | CRC | Atendimentos ao cliente (Customer Service) |
| **Spartan** | Comercial | App principal de vendas |
| **Adobe Sign** | Contratos | Assinatura digital (em vias de mudar de tecnologia) |
| **Área do Cliente** | Online | Canvas App + mesmas tabelas Dataverse (squad separada) |

---

## 3. PROCESSO COMERCIAL - JORNADA COMPLETA

### 3.1 Fluxo Principal
```
Conta → Contato (Rep. Legal) → Oportunidade → Produtos da Oportunidade → Proposta → Aprovação → Contrato → Adobe Sign → TOTVS
```

### 3.2 Criação de Conta
- Consultores, anjas, coordenadores, gerentes podem criar. **Exceção**: consultores do PNLD NÃO podem criar contas (apenas administram/compartilham contas existentes).
- Campos obrigatórios: Nome (da Receita Federal), CNPJ (com validação de duplicidade), CEP (auto-preenche endereço), tipo instituição
- **Validações automáticas**:
  - CNPJ duplicado → **BLOQUEIO** (impede salvar)
  - Código MEC duplicado → **ALERTA** (permite salvar)
  - Campos obrigatórios não preenchidos → impede salvar
- Código MEC: opcional
- Proprietário = consultor (carterização manual, sem equipe de vendas)
- Sem round-robin ou balanceamento automático
- Desligamento de consultor: transferência manual de carteira
- **Alunos na conta**: se não cadastrados, quantidade padrão = 1 por produto (mínimo para venda). Impacta cálculos de proposta.

### 3.3 Contatos
- Representante legal: OBRIGATÓRIO para proposta (assina contrato)
- CPF obrigatório quando rep. legal
- Vinculado à conta

### 3.4 Oportunidade
- Campos: modelo de venda, entrega primária, taxa admin, safra, vigência, tipo contrato
- Produtos adicionados na oportunidade (individual ou lote)
- **Grade de indicação**: sugestões baseadas no histórico + cluster da escola
- Funil de vendas existe mas navegabilidade ruim
- **PROBLEMA**: Muitos campos duplicados entre Oportunidade e Proposta (dados ficam desatualizados)
- Oportunidade será "ressignificada" (simplificada)
- **Campo novo necessário**: Classificação do tipo de negociação (3 categorias que não existem hoje no CRM):
  1. Venda para escola (Mercado Privado)
  2. Venda para revendedor/livreiro
  3. Venda para família (e-commerce aberto/B2C)
- Cada categoria tem linhas de negócio permitidas diferentes (ex: livreiros só acessam algumas linhas)

### 3.5 Proposta Comercial
- Nasce com ID revisão = 0
- Revisões podem chegar a 27-28 (negociação intensa)
- Status: Rascunho → Ativo/Em Aprovação → Ativo Aprovado / Fechado Perdido / Cancelado
- Proposta REVISÁVEL até contrato assinado
- Revisão gera novo ID de revisão
- **DOR PRINCIPAL**: Consultor leva até **3 HORAS** para criar uma proposta
  - Causa 1: Não cadastra conta antes, faz tudo junto
  - Causa 2: 1 clique por produto, propostas com até 200 produtos (3 cliques cada = 600 cliques)
  - Causa 3: Tempo de carregamento do D365 (forms, grids)

### 3.6 Produtos e Linhas de Negócio
- **Linhas de negócio**: Sistema de Ensino, Didático, Bilíngue, Espanhol, Literatura, APE
- Cada linha tem regras diferentes:
  - Literatura: sem adição em lote (produtos individuais tipo Pequeno Príncipe)
  - Sistema de Ensino: permite majoração (preço acima da tabela)
  - Didático: NÃO permite majoração
  - Bilíngue: permite majoração
- **Coleções**: Trilhas (~15 materiais por série), etc.
- **Produto Grade**: customizado para ensino médio, específico por escola
- **Produto Customizado Compartilhado**: alteração leve (tirar capítulo) - qualquer escola pode usar
- **Produto Personalizado**: alteração pesada (capa, mascote) - vinculado a escola/mantenedora específica
- **Código Substituto**: versionamento entre safras (produto antigo → novo)
- **PROBLEMA CRÍTICO**: Tabela de produtos poluída (filtro que deveria retornar 15 linhas retorna 1.283)
- **12+ tabelas de preço** (usadas para resolver problema de visibilidade, não de pricing real)

### 3.7 Canais de Venda (Modelos de Venda)
- **FTD com Você (Lumisfera)**: e-commerce principal - escola compartilha link, famílias compram
- **Venda Direta**: faturamento direto para escola (invoice)
- **Frente de Loja / Balcão Escola**
- **Smart POS**: plantão de vendas presencial
- **MUDANÇA CONCEITUAL**: Era 1 proposta por canal → agora 1 proposta com múltiplos canais
- Sub-lista de produtos por canal (quais produtos disponíveis em cada canal)
- Não limita quantidade por canal, apenas define quais produtos estão disponíveis

### 3.8 Condições Comerciais
- **Desconto**: aplicado por produto (% sobre preço de tabela)
- **Majoração/Markup**: preço acima da tabela (escola define preço família)
- **Taxa de Administração**: condição comercial que afeta nível de alçada
- **Royalty**: diferença entre preço negociado e preço família (acumula, pago em maio/agosto)
- **Adiantamento**: antecipação do royalty (fluxo de caixa, não valor extra)
- **Benefícios**: cupons para filhos de professores (40% desconto), doações (90% ou 100%)
- **Patrocínios**: investimentos em dinheiro na escola (laptops, fachada, jardim)
- **Parcelamento na loja**: afeta nível de alçada

### 3.9 Alçadas de Aprovação (7 níveis - Refinamento 16/Mar)
| Nível | Aprovador | Critério (Cumulativo) |
|-------|-----------|-----------------------|
| 1 | Consultor | Auto-aprovação (dentro da política) |
| 2 | Coordenador | Descontos nível 1 |
| 3 | Gerente Filial | Descontos nível 2 / Estornos |
| 4 | Gerente Regional | Descontos nível 3 |
| 5 | Diretor Adjunto (DG) | Patrocínios / Adiantamentos altos |
| 6 | Diretor Superintendente | Casos críticos / Royalties |
| 7 | Presidente | Alçadas máximas (Grupo Marista) |

- **Motor de Aprovação**: Independente das regras de alçada (pode ser entregue antes).
- **Regras de alçada**: Sendo revistas pela Mônica (Comercial).
- **Lógica de resubmissão**: Se totalizadores de preço não mudaram, pula níveis e vai direto para quem recusou.
- **Vulcano**: Será totalmente eliminado (aprovação 100% interna D365).

### 3.10 Contratos e Adobe Sign
- Proposta aprovada → contrato gerado
- 50+ templates Word/PDF para diferentes tipos de contrato/aditivo
- Processo MANUAL: consultor faz download de vários templates, copia/cola no Word, salva PDF
- Modularização do contrato: precisa revisão com jurídico
- Adobe Sign: assinatura digital (tecnologia em vias de mudar)
- Tipos: Contrato novo, Aditivo (encerramento, renovação, etc.)

### 3.11 Matriz de Serviços
- Botão "Validar Serviços" na proposta
- Processamento: 10-15 minutos (Power Automate + análise de cada produto)
- Notificação via Teams quando completo
- Gera lista de serviços educacionais vinculados aos produtos contratados
- Sendo inteiramente revisitada

---

## 4. SIMULADOR COMERCIAL (POWER PAGES)

### Decisão Arquitetural
- **Escolhido**: Power Pages (frontend) + CRM (motor/backend)
- **Descartados**: Canvas App (2 manutenções: desktop + mobile), Custom Page (limitações mobile D365)
- **Licenciamento**: Sem custo extra - autenticação via Entra ID (usuários internos com licença Enterprise)
- **Confirmado pela Microsoft**: licenciamento OK para uso interno com Entra ID

### Arquitetura Técnica
- Single Page Application (por enquanto)
- Mínimo de consultas ao Dataverse (cache heavy)
- Cálculos em **real-time no frontend** (JavaScript/PowerFX)
- Validação no **backend** (Azure Functions) - proteção contra bypass
- Regra de débounce: ≤50 produtos → plugin síncrono; >50 → Azure Function
- Reutiliza Azure Functions existentes (com refactoring)
- Filtros dinâmicos client-side (sem FetchXML repetitivo)

### MVP (Deadline: 31 Março 2026)
- **Entrega**: Adição individual de produtos (sem lote)
- Botão no Spartan → redireciona para Power Pages
- Grid de produtos com totalizadores em real-time
- Reorganização de campos nas etapas do CRM

### 6 Etapas do Simulador
1. **Dados da Proposta**: Cliente, sócio, destinatário, tipo (contrato/aditivo), safra, vigência, alunado por série
2. **Produtos e Serviços**: Grid principal, adição individual e em lote, filtros por linha de negócio/coleção/série, totalizadores real-time
3. **Benefícios, Doações, Patrocínios, Adiantamento**: Grid por tipo, quantidades por série, impacto na receita
4. **Configuração de Vendas**: Canais de venda, parâmetros logísticos, sub-listas de produtos por canal
5. **Matriz de Serviços**: Em revisão (fora do escopo atual)
6. **Análise e Aprovação**: Big numbers, comparativo com proposta anterior, regras de alçada visuais

### Funcionalidades Futuras
- Adição em lote (principal valor - de 200 cliques para ~5)
- Copiar proposta anterior (com código substituto + reajuste IPCA)
- Inteligência comercial: upsell, cross-sell, pacotes pré-prontos
- Comissionamento do consultor visível
- Tutorial de produto / base de conhecimento
- IA: "muito longe" - maturidade de dados baixa

---

## 5. INTEGRAÇÕES

### TOTVS/Datasul
- Sincronismo de contas: ETLs bidirecionais (CRM ↔ TOTVS)
- Código ERP: sincroniza 1x/dia às 6h
- Cadastro duplo: criar em AMBOS os sistemas, esperar sincronização
- **PROBLEMA**: Não existe owner da informação (cada área cadastra onde quer)
- Faturamento, estoque, financeiro vêm do TOTVS
- **Marcio Jovanello**: "CRM deveria ser fonte de verdade para contas"
- CNPJ alignment CRM ↔ TOTVS: gap analysis necessário

### ISA (Sistema de Cadastro de Produto)
- Sincronização: ISA ↔ TOTVS ↔ CRM
- Integração a ser redesenhada junto com novo cadastro de produto

### Adobe Acrobat Sign
- Plugin "Adobe Agreement" no CRM
- Em vias de mudar de tecnologia
- Template de contrato/aditivo definido pelo tipo de proposta

### Área do Cliente (Portal)
- Equipe separada desenvolve
- Escola visualiza proposta, faz comentários, recusa com motivo
- Histórico de versões da proposta

### 5.1 Detalhamento Lumisfera (E-commerce)
Adobe Commerce com 4 frentes:
1. **Lumisfera Aberto**: Venda B2C pura (Amazon model). Não passa pelo CRM.
2. **Lumisfera Vende Escola**: Venda B2C vinculada a escola. Alinhada com Proposta CRM (modelo "FTD com Você").
3. **Lumisfera B2B – Funcionário/Representante**: Venda interna.
4. **Lumisfera B2B – Livreiros**: NOVO. Substituirá o "Pede Livreiro". Livreiros terão Conta e Proposta no CRM.

### 5.2 Modelo de Venda: Livreiro (Parceiro)
- Migração do sistema legado para CRM.
- 3 cenários: Independente, Parceiro de 1 Escola, Parceiro de Múltiplas Escolas.
- Objetivo: Eliminar o gap de visibilidade comercial dos livreiros.

---

## 6. PROBLEMAS CONHECIDOS / DÉBITOS TÉCNICOS

### Críticos
- ⚠️ **Dataverse em nível crítico** (já passou do limite de storage)
- ⚠️ **Tabela de produtos poluída** (1.283 registros vs 15 esperados) — falta flag de Produto Prateleira vs Customizado
- ⚠️ **Desalinhamento Taxonômico**: ISA (Família SKU) ≠ TOTVS (Família Comercial) ≠ CRM (Linha Negócio)
- ⚠️ **Categorização de Contas**: campo `tipo_instituicao` mistura conceitos (Confessional, KA, Rede S)
- ⚠️ **Higienização**: 101.000 contas precisam de limpeza (recorte 2025-2026)
- ⚠️ **Oportunidades desvinculadas**: dados históricos sem vínculo com Proposta, impede BI
- ⚠️ **Contrato manual**: ~50 templates Word/PDF sem modularização
- ⚠️ **Alçadas**: Atualmente muitas propostas travam no Nível 4 por regras mal parametrizadas

### Importantes
- Security roles não equalizadas entre ambientes (roadmap de consolidação)
- Vulcano (ferramenta externa de aprovação) - será eliminado
- Cadastro de contas sem processo definido (CRM vs TOTVS race condition)
- Repositório desacoplado dos PBIs (migração em andamento)
- Dev branch = source of truth (master não usado para deploys)
- Plugin deploy manual para Dev

### Em Andamento
- Refatoração de Power Automate (especialmente fluxo de aprovação - muito extenso)
- Refatoração de JavaScript web resources (assessment prévio)
- Refatoração de plugins (parcial)
- Bug rate já muito baixo após 1.5 ano de reestruturação

---

## 7. EQUIPES E STAKEHOLDERS

### FTD
| Pessoa | Papel |
|--------|-------|
| **Julio Cesar** | Tech Lead - Squad CRM (melhorias + sustentação) |
| **Fernando** | Dev - Power Pages, Power Automate, Azure Functions |
| **Kevellin** | UX/Dev - Power Pages, protótipos |
| **Oscar de Rooij** | PO/Business Analyst - especificações, simulador comercial |
| **Eduardo** | Functional Consultant - demo CRM, treinamento |
| **Thiago Veiga** | Arquiteto de integrações (time separado) |
| **Giselle** | Infraestrutura - abertura de GMUDs |
| **Karina** | Apoio em higienização de dados |
| **Claudia** | Comercial (desde época do Danilo) |

### Avanade
| Pessoa | Papel |
|--------|-------|
| **Danilo Macedo** | Tech Lead Avanade (conhece FTD desde 2022) |
| **João Carlos Figueirôa** | Arquiteto |
| **Camila Moraes** | Analyst/PM |
| **Rodrigo Silva** | Architect |
| **Marcio Jovanello** | Manager Avanade |
| **Fabiana Bacini** |Scrum Master / PM |
| **Tercio** | Gestão de acessos |

### Cerimônias
| Dia | Reunião | Participantes |
|-----|---------|---------------|
| Segunda | Squad trabalho | Dev + UX + Oscar + Kevellin + Fabi |
| Terça | Negócio | Área de negócio |
| Quinta | Tecnologia | Arquitetos + Oscar + Medella + Jovanello |

---

## 8. ROADMAP / ONDAS DE ENTREGA

### MVP Primeira Onda (Deadline: 31 Março 2026)
- Time FTD desenvolvendo
- Entrega: Adição individual de produtos no Power Pages
- Reorganização de campos nas etapas CRM

### Onda 1 Pós-MVP (Estimativa: ~Agosto 2026)
- **Avanade assume desenvolvimento**
- Faxina: revisão canais de venda, cadastro de produto, tabela de preço
- Adicionar produto em lote
- Copiar proposta anterior
- Benefícios/Doações/Patrocínios (Etapa 3 do simulador)
- Configuração de vendas/canais (Etapa 4)
- Aprovação de proposta (eliminar Vulcano)
- Integração cadastro de produto (ISA ↔ TOTVS ↔ CRM)
- Higienização de base (CNPJ, contas, produtos)

### Pós-Venda
- Comissionamento
- Inteligência comercial (upsell, cross-sell)
- IA em propostas

---

## 9. DECISÕES TÉCNICAS RELEVANTES

| Decisão | Escolha | Motivo |
|---------|---------|--------|
| Frontend simulador | Power Pages | Responsivo, liberdade de layout, licença inclusa, single codebase |
| Auth Power Pages | Entra ID | Usuários internos, sem custo extra |
| Cálculos real-time | JavaScript frontend | Performance, UX sem delay |
| Validação backend | Azure Functions | Proteção contra bypass, >50 produtos |
| Débounce plugins | <50 → plugin sync; >50 → Azure Function | Respeitar limite 2min plugin |
| Cache strategy | Client-side cache heavy | Minimizar roundtrips ao Dataverse |
| Integração | Consumir endpoints do time Veiga | CRM não expõe APIs |

---

## 10. GLOSSÁRIO FTD

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

## 11. CONTEXTO DE MERCADO E NOTÍCIAS RECENTES (2025-2026)

### 11.1 Projeções e Volume
- **Safra 2026**: Previsão de mais de **500.000 pedidos** de materiais didáticos e literatura.
- **Transformação Digital**: Faturamento alvo de R$ 3 bilhões até 2030 (Expectativa 2025: R$ 1,5 bilhão).
- **IA e Automação**: Foco massivo em IA generativa para eficiência no atendimento ao cliente (Volta às Aulas 2026).

### 11.2 Ecossistema Digital & Edtechs
A FTD Educação atua como um ecossistema integrado que inclui:
- **iônica**: Ambiente digital principal de aprendizagem.
- **Pontue**: Inteligência Artificial para correção de redações (adquirida pela FTD).
- **Estuda.com**: Plataforma de avaliação e simulados (participação minoritária).
- **Diário Escola**: Sistema de gestão e comunicação escolar (parceiro estratégico).
- **Quantbot**: Projeto Azure/Microsoft de robótica e programação gamificada.

### 11.3 Premiações e Certificações
- **Selo Top Employer 2025**: Reconhecida pela excelência em RH e gestão de pessoas.
- **Top 50 Globais**: Reconhecida mundialmente entre as maiores editoras pela Publishers Weekly.
- **Compliance BNCC**: 100% dos materiais alinhados à Base Nacional Comum Curricular.

---
*Gerado com base em: Knowledge Base FTD, Transcrições 2026 e Dados Públicos (Microsoft Case, PublishNews).*

