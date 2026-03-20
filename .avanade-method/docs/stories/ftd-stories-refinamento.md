# FTD Educação — Backlog de Épicos, Stories e Tasks
## Origem: Refinamento de Processo (16/Mar/2026) + Especificações Aprovadas
## Gerado por: Paula (PO Avanade) — 17/Mar/2026
## Versão: 2.0 (Detalhamento de Tasks — 17/Mar/2026)

---

> **Prioridade definida em reunião**: Oscar e Danilo concordaram que **Aprovação de Propostas** é o próximo item porque a especificação já está pronta, não há dependências de cadastro/preço, e há ganho direto para stakeholders (Alisson, Quintela).

> **Convenções técnicas**: Este documento segue o padrão BCA v3 (Plugin→Service→Repository, Early Bound, TypeScript Contract/Controller). Referências:
> - Backend: `src/Backend/` — C# Plugins (`.csproj`), Services, Repositories
> - Frontend: `src/Web/` — TypeScript form scripts, ribbon scripts
> - Solutions: `src/Solutions/` — solution XML (unpack via CLI)
> - Data: `src/Data/` — EMC arquivos CSV para carga de dados de configuração
> - Testes: `src/Backend/Tests/` (MSTest+NSubstitute), `src/Web/Tests/` (Jest)
> - Fluxos: Power Automate Cloud Flows (via make.powerapps.com → Customization Master)
> - Repo: `FTD Dynamics`, branch `dev` = source of truth

---

# Épico 1: Aprovação de Propostas — Motor de Alçadas e Fluxo
> Automatizar 100% do fluxo de aprovações comerciais dentro do CRM (D365 CE), implementando motor de alçadas dinâmico que avalie o impacto da proposta e direcione para os aprovadores corretos. Eliminar completamente o Vulcano do processo de aprovação de propostas.
>
> **Fonte**: `especificacao-aprovacao-notion.md` (spec completa)
> **Prioridade**: 🔴 MUST — Próxima Sprint
> **Dependências externas**: Nenhuma (independente de cadastro/preço)
> **App D365**: Spartan (app principal vendas)
> **Entidade principal**: `ftd_proposta` (já existe)

---

## Story 1.1: Configurar Estrutura de Alçadas de Aprovação (7 Níveis)
**Como** administrador do CRM, **quero** configurar os 7 níveis de alçada de aprovação no D365 CE, **para que** o motor de aprovação saiba quem deve aprovar cada proposta conforme o nível de risco.

**Acceptance Criteria:**
- [ ] 7 níveis de alçada configurados no CRM: Consultor, Coordenador+Gerente, Dir. Adjunto, Dir. Comercial, Dir. Geral, Superintendência, Presidência
- [ ] Cada nível possui campo de cargo/role vinculado ao usuário do sistema
- [ ] Hierarquia de alçadas é cumulativa (Nível 3 = aprovação do Nível 2 + Dir. Adjunto)
- [ ] Alçadas são parametrizáveis sem necessidade de deploy de código
- [ ] Testes unitários cobrem cenários de cada nível

**Tasks:**

**T1.1.1 — Criar entidade `ftd_alcada_aprovacao`**
- **Onde**: make.powerapps.com → Customization Master → Entities → New
- **Campos**:
  - `ftd_nivel` (Whole Number, 1-7, business required) — nível da alçada
  - `ftd_cargo` (Single Line Text, 200 chars) — ex: "Diretor Adjunto"
  - `ftd_descricao` (Multiple Lines Text) — descrição livre
  - `ftd_ativo` (Two Options, default: Sim) — permite desativar sem deletar
  - `ftd_ordem_sequencia` (Whole Number) — ordem de execução no fluxo sequencial
  - `ftd_cumulativo` (Two Options, default: Sim) — indica se alçada é cumulativa (todas anteriores também aprovam)
- **Display Name**: "Alçada de Aprovação", Plural: "Alçadas de Aprovação"
- **Ownership**: Organization (não user-owned, pois é config global)
- **Após criar**: rodar CLI `UnpackSolution` para trazer XML para repo, depois `GenerateEarlyBound` para gerar classes C#/TS
- **Incluir na solution**: adicionar à solution de Vendas (ftd_sales ou equivalente numerada)

**T1.1.2 — Criar entidade `ftd_aprovador`**
- **Onde**: make.powerapps.com → Customization Master → Entities → New
- **Campos**:
  - `ftd_usuario` (Lookup → SystemUser, business required) — quem é o aprovador
  - `ftd_alcada` (Lookup → ftd_alcada_aprovacao, business required) — qual nível ele aprova
  - `ftd_estabelecimento` (Lookup → ftd_estabelecimento, nullable) — filial (obrigatório para níveis 1-3 que variam por região; null para níveis 4+ que são pessoa única nacional)
  - `ftd_ativo` (Two Options, default: Sim)
  - `ftd_substituto` (Lookup → SystemUser, nullable) — aprovador substituto (férias/ausências)
  - `ftd_data_inicio` (Date Only) — vigência
  - `ftd_data_fim` (Date Only, nullable) — fim da vigência (null = indefinido)
- **Ownership**: Organization
- **Relacionamentos**: 1:N de `ftd_alcada_aprovacao` para `ftd_aprovador`; N:1 para SystemUser
- **Após criar**: `UnpackSolution` + `GenerateEarlyBound`

**T1.1.3 — Popular dados iniciais via EMC (Entity Management Cockpit)**
- **O que é o EMC**: ferramenta BCA para import/export de dados de configuração via CSV. Substitui o Configuration Migration Tool da Microsoft. Garante que dados de config estejam idênticos em todos os ambientes (DEV, OAT, PROD).

**Fluxo completo do EMC:**
```
┌─────────────────────────────────────────────────────────────────────────┐
│                    FLUXO EMC — Carga de Dados de Config                │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  1. CRIAR CSV          2. VALIDAR              3. IMPORTAR              │
│  ┌──────────────┐     ┌──────────────┐       ┌──────────────────┐       │
│  │ Editar CSV   │───►│ Revisar dados │───►  │ CLI import DEV   │       │
│  │ no repo      │     │ via PR/review │       │ (manual)         │       │
│  └──────────────┘     └──────────────┘       └────────┬─────────┘       │
│                                                        │                │
│                                                        ▼                │
│  4. VERIFICAR          5. PROMOVER             6. PROMOVER              │
│  ┌──────────────┐     ┌──────────────┐       ┌──────────────────┐       │
│  │ Testar no    │───►│ Pipeline      │───►  │ Pipeline + GMUD  │       │
│  │ DEV (manual) │     │ auto p/ OAT   │       │ para PROD        │       │
│  └──────────────┘     └──────────────┘       └──────────────────┘       │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

**Passo a passo detalhado:**

**Passo 1 — Criar o arquivo CSV no repositório:**
- **Caminho**: `src/Data/ConfigData/ftd_alcada_aprovacao.csv`
- **Formato CSV**: separador `|` (pipe), encoding UTF-8, primeira linha = header com nomes lógicos dos campos
- **Exemplo de conteúdo do CSV `ftd_alcada_aprovacao.csv`**:
  ```csv
  ftd_nome|ftd_descricao|ftd_nivel|ftd_ativo|ftd_requer_justificativa|ftd_permite_pulo
  Consultor|Auto-aprovação — consultor valida própria proposta|1|Sim|Não|Não
  Coordenador + Gerente|Aprovação regional — coordenador da filial|2|Sim|Não|Sim
  Gerente de Filial|Aprovação gerencial da filial|3|Sim|Não|Sim
  Gerente Regional (Quintela)|Aprovação regional consolidada|4|Sim|Sim|Sim
  Diretor Comercial (Alisson)|Aprovação da diretoria comercial|5|Sim|Sim|Sim
  Superintendente|Aprovação da superintendência|6|Sim|Sim|Sim
  Presidência|Aprovação final — presidente|7|Sim|Sim|Não
  ```

**Passo 2 — Criar o CSV de aprovadores:**
- **Caminho**: `src/Data/ConfigData/ftd_aprovador.csv`
- **Exemplo**:
  ```csv
  ftd_alcada_aprovacao|ftd_usuario|ftd_estabelecimento|ftd_substituto|ftd_ativo
  4|quintela@ftd.com.br|*|NULL|Sim
  5|alisson@ftd.com.br|*|quintela@ftd.com.br|Sim
  2|coordenador.sp@ftd.com.br|Filial SP|gerente.sp@ftd.com.br|Sim
  3|gerente.sp@ftd.com.br|Filial SP|quintela@ftd.com.br|Sim
  ```
- **Nota**: `*` no estabelecimento = todas as filiais (aprovadores de nível 4+)
- **Nota**: emails são resolvidos para SystemUser.Id durante o import

**Passo 3 — Importar para DEV (primeira vez, manual):**
- **Comando no terminal** (PowerShell, na raiz do repo):
  ```powershell
  .\bizapps-cli.ps1 ExportImportData -Action Import -DataType ConfigData -Environment DEV
  ```
- O CLI lê todos os CSVs em `src/Data/ConfigData/`, resolve lookups (ex: email → SystemUser.Id), e cria/atualiza registros no ambiente DEV
- **Match key**: EMC usa o campo marked como "alternate key" da entidade para decidir se cria novo registro ou atualiza existente. Configurar `ftd_nome` + `ftd_nivel` como alternate key de `ftd_alcada_aprovacao`.

**Passo 4 — Validar no ambiente DEV:**
- Abrir D365 DEV → tabela `ftd_alcada_aprovacao` → confirmar 7 registros, campos preenchidos corretamente
- Abrir `ftd_aprovador` → confirmar aprovadores vinculados às alçadas corretas

**Passo 5 — Promover para OAT (via pipeline):**
- Ao fazer merge do PR no branch `dev`, a pipeline de deploy executa automaticamente o EMC import para OAT
- O CSV no repo é a **source of truth** — qualquer mudança passa por PR + review

**Passo 6 — Promover para PROD (com GMUD):**
- Pipeline de PROD executa EMC import após aprovação de GMUD
- Os dados serão **idênticos** em todos os ambientes (garantido pelo CSV versionado)

**IMPORTANTE — Gestão de mudanças futuras:**
- Para adicionar/alterar alçada: editar CSV → PR → merge → pipeline reimporta automaticamente
- NUNCA editar dados de config diretamente no ambiente (OAT/PROD) — sempre via CSV no repo
- Para EXPORTAR dados de um ambiente (ex: ajuste feito direto no DEV que precisa ser capturado):
  ```powershell
  .\bizapps-cli.ps1 ExportImportData -Action Export -DataType ConfigData -Environment DEV
  ```
  Isso sobrescreve os CSVs locais com os dados do ambiente. Depois: commit + PR.

**T1.1.4 — Plugin de validação de exclusão**
- **Onde**: `src/Backend/Plugins/AlcadaAprovacao/`
- **Criar**: `ValidarExclusaoAlcadaPlugin.cs` herdando `Plugin<ftd_alcada_aprovacao>`
- **Registration**: `[PluginRegistration(MessageName = "Delete", Stage = PluginStage.PreValidation)]`
- **Lógica no Service** `AlcadaAprovacaoService.ValidarExclusao()`: consultar `ftd_aprovacao_historico` (criada na Story 1.3) para verificar se existem aprovações vinculadas a este nível. Se sim, lançar `InvalidPluginExecutionException` com mensagem clara.
- **Repository**: `IRepository<ftd_aprovacao_historico>` com filtro por `ftd_nivel_alcada`

**T1.1.5 — Testes unitários**
- **Onde**: `src/Backend/Tests/AlcadaAprovacao/`
- **Criar**: `ValidarExclusaoAlcadaServiceTests.cs`
- **Cenários**:
  - `ValidarExclusao_Should_ThrowException_When_AlcadaTemAprovacoes`
  - `ValidarExclusao_Should_AllowDelete_When_AlcadaSemAprovacoes`
  - `ValidarExclusao_Should_ThrowException_When_AlcadaTemAprovadoreVinculados`
- **Mock**: `Substitute.For<IRepository<ftd_aprovacao_historico>>()` retornando registros simulados

**Estimativa**: M
**Prioridade**: Must
**Dependências**: Nenhuma

---

## Story 1.2: Implementar Motor de Cálculo de Alçadas (5 Grupos de Gatilho)
**Como** sistema CRM, **quero** calcular automaticamente qual nível de alçada uma proposta exige com base nas condições comerciais negociadas, **para que** a proposta seja direcionada ao aprovador correto sem intervenção manual.

**Acceptance Criteria:**
- [ ] Motor avalia 5 grupos de critérios: Regras Percentuais, Valores Brutos, Formas de Pagamento, Desconto de Produto (existente), Contrato (existente)
- [ ] Regras Percentuais: Royalties (≤50%→N2, >50%≤70%→N3, >70%→N4), Adiantamento (<50%→N4, ≥50%→N4), Patrocínio (0→N1, ≤3%→N2, >3%→N3), Taxa Admin (<5%→N4, ≥5%≤20%→N1, >20%→N2)
- [ ] Regras Valores Brutos: Adiantamento (0→N1, ≤100K→N4, ≥100K→N5), Royalties (0→N1, <50K→N2, ≥50K→N4, ≥250K→N5, ≥500K→N6, ≥1M→N7)
- [ ] Regras Pagamento: Parcelamento (≤6→N1, ≥7→N4)
- [ ] Resultado final = nível MAIS ALTO entre todos os grupos
- [ ] Motor é executado via plugin síncrono no Update da Proposta (quando Estágio muda para "Em Aprovação")
- [ ] Resultados do cálculo são persistidos em campos da proposta para auditoria
- [ ] Regras são parametrizáveis em entidade de configuração (sem hardcode)

> **NOTA IMPORTANTE**: Existe um plugin de cálculo de alçadas no CRM (níveis 1-4). Antes de desenvolver, fazer sessão técnica com Julio/Fernando para: (a) validar se regras estão hardcoded ou em tabela, (b) avaliar se o plugin existente pode ser estendido ou precisa ser reescrito no padrão BCA. Se for reescrita, o antigo deve ser desativado, não deletado.

**Tasks:**

**Estrutura de pastas (padrão BCA: Plugin → Service → Repository → Tests):**
```
src/Backend/
├── Plugins/
│   └── Aprovacao/
│       └── CalcularAlcadaPlugin.cs           ← Plugin (entry point, sem lógica)
├── Services/
│   └── Aprovacao/
│       ├── IAlcadaCalculationService.cs       ← Interface
│       └── AlcadaCalculationService.cs        ← SERVICE (lógica de negócio)
├── Repositories/
│   └── Aprovacao/
│       └── AlcadaRegrasRepository.cs          ← Repository (acesso a dados)
└── Tests/
    └── Aprovacao/
        └── AlcadaCalculationServiceTests.cs   ← Testes do Service
```

**T1.2.1 — Criar entidade `ftd_regra_alcada` (parametrização)**
- **Onde**: make.powerapps.com → Customization Master → Entities → New
- **Campos**:
  - `ftd_grupo` (OptionSet: Percentual=1, ValorBruto=2, Pagamento=3) — grupo de regra
  - `ftd_tipo_indicador` (OptionSet: Royalties=1, Adiantamento=2, Patrocínio=3, TaxaAdmin=4, Parcelamento=5) — qual indicador
  - `ftd_operador_min` (Decimal, precision 4, nullable) — valor mínimo do range (inclusive)
  - `ftd_operador_max` (Decimal, precision 4, nullable) — valor máximo do range (exclusive). Null = sem limite superior
  - `ftd_nivel_alcada` (Whole Number, 1-7) — nível resultante se cair nesse range
  - `ftd_ativo` (Two Options, default: Sim)
  - `ftd_descricao` (Single Line Text) — legível para gestores, ex: "Royalty até 50% → Nível 2"
- **Ownership**: Organization
- **Validação visual**: criar view "Regras por Grupo" agrupada por `ftd_grupo` e `ftd_tipo_indicador`, ordenada por `ftd_operador_min`
- **Após criar**: `UnpackSolution` + `GenerateEarlyBound`

**T1.2.2 — Popular `ftd_regra_alcada` via EMC (Entity Management Cockpit)**
- **Caminho do CSV**: `src/Data/ConfigData/ftd_regra_alcada.csv`
- **Exemplo de conteúdo do CSV**:
  ```csv
  ftd_grupo|ftd_tipo_indicador|ftd_operador_min|ftd_operador_max|ftd_nivel_alcada|ftd_ativo|ftd_descricao
  Percentual|Royalties|0|50|2|Sim|Royalty ≤50% = Nível 2
  Percentual|Royalties|50.01|70|3|Sim|Royalty 50-70% = Nível 3
  Percentual|Royalties|70.01|100|4|Sim|Royalty 70-100% = Nível 4
  Percentual|Adiantamento|0|30|2|Sim|Adiantamento ≤30% = Nível 2
  Percentual|Adiantamento|30.01|50|4|Sim|Adiantamento 30-50% = Nível 4
  Percentual|Adiantamento|50.01|100|5|Sim|Adiantamento >50% = Nível 5
  Percentual|Patrocinio|0|20|2|Sim|Patrocínio ≤20% = Nível 2
  Percentual|Patrocinio|20.01|40|4|Sim|Patrocínio 20-40% = Nível 4
  Percentual|TaxaAdmin|0|10|2|Sim|Taxa Admin ≤10% = Nível 2
  ValorBruto|ReceitaBruta|0|100000|2|Sim|Receita até 100K = Nível 2
  ValorBruto|ReceitaBruta|100000.01|500000|4|Sim|Receita 100-500K = Nível 4
  ValorBruto|ReceitaBruta|500000.01|1000000|5|Sim|Receita 500K-1M = Nível 5
  ValorBruto|ReceitaBruta|1000000.01||7|Sim|Receita >1M = Nível 7 (Presidência)
  Pagamento|Parcelamento|0|6|2|Sim|Até 6 parcelas = Nível 2
  Pagamento|Parcelamento|7|12|4|Sim|7-12 parcelas = Nível 4
  Pagamento|Parcelamento|13||5|Sim|>12 parcelas = Nível 5
  ```
- **Nota**: `operador_max` vazio = sem limite superior (ex: receita >1M, qualquer valor acima)
- **Devem ser ~15-20 linhas** cobrindo todos os ranges da spec. Valores exatos a confirmar com Oscar/Kevellin.

**Fluxo de import (mesmo padrão do T1.1.3):**
```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────┐
│ Editar CSV   │───►│ PR + Review  │───►│ CLI Import   │───►│ Validar  │
│ no repo      │     │ (code review)│     │ DEV (manual) │     │ no DEV   │
└──────────────┘     └──────────────┘     └──────┬───────┘     └──────────┘
                                                  │
                      ┌──────────────┐     ┌──────▼───────┐
                      │ PROD + GMUD  │◄───│ OAT (auto    │
                      │ (pipeline)   │     │ via pipeline) │
                      └──────────────┘     └──────────────┘
```
- **Comando**: `.\bizapps-cli.ps1 ExportImportData -Action Import -DataType ConfigData -Environment DEV`
- **Alternate Key** (para upsert): `ftd_grupo` + `ftd_tipo_indicador` + `ftd_operador_min` — garante que reimport atualiza em vez de duplicar
- **Validação pós-import**: verificar que não há gaps nem overlaps nos ranges para cada grupo+indicador. Abrir view "Regras por Grupo" no D365 e conferir visualmente.

**T1.2.3 — Adicionar campos calculados na Proposta (`ftd_proposta`)**
- **Onde**: make.powerapps.com → Customization Master → Entities → ftd_proposta → Fields → New
- **Novos campos**:
  - `ftd_nivel_alcada_calculado` (Whole Number, 1-7) — resultado final (máximo de todos)
  - `ftd_nivel_percentual` (Whole Number) — resultado do grupo Percentual
  - `ftd_nivel_valor_bruto` (Whole Number) — resultado do grupo Valor Bruto
  - `ftd_nivel_pagamento` (Whole Number) — resultado do grupo Pagamento
  - `ftd_nivel_desconto_produto` (Whole Number) — resultado do motor existente de desconto
  - `ftd_nivel_contrato` (Whole Number) — resultado da regra de existência de contrato
  - `ftd_calculo_alcada_timestamp` (DateTime) — quando foi calculado pela última vez
- **Nota**: NÃO remover campos do motor antigo — depreciá-los (hidden no form, mantidos no banco)

**T1.2.4 — Criar Service `AlcadaCalculationService`**
- **Onde**: `src/Backend/Services/Aprovacao/AlcadaCalculationService.cs`
- **Interface**: `IAlcadaCalculationService` com método `CalcularNivelAlcada(ftd_proposta proposta) : AlcadaResult`
- **Struct `AlcadaResult`**: NivelFinal, NivelPercentual, NivelValorBruto, NivelPagamento, NivelDesconto, NivelContrato
- **Lógica**:
  1. Buscar todas as regras ativas via `IRepository<ftd_regra_alcada>` (filtro `ftd_ativo == true`)
  2. Para cada grupo (Percentual, ValorBruto, Pagamento): iterar regras, verificar se o valor da proposta (ex: `ftd_royalty_percentual`) cai no range `[min, max)`
  3. Retornar nível encontrado por grupo; se nenhuma regra match → Nível 1 (auto-aprovação)
  4. Nível Final = `Math.Max(nivelPercentual, nivelValorBruto, nivelPagamento, nivelDesconto, nivelContrato)`
- **Dependência DI**: `IRepository<ftd_regra_alcada>` — injetado automaticamente pela convenção BCA (Unity container)
- **Nota sobre desconto e contrato**: esses 2 motores já existem no plugin antigo. Opção 1: chamar o motor antigo via `IOrganizationService.Execute()`. Opção 2: reimplementar no novo service (preferível se o código antigo não for limpo). Decidir com Julio na sessão técnica.

**T1.2.5 — Criar Repository `AlcadaRegrasRepository`**
- **Onde**: `src/Backend/Repositories/Aprovacao/AlcadaRegrasRepository.cs`
- **Interface**: `IAlcadaRegrasRepository : IRepository<ftd_regra_alcada>`
- **Métodos personalizados**: `GetRegrasPorGrupo(int grupoId) : List<ftd_regra_alcada>` — FetchXML com filtro em `ftd_grupo` e `ftd_ativo == true`, ordenado por `ftd_operador_min`
- **Caching**: considerar usar `DefaultCache` do BCA para cachear regras (mudam raramente), TTL de 1 hora

**T1.2.6 — Criar Plugin `CalcularAlcadaPlugin`**
- **Onde**: `src/Backend/Plugins/Aprovacao/CalcularAlcadaPlugin.cs`
- **Herança**: `Plugin<ftd_proposta>`
- **Registration**: `[PluginRegistration(MessageName = "Update", Stage = PluginStage.PreOperation, FilteringAttributes = new[] { "ftd_estagio" })]`
- **Lógica do plugin**: verificar com `AttributeChangedService<ftd_proposta>.HasChangedTo("ftd_estagio", EstagioEnum.EmAprovacao)`. Se sim, chamar `IAlcadaCalculationService.CalcularNivelAlcada(target)` e setar os campos de resultado no Target.
- **Early Bound**: usar entidade fortemente tipada `ftd_proposta` gerada pelo CLI

**T1.2.7 — Testes unitários**
- **Onde**: `src/Backend/Tests/Aprovacao/AlcadaCalculationServiceTests.cs`
- **Cenários**:
  - `CalcularNivel_Should_ReturnNivel2_When_RoyaltyIs50Percent` (borda exata ≤50%)
  - `CalcularNivel_Should_ReturnNivel4_When_RoyaltyIs70Percent` (borda exata >70%)
  - `CalcularNivel_Should_ReturnNivel5_When_AdiantamentoAbove100K`
  - `CalcularNivel_Should_ReturnNivel1_When_AllZero` (sem valores → auto-aprovação)
  - `CalcularNivel_Should_ReturnMaxLevel_When_MultipleGroupsDiffer` (grupo1=N2, grupo2=N5 → N5)
  - `CalcularNivel_Should_ReturnNivel4_When_ParcelamentoIs7`
  - `CalcularNivel_Should_ReturnNivel7_When_RoyaltyAbove1M`
- **Mock**: `Substitute.For<IAlcadaRegrasRepository>()` com dados de regras montados inline

**Estimativa**: L
**Prioridade**: Must
**Dependências**: Story 1.1

---

## Story 1.3: Implementar Fluxo de Aprovação Sequencial com Avanço Automático
**Como** sistema CRM, **quero** orquestrar o fluxo de aprovação sequencial passando por cada alçada necessária, **para que** a proposta percorra automaticamente todos os aprovadores na ordem correta, da menor para a maior alçada.

**Acceptance Criteria:**
- [ ] Fluxo inicia quando cliente aprova a proposta (Estágio → "Em Aprovação")
- [ ] CRM identifica primeiro aprovador com base no nível calculado e filial do consultor
- [ ] Após aprovação de uma alçada, sistema avança automaticamente para a próxima
- [ ] Se aprovador é o último, Estágio da Proposta → "Aprovada não assinada"
- [ ] Registra cada aprovação com `usuário`, `timestamp` e `nível`
- [ ] Cria registros de histórico de aprovação na entidade `ftd_aprovacao_historico`
- [ ] Status e Estágio da Oportunidade acompanham o da Proposta

> **NOTA**: O Power Automate Flow atual orquestra aprovações até nível 4. Avaliar se é melhor estender o Flow existente ou substituí-lo por plugin+service (recomendado se complexidade de pulo e herança for alta). A recomendação é manter a orquestração de notificações em Flow e a lógica de estado em plugins.

**Tasks:**

**Estrutura de pastas (padrão BCA: Plugin → Service → Repository → Tests):**
```
src/Backend/
├── Plugins/
│   └── Aprovacao/
│       ├── IniciarFluxoAprovacaoPlugin.cs     ← Plugin (dispara fluxo no PostOp)
│       └── RegistrarAprovacaoPlugin.cs        ← Plugin (avança alçada no PostOp)
├── Services/
│   └── Aprovacao/
│       ├── IFluxoAprovacaoService.cs           ← Interface
│       └── FluxoAprovacaoService.cs            ← SERVICE (orquestração do fluxo)
└── Tests/
    └── Aprovacao/
        └── FluxoAprovacaoServiceTests.cs       ← Testes do Service
```

**T1.3.1 — Criar entidade `ftd_aprovacao_historico`**
- **Onde**: make.powerapps.com → Customization Master → Entities → New
- **Campos**:
  - `ftd_proposta` (Lookup → ftd_proposta, business required) — proposta sendo aprovada
  - `ftd_aprovador` (Lookup → SystemUser, nullable) — quem aprovou/recusou (null enquanto pendente)
  - `ftd_aprovador_esperado` (Lookup → SystemUser, business required) — quem deveria aprovar nesta alçada
  - `ftd_nivel_alcada` (Whole Number, 1-7) — nível desta aprovação
  - `ftd_decisao` (OptionSet: Pendente=1, Aprovado=2, Recusado=3, Herdada=4) — estado da decisão. "Herdada" indica aprovação capturada de versão anterior
  - `ftd_timestamp_decisao` (DateTime, nullable) — quando decidiu
  - `ftd_justificativa` (Multiple Lines Text, nullable) — obrigatória para recusa
  - `ftd_versao_proposta` (Single Line Text) — ex: "1.3" — para rastreabilidade
  - `ftd_proposta_origem_heranca` (Lookup → ftd_proposta, nullable) — referência à proposta original quando decisão = Herdada
  - `ftd_ordem` (Whole Number) — posição na sequência (1=primeiro aprovador, 2=segundo, etc.)
- **Ownership**: Organization (dados de auditoria, não privados)
- **Nota**: habilitar Audit Log nesta entidade (Settings → Auditing → Entity → Enable)
- **Após criar**: `UnpackSolution` + `GenerateEarlyBound`

**T1.3.2 — Criar Service `FluxoAprovacaoService`**
- **Onde**: `src/Backend/Services/Aprovacao/FluxoAprovacaoService.cs`
- **Interface**: `IFluxoAprovacaoService`
- **Métodos**:
  - `IniciarFluxo(ftd_proposta proposta)`:
    1. Ler `ftd_nivel_alcada_calculado` da proposta
    2. Buscar todos aprovadores necessários (nível 1 até nível calculado) via `IRepository<ftd_aprovador>`, filtrando por `ftd_estabelecimento` = filial da conta da proposta (para N1-3) ou sem filtro de filial (para N4+)
    3. Criar registros `ftd_aprovacao_historico` com `ftd_decisao = Pendente` para cada aprovador, na ordem da alçada
    4. Marcar o nível 1 como "ativo" (consultor auto-aprova → registrar decisão=Aprovado automaticamente)
    5. Retornar próximo aprovador pendente
  - `AvancarAlcada(ftd_aprovacao_historico aprovacaoAtual)`:
    1. Gravar decisão e timestamp no registro atual
    2. Buscar próximo registro com `ftd_decisao = Pendente` e `ftd_ordem` = ordem+1
    3. Se existir: disparar evento para notificação (criar registro com Pendente)
    4. Se não existir: todos aprovaram → retornar flag "fluxo concluído"
  - `VerificarUltimaAlcada(Guid propostaId) : bool` — verifica se todos registros estão Aprovado/Herdada
  - `ObterAprovadorAtual(Guid propostaId) : ftd_aprovacao_historico` — retorna registro Pendente com menor ordem
- **Repository**: `IRepository<ftd_aprovacao_historico>`, `IRepository<ftd_aprovador>`
- **DI**: registrar automaticamente por convenção `IFluxoAprovacaoService` → `FluxoAprovacaoService`

**T1.3.3 — Criar Plugin `IniciarFluxoAprovacaoPlugin`**
- **Onde**: `src/Backend/Plugins/Aprovacao/IniciarFluxoAprovacaoPlugin.cs`
- **Herança**: `Plugin<ftd_proposta>`
- **Registration**: `[PluginRegistration(MessageName = "Update", Stage = PluginStage.PostOperation, FilteringAttributes = new[] { "ftd_estagio" })]`
- **Lógica**: verificar `AttributeChangedService.HasChangedTo("ftd_estagio", EstagioEnum.EmAprovacao)`. Se sim, chamar `IFluxoAprovacaoService.IniciarFluxo(target)`. Nível 1 (consultor) é auto-aprovado → já avança para N2.
- **Cuidado**: este plugin dispara APÓS o `CalcularAlcadaPlugin` (PreOperation) — os campos de nível já estarão preenchidos

**T1.3.4 — Criar Plugin `RegistrarAprovacaoPlugin`**
- **Onde**: `src/Backend/Plugins/Aprovacao/RegistrarAprovacaoPlugin.cs`
- **Herança**: `Plugin<ftd_aprovacao_historico>`
- **Registration**: `[PluginRegistration(MessageName = "Update", Stage = PluginStage.PostOperation, FilteringAttributes = new[] { "ftd_decisao" })]`
- **Lógica**:
  1. Verificar `AttributeChangedService.HasChangedTo("ftd_decisao", DecisaoEnum.Aprovado)`
  2. Chamar `IFluxoAprovacaoService.AvancarAlcada(target)`
  3. Se retorno indicar "fluxo concluído": atualizar `ftd_proposta.ftd_estagio = "Aprovada não assinada"` via `IRepository<ftd_proposta>.Update()`
  4. Se decisão = Recusado: atualizar proposta para "Recusada" (tratado na Story 1.5)

**T1.3.5 — Sincronizar Estágio da Oportunidade**
- **Onde**: no mesmo `RegistrarAprovacaoPlugin` ou em plugin separado `SincronizarOportunidadePlugin : Plugin<ftd_proposta>`
- **Registration**: `[PluginRegistration(MessageName = "Update", Stage = PluginStage.PostOperation, FilteringAttributes = new[] { "ftd_estagio" })]`
- **Lógica**: ao mudar estágio da proposta, buscar Opportunity vinculada (lookup `ftd_oportunidade` na proposta) e atualizar o campo `stepname` do BPF ou campo customizado `ftd_estagio_oportunidade` correspondente. **Oportunidade NUNCA regride** — só avança.
- **Cuidado**: verificar se oportunidade existe (propostas antigas podem não ter)

**T1.3.6 — Testes unitários**
- **Onde**: `src/Backend/Tests/Aprovacao/FluxoAprovacaoServiceTests.cs`
- **Cenários**:
  - `IniciarFluxo_Should_CreateHistoricoRecords_When_NivelIs4` (deve criar 4 registros: N1-N4)
  - `IniciarFluxo_Should_AutoApproveNivel1_When_Started` (N1 = consultor, auto-aprovação)
  - `AvancarAlcada_Should_ReturnProximoAprovador_When_NotLastLevel`
  - `AvancarAlcada_Should_ReturnFluxoConcluido_When_LastLevelApproved`
  - `AvancarAlcada_Should_UpdatePropostaEstagio_When_FluxoConcluido`
  - `ObterAprovadorAtual_Should_ReturnSubstituto_When_AprovadorIndisponivel` (usa ftd_substituto)
- **Mock**: repositórios com dados inline

**Estimativa**: L
**Prioridade**: Must
**Dependências**: Story 1.1, Story 1.2

---

## Story 1.4: Implementar Notificações de Aprovação (Email + Teams)
**Como** aprovador/consultor, **quero** receber notificações por Email e Teams sobre propostas pendentes de aprovação, aprovações parciais ou recusas, **para que** eu possa agir rapidamente sem precisar monitorar o CRM.

**Acceptance Criteria:**
- [ ] Notificação de "Proposta para Aprovação" enviada ao aprovador da alçada em vigor (Email + Teams)
- [ ] Link na notificação leva direto para Etapa 6 da proposta no Simulador
- [ ] Notificação de "Aprovação Parcial" enviada ao Consultor quando uma alçada aprova (com nome do aprovador e próximo aprovador)
- [ ] Notificação de "Proposta Aprovada" enviada ao Consultor, Coordenador e Gerente de Filial quando última alçada aprova
- [ ] Notificação de "Proposta Recusada" enviada ao Consultor com nome de quem recusou e razão da recusa
- [ ] Notificação de "Oportunidade Perdida" enviada ao Coordenador e Gerente com razão da perda
- [ ] Notificação de "Proposta Assinada" com conteúdo celebrativo
- [ ] Cada tipo de notificação tem template configurável

> **Decisão arquitetural**: Notificações por **email** via `EmailService` do BCA (backend, templates D365). Notificações por **Teams** via **Power Automate Cloud Flow** (Adaptive Cards com botões de ação). A lógica de "quem notificar" fica no plugin; o disparo de Teams fica no Flow.

**Tasks:**

**Estrutura de pastas (padrão BCA: Service + Power Automate):**
```
src/Backend/
├── Services/
│   └── Notificacao/
│       ├── INotificacaoAprovacaoService.cs    ← Interface
│       └── NotificacaoAprovacaoService.cs     ← SERVICE (lógica de notificação)
└── Tests/
    └── Notificacao/
        └── NotificacaoAprovacaoServiceTests.cs ← Testes do Service
Power Automate/
├── Flow-NotificarAprovadorTeams.zip           ← Cloud Flow (Adaptive Card)
└── Flow-NotificarConsultorTeams.zip           ← Cloud Flow (status updates)
```

**T1.4.1 — Criar templates de email no D365 CE**
- **Onde**: make.powerapps.com → Customization Master → Settings → Templates → Email Templates
- **Criar 7 templates** (tipo: Global, disponível para entidade ftd_aprovacao_historico):
  1. `FTD - Pendente Aprovação` — assunto: "[FTD] Proposta {proposta.ftd_name} aguarda sua aprovação". Corpo: big numbers resumidos + link para Etapa 6 do Simulador
  2. `FTD - Aprovação Parcial` — assunto: "[FTD] Proposta {proposta.ftd_name} aprovada por {aprovador.fullname}". Corpo: nível aprovado, quem é o próximo
  3. `FTD - Proposta Aprovada` — assunto: "[FTD] ✅ Proposta {proposta.ftd_name} totalmente aprovada"
  4. `FTD - Proposta Recusada` — assunto: "[FTD] ❌ Proposta {proposta.ftd_name} recusada". Corpo: nome recusador + razão da recusa
  5. `FTD - Oportunidade Perdida` — assunto: "[FTD] Oportunidade perdida - {conta.name}"
  6. `FTD - Assinatura Recusada` — assunto: "[FTD] ⚠️ Assinatura recusada - {proposta.ftd_name}"
  7. `FTD - Proposta Assinada` — assunto: "[FTD] 🎉 Proposta {proposta.ftd_name} assinada!"
- **Usar `DynamicExpressionService` do BCA** para resolver campos multi-nível (ex: proposta → conta → nome) nos templates
- **Incluir na solution** de Vendas

**T1.4.2 — Criar Service `NotificacaoAprovacaoService`**
- **Onde**: `src/Backend/Services/Aprovacao/NotificacaoAprovacaoService.cs`
- **Interface**: `INotificacaoAprovacaoService`
- **Métodos**: `NotificarPendente(ftd_aprovacao_historico)`, `NotificarAprovacao(ftd_aprovacao_historico)`, `NotificarRecusa(ftd_aprovacao_historico)`, `NotificarConcluido(ftd_proposta)`, `NotificarAssinatura(ftd_proposta)`
- **Implementação**: usar `EmailService` do BCA framework para envio de emails utilizando templates. O `EmailService` já resolve placeholders, attachments e tracking.
- **Destinatários**: buscar via `IRepository<ftd_aprovador>` (aprovador da alçada) e lookup do consultor/coordenador/gerente na conta

**T1.4.3 — Configurar deep links para Simulador (Power Pages)**
- **Onde**: no template de email e na configuração do Flow
- **Formato URL**: `https://{power-pages-domain}/simulador/proposta/{propostaguid}/etapa6` — confirmar formato exato com Fernando (Power Pages dev)
- **Segurança**: link deve funcionar com autenticação Entra ID do aprovador — verificar se Power Pages aceita deep link com parâmetro de proposta

**T1.4.4 — Criar Power Automate Flow "Notificar Aprovação Pendente via Teams"**
- **Onde**: make.powerapps.com → Customization Master → Cloud Flows → New
- **Trigger**: When a row is added or modified (Dataverse) → entidade `ftd_aprovacao_historico`, filtro: `ftd_decisao = Pendente` e `ftd_ordem > 1` (N1 é auto-aprovado, não notifica)
- **Ação 1**: Obter linha da proposta vinculada (para pegar big numbers)
- **Ação 2**: Obter usuário aprovador esperado (`ftd_aprovador_esperado`)
- **Ação 3**: Post Adaptive Card to Teams channel/chat → `@mention` do aprovador, resumo de big numbers, botão "Abrir no Simulador" apontando para deep link da Etapa 6
- **Adaptive Card**: usar layout com FactSet mostrando Receita Bruta, Adiantamento, Royalty, Nível de Alçada, Nome do Cliente
- **Error handling**: se Teams falhar, logar erro mas não bloquear fluxo (email é o canal principal)

**T1.4.5 — Criar Power Automate Flow "Notificar Resultado Aprovação via Teams"**
- **Onde**: make.powerapps.com → Cloud Flows → New
- **Trigger**: When a row is modified (Dataverse) → `ftd_aprovacao_historico`, filtro: `ftd_decisao in (Aprovado, Recusado)`
- **Ação**: Post mensagem no chat do Teams para o consultor dono da proposta
  - Se Aprovado: mensagem positiva com nome do aprovador e indicação de próximo passo
  - Se Recusado: mensagem com razão da recusa e orientação para revisar
- **Incluir na solution** de Vendas

**T1.4.6 — Chamar NotificacaoService a partir dos Plugins**
- **Onde**: `RegistrarAprovacaoPlugin` (Story 1.3) e `RecusarPropostaPlugin` (Story 1.5)
- **Como**: no PostOperation, após sucesso da operação, chamar `INotificacaoAprovacaoService.NotificarAprovacao()` ou `.NotificarRecusa()`
- **Nota**: o email via `EmailService` é síncrono dentro do plugin — ok para email. O Teams é via Flow assíncrono (trigger na mudança do registro)

**T1.4.7 — Testes**
- **Backend**: `NotificacaoAprovacaoServiceTests.cs` — mock `EmailService`, verificar que `Send()` é chamado com template correto e destinatário correto
- **E2E (manual)**: criar proposta em DEV, submeter aprovação, verificar email recebido e Adaptive Card no Teams

**Estimativa**: L
**Prioridade**: Must
**Dependências**: Story 1.3

---

## Story 1.5: Implementar Tratativa de Recusa de Proposta
**Como** consultor, **quero** visualizar a razão da recusa, criar nova versão da proposta a partir da recusada e gerenciar o reenvio para aprovação, **para que** eu possa ajustar a proposta conforme o feedback do aprovador sem perder o histórico.

**Acceptance Criteria:**
- [ ] Aprovador que recusa deve preencher campo obrigatório "Razão da Recusa" (texto)
- [ ] Recusa registra `usuário`, `timestamp` e `justificativa` em `ftd_aprovacao_historico`
- [ ] Estágio da Proposta muda para "Recusada"
- [ ] Consultor pode criar nova versão a partir da recusada (copia todos dados, incrementa ID_Revisão, mantém ID_Proposta)
- [ ] Proposta recusada muda de Status "Aberta" → "Histórico"
- [ ] Nova proposta criada com Status "Aberta", Estágio "Rascunho"
- [ ] Razão da recusa é exibida na nova proposta para referência do consultor

> **NOTA**: Já existe funcionalidade de "duplicar proposta" no CRM que funciona apenas quando recusada. Verificar com Julio se o código existente pode ser reaproveitado/estendido. Se reaproveitável, adaptar; se não, criar novo no padrão BCA.

**Tasks:**

**Estrutura de pastas (padrão BCA: Plugin → Service → Custom API → Tests):**
```
src/Backend/
├── Plugins/
│   └── Aprovacao/
│       └── ProcessarRecusaPlugin.cs                ← Plugin (processa decisão de recusa)
│   └── Proposta/
│       └── CriarNovaVersaoPropostaApi.cs            ← Custom API (chamada pelo frontend)
├── Services/
│   └── Proposta/
│       ├── ICopiarPropostaService.cs                ← Interface
│       └── CopiarPropostaService.cs                 ← SERVICE (cópia completa da proposta)
└── Tests/
    └── Proposta/
        └── CopiarPropostaServiceTests.cs            ← Testes do Service
```

**T1.5.1 — Adicionar campos de recusa na Proposta**
- **Onde**: make.powerapps.com → Customization Master → ftd_proposta → Fields → New
- **Campos**:
  - `ftd_razao_recusa` (Multiple Lines Text, 4000 chars) — preenchido auto pelo plugin quando aprovador recusa
  - `ftd_recusado_por` (Lookup → SystemUser) — quem recusou
  - `ftd_data_recusa` (DateTime) — quando foi recusada
  - `ftd_razao_recusa_visivel` (Calculated Field ou simples, Single Line 200) — resumo exível em views/cards (primeiros 200 chars do texto completo)
- **Formulário**: adicionar seção "Última Recusa" no form da Proposta, visível somente quando Estágio = Recusada. Usar Display Rule para mostrar/esconder.
- **Após`: `UnpackSolution` + `GenerateEarlyBound`

**T1.5.2 — Criar plugin `ProcessarRecusaPlugin`**
- **Onde**: `src/Backend/Plugins/Aprovacao/ProcessarRecusaPlugin.cs`
- **Herança**: `Plugin<ftd_aprovacao_historico>`
- **Registration**: `[PluginRegistration(MessageName = "Update", Stage = PluginStage.PostOperation, FilteringAttributes = new[] { "ftd_decisao" })]`
- **Lógica**: verificar `HasChangedTo("ftd_decisao", DecisaoEnum.Recusado)`. Se sim:
  1. Validar que `ftd_justificativa` não está vazio (se estiver, lançar `InvalidPluginExecutionException("Justificativa de recusa é obrigatória")`)
  2. Ler proposta vinculada via lookup `ftd_proposta`
  3. Atualizar proposta: `ftd_estagio = Recusada`, `ftd_razao_recusa = justificativa`, `ftd_recusado_por = aprovador`, `ftd_data_recusa = DateTime.UtcNow`
  4. Chamar `INotificacaoAprovacaoService.NotificarRecusa()`
- **Nota**: pode ser unificado com o `RegistrarAprovacaoPlugin` — decisão de design: manter separado para SRP ou unificar para menos plugins? Recomendação: manter no mesmo plugin com branching por decisão.

**T1.5.3 — Criar Service `CopiarPropostaService`**
- **Onde**: `src/Backend/Services/Proposta/CopiarPropostaService.cs`
- **Interface**: `ICopiarPropostaService`
- **Método**: `CriarNovaVersao(Guid propostaOrigemId) : Guid` (retorna ID da nova proposta)
- **Lógica**:
  1. Ler proposta origem com **todos os campos** via `IRepository<ftd_proposta>.Retrieve(id, allColumns)`
  2. Ler registros filhos: `ftd_produto_proposta` (produtos), `ftd_beneficio`, `ftd_patrocinio`, `ftd_adiantamento`, e qualquer outra entidade relacionada (verificar com Julio a lista completa de filhos)
  3. Criar nova proposta com: todos os campos copiados **exceto** ID, Status (= Aberta), Estágio (= Rascunho), `ftd_id_revisao` (= anterior + 1), `ftd_proposta_anterior` (= lookup para proposta origem), campos de recusa (vazios), campos de aprovação (vazios)
  4. Copiar todos os registros filhos vinculando à nova proposta
  5. Atualizar proposta origem: `ftd_proposta_posterior` = nova proposta, Status = Histórico
  6. Copiar `ftd_razao_recusa` para campo informativo na nova proposta (para referência do consultor)
- **CUIDADO**: esta operação pode ser pesada (muitos registros filhos) — se > 50 produtos, considerar `AsAdmin = true` para evitar timeout. Monitorar IPC limit do plugin (2 min sync).
- **Reaproveitamento**: verificar se há código existente no repo (`FTD Dynamics`) que faz duplicação da proposta — se existir, extrair a lógica para o novo Service

**T1.5.4 — Criar Custom API `CriarNovaVersaoProposta`**
- **Onde**: `src/Backend/Plugins/Proposta/CriarNovaVersaoPropostaApi.cs`
- **Herança**: `CustomApiPlugin<CriarNovaVersaoRequest, CriarNovaVersaoResponse>` (padrão BCA)
- **Request**: `PropostaOrigemId` (Guid, required)
- **Response**: `NovaPropostaId` (Guid)
- **Registro**: criar Custom API no Customization Master (Settings → Custom APIs → New) com nome `ftd_CriarNovaVersaoProposta`, tipo Plugin
- **Frontend**: será chamada via botão na interface (ribbon button ou botão no Simulador)

**T1.5.5 — Testes unitários**
- **Onde**: `src/Backend/Tests/Proposta/CopiarPropostaServiceTests.cs`
- **Cenários**:
  - `CriarNovaVersao_Should_CopyAllFields_When_PropostaHasProducts` — verificar que campos são copiados e produtos filhos replicados
  - `CriarNovaVersao_Should_IncrementRevisao_When_OrigemIs3` → nova deve ser 4
  - `CriarNovaVersao_Should_SetPropostaAnterior_When_Created`
  - `CriarNovaVersao_Should_SetOrigemToHistorico_When_Created`
  - `CriarNovaVersao_Should_ClearApprovalFields_When_Created`
  - `CriarNovaVersao_Should_KeepRazaoRecusa_When_ForReference`
- **Mock**: repositórios de proposta e entidades filhas

**Estimativa**: L
**Prioridade**: Must
**Dependências**: Story 1.3

---

## Story 1.6: Implementar Lógica de Reenvio Inteligente (Atalho de Alçadas)
**Como** sistema CRM, **quero** comparar os Totalizadores entre a versão nova e a anterior da proposta para determinar se o fluxo deve reiniciar do zero ou ir direto para a alçada que recusou, **para que** aprovações operacionais não gerem retrabalho desnecessário.

**Acceptance Criteria:**
- [ ] Se Totalizadores mudaram (Receita Bruta, Receita após Benefícios, Total Investido, Adiantamento, Parcelas): fluxo reinicia do zero, requer aprovação do cliente
- [ ] Se Totalizadores iguais e Estágio anterior = "Recusada": proposta vai direto para alçada que recusou; aprovações anteriores são herdadas com marcação "Aprovação na proposta anterior" + timestamp original
- [ ] Se Totalizadores iguais e Estágio anterior = "Assinatura recusada": todas aprovações são herdadas; consultor + administrativo + jurídico ajustam e reenviam contrato
- [ ] Comparação é feita automaticamente no momento de submissão da nova versão
- [ ] Registros de aprovação herdada são visíveis no histórico

> **Regra-chave da reunião**: A **versão 1** de qualquer proposta ou aditivo SEMPRE passa por todas as alçadas. Pulo só é válido para v2+ após recusa.

**Tasks:**

**Estrutura de pastas (padrão BCA: Service → Tests):**
```
src/Backend/
├── Services/
│   └── Aprovacao/
│       ├── IComparadorTotalizadoresService.cs  ← Interface
│       └── ComparadorTotalizadoresService.cs   ← SERVICE (compara totalizadores)
└── Tests/
    └── Aprovacao/
        └── ComparadorTotalizadoresServiceTests.cs ← Testes do Service
```

**T1.6.1 — Criar Service `ComparadorTotalizadoresService`**
- **Onde**: `src/Backend/Services/Aprovacao/ComparadorTotalizadoresService.cs`
- **Interface**: `IComparadorTotalizadoresService`
- **Método**: `CompareTotalizadores(ftd_proposta atual, ftd_proposta anterior) : TotalizadorResult`
  - `TotalizadorResult` = struct com `bool Mudou` + `List<string> CamposDiferentes`
- **Campos comparados** (da spec):
  - `ftd_receita_bruta` (Currency)
  - `ftd_receita_apos_beneficios` (Currency)
  - `ftd_total_investido_escola` (Currency)
  - `ftd_adiantamento` (Currency)
  - `ftd_parcelas` (Whole Number)
- **Lógica**: comparar campo a campo entre as 2 propostas. Se QUALQUER diferença → `Mudou = true`. Usar tolerância de R$ 0,01 para comparação de Currency (evitar floating point issues).
- **Predecessora**: precisa do lookup `ftd_proposta_anterior` (da Story 1.7) estar preenchido

**T1.6.2 — Integrar comparação no `FluxoAprovacaoService.IniciarFluxo()`**
- **Onde**: `src/Backend/Services/Aprovacao/FluxoAprovacaoService.cs` — método `IniciarFluxo`
- **Antes da criação dos registros de approved_historico**, adicionar:
  1. Verificar se `ftd_proposta_anterior` != null e `ftd_id_revisao > 1` (não é versão 1)
  2. Se sim, chamar `IComparadorTotalizadoresService.CompareTotalizadores()`
  3. Se `Mudou = true`: fluxo normal (cria todos registros Pendente), mas BLOQUEAR entrada em aprovação se cliente não re-aprovou (verificar campo `ftd_aprovacao_cliente` = true)
  4. Se `Mudou = false` e proposta anterior foi "Recusada":
    - Buscar registros de `ftd_aprovacao_historico` da proposta anterior que estavam Aprovado
    - Criar registros "Herdada" para esses níveis na nova proposta (com `ftd_proposta_origem_heranca` = proposta anterior, `ftd_timestamp_decisao` = timestamp original)
    - Criar registro "Pendente" apenas para a alçada que recusou e as subsequentes
  5. Se `Mudou = false` e proposta anterior foi "Assinatura recusada":
    - Herdar TODAS aprovações; fluxo vai direto para contrato sem re-aprovação

**T1.6.3 — Plugin de bloqueio para re-aprovação do cliente**
- **Onde**: no `CalcularAlcadaPlugin` ou plugin separado
- **Lógica**: se totalizadores mudaram na v2+, o consultor precisa marcar `ftd_aprovacao_cliente = true` antes de submeter para aprovação interna. Se tentar mudar Estágio → "Em Aprovação" sem essa flag, lançar `InvalidPluginExecutionException("Totalizadores foram alterados. O cliente precisa aprovar novamente antes do fluxo de aprovação interno.")`

**T1.6.4 — Testes unitários**
- **Onde**: `src/Backend/Tests/Aprovacao/ComparadorTotalizadoresServiceTests.cs`
- **Cenários**:
  - `Compare_Should_ReturnMudou_When_ReceitaBrutaDiffers`
  - `Compare_Should_ReturnNaoMudou_When_AllFieldsEqual`
  - `Compare_Should_ReturnNaoMudou_When_DifferenceBelow1Centavo` (tolerância)
  - `IniciarFluxo_Should_HerdarAprovacoes_When_TotalizadoresIguaisERecusa` — verificar que registros Herdada foram criados e Pendente começa na alçada que recusou
  - `IniciarFluxo_Should_RestartFluxo_When_TotalizadoresDiferentes`
  - `IniciarFluxo_Should_HerdarTodas_When_AssinaturaRecusada`
  - `IniciarFluxo_Should_AlwaysFullFlux_When_VersaoIs1` (nunca pula na v1)

**Estimativa**: M
**Prioridade**: Must
**Dependências**: Story 1.5

---

## Story 1.7: Implementar Cadeia de Propostas (Versionamento e Vínculo)
**Como** consultor, **quero** que as propostas sejam encadeadas (vínculo versão anterior ↔ posterior), inclusive entre anos-safra diferentes, **para que** eu tenha rastreabilidade completa do histórico de negociação com cada cliente.

**Acceptance Criteria:**
- [ ] Toda nova proposta (exceto a primeira do cliente) possui lookup para proposta anterior
- [ ] Toda proposta substituída possui lookup para proposta posterior
- [ ] Encadeamento funciona dentro do mesmo ano-safra (revisões) e entre anos-safra diferentes
- [ ] Proposta aprovada e assinada: Status anterior → "Histórico", Status atual → "Ativa"
- [ ] Apenas 1 proposta pode ter Status "Ativa" por Oportunidade
- [ ] Funcionalidade "copiar proposta anterior" utiliza o vínculo entre anos-safra

> **Contexto da reunião**: versionamento usa notação `X.Y` onde X = ano-safra e Y = revisão. Ex: 1.0, 1.1, 1.2 (recusas dentro de 2025), 2.0 (cópia para 2026), 2.1 (ajuste). Novo aditivo sobre contrato assinado reinicia como versão 1 dessa cadeia.

**Tasks:**

**Estrutura de pastas (padrão BCA: Plugin → Tests):**
```
src/Backend/
├── Plugins/
│   └── Proposta/
│       └── CadeiaPropostaPlugin.cs            ← Plugin (auto-gera revisão e cadeia)
└── Tests/
    └── Proposta/
        └── CadeiaPropostaPluginTests.cs       ← Testes do Plugin
```

**T1.7.1 — Adicionar campos de cadeia na Proposta**
- **Onde**: make.powerapps.com → Customization Master → ftd_proposta → Fields
- **Campos**:
  - `ftd_proposta_anterior` (Lookup → ftd_proposta, nullable) — self-referencing lookup. Display Name: "Proposta Anterior"
  - `ftd_proposta_posterior` (Lookup → ftd_proposta, nullable) — self-referencing. Display Name: "Proposta Posterior"
  - `ftd_id_cadeia` (Single Line Text, 50) — identificador da cadeia de propostas (ex: "FTD-2026-ESCOLA-001"). Gerado automaticamente.
  - `ftd_id_revisao` (Whole Number, default 1) — auto-incrementa dentro da cadeia. Display Name: "Versão"
  - `ftd_tipo_proposta` (OptionSet: Contrato=1, Aditivo=2) — distingue contrato novo de aditivo (não existe hoje — requisito confirmado na reunião, será usado no simulador Etapa 1)
- **Formulário**: adicionar seção "Cadeia de Propostas" com campos visíveis e readonly (preenchidos por automação). Adicionar subgrid mostrando todas propostas da mesma cadeia.
- **Views**: criar view "Histórico de Propostas" filtrada por `ftd_id_cadeia`, ordenada por `ftd_id_revisao DESC`
- **Após**: `UnpackSolution` + `GenerateEarlyBound`

**T1.7.2 — Criar plugin `CadeiaPropostaPlugin`**
- **Onde**: `src/Backend/Plugins/Proposta/CadeiaPropostaPlugin.cs`
- **Herança**: `Plugin<ftd_proposta>`
- **Registration**: `[PluginRegistration(MessageName = "Create", Stage = PluginStage.PreOperation)]`
- **Lógica**:
  1. Se `ftd_proposta_anterior` está preenchido (proposta não é a primeira):
    - Ler proposta anterior → pegar `ftd_id_cadeia` e `ftd_id_revisao`
    - Setar na nova: `ftd_id_cadeia = anteriorCadeia`, `ftd_id_revisao = anteriorRevisao + 1`
    - Atualizar proposta anterior: `ftd_proposta_posterior = novaPropostaId` (no PostOperation)
  2. Se `ftd_proposta_anterior` está vazio (primeira proposta da cadeia):
    - Gerar `ftd_id_cadeia` automaticamente: `"FTD-{anoSafra}-{contaId.Substring(0,8)}-{seq}"`. Usar auto-numbering do D365 ou sequence customizada.
    - `ftd_id_revisao = 1`

**T1.7.3 — Criar Business Rule para unicidade de proposta ativa**
- **Onde**: make.powerapps.com → Customization Master → ftd_proposta → Business Rules → New
- **Regra**: ao tentar mudar Status para "Ativa", validar que não existe outra proposta com Status "Ativa" na mesma Oportunidade. Se existir, bloquear com mensagem.
- **Alternativa (mais segura para concorrência)**: implementar via plugin PreOperation no Update de `ftd_status` verificando com `RetrieveMultiple` se há outra proposta Ativa na mesma oportunidade. Plugin é melhor que Business Rule para garantir atomicidade.

**T1.7.4 — Testes unitários**
- **Onde**: `src/Backend/Tests/Proposta/CadeiaPropostaPluginTests.cs`
- **Cenários**:
  - `Create_Should_SetRevisao1_When_PrimeiraPropostaDaCadeia`
  - `Create_Should_IncrementRevisao_When_PropostaAnteriorExiste`
  - `Create_Should_GenerateCadeiaId_When_PrimeiraProposta`
  - `Create_Should_UseSameCadeiaId_When_PropostaAnteriorExiste`
  - `Create_Should_UpdateAnteriorPosterior_When_Linked`
  - `Update_Should_BlockAtiva_When_OutraPropostaAtivaExisteNaOportunidade`
  - cenário de cadeia cross-safra: 1.4 (2025) → 2.0 (2026) mantém mesmo `ftd_id_cadeia`

**Estimativa**: M
**Prioridade**: Must
**Dependências**: Nenhuma

---

## Story 1.8: Implementar Encerramento de Negociação (Perda)
**Como** consultor, **quero** encerrar uma negociação registrando a razão da perda de forma estruturada, **para que** a FTD tenha inteligência sobre os motivos de perda e possa agir estrategicamente.

**Acceptance Criteria:**
- [ ] Ação "Encerrar negociação" disponível na interface da proposta recusada
- [ ] Campo obrigatório "Razão da Perda" com opções padronizadas: Preço/Condições Comerciais, Logística, Plataforma eCommerce, Plataforma Educacional, Atendimento Pedagógico, Qualidade materiais, Escola não adaptou, Mudança mantenedora/rede, Material próprio, Escola encerrou, Outros (especificar)
- [ ] Status da Proposta → "Histórico"
- [ ] Status da Oportunidade → "Perda" com razão no campo correspondente
- [ ] Notificação de "Oportunidade Perdida" enviada ao Coordenador e Gerente

**Tasks:**

**Estrutura de pastas (padrão BCA: Custom API → Service → Tests):**
```
src/Backend/
├── Plugins/
│   └── Proposta/
│       └── EncerrarNegociacaoApi.cs            ← Custom API (entry point)
├── Services/
│   └── Proposta/
│       ├── IEncerrarNegociacaoService.cs        ← Interface
│       └── EncerrarNegociacaoService.cs         ← SERVICE (lógica de encerramento)
└── Tests/
    └── Proposta/
        └── EncerrarNegociacaoServiceTests.cs    ← Testes do Service
```

**T1.8.1 — Criar/atualizar OptionSet global `ftd_razao_perda`**
- **Onde**: make.powerapps.com → Customization Master → Components → Option Sets → New Global
- **Nome interno**: `ftd_razao_perda`
- **Valores** (11 itens):
  - 100000000 = Preço/Condições Comerciais
  - 100000001 = Logística
  - 100000002 = Plataforma eCommerce
  - 100000003 = Plataforma Educacional
  - 100000004 = Atendimento Pedagógico
  - 100000005 = Qualidade dos Materiais
  - 100000006 = Escola não se Adaptou
  - 100000007 = Mudança de Mantenedora/Rede
  - 100000008 = Material Próprio
  - 100000009 = Escola Encerrou Atividades
  - 100000010 = Outros
- **Campos adicionais na `ftd_proposta`**:
  - `ftd_razao_perda` (OptionSet global acima) — obrigatório quando Estágio = Perda
  - `ftd_razao_perda_detalhe` (Single Line Text, 500) — obrigatório quando razão = "Outros"
- **Na Oportunidade** (`opportunity`): verificar se já tem campo `closeprobability` ou razão de perda nativa. Se não, adicionar `ftd_razao_perda_oportunidade` (OptionSet ou texto mapeado)
- **Após**: `UnpackSolution` + `GenerateEarlyBound`

**T1.8.2 — Criar Custom API `ftd_EncerrarNegociacao`**
- **Onde**: `src/Backend/Plugins/Proposta/EncerrarNegociacaoApi.cs`
- **Herança**: `CustomApiPlugin<EncerrarNegociacaoRequest, EncerrarNegociacaoResponse>` (padrão BCA)
- **Request**: `PropostaId` (Guid), `RazaoPerda` (int, valor do optionset), `Detalhe` (string, opcional)
- **Response**: `Sucesso` (bool)
- **Lógica do Service** (`EncerrarNegociacaoService.cs`):
  1. Validar que proposta está no Estágio "Recusada" ou "Assinatura recusada" (único ponto de entrada válido)
  2. Se `RazaoPerda == Outros` e `Detalhe` está vazio → lançar exceção
  3. Atualizar Proposta: `ftd_razao_perda`, `ftd_razao_perda_detalhe`, Status = Histórico, Estágio = Perda
  4. Atualizar Oportunidade vinculada: Status = Lost (usar `LoseOpportunityRequest` nativo do D365 — é o jeito correto de fechar como perda, pois preenche `State = Lost` e `StatusReason = Canceled/Lost`)
  5. Chamar `INotificacaoAprovacaoService.NotificarPerda()` (email + Teams para Coordenador e Gerente)
- **Registro no CRM**: Settings → Custom APIs → New → `ftd_EncerrarNegociacao`, Binding Type = Global

**T1.8.3 — Testes unitários**
- **Onde**: `src/Backend/Tests/Proposta/EncerrarNegociacaoServiceTests.cs`
- **Cenários**:
  - `Encerrar_Should_SetHistorico_When_PropostaRecusada`
  - `Encerrar_Should_LoseOpportunity_When_Called`
  - `Encerrar_Should_RequireDetalhe_When_RazaoIsOutros`
  - `Encerrar_Should_ThrowException_When_EstagioInvalido` (não pode encerrar proposta Em Aprovação)
  - `Encerrar_Should_NotifyCoordEnadorEGerente_When_Completed`

**Estimativa**: S
**Prioridade**: Must
**Dependências**: Story 1.4

---

## Story 1.9: Implementar Tratativa de Assinatura (DocuSign Integration)
**Como** sistema CRM, **quero** processar os eventos de assinatura e recusa do DocuSign atualizando automaticamente Status/Estágio da proposta e criando o Pedido, **para que** o fluxo pós-aprovação seja automatizado end-to-end.

**Acceptance Criteria:**
- [ ] Proposta assinada: Estágio → "Aprovada e assinada", Status → "Ativa"
- [ ] Proposta anterior (se existir): Status → "Histórico"
- [ ] Pedido (Order) é criado automaticamente como cópia da Proposta assinada (incluindo contrato anexado)
- [ ] Proposta recusada no DocuSign: Estágio → "Assinatura recusada"
- [ ] Consultor pode criar nova versão ou encerrar negociação a partir de assinatura recusada
- [ ] Notificação de "Proposta Assinada" (celebrativa) ou "Assinatura Recusada" enviada

> **NOTA**: O DocuSign já está integrado ao CRM. O plugin existente precisa ser ESTENDIDO, não substituído. Verificar com Julio/Tercio o código existente e entender os webhooks/callbacks atuais.

**Tasks:**

**Estrutura de pastas (padrão BCA: Service → Tests):**
```
src/Backend/
├── Plugins/
│   └── Integracao/
│       └── DocuSignPlugin.cs (EXISTENTE)       ← Estender, não substituir
├── Services/
│   └── Proposta/
│       ├── IPosAssinaturaService.cs             ← Interface
│       └── PosAssinaturaService.cs              ← SERVICE (pós-assinatura)
│   └── Pedido/
│       ├── ICriarPedidoService.cs               ← Interface
│       └── CriarPedidoService.cs                ← SERVICE (cria pedido)
└── Tests/
    ├── Proposta/
    │   └── PosAssinaturaServiceTests.cs         ← Testes
    └── Pedido/
        └── CriarPedidoServiceTests.cs           ← Testes
```

**T1.9.1 — Mapear integração DocuSign existente**
- **Onde**: Repo `FTD Dynamics` → buscar plugins com "DocuSign", "AdobeSign" ou "Assinatura" no nome
- **Ação**: levantar quais webhooks/callbacks já estão configurados, como o plugin existente processa eventos, quais status da proposta ele atualiza hoje
- **Deliverable**: documento curto (2 páginas) com: endpoints existentes, campos mapeados, gap para novos estágios
- **Risco**: se não chamar "DocuSign" pode ser Adobe Sign (confirmar com FTD — na reunião mencionaram DocuSign e Adobe Sign em momentos diferentes)

**T1.9.2 — Estender plugin de assinatura para novos Estágios**
- **Onde**: plugin existente de DocuSign/AdobeSign no repo (path a confirmar após T1.9.1)
- **Modificações**:
  - Adicionar handling para evento `signed/completed`: setar Estágio = "Aprovada e assinada", Status = "Ativa"
  - Adicionar handling para evento `declined/voided`: setar Estágio = "Assinatura recusada"
  - Registrar evento no `ftd_aprovacao_historico` com `ftd_decisao = Assinado` ou `ftd_decisao = AssinaturaRecusada`
  - Após assinatura: atualizar proposta anterior (se cadeia) para Status = Histórico via `ftd_proposta_anterior`

**T1.9.3 — Criar Service `PosAssinaturaService`**
- **Onde**: `src/Backend/Services/Proposta/PosAssinaturaService.cs`
- **Interface**: `IPosAssinaturaService`
- **Métodos**:
  - `ProcessarAssinatura(Guid propostaId)` — atualiza status, cria pedido, notifica
  - `ProcessarRecusaAssinatura(Guid propostaId)` — atualiza estágio, notifica
- **Lógica de `ProcessarAssinatura()`**:
  1. Proposta: Estágio = "Aprovada e assinada", Status = "Ativa"
  2. Se `ftd_proposta_anterior` != null → proposta anterior: Status = "Histórico"
  3. Chamar `CriarPedidoService.CriarPedidoAPartirDaProposta(propostaId)` (ver T1.9.4)
  4. Chamar `INotificacaoAprovacaoService.NotificarAssinatura()` (email celebrativo)

**T1.9.4 — Criar Service `CriarPedidoService`**
- **Onde**: `src/Backend/Services/Pedido/CriarPedidoService.cs`
- **Interface**: `ICriarPedidoService`
- **Método**: `CriarPedidoAPartirDaProposta(Guid propostaId) : Guid` (retorna ID do novo pedido)
- **Lógica**:
  1. Ler proposta e todos produtos filhos
  2. Criar `SalesOrder` (entidade nativa) com mapeamento Proposta → Pedido:
     - `ftd_proposta_origem` (lookup) = proposta assinada
     - `customerid` = conta da proposta
     - `ftd_contrato_assinado` (Attachment/Note) = anexo do contrato DocuSign
     - demais campos conforme mapeamento a definir com FTD
  3. Criar `SalesOrderDetail` para cada produto da proposta
- **CUIDADO**: verificar se hoje o Pedido já é criado por outro processo — se sim, estender em vez de duplicar

**T1.9.5 — Testes unitários**
- **Onde**: `src/Backend/Tests/Proposta/PosAssinaturaServiceTests.cs`, `src/Backend/Tests/Pedido/CriarPedidoServiceTests.cs`
- **Cenários**:
  - `ProcessarAssinatura_Should_SetAtiva_When_DocuSignReturnsCompleted`
  - `ProcessarAssinatura_Should_CreateOrder_When_PropostaAssinada`
  - `ProcessarAssinatura_Should_SetAnteriorHistorico_When_CadeiaExiste`
  - `ProcessarRecusaAssinatura_Should_SetAssinaturaRecusada`
  - `CriarPedido_Should_MapAllProducts_When_PropostaHasProducts`
  - `CriarPedido_Should_AttachContrato_When_DocPayloadContainsFile`

**Estimativa**: L
**Prioridade**: Must
**Dependências**: Story 1.7, Story 1.4

---

## Story 1.10: Implementar Interface de Aprovação na Etapa 6 do Simulador (Big Numbers)
**Como** aprovador (Diretor/Gerente), **quero** visualizar os principais indicadores da proposta (big numbers) na Etapa 6 do Simulador com botões de Aprovar/Recusar, **para que** eu possa tomar decisões rápidas com todas as informações relevantes consolidadas.

**Acceptance Criteria:**
- [ ] Etapa 6 apresenta big numbers: Receita Bruta, Receita após Benefícios e Patrocínios, Total Investido na Escola, Adiantamento, Parcelas, Análise por Linha de Negócio
- [ ] Comparativo com proposta anterior (se existir)
- [ ] Visualização de quem já aprovou (com timestamp), quem falta, quem recusou
- [ ] Aprovações "herdadas" de versão anterior claramente identificadas
- [ ] Botão "Aprovar" e "Recusar" (com campo obrigatório de justificativa na recusa)
- [ ] Justificativa do consultor visível para aprovadores
- [ ] Interface Mobile First e responsiva
- [ ] Para D365: view customizada com campos calculados disponível no formulário da Proposta

> **Decisão arquitetural pendente**: a Etapa 6 será no **Power Pages** (Simulador web) ou dentro do **D365 model-driven form**? Na reunião falou-se de ambos. Recomendação: inicialmente implementar no D365 (model-driven form) para aprovadores que não usam Simulador. Depois, replicar UI no Power Pages para quem acessa via Simulador.

**Tasks:**

**Estrutura de pastas (padrão BCA: TypeScript Contract/Controller → Tests):**
```
src/Frontend/
├── Forms/
│   └── Proposta/
│       ├── AprovacaoPropostaContract.ts        ← Contract (campos, regras)
│       ├── AprovacaoPropostaController.ts      ← Controller (lógica de UI)
│       └── AprovacaoPropostaRules.ts           ← Fluent Rules (validação)
└── Tests/
    └── Proposta/
        └── AprovacaoPropostaController.test.ts ← Testes Jest
```

**T1.10.1 — Criar formulário D365 "Aprovação de Proposta"**
- **Onde**: make.powerapps.com → Customization Master → ftd_proposta → Forms → New Main Form
- **Nome**: "Aprovação de Proposta" (separado do form principal do consultor)
- **Layout**:
  - **Tab 1 — Resumo**: seção com big numbers (read-only):
    - `ftd_receita_bruta` (Currency, rolllup ou calculado)
    - `ftd_receita_apos_beneficios` (Currency)
    - `ftd_total_investido_escola` (Currency)
    - `ftd_adiantamento` (Currency)
    - `ftd_parcelas` (Whole Number)
    - `ftd_analise_por_linha_negocio` (subgrid ou PCF para breakdown)
  - **Tab 2 — Comparativo**: Quick View Form mostrando campos da `ftd_proposta_anterior` lado a lado. Se proposta anterior não existe, tab escondida.
  - **Tab 3 — Aprovações**: subgrid de `ftd_aprovacao_historico` com colunas: Nível, Aprovador, Decisão, Data/Hora, Justificativa, Proposta Origem (para herdadas)
  - **Tab 4 — Justificativa do Consultor**: campo `ftd_justificativa_consultor` read-only
- **Security**: form disponível apenas para Security Roles "FTD Aprovador" e "FTD Aprovador Executivo" (via Form Selector)

**T1.10.2 — Implementar subgrid de Histórico de Aprovações**
- **Onde**: no formulário criado em T1.10.1, Tab 3
- **Entidade relacionada**: `ftd_aprovacao_historico` com filtro `ftd_proposta = {currentRecordId}`
- **Colunas**: `ftd_nivel_alcada_nome` (lookup), `ftd_aprovador_nome` (lookup Systemuser), `ftd_decisao` (optionset — Pendente/Aprovado/Recusado/Herdada), `ftd_timestamp_decisao`, `ftd_justificativa`, `ftd_proposta_origem_heranca`
- **Formatação condicional** (via PCF ou CSS customizado): Aprovado = verde, Recusado = vermelho, Herdada = azul, Pendente = cinza
- **Ordenação**: por `ftd_nivel_alcada.ftd_nivel` ASC (menor nível primeiro)

**T1.10.3 — Criar TypeScript form scripts**
- **Onde**: `src/Frontend/Forms/Proposta/AprovacaoPropostaContract.ts` + `AprovacaoPropostaController.ts`
- **Padrão BCA**: Contract define campos e regras, Controller implementa lógica
- **Contract**:
  - Campos: `ftd_estagio_proposta`, `ftd_proposta_anterior`, campos de big numbers, `ftd_justificativa_consultor`
  - Rules: `ShowHideComparativoTab` (visível se proposta anterior existe), `ShowHideAcaoButtons` (visível se Estágio = "Em Aprovação" e usuário é aprovador da vez)
- **Controller**:
  - `onLoad()`: carregar dados, verificar se usuário é aprovador pendente, mostrar/esconder botões e tabs
  - `onAprovar()`: exibir confirm dialog, chamar Custom API `ftd_AprovarProposta`
  - `onRecusar()`: exibir modal com textarea obrigatória para justificativa, chamar Custom API `ftd_RecusarProposta`
  - Verificação de permissão: usar `Xrm.Utility.getGlobalContext().userSettings.userId` comparado com `ftd_aprovador` do registro pendente

**T1.10.4 — Criar Quick View Form para comparativo**
- **Onde**: make.powerapps.com → Customization Master → ftd_proposta → Forms → New Quick View Form
- **Nome**: "Comparativo Proposta Anterior"
- **Campos**: todos os big numbers + `ftd_estagio_proposta` + `ftd_data_submissao` da proposta anterior via lookup `ftd_proposta_anterior`
- **Uso**: embutir no Tab 2 do formulário de aprovação

**T1.10.5 — Criar ribbon buttons "Aprovar" e "Recusar"**
- **Onde**: Customization Master → ftd_proposta → Ribbon → Add Custom Button (via Ribbon Workbench ou comando direto no XML)
- **Botão "Aprovar"**: cor verde, ícone check, Enable Rule = JS function `isAprovacaoPendente()`, Action = `Xrm.Page.data.entity.save()` + chamar Custom API
- **Botão "Recusar"**: cor vermelha, ícone X, Enable Rule = JS function `isAprovacaoPendente()`, Action = abrir dialog de justificativa
- **Display Rules BCA**: usar Fluent Rules para habilitar/desabilitar conforme Estágio e Role do usuário
- **Alternativa (mais moderna)**: usar **Command Bar** do Power Apps (novo) ao invés do Ribbon Workbench legado. Depende da versão do ambiente.

**T1.10.6 — Implementar Fluent Rules para validação**
- **Onde**: `src/Frontend/Forms/Proposta/AprovacaoPropostaRules.ts`
- **Regras**:
  - `validateRecusaJustificativa`: campo justificativa obrigatório ≥ 20 caracteres antes de permitir recusa
  - `validateAprovadorAutorizado`: verificar que usuário logado é de fato o aprovador do nível pendente
  - `validateEstagioCorreto`: só permitir ação se Estágio = "Em Aprovação"

**T1.10.7 — Testes Jest para form scripts**
- **Onde**: `src/Frontend/Tests/Proposta/AprovacaoPropostaController.test.ts`
- **Cenários**:
  - `onLoad_Should_ShowComparativoTab_When_PropostaAnteriorExists`
  - `onLoad_Should_HideButtons_When_UserIsNotAprovador`
  - `onLoad_Should_ShowButtons_When_UserIsAprovadorPendente`
  - `onRecusar_Should_RequireJustificativa_When_TextIsEmpty`
  - `onAprovar_Should_CallCustomApi_When_Confirmed`
  - `isAprovacaoPendente_Should_ReturnFalse_When_EstagioIsRascunho`

**Estimativa**: XL
**Prioridade**: Must
**Dependências**: Story 1.3, Story 1.7

---

## Story 1.11: Configurar Novos Usuários Aprovadores no CRM
**Como** administrador do CRM, **quero** provisionar os novos usuários aprovadores (que hoje usam Vulcano) no D365 CE com os Security Roles apropriados, **para que** todos os participantes do fluxo tenham acesso ao sistema.

**Acceptance Criteria:**
- [ ] Levantamento de novos usuários que precisam de acesso (Diretoria, Superintendência, Presidência)
- [ ] Security Role "FTD Aprovador" criado com permissões mínimas: ler Proposta, ler/escrever `ftd_aprovacao_historico`, ler Conta, ler Oportunidade
- [ ] Security Roles atribuídos por nível de alçada
- [ ] Onboarding de usuários com treinamento do fluxo

**Tasks:**

**T1.11.1 — Criar Security Role "FTD Aprovador"**
- **Onde**: make.powerapps.com → Customization Master → Settings → Security → Security Roles → New
- **Nome**: "FTD Aprovador"
- **Permissões** (princípio do menor privilégio):
  - `ftd_proposta`: Read (BU), Write **NENHUM** (aprovador não edita proposta, só o histórico)
  - `ftd_aprovacao_historico`: Read (BU), Write (User — só seu registro), Create (NENHUM — criado por plugin)
  - `Account`: Read (BU)
  - `Opportunity`: Read (BU)
  - `ftd_produto_proposta`: Read (BU) — para ver detalhes na interface
  - `ftd_beneficio`, `ftd_patrocinio`: Read (BU)
  - `ftd_alcada_aprovacao`, `ftd_aprovador`: Read (Org) — referência
  - Tabs Core Records, Custom Entities, Customization: mínimo necessário
- **Scope BU vs Org**: Aprovador operacional (níveis 1-4) = BU scope. Aprovador executivo (níveis 5-7) = ver T1.11.2

**T1.11.2 — Criar Security Role "FTD Aprovador Executivo"**
- **Onde**: mesmo local
- **Diferenças do "FTD Aprovador"**:
  - `ftd_proposta`: Read (**Organization**) — diretoria e presidência precisam ver propostas de todas BUs
  - `ftd_aprovacao_historico`: Read (**Organization**), Write (User)
  - `Account`, `Opportunity`: Read (**Organization**)
- **Atribuição**: níveis 5 (Diretor Comercial), 6 (Superintendente), 7 (Presidência)
- **Nota**: manter roles separados para permitir ajuste fino se necessário depois

**T1.11.3 — Levantar lista de novos usuários**
- **Onde**: coordenar com Kevellin (FTD) — ela ficou responsável por enviar o levantamento
- **Deliverable**: planilha Excel com: Nome, Email, Cargo, Nível de Alçada, BU, Security Role a atribuir
- **Bloqueio**: sem a lista não é possível provisionar. Marcar como dependência externa.

**T1.11.4 — Provisionar usuários e atribuir Security Roles**
- **Onde**: admin.powerplatform.microsoft.com → Environments → DEV/OAT/PROD → Settings → Users
- **Processo** (por usuário):
  1. Adicionar usuário ao ambiente (deve ter licença D365 Customer Engagement atribuída no M365 Admin Center)
  2. Atribuir Security Role correto ("FTD Aprovador" ou "FTD Aprovador Executivo")
  3. Atribuir Team (se BU-based) via Administration → Teams
- **Automação**: se > 10 usuários, usar script PowerShell com módulo `Microsoft.Xrm.Data.PowerShell` para batch assignment
- **Validação**: após provisioning, testar login de 2-3 usuários representativos em DEV

**T1.11.5 — Documentar procedimento de onboarding**
- **Onde**: documentação de projeto (Paige tech-writer)
- **Conteúdo**: guia passo-a-passo para futuras adições de aprovadores, com screenshots
- **Inclui**: como adicionar usuário, atribuir role, configurar como aprovador na entidade `ftd_aprovador`

**Estimativa**: S
**Prioridade**: Must
**Dependências**: Story 1.1

---

## Story 1.12: Interface de Manutenção da Política Comercial
**Como** gestor da política comercial, **quero** poder ajustar os valores e percentuais das regras de alçada diretamente no CRM sem necessidade de alteração técnica, **para que** mudanças de política possam ser aplicadas rapidamente.

**Acceptance Criteria:**
- [ ] Interface (model-driven form) para gerenciar registros de `ftd_regra_alcada`
- [ ] Gestor pode alterar valores mínimos/máximos e nível de alçada resultante
- [ ] Alterações ficam registradas em audit log do D365
- [ ] Novas regras que não se encaixem na estrutura existente requerem customização (documentado)
- [ ] Validação de integridade: impedir overlap de ranges dentro do mesmo grupo/indicador

**Tasks:**

**Estrutura de pastas (padrão BCA: Plugin → Tests):**
```
src/Backend/
├── Plugins/
│   └── Configuracao/
│       └── ValidarRegraAlcadaPlugin.cs         ← Plugin (valida overlap de ranges)
└── Tests/
    └── Configuracao/
        └── ValidarRegraAlcadaPluginTests.cs    ← Testes do Plugin
```

**T1.12.1 — Criar model-driven App view "Política Comercial"**
- **Onde**: make.powerapps.com → Customization Master → ftd_regra_alcada → Views → New
- **View "Regras de Alçada — Visão Geral"**:
  - Colunas: `ftd_grupo` (OptionSet), `ftd_tipo_indicador` (OptionSet), `ftd_valor_minimo`, `ftd_valor_maximo`, `ftd_tipo_comparacao`, `ftd_nivel_alcada_resultante` (Lookup), `ftd_ativo` (bool)
  - Agrupamento: por `ftd_grupo` (1° nível) e `ftd_tipo_indicador` (2° nível)
  - Filtro padrão: `ftd_ativo = true`
  - **Editable Grid**: habilitar Editable Grid control na view para permitir edição inline
- **App**: criar ou adicionar ao Model-Driven App "Spartan" uma área "Configurações" → "Política Comercial" com a entidade `ftd_regra_alcada`
- **Security**: acesso apenas para roles "System Administrator" e novo role "FTD Gestor Política" (a criar)

**T1.12.2 — Criar Business Rule de validação de overlap**
- **Onde**: Customization Master → ftd_regra_alcada → Business Rules → New
- **Problema**: se dois registros do mesmo `ftd_tipo_indicador` cobrem ranges que se sobrepõem (ex: 0-100 e 50-200), o motor de cálculo não saberia qual nível usar
- **Implementação recomendada**: Business Rule não consegue fazer cross-record validation. Implementar via **Plugin PreOperation** no Create/Update de `ftd_regra_alcada`:
  - `src/Backend/Plugins/Configuracao/ValidarRegraAlcadaPlugin.cs`
  - `Plugin<ftd_regra_alcada>`, Registration: `[PluginRegistration(MessageName = "Create", Stage = PluginStage.PreOperation)]` e `[PluginRegistration(MessageName = "Update", Stage = PluginStage.PreOperation, FilteringAttributes = new[] { "ftd_valor_minimo", "ftd_valor_maximo", "ftd_tipo_indicador", "ftd_grupo" })]`
  - Lógica: buscar todos registros ativos do mesmo `ftd_grupo` + `ftd_tipo_indicador`. Verificar se o range [min, max] do registro atual se sobrepõe com algum existente. Se sim, lançar `InvalidPluginExecutionException("Range sobrepõe regra existente: {nome_regra}")`

**T1.12.3 — Habilitar Audit na entidade `ftd_regra_alcada`**
- **Onde**: make.powerapps.com → Customization Master → ftd_regra_alcada → Properties → Enable Auditing = Yes
- **Verificar**: que o Audit está habilitado no nível da organização (Settings → Auditing → Global Audit Settings → Start Auditing)
- **Campos auditados**: todos os campos relevantes (valor_minimo, valor_maximo, nivel_alcada_resultante, ativo)
- **Retenção**: configurar retenção de audit log conforme política FTD (padrão: 90 dias ou mais)

**T1.12.4 — Criar guia de uso para gestores**
- **Onde**: documento de treinamento (coordenar com Paige)
- **Conteúdo**: como acessar, como editar regras, o que significa cada campo, quando solicitar customização (novas regras fora da estrutura existente), como consultar o audit log

**Estimativa**: S
**Prioridade**: Should
**Dependências**: Story 1.2

---

## Story 1.13: Simplificação de Status/Estágio da Proposta
**Como** consultor/gestor, **quero** que os campos de Status e Estágio da Proposta sejam simplificados conforme a nova especificação, **para que** o ciclo de vida da proposta seja claro e sem ambiguidades.

**Acceptance Criteria:**
- [ ] Status da Proposta: Aberto, Ativo, Histórico (3 valores, preenchimento automático)
- [ ] Estágio da Proposta (novo campo): Rascunho, Em Negociação, Avaliação Cliente, Em Aprovação, Recusada, Aprovada não assinada, Assinatura recusada, Aprovada e assinada
- [ ] Campos antigos são depreciados (hidden nos formulários, mantidos no banco para migração)
- [ ] Automação de transição de estágio por meio de plugins (não manual)
- [ ] Dashboards e views atualizados para usar novos campos

**Tasks:**

**Estrutura de pastas (padrão BCA: Plugin → Tests):**
```
src/Backend/
├── Plugins/
│   └── Proposta/
│       └── TransicaoEstagioPlugin.cs           ← Plugin (máquina de estados)
└── Tests/
    └── Proposta/
        └── TransicaoEstagioPluginTests.cs      ← Testes do Plugin
```

**T1.13.1 — Criar OptionSet `ftd_estagio_proposta`**
- **Onde**: make.powerapps.com → Customization Master → ftd_proposta → Fields → New
- **Tipo**: Local OptionSet (ou Global se for reutilizado em outras entidades)
- **Valores** (8 estágios):
  - 100000000 = Rascunho (default na criação)
  - 100000001 = Em Negociação
  - 100000002 = Avaliação Cliente
  - 100000003 = Em Aprovação
  - 100000004 = Recusada
  - 100000005 = Aprovada não assinada (entre aprovação última alçada e envio DocuSign)
  - 100000006 = Assinatura recusada (DocuSign rejeitada)
  - 100000007 = Aprovada e assinada (DocuSign completa)
- **Campo**: `ftd_estagio_proposta` (OptionSet acima), Required Level = Application Recommended, Default = Rascunho
- **Após**: `UnpackSolution` + `GenerateEarlyBound`

**T1.13.2 — Atualizar Status da Proposta (State + Status Reason)**
- **Onde**: make.powerapps.com → Customization Master → ftd_proposta → Fields → Status/Status Reason
- **Opção A** (recomendada): usar campos nativos `statecode`/`statuscode`:
  - State = Active (0): Status Reasons = Aberto (1, default), Ativo (100000000)
  - State = Inactive (1): Status Reason = Histórico (100000001)
- **Opção B**: criar campo custom `ftd_status_proposta` (OptionSet: Aberto=1, Ativo=2, Histórico=3) se a opção A trouxer complexidade com o pipeline existente
- **Decisão**: verificar com equipe como `ftd_proposta` usa statecode hoje; se já há customizações, opção B é mais segura
- **Depreciar**: campos de status antigos (identificar com Julio quais são, ex: `ftd_estado`, `ftd_situacao`, `statuscode` antigos) — marcar como hidden em todos os forms, NÃO remover do banco

**T1.13.3 — Criar plugin `TransicaoEstagioPlugin`**
- **Onde**: `src/Backend/Plugins/Proposta/TransicaoEstagioPlugin.cs`
- **Herança**: `Plugin<ftd_proposta>`
- **Registration**: `[PluginRegistration(MessageName = "Update", Stage = PluginStage.PreValidation, FilteringAttributes = new[] { "ftd_estagio_proposta" })]`
- **Lógica — Máquina de estados**: validar transições permitidas:
  ```
  Rascunho → Em Negociação
  Em Negociação → Avaliação Cliente
  Avaliação Cliente → Em Aprovação
  Em Aprovação → Recusada | Aprovada não assinada
  Aprovada não assinada → Aprovada e assinada | Assinatura recusada
  Assinatura recusada → Em Negociação (nova versão) | Perda (encerramento)
  Recusada → Em Negociação (nova versão via copy) | Perda (encerramento)
  ```
- **Implementação**: dictionary `Dictionary<EstagioEnum, HashSet<EstagioEnum>>` com transições válidas. No PreValidation, se `Target.ftd_estagio_proposta` está no filtro, verificar se `PreImage.ftd_estagio_proposta → Target.ftd_estagio_proposta` é uma transição válida. Se não, `throw new InvalidPluginExecutionException($"Transição inválida: {de} → {para}")`.
- **PreImage**: registrar PreImage com `ftd_estagio_proposta` para comparar antes/depois

**T1.13.4 — Atualizar formulários e views**
- **Onde**: make.powerapps.com → Customization Master → ftd_proposta → Forms e Views
- **Forms**: 
  - Esconder campos de status antigos em TODOS os forms (main, quick view, quick create)
  - Adicionar `ftd_estagio_proposta` como campo header (Business Process Flow header ou form header) — visível e read-only (gerenciado por automação)
  - Adicionar `ftd_status_proposta` ou `statuscode` no header também
- **Views**:
  - "Minhas Propostas Ativas": filtro por `ftd_status_proposta = Aberto OU Ativo + ftd_estagio != Histórico`
  - "Propostas em Aprovação": filtro por `ftd_estagio_proposta = Em Aprovação`
  - "Propostas Recusadas": filtro por `ftd_estagio_proposta = Recusada`
  - Atualizar todas as views existentes para usar novos campos
- **Dashboards**: atualizar charts/dashboards que usavam status antigo

**T1.13.5 — Script de migração para propostas existentes**
- **Onde**: script PowerShell ou Console App para executar uma única vez
- **Lógica**: mapear valores antigos de status/estágio para novos valores. Para cada proposta existente:
  - Ler status antigo → mapear para `ftd_estagio_proposta` + `ftd_status_proposta`
  - Atualizar registro
- **Segurança**: executar primeiro em DEV, validar contagens, depois OAT, depois PROD (com GMUD)
- **Backup**: antes de executar em PROD, solicitar backup do banco (ou export CRM da solução com dados)
- **CUIDADO**: desabilitar temporariamente o `TransicaoEstagioPlugin` durante a migração (pois vai fazer transições "inválidas" como setar diretamente). Ou adicionar parâmetro `bypassValidation` no plugin para cenário de migração.

**T1.13.6 — Testes unitários**
- **Onde**: `src/Backend/Tests/Proposta/TransicaoEstagioPluginTests.cs`
- **Cenários** (todas as transições — 12 cenários):
  - `Update_Should_Allow_Rascunho_To_EmNegociacao`
  - `Update_Should_Allow_EmAprovacao_To_Recusada`
  - `Update_Should_Allow_EmAprovacao_To_AprovadaNaoAssinada`
  - `Update_Should_Block_Rascunho_To_EmAprovacao` (pula etapa)
  - `Update_Should_Block_AprovadaEAssinada_To_Rascunho` (regressão)
  - `Update_Should_Block_Recusada_To_AprovadaEAssinada` (bypass sem re-aprovação)
  - cenário de todas transições válidas e 4-5 inválidas representativas

**Estimativa**: M
**Prioridade**: Must
**Dependências**: Nenhuma

---

# Épico 2: Revisão da Estrutura de Cadastro de Produtos
> Revisar e padronizar o cadastro de produtos no CRM para resolver o problema de 1.283 produtos aparecendo onde deveriam ser ~17 (prateleira). Alinhar taxonomia entre ISA, TOTVS e CRM. Habilitar filtros adequados para o Simulador.
>
> **Fonte**: Refinamento 16/Mar — seção "Estrutura de Cadastro de Produtos"
> **Prioridade**: 🟠 SHOULD — Em andamento (paralelo com ISA/TOTVS), sem deadline definido
> **Dependências**: Resultado de reunião ISA/TOTVS (quinta-feira semana de 16/Mar)

---

## Story 2.1: Definir e Implementar Nova Taxonomia de Linha de Negócio
**Como** equipe comercial, **quero** que as linhas de negócio no CRM reflitam a categorização padronizada entre ISA, TOTVS e CRM, **para que** relatórios de vendas sejam consistentes entre todas as áreas.

**Acceptance Criteria:**
- [ ] Nova taxonomia de linhas de negócio definida e aprovada por Comercial, Produtos e Controladoria
- [ ] Anomalias eliminadas: FC Zoom dentro de Serviços Educacionais, Dicionário e Agenda removidos como linhas de negócio, Evolution dentro de Sistema de Ensino
- [ ] Optionset/entidade de Linha de Negócio atualizada no CRM
- [ ] Mapeamento ISA ↔ TOTVS ↔ CRM documentado
- [ ] Migração de dados existentes para nova taxonomia

**Tasks:**

**T2.1.1 — Criar documento de mapeamento ISA ↔ TOTVS ↔ CRM**
- **Onde**: documento compartilhado (SharePoint ou repo `docs/arquitetura/`)
- **Participantes**: Oscar (comercial), equipe ISA, Mônica (TOTVS), Avanade
- **Conteúdo**: tabela com: ISA "Família de Produto" | TOTVS "Família Comercial" | CRM "Linha de Negócio" atual | CRM "Linha de Negócio" nova
- **Anomalias a resolver**: FC Zoom dentro de Serviços Educacionais (deve ir para Integrado Digital), Dicionário e Agenda → não são linhas (são tipos de produto), Evolution → dentro de Sistema de Ensino
- **Deliverable**: planilha aprovada pelos 3 times

**T2.1.2 — Criar/atualizar OptionSet ou entidade de Linha de Negócio**
- **Onde**: make.powerapps.com → Customization Master
- **Decisão de design**: OptionSet global (`ftd_linha_negocio`) vs. entidade `ftd_linha_negocio` (lookup). Recomendação: **entidade** se espera-se evolução frequente (novas linhas) ou se precisa de metadados (código ISA, código TOTVS no registro). OptionSet se a lista é fixa e pequena.
- **Campos da entidade** (se entidade): `ftd_nome`, `ftd_codigo_isa`, `ftd_codigo_totvs`, `ftd_ativo`, `ftd_tipo` (optionset: Principal/Sub)
- **Uso**: substituir campo atual de Linha de Negócio no Product por lookup para nova entidade
- **Após**: `UnpackSolution` + `GenerateEarlyBound`

**T2.1.3 — Script de migração de produtos existentes**
- **Onde**: Console App C# ou script PowerShell
- **Lógica**: para cada produto no CRM, ler linha de negócio atual → buscar mapeamento → atualizar para novo valor
- **Segurança**: executar em DEV, validar contagens, depois OAT, depois PROD
- **Backup**: export de Products antes da migração

**T2.1.4 — Atualizar integração TOTVS → CRM**
- **Onde**: Azure Function de integração (ETL diário 6h) ou processo de integração existente
- **Modificação**: ao sincronizar produtos do TOTVS, mapear `Família Comercial` TOTVS → nova `ftd_linha_negocio` CRM
- **Validação**: criar 5 produtos de teste no TOTVS com diferentes famílias comerciais, executar sync, validar mapeamento no CRM

**T2.1.5 — Validar com BI/Analytics**
- **Onde**: coordenar com equipe de Analytics/Power BI da FTD
- **Ação**: verificar se relatórios existentes usam o campo de Linha de Negócio antigo. Se sim, atualizar queries para novo campo.
- **Risco**: relatórios históricos podem quebrar se compararem anos-safra com taxonomia diferente

**Estimativa**: L
**Prioridade**: Should
**Dependências**: Reunião ISA/TOTVS (resultado pendente)

---

## Story 2.2: Implementar Conceito de Variação de Produto
**Como** consultor, **quero** que produtos com variações (ex: Ponto e Ponto 4 ou 8 correções, Trilhas maiúsculas/minúsculas) estejam corretamente categorizados com variações, **para que** eu possa selecionar a versão correta no Simulador.

**Acceptance Criteria:**
- [ ] Conceito de "Variação" implementado no cadastro de produto (campo ou entidade relacionada)
- [ ] Trilhas: variação maiúsculas (infantil) e minúsculas
- [ ] Ponto e Ponto Digital: variação 4 correções e 8 correções
- [ ] Variações geram preços diferentes mas compartilham produto-pai
- [ ] Simulador usa variações para apresentar opções ao consultor

**Tasks:**

**T2.2.1 — Definir modelo de dados para variações**
- **Onde**: sessão de design com Oscar + equipe de Produtos
- **Opção A** (recomendada): criar entidade `ftd_variacao_produto` com campos:
  - `ftd_produto_pai` (Lookup → Product) — produto principal
  - `ftd_nome_variacao` (Single Line, 200) — ex: "4 correções", "Maiúsculas"
  - `ftd_codigo_variacao` (Single Line, 50) — código ISA/TOTVS
  - `ftd_produto_sku` (Lookup → Product) — referência ao SKU específico da variação (pode ter preço diferente)
  - `ftd_ativo` (Boolean)
- **Opção B**: usar campo `ftd_variacao` (OptionSet) diretamente no Product. Mais simples mas menos flexível.
- **Decisão depende de**: quantas variações existem no total e se novas variações surgem frequentemente

**T2.2.2 — Criar entidade e popular dados iniciais**
- **Onde**: make.powerapps.com → Customization Master → New Entity (se opção A) ou New Field (se opção B)
- **Dados iniciais** (via EMC ou script):
  - Trilhas → variação "Maiúsculas (Infantil)" + variação "Minúsculas"
  - Ponto e Ponto Digital → variação "4 correções" + variação "8 correções"
- **Formulário**: criar form simples para `ftd_variacao_produto` com subgrid no form do Product
- **Após**: `UnpackSolution` + `GenerateEarlyBound`

**T2.2.3 — Integrar variações na seleção de produtos do Simulador**
- **Onde**: Simulador (Power Pages) — tela de seleção de produtos
- **Lógica**: quando consultor seleciona produto-pai que possui variações, exibir dropdown/modal para selecionar qual variação. O preço carregado será o do SKU da variação selecionada.
- **Dependência**: precisa da interface do Simulador pronta (Épico 1, Etapa 6+)

**T2.2.4 — Atualizar integração ISA → TOTVS → CRM**
- **Onde**: processo de sincronização de produtos existente
- **Ação**: mapear campo de variação do ISA para a entidade/campo no CRM

**Estimativa**: M
**Prioridade**: Should
**Dependências**: Story 2.1

---

## Story 2.3: Adicionar Metadados Estruturantes ao Cadastro de Produto
**Como** Simulador Comercial, **quero** que produtos tenham metadados completos (Linha de Negócio, Componente Curricular, Coleção, Variação, Série), **para que** filtros inteligentes possam reduzir 1.283 produtos para os ~17 relevantes por contexto.

**Acceptance Criteria:**
- [ ] Campos obrigatórios no produto: Linha de Negócio, Coleção, Série(s), Tipo (Prateleira/Customizado/Personalizado)
- [ ] Campo "Componente Curricular" populado para didáticos
- [ ] Campo "É Customizado" (bool) para distinguir prateleira de customizado
- [ ] Metadados permitem filtro cruzado: por Linha de Negócio + Série + Tipo → resultado esperado ~17 para trilhas
- [ ] Integração TOTVS→CRM atualizada para trazer novos campos

**Tasks:**

**Estrutura de pastas (padrão BCA: Plugin → Tests):**
```
src/Backend/
├── Plugins/
│   └── Produto/
│       └── ValidarMetadadosProdutoPlugin.cs    ← Plugin (valida metadados obrigatórios)
└── Tests/
    └── Produto/
        └── ValidarMetadadosProdutoPluginTests.cs ← Testes do Plugin
```

**T2.3.1 — Adicionar campos no Product**
- **Onde**: make.powerapps.com → Customization Master → Product → Fields → New
- **Novos campos**:
  - `ftd_tipo_produto` (OptionSet: Prateleira=1, Customizado=2, Personalizado=3) — mapeado da classificação ISA
  - `ftd_componente_curricular` (OptionSet ou Lookup → entidade `ftd_componente_curricular`: Português, Matemática, Ciências, etc.)
  - `ftd_colecao` (Lookup → entidade `ftd_colecao` ou optionset) — coleção editorial (ex: Trilhas, Conquista, Panoramas)
  - `ftd_serie` (Multi-select OptionSet ou N:N com entidade Série) — séries atendidas pelo produto (pode ser mais de uma)
  - `ftd_e_customizado` (Boolean, default false) — flag rápida para filtros
- **Formulário**: adicionar seção "Classificação Comercial" no form de Product com os novos campos
- **Required Level**: `ftd_tipo_produto` e `ftd_colecao` como Application Required quando `statecode = Active`
- **Após**: `UnpackSolution` + `GenerateEarlyBound`

**T2.3.2 — Definir regras de preenchimento com equipe de Produtos**
- **Onde**: sessão com equipe ISA/Produtos
- **Deliverable**: planilha de mapeamento "Produto CRM → valores de cada metadado" baseada nos dados ISA
- **Regras**: quem define Componente Curricular (ISA), quem define Coleção (editorial), quando tipo_produto muda (nunca muda após cadastro?)

**T2.3.3 — Script de enriquecimento massivo dos 1.283 produtos**
- **Onde**: Console App C# ou Azure Function (batch job)
- **Input**: planilha de mapeamento da T2.3.2 (CSV com Product ID → valores de cada campo)
- **Lógica**: para cada produto, buscar no CRM, verificar se campos novos estão vazios, preencher conforme planilha
- **Validação**: gerar relatório pós-execução com contagem de produtos atualizados, erros, e produtos sem mapeamento

**T2.3.4 — Atualizar integração TOTVS → CRM para novos campos**
- **Onde**: Azure Function de integração ou job de sincronização existente
- **Mapeamento**: TOTVS campo X → CRM `ftd_tipo_produto`, TOTVS campo Y → CRM `ftd_componente_curricular`, etc.
- **Validação**: testar com 10 produtos novos criados no TOTVS, verificar sync no CRM

**T2.3.5 — Plugin de validação de metadados obrigatórios**
- **Onde**: `src/Backend/Plugins/Produto/ValidarMetadadosProdutoPlugin.cs`
- **Herança**: `Plugin<Product>`
- **Registration**: `[PluginRegistration(MessageName = "Update", Stage = PluginStage.PreOperation, FilteringAttributes = new[] { "statecode" })]`
- **Lógica**: quando `statecode` muda para Active, verificar que `ftd_tipo_produto`, `ftd_colecao` e Linha de Negócio estão preenchidos. Se não, bloquear ativação.

**Estimativa**: L
**Prioridade**: Should
**Dependências**: Story 2.1

---

## Story 2.4: Padronizar Cadastro de Séries
**Como** sistema CRM, **quero** que o campo Série tenha valores padronizados e sem duplicação, **para que** filtros de produto por série funcionem corretamente.

**Acceptance Criteria:**
- [ ] Séries padronizadas: EI (1-5 anos), EFAI (1º-5º), EFAF (6º-9º), EM (1ª-3ª)
- [ ] Valores duplicados limpos (campo preenchido por integração com duplicação)
- [ ] Novo campo `ftd_serie` (se necessário) ou cleanup do campo existente
- [ ] Integração TOTVS→CRM atualizada para utilizar valores padronizados

**Tasks:**

**T2.4.1 — Auditar campo Série atual**
- **Onde**: D365 CE → Advanced Find → Product → campo Série → agrupar & contar
- **Alternativa**: FetchXML com `aggregate` para listar valores distintos e contagem de cada
- **Deliverable**: relatório com: valor atual | quantidade de registros | valor padronizado proposto

**T2.4.2 — Criar OptionSet `ftd_serie_padronizada`**
- **Onde**: make.powerapps.com → Customization Master → Global OptionSets → New
- **Valores** (17 fixos):
  - 100000000-100000004 = EI 1 Ano, EI 2 Anos, EI 3 Anos, EI 4 Anos, EI 5 Anos
  - 100000005-100000009 = EFAI 1º Ano, EFAI 2º Ano, EFAI 3º Ano, EFAI 4º Ano, EFAI 5º Ano
  - 100000010-100000013 = EFAF 6º Ano, EFAF 7º Ano, EFAF 8º Ano, EFAF 9º Ano
  - 100000014-100000016 = EM 1ª Série, EM 2ª Série, EM 3ª Série
- **Uso**: Multi-Select OptionSet no Product (um produto pode atender múltiplas séries)
- **Após**: `UnpackSolution` + `GenerateEarlyBound`

**T2.4.3 — Script de migração de valores legados**
- **Onde**: Console App C# ou PowerShell
- **Lógica**: usar mapeamento da T2.4.1 (valor antigo → valor novo). Para cada produto, ler série antiga, converter, atualizar novo campo.
- **Cuidado**: se campo atual é texto livre, haverá muitas variações ("1o ano", "1° Ano", "Primeiro Ano") — normalizar com regex/dictionary

**T2.4.4 — Atualizar integração TOTVS → CRM**
- **Onde**: job de sync existente
- **Ação**: mapear campo Série do TOTVS para novo OptionSet, com regra de normalização

**T2.4.5 — Validar filtros no Simulador**
- **Onde**: Simulador (Power Pages) — tela de filtro de produtos
- **Teste**: selecionar "EFAI 3º Ano" no filtro → deve mostrar apenas produtos que atendem essa série. Confirmar com consultor que resultado faz sentido.

**Estimativa**: S
**Prioridade**: Should
**Dependências**: Nenhuma

---

# Épico 3: Consolidação de Tabelas de Preço
> Reduzir as 12+ tabelas de preço para no máximo 5, refletindo a realidade de preço único por produto com recortes por mercado (público/privado) e tipo de cliente (escola/livreiro).
>
> **Fonte**: Refinamento 16/Mar — seção "Tabelas de Preço"
> **Prioridade**: 🟡 COULD — Dependente da consultoria de pricing (resultado final de março/2026)

---

## Story 3.1: Racionalizar Tabelas de Preço (12+ → 5)
**Como** administrador comercial, **quero** que as tabelas de preço sejam reduzidas de 12+ para no máximo 5, organizadas por mercado/tipo de cliente, **para que** o consultor não precise saber "em qual catálogo está o produto".

**Acceptance Criteria:**
- [ ] Máximo 5 tabelas: Escola Privado, Livreiro Privado, Escola Público, E-commerce (Lumisfera Aberto), Customizados
- [ ] Cada produto ativo está em exatamente 1 tabela de preço (exceto customizados que podem estar em múltiplas)
- [ ] Preço é único por produto dentro de cada tabela (confirmado por Oscar: "única tabela de preços para o Brasil inteiro")
- [ ] Tabelas legadas são desativadas/arquivadas

**Tasks:**

**T3.1.1 — Auditar tabelas de preço atuais**
- **Onde**: D365 CE → Sales → Price Lists (ou Advanced Find → PriceLevel)
- **Levantar**: nome da tabela, quantidade de itens, data última modificação, quais produtos estão em cada uma, sobreposições (mesmo produto em 2+ tabelas com preços diferentes)
- **Deliverable**: planilha comparativa com recomendação de mapeamento para as 5 novas

**T3.1.2 — Definir mapeamento com equipe comercial**
- **Onde**: sessão com Oscar + consultoria de pricing
- **Resultado**: documento com: tabela legada A → nova tabela X, tabela legada B → nova tabela X, etc.
- **Resolver**: o que fazer com produtos que estão em múltiplas tabelas legadas com preços diferentes (qual preço prevalece?)

**T3.1.3 — Criar 5 novas Price Lists no D365 CE**
- **Onde**: D365 CE → Sales → Price Lists → New
- **Nomes**:
  - "FTD - Escola Privado" (Currency: BRL, Default = para propostas de escolas privadas)
  - "FTD - Livreiro Privado" (Currency: BRL)
  - "FTD - Escola Público" (Currency: BRL)
  - "FTD - Lumisfera Aberto" (E-commerce, Currency: BRL)
  - "FTD - Customizados" (preços por demanda)
- **Cada Price List**: configurar Default Unit, Begin/End dates (se safra-based)

**T3.1.4 — Migrar Price List Items para novas tabelas**
- **Onde**: script C# Console App ou Azure Function
- **Lógica**: para cada Price List Item das tabelas legadas, criar novo Item na tabela nova correspondente (conforme mapeamento T3.1.2). Se conflito de preço (mesmo produto em 2 tabelas legadas), usar regra de prevalência definida com comercial.
- **Validação**: contagem total de Price List Items antes e depois deve bater

**T3.1.5 — Desativar tabelas legadas**
- **Onde**: D365 CE → cada Price List legada → Deactivate
- **CUIDADO**: NÃO deletar — propostas históricas referenciam essas tabelas. Desativar impede uso em novas propostas mas mantém referência.
- **Timing**: desativar somente após validação completa das novas tabelas em DEV e OAT

**T3.1.6 — Atualizar integração TOTVS → CRM**
- **Onde**: job de sync de preços existente
- **Ação**: alterar target para as 5 novas tabelas em vez das 12+ tabelas antigas
- **Regra de roteamento**: TOTVS deve informar qual tabela o produto pertence (por mercado/tipo) ou usar regra de mapeamento no CRM

**Estimativa**: L
**Prioridade**: Could
**Dependências**: Resultado da consultoria de pricing (final Mar/2026)

---

## Story 3.2: Implementar Recorte Automático de Tabela de Preço por Contexto da Proposta
**Como** Simulador Comercial, **quero** que a tabela de preço aplicável seja determinada automaticamente com base no mercado (público/privado) e tipo de cliente (escola/livreiro), **para que** o consultor veja apenas produtos elegíveis sem precisar selecionar tabela manualmente.

**Acceptance Criteria:**
- [ ] Ao criar proposta, sistema determina tabela de preço com base em: mercado da conta (público/privado) + tipo de conta (escola/livreiro)
- [ ] Consultor não precisa "escolher" tabela de preço — é automático
- [ ] Apenas produtos da tabela selecionada são exibidos para adição na proposta
- [ ] Se tipo de conta não está definido, sistema bloqueia criação de proposta com mensagem orientativa

**Tasks:**

**Estrutura de pastas (padrão BCA: Plugin → Service → Tests):**
```
src/Backend/
├── Plugins/
│   └── Proposta/
│       └── AutoSelectPriceListPlugin.cs        ← Plugin (auto-seleciona tabela)
├── Services/
│   └── Proposta/
│       ├── ISelecionarTabelaPrecoService.cs     ← Interface
│       └── SelecionarTabelaPrecoService.cs      ← SERVICE (mapeamento conta→tabela)
└── Tests/
    └── Proposta/
        └── SelecionarTabelaPrecoServiceTests.cs ← Testes do Service
```

**T3.2.1 — Criar Service de mapeamento Conta → Price List**
- **Onde**: `src/Backend/Services/Proposta/SelecionarTabelaPrecoService.cs`
- **Interface**: `ISelecionarTabelaPrecoService`
- **Método**: `DeterminarTabela(Account conta) : EntityReference` (retorna referência à Price List)
- **Lógica de mapeamento**:
  ```
  Conta.mercado = Privado + Conta.tipo = Escola → "FTD - Escola Privado"
  Conta.mercado = Privado + Conta.tipo = Livreiro → "FTD - Livreiro Privado"
  Conta.mercado = Público + Conta.tipo = Escola → "FTD - Escola Público"
  Conta.tipo = E-commerce/Lumisfera → "FTD - Lumisfera Aberto"
  Conta.tipo = Customizado → "FTD - Customizados"
  ```
- **IDs das Price Lists**: ler por nome via `RetrieveMultiple` e cachear (ou armazenar IDs em configuração EMC)

**T3.2.2 — Criar plugin `AutoSelectPriceListPlugin`**
- **Onde**: `src/Backend/Plugins/Proposta/AutoSelectPriceListPlugin.cs`
- **Herança**: `Plugin<ftd_proposta>`
- **Registration**: `[PluginRegistration(MessageName = "Create", Stage = PluginStage.PreOperation)]`
- **Lógica**:
  1. Ler conta da proposta via `ftd_conta` (lookup)
  2. Chamar `ISelecionarTabelaPrecoService.DeterminarTabela(conta)`
  3. Se retorna null (tipo de conta não definido) → `throw new InvalidPluginExecutionException("Não é possível criar proposta: tipo de conta não definido. Atualize o cadastro da conta.")`
  4. Se retorna Price List → setar `pricelevelid` da proposta

**T3.2.3 — Filtrar produtos no Simulador por Price List**
- **Onde**: Simulador (Power Pages) — tela de adição de produtos
- **Lógica**: ao listar produtos disponíveis, filtrar por `PriceListItemId` da Price List da proposta. Usar FetchXML ou OData com join entre Product e PriceListItem.
- **UX**: consultor não vê o filtro — simplesmente os produtos exibidos já são os da tabela correta

**T3.2.4 — Testes unitários**
- **Onde**: `src/Backend/Tests/Proposta/SelecionarTabelaPrecoServiceTests.cs`
- **Cenários**:
  - `Determinar_Should_ReturnEscolaPrivado_When_MercadoPrivadoETipoEscola`
  - `Determinar_Should_ReturnLivreiroPrivado_When_MercadoPrivadoETipoLivreiro`
  - `Determinar_Should_ReturnNull_When_TipoContaIndefinido`
  - `Plugin_Should_ThrowException_When_TabelaNaoEncontrada`

**Estimativa**: M
**Prioridade**: Could
**Dependências**: Story 3.1, Story 4.2

---

# Épico 4: Revisão de Categorização de Contas
> Reorganizar a categorização de contas (tipo de instituição) para eliminar mistura de conceitos (categorização vs. Key Accounts vs. redes) e habilitar relatórios precisos.
>
> **Fonte**: Refinamento 16/Mar — seção "Categorização de Contas"
> **Prioridade**: 🟠 SHOULD — Em Discovery, Oscar recebeu primeira versão

---

## Story 4.1: Implementar Nova Categorização de Contas em 3 Níveis
**Como** equipe comercial, **quero** que as contas sejam categorizadas em 3 níveis hierárquicos (Nível 1, 2, 3) substituindo o campo "Tipo de Instituição" atual, **para que** relatórios comerciais reflitam a realidade sem misturar conceitos.

**Acceptance Criteria:**
- [ ] 3 campos novos na Conta: Nível 1 (ex: Público/Privado), Nível 2 (ex: Confessional/Laica/Militar), Nível 3 (detalhe)
- [ ] Relação hierárquica: Nível 2 depende de Nível 1, Nível 3 depende de Nível 2
- [ ] Key Accounts (Marista, Rede S, Inspira) categorizadas como atributo separado, não misturadas com a classificação
- [ ] Campo "Tipo de Instituição" antigo mantido como referência mas hidden nos formulários
- [ ] Migração das 101.000 contas (recorte 2025-2026 como prioridade)

**Tasks:**

**T4.1.1 — Criar campos de categorização na Conta**
- **Onde**: make.powerapps.com → Customization Master → Account → Fields → New
- **Campos**:
  - `ftd_categoria_nivel1` (OptionSet local):
    - 1 = Público
    - 2 = Privado
  - `ftd_categoria_nivel2` (OptionSet local — filtrado por nível 1):
    - Para Público: Federal, Estadual, Municipal
    - Para Privado: Confessional, Laica, Militar, Filantrópica
  - `ftd_categoria_nivel3` (OptionSet local — filtrado por nível 2):
    - Para Confessional: Católica, Evangélica, Judaica, Outras
    - Para Laica: Rede (grupo), Independente
    - (demais combinações a definir com Oscar)
  - `ftd_key_account` (Lookup → nova entidade `ftd_key_account` ou OptionSet: Marista, Rede S, Inspira, PNLD, Nenhuma)
- **Formulário**: adicionar seção "Categorização" com os 4 campos, substituindo o campo antigo "Tipo de Instituição" (que será hidden)
- **Após**: `UnpackSolution` + `GenerateEarlyBound`

**T4.1.2 — Implementar filtro hierárquico (cascading)**
- **Onde**: make.powerapps.com → Customization Master → Account → Business Rules → New
- **Regra 1**: quando Nível 1 muda → limpar Nível 2 e Nível 3, filtrar opções de Nível 2
- **Regra 2**: quando Nível 2 muda → limpar Nível 3, filtrar opções de Nível 3
- **Implementação**: Business Rules do D365 suportam `Set Field Value` e `Show/Hide` mas NÃO filtro de OptionSet nativo. Duas opções:
  - **Opção A** (cascading OptionSets): usar TypeScript form script com `formContext.getControl("ftd_categoria_nivel2").removeOption()` + `addOption()` conforme seleção do Nível 1
  - **Opção B** (lookups com entidades): criar entidades `ftd_categoria_n1`, `ftd_categoria_n2`, `ftd_categoria_n3` com lookups em cascata e filtro de lookup (Custom Lookup View). Mais flexível para futuras mudanças.
- **Recomendação**: Opção A se a lista é estável; Opção B se precisa de frequente manutenção

**T4.1.3 — TypeScript para filtro cascading (se Opção A)**
- **Onde**: `src/Frontend/Forms/Conta/CategorizacaoContaContract.ts` + `CategorizacaoContaController.ts`
- **Padrão BCA**: Contract/Controller
- **onLoad**: carregar valores atuais, aplicar filtros iniciais
- **onNivel1Change**: limpar Nível 2 e 3, carregar opções de Nível 2 compatíveis
- **onNivel2Change**: limpar Nível 3, carregar opções de Nível 3 compatíveis
- **Mapeamento**: hardcoded no script OU ler de configuração (entidade de config EMC)

**T4.1.4 — Definir valores finais com equipe comercial**
- **Onde**: sessão com Oscar
- **Status**: Oscar recebeu primeira versão na reunião e ficou de revisar
- **Bloqueio**: valores definitivos de Nível 2 e Nível 3 dependem de validação de Oscar

**T4.1.5 — Script de migração massiva**
- **Onde**: Console App C# para batch update
- **Escopo**: contas ativas em 2025-2026 (recorte de ~20.000 das 101.000)
- **Lógica**: ler campo "Tipo de Instituição" atual → mapear para Nível 1, 2, 3 conforme tabela de de-para
- **De-para**: planilha com: valor antigo → N1 + N2 + N3 + Key Account. Ex: "Escola Marista" → Privado + Confessional + Católica + KA: Marista
- **Execução**: DEV → OAT → PROD (com GMUD). ~400 records/min no bulk update.

**Estimativa**: L
**Prioridade**: Should
**Dependências**: Resultado da categorização em Discovery (Oscar)

---

## Story 4.2: Separar Conceito de Mantenedora vs. Parceiro Comercial
**Como** equipe comercial/CRM, **quero** que o campo "Mantenedora" não seja mais reutilizado para Parceiro Comercial e outros fins, **para que** cada relação entre contas tenha seu campo específico.

**Acceptance Criteria:**
- [ ] Campo `ftd_mantenedora` (lookup Account) dedicado exclusivamente à mantenedora jurídica
- [ ] Novo campo `ftd_parceiro_comercial` (lookup Account) para vincular livreiro/parceiro à conta escola
- [ ] Relacionamento N:N entre escola e parceiro comercial (livreiro pode estar em múltiplas escolas)
- [ ] Dados migrados: registros com "Parceiro Comercial" no campo mantenedora movidos para novo campo
- [ ] Campo mantenedora antigo limpo dos registros que não eram mantenedora real

**Tasks:**

**T4.2.1 — Criar entidade associativa e campos**
- **Onde**: make.powerapps.com → Customization Master
- **Entidade N:N**: criar relacionamento Many-to-Many entre Account e Account via entidade associativa `ftd_escola_parceiro`:
  - `ftd_escola` (Lookup → Account)
  - `ftd_parceiro` (Lookup → Account)
  - `ftd_principal` (Boolean) — indica se é o parceiro principal
  - `ftd_vigencia_inicio` (Date), `ftd_vigencia_fim` (Date) — se a relação é por safra
- **Campo**: `ftd_parceiro_comercial_principal` (Lookup → Account) no Account — atalho para o parceiro principal (preenchido automaticamente pelo mais recente ativo)
- **Nota**: D365 NÃO suporta self-referencing N:N relationship nativo. Usar entidade intermediária custom `ftd_escola_parceiro` é o approach correto.
- **Após**: `UnpackSolution` + `GenerateEarlyBound`

**T4.2.2 — Auditar campo Mantenedora atual**
- **Onde**: Advanced Find → Account → campo Mantenedora (lookup) → exportar para Excel
- **Classificar**: para cada registro com Mantenedora preenchida, verificar se o valor é realmente uma mantenedora jurídica ou se é parceiro/livreiro
- **Critério**: se a conta referenciada no campo Mantenedora é do tipo "Livreiro" ou "Distribuidor" → é parceiro, não mantenedora
- **Deliverable**: planilha com: Account ID | Nome | Mantenedora atual | Classificação (Real/Parceiro)

**T4.2.3 — Script de migração**
- **Onde**: Console App C#
- **Lógica**: para cada registro classificado como "Parceiro" na T4.2.2:
  1. Criar registro em `ftd_escola_parceiro` (escola = conta, parceiro = valor do campo mantenedora)
  2. Limpar campo `ftd_mantenedora` da conta (setar null)
  3. Setar `ftd_parceiro_comercial_principal` = parceiro
- **Para registros classificados como "Real"**: manter inalterado

**T4.2.4 — Atualizar formulários**
- **Onde**: make.powerapps.com → Account Forms
- **Layout**: manter campo Mantenedora em seção "Vínculo Jurídico" (para instituições com mantenedora real). Adicionar seção "Parceiros Comerciais" com subgrid de `ftd_escola_parceiro` + campo `ftd_parceiro_comercial_principal`
- **Validação**: Business Rule — se tipo conta = Livreiro, seção Parceiros não funcional (livreiro não tem parceiro, ele É o parceiro)

**Estimativa**: M
**Prioridade**: Should
**Dependências**: Story 4.1

---

## Story 4.3: Cadastrar Livreiros como Contas no CRM
**Como** equipe comercial, **quero** que livreiros sejam cadastrados como contas no CRM (hoje estão apenas no sistema "Pede Livreiro"), **para que** propostas para livreiros sejam geridas no CRM e o sistema "Pede Livreiro" seja eliminado.

**Acceptance Criteria:**
- [ ] Tipo de conta "Livreiro" disponível no CRM para criação/importação
- [ ] Livreiros podem ter propostas próprias (linhas de negócio restritas: sem Sistema de Ensino)
- [ ] Livreiros podem ser vinculados como parceiro comercial de escolas (N:N)
- [ ] Carteirização: livreiro vinculado a consultor proprietário
- [ ] Importação inicial de livreiros do "Pede Livreiro" → CRM

**Tasks:**

**Estrutura de pastas (padrão BCA: Plugin → Tests):**
```
src/Backend/
├── Plugins/
│   └── Proposta/
│       └── ValidarProdutoLivreiroPlugin.cs     ← Plugin (bloqueia Sistema de Ensino p/ livreiro)
└── Tests/
    └── Proposta/
        └── ValidarProdutoLivreiroPluginTests.cs ← Testes do Plugin
```

**T4.3.1 — Adicionar tipo "Livreiro" no campo de tipo de conta**
- **Onde**: make.powerapps.com → Customization Master → Account → campo `ftd_tipo_conta` (ou campo existente)
- **Ação**: adicionar valor "Livreiro" ao OptionSet (se não existir). Verificar se já existe campo tipo de conta — se não, criar: `ftd_tipo_conta` (OptionSet: Escola=1, Livreiro=2, Mantenedora=3, Distribuidora=4)
- **Formulário**: criar form variant ou seção condicional para Livreiro com campos específicos (CNPJ da livraria, região de atuação, etc.)

**T4.3.2 — Criar Business Rule para restrição de produtos**
- **Onde**: make.powerapps.com → Customization Master → ftd_proposta → Business Rules → New
- **Regra**: se a conta da proposta é do tipo "Livreiro", bloquear adição de produtos da Linha de Negócio "Sistema de Ensino" (livreiros não vendem Sistema de Ensino — da reunião com Oscar)
- **Implementação**: Business Rule no form da proposta NÃO consegue validar cross-entity (produto ↔ proposta ↔ conta). Implementar via **Plugin PreOperation no Create de `ftd_produto_proposta`**:
  - `src/Backend/Plugins/Proposta/ValidarProdutoLivreiroPlugin.cs`
  - Ler conta da proposta → verificar tipo → se Livreiro e produto é Sistema de Ensino → bloquear

**T4.3.3 — Processo de importação do "Pede Livreiro"**
- **Onde**: ETL via Console App ou Azure Function
- **Fonte**: sistema "Pede Livreiro" (verificar formato de export: CSV, API, banco direto?)
- **Mapeamento**: campos do Pede Livreiro → campos do Account no CRM + tipo = Livreiro
- **Deduplicação**: verificar se algum livreiro já existe no CRM (por CNPJ) — se sim, atualizar em vez de criar
- **Volume**: levantar com equipe quantos livreiros existem no sistema legado

**T4.3.4 — Configurar Security para carteirização**
- **Onde**: Security Roles + field-level security
- **Regra**: consultor vê apenas contas (incluindo livreiros) do seu território/carteira. Usar BU scope ou territory-based access.
- **Owner**: setar `ownerid` do Account (livreiro) como o consultor responsável

**T4.3.5 — Higienizar CNPJs de livraria usados para faturamento**
- **Onde**: consulta no CRM para identificar contas (escolas) que usam CNPJ de livraria no campo de faturamento
- **Problema da reunião**: algumas escolas têm CNPJ da livraria no campo de faturamento porque o faturamento é via livreiro. Isso precisa ser corrigido para: conta escola tem CNPJ da escola, parceiro comercial (livreiro) tem CNPJ da livraria.
- **Script**: identificar por range de CNPJ ou por tipo de atividade econômica (CNAE de livraria vs. escola)

**Estimativa**: L
**Prioridade**: Should
**Dependências**: Story 4.1, Story 4.2

---

# Épico 5: Higienização de Base e Criação Massiva de Oportunidades
> Higienizar base de 101.000 contas (recorte 2025-2026) e criar oportunidades massivamente para vincular às propostas existentes, habilitando relatórios de gestão.
>
> **Fonte**: Refinamento 16/Mar — seções "Contas" e "Oportunidades e Relatórios"
> **Prioridade**: 🟠 SHOULD — Em planejamento (reunião comercial amanhã 17/Mar)

---

## Story 5.1: Diagnosticar Qualidade da Base de Contas (Recorte 2025-2026)
**Como** PO, **quero** um diagnóstico quantitativo da qualidade da base de contas com recorte de 2 anos (2025-2026), **para que** possamos dimensionar o esforço de higienização e definir regras de correção.

**Acceptance Criteria:**
- [ ] Exportação de contas ativas em 2025 e 2026 do CRM e do TOTVS
- [ ] Relatório de divergências: campos com valores diferentes entre CRM e TOTVS
- [ ] Identificação de CNPJs duplicados no CRM
- [ ] Percentual de contas sincronizadas vs. divergentes
- [ ] Relatório entregue à equipe comercial para definição de regras de correção

**Tasks:**

**T5.1.1 — Extrair contas ativas 2025-2026 do CRM**
- **Onde**: D365 CE → Advanced Find → Account ou via FetchXML avançado
- **Critério de "ativa 2025-2026"**: contas que possuem pelo menos 1 Oportunidade ou Proposta com `ftd_ano_safra IN (2025, 2026)` OU `statecode = Active` com `modifiedon >= 2024-01-01`
- **FetchXML**: join Account → Opportunity (filtro ano-safra) + Account → ftd_proposta (filtro ano-safra)
- **Export**: CSV com todos os campos de conta (CNPJ, Nome, Endereço, Cidade, Estado, Tipo, Mantenedora, Owner, etc.)
- **Volume esperado**: ~20.000 contas do universo de 101.000
- **Ferramenta**: usar XrmToolBox → FetchXML Builder para construir a query, depois exportar via Data Export ou script PowerShell

**T5.1.2 — Extrair contas correspondentes do TOTVS**
- **Onde**: coordenar com Mônica (equipe TOTVS) para export do Datasul
- **Critério**: mesmo recorte temporal (2025-2026), por Código do Cliente ou CNPJ
- **Formato**: CSV ou Excel, com campos: Código TOTVS, CNPJ, Razão Social, Endereço, campos adicionais que existirem
- **Nota**: o cadastro do TOTVS é considerado "master" para dados fiscais (CNPJ, Razão Social, Endereço)

**T5.1.3 — Script de comparação campo a campo**
- **Onde**: script Python (pandas) ou C# Console App
- **Chave de join**: CNPJ (principal) ou Código TOTVS ↔ CRM
- **Comparação**: para cada conta, comparar: Nome, Endereço (logradouro, número, bairro, cidade, estado, CEP), CNPJ, Status
- **Output**: relatório com: contas match (iguais), contas divergentes (por campo), contas orphan CRM (não existe no TOTVS), contas orphan TOTVS (não existe no CRM)
- **Métricas**: % sincronizado, % divergente, top 5 campos com mais divergências

**T5.1.4 — Identificar CNPJs duplicados no CRM**
- **Onde**: FetchXML com aggregate ou script de análise
- **Query**: agrupar Account por CNPJ → filtrar onde COUNT > 1
- **Output**: lista de CNPJs duplicados com: CNPJ, quantidade de registros, IDs, quem é owner, data de criação (para decidir qual manter)

**T5.1.5 — Gerar relatório consolidado**
- **Onde**: Power BI dashboard ou Excel avançado (pivot + charts)
- **Conteúdo**: total de contas analisadas, % divergentes, % duplicadas, top campos com problema, recomendações de correção
- **Destinatário**: Oscar + equipe comercial para definir regras de prevalência

**Estimativa**: M
**Prioridade**: Should
**Dependências**: Nenhuma (Julio já começou)

---

## Story 5.2: Criar Oportunidades Massivamente para 2025-2026
**Como** equipe de gestão comercial, **quero** que todas as propostas ativas de 2025-2026 estejam vinculadas a Oportunidades correspondentes, **para que** relatórios de gestão de pipeline funcionem corretamente.

**Acceptance Criteria:**
- [ ] Regra: 1 Oportunidade por Conta por Ano-Safra (não pode ter mais de 1 aberta por ano-safra)
- [ ] Oportunidades criadas massivamente para propostas 2025-2026 que estão "soltas"
- [ ] Vínculo Oportunidade ↔ Proposta estabelecido corretamente
- [ ] Campos da Oportunidade preenchidos automaticamente a partir dos dados da Proposta e Conta
- [ ] Campos duplicados entre Oportunidade e Proposta identificados e sinalizados para deprecação

**Tasks:**

**Estrutura de pastas (padrão BCA: Plugin → Tests):**
```
src/Backend/
├── Plugins/
│   └── Oportunidade/
│       └── ValidarOportunidadeUnicaPlugin.cs   ← Plugin (impede duplicação por conta/safra)
└── Tests/
    └── Oportunidade/
        └── ValidarOportunidadeUnicaPluginTests.cs ← Testes do Plugin
```

**T5.2.1 — Auditoria: quantas propostas estão sem oportunidade**
- **Onde**: Advanced Find → ftd_proposta → filtro `ftd_oportunidade IS NULL` + `ftd_ano_safra IN (2025, 2026)`
- **Output**: contagem + lista de propostas órfãs com: ID, Conta, Ano-Safra, Valor, Status
- **Expectativa**: na reunião indicaram que muitas propostas estão sem oportunidade vinculada

**T5.2.2 — Criar script de criação massiva de Oportunidades**
- **Onde**: Console App C# (melhor que Azure Function para job one-shot)
- **Lógica**:
  1. Ler todas propostas de 2025-2026 sem oportunidade vinculada
  2. Agrupar por Conta + Ano-Safra → cada grupo = 1 Oportunidade
  3. Para cada grupo:
    - Verificar se já existe Oportunidade para mesma Conta + Ano-Safra no CRM
    - Se NÃO: criar Opportunity com: `name = "Oportunidade {Conta.Nome} - Safra {ano}"`, `customerid = Conta`, `ftd_ano_safra = ano`, `estimatedclosedate = final da safra`, `estimatedvalue = soma das propostas do grupo`
    - Vincular todas propostas do grupo à Oportunidade criada (`ftd_oportunidade = Oportunidade.Id`)
    - Se SIM (já existe): apenas vincular propostas órfãs à Oportunidade existente
- **Batch size**: 50 records por batch (usar `ExecuteMultipleRequest` para performance)
- **Execução**: DEV primeiro, validar contagens, depois OAT, depois PROD

**T5.2.3 — Business Rule: impedir duplicação de Oportunidade por Conta/Ano-Safra**
- **Onde**: plugin (Business Rule não faz cross-record validation)
- **Plugin**: `src/Backend/Plugins/Oportunidade/ValidarOportunidadeUnicaPlugin.cs`
- **Registration**: `[PluginRegistration(MessageName = "Create", Stage = PluginStage.PreValidation)]` sobre Opportunity
- **Lógica**: verificar se já existe Opportunity ativa com mesma `customerid` + `ftd_ano_safra`. Se sim, bloquear com mensagem: "Já existe oportunidade para esta conta na safra {ano}."

**T5.2.4 — Documentar campos duplicados**
- **Onde**: `docs/discovery/campos-duplicados-oportunidade-proposta.md`
- **Conteúdo**: lista de campos que existem TANTO na Opportunity quanto na ftd_proposta (identificados por Oscar na reunião como "vermelhos")
- **Recomendação**: para cada campo duplicado, indicar fonte de verdade (proposta ou oportunidade) e plano de deprecação

**T5.2.5 — Validar resultado**
- **Onde**: Advanced Find → ftd_proposta → filtro `ftd_oportunidade IS NULL` + `ftd_ano_safra IN (2025, 2026)` → resultado deve ser 0
- **Dashboard**: criar view "Propostas sem Oportunidade" para monitoramento contínuo

**Estimativa**: L
**Prioridade**: Should
**Dependências**: Story 5.1

---

## Story 5.3: Simplificar Campos da Oportunidade (Eliminar Duplicação com Proposta)
**Como** consultor/gestor, **quero** que os campos duplicados entre Oportunidade e Proposta sejam eliminados da Oportunidade, **para que** cada informação tenha uma única fonte de verdade.

**Acceptance Criteria:**
- [ ] Campos marcados como "vermelho" por Oscar são ocultados do formulário de Oportunidade
- [ ] Oportunidade mantém apenas: Conta, Ano-Safra, Estágio, Receita Estimada, Razão de Perda
- [ ] Informações detalhadas ficam exclusivamente na Proposta
- [ ] Views e dashboards atualizados para buscar dados da Proposta (não da Oportunidade)

**Tasks:**

**T5.3.1 — Documentar lista de campos duplicados**
- **Onde**: complementar documento da T5.2.4
- **Fonte**: lista de Oscar ("vermelhos") + auditoria técnica (comparar fields da Opportunity e ftd_proposta side-by-side)
- **Classificação**: para cada campo duplicado → Ação: "Ocultar na Oportunidade" | "Manter em ambos (necessário)" | "Calcular na Oportunidade via rollup da Proposta"

**T5.3.2 — Ocultar campos no formulário de Oportunidade**
- **Onde**: make.powerapps.com → Customization Master → Opportunity → Forms → Main Form
- **Ação**: para cada campo marcado "Ocultar": remover do form layout (não deletar o campo da entidade)
- **Manter visíveis**: `customerid`, `ftd_ano_safra`, `opportunityratingcode` (estimativa), `estimatedvalue`, `closeprobability`, `ftd_razao_perda`, `statecode/statuscode`
- **Seção nova**: adicionar seção "Propostas" com subgrid de `ftd_proposta` vinculada à oportunidade (para acessar detalhes via proposta)

**T5.3.3 — Atualizar views padrão de Oportunidade**
- **Onde**: make.powerapps.com → Customization Master → Opportunity → Views
- **Views a atualizar**: "Active Opportunities", "My Open Opportunities", "Oportunidades por Safra" (custom)
- **Modificação**: remover colunas de campos ocultados, adicionar colunas calculadas/rollup da proposta se necessário (ex: `ftd_valor_total_propostas` como rollup)

**T5.3.4 — Atualizar dashboards**
- **Onde**: make.powerapps.com → Dashboards
- **Ação**: identificar dashboards que usam campos de Oportunidade que foram ocultados → atualizar para usar dados da Proposta (via related entity charts ou views com linked entity)
- **Cuidado**: D365 charts nativos não suportam cross-entity facilmente. Se necessário, usar Power BI embedded dashboard ou fetch-based reports

**T5.3.5 — Validar com consultores**
- **Onde**: sessão de validação com 2-3 consultores
- **Critério**: confirmar que nenhuma informação crítica foi perdida ao ocultar campos da Oportunidade
- **Ajuste**: se algum campo oculto é necessário na Oportunidade, re-adicionar ao form

**Estimativa**: M
**Prioridade**: Should
**Dependências**: Story 5.2

---

# Épico 6: Relatórios de Gestão — Onda 1 Pós-MVP
> Entregar para a liderança comercial, pela primeira vez na história da FTD, relatórios de gestão de CRM confiáveis para os anos-safra 2025-2026. Este é o objetivo macro declarado da Onda 1.
>
> **Fonte**: Refinamento 16/Mar — declaração de Oscar: "entregar pela primeira vez na história da FTD... relatórios de gestão para 2025-2026"
> **Prioridade**: 🟠 SHOULD — Depende dos Épicos 2-5

---

## Story 6.1: Relatório de Funil de Oportunidades
**Como** gestor comercial, **quero** visualizar o funil de oportunidades segmentado por tipo (Novo Cliente, Ampliação, Renovação Estratégica, Renovação Manutenção), **para que** eu possa acompanhar o pipeline de vendas e prever receita.

**Acceptance Criteria:**
- [ ] Dashboard D365 com funil baseado no Business Flow de Oportunidades
- [ ] Filtros: Ano-Safra, Filial, Consultor, Linha de Negócio, Tipo de Oportunidade
- [ ] Métricas: quantidade de oportunidades e valor total por estágio
- [ ] Drill-down para lista de oportunidades de cada estágio

**Tasks:**

**T6.1.1 — Criar System Dashboard "Funil Comercial"**
- **Onde**: make.powerapps.com → App "Spartan" → Dashboards → New System Dashboard
- **Layout**: 3-column layout
  - **Coluna esquerda**: gráfico de funil (Chart) — Opportunity por `ftd_estagio`/BPF stage, com valor `estimatedvalue` como métrica
  - **Coluna central**: lista (View) — "Oportunidades Abertas" com colunas: Conta, Consultor, Ano-Safra, Valor, Estágio
  - **Coluna direita**: gráfico de pizza — Oportunidades por Tipo (Novo/Ampliação/Renovação Estratégica/Renovação Manutenção)
- **Nota**: gráficos nativos do D365 podem ser limitados para funnel real. Se insuficiente, usar **Power BI Embedded Dashboard** (mais flexível)

**T6.1.2 — Configurar gráfico de funil**
- **Onde**: make.powerapps.com → Opportunity → Charts → New
- **Tipo**: Funnel Chart (disponível no D365 chart editor)
- **Eixo X**: `ftd_estagio` ou campo de estágio do BPF
- **Valor**: Count de oportunidades ou SUM de `estimatedvalue`
- **Filtro par defaul**: `ftd_ano_safra = {ano atual}`, `statecode = Open`

**T6.1.3 — Adicionar filtros interativos**
- **Onde**: dashboard criado em T6.1.1
- **Filtros**: D365 dashboards suportam Interactive Filters na parte superior
  - Ano-Safra (OptionSet ou campo)
  - Filial/BU (BusinessUnit ou Territory)
  - Consultor (Owner/SystemUser)
  - Linha de Negócio (da proposta — requer view com linked entity)
- **Limitação**: dashboards nativos D365 não suportam todos os filtros cross-entity. Se necessário, usar **Interactive Dashboard** (tipo Tier 1) ou Power BI

**T6.1.4 — Criar views de drill-down**
- **Onde**: make.powerapps.com → Opportunity → Views → New
- **Views** (uma por estágio):
  - "Oportunidades - Prospecção" (filtro estágio = Prospecção)
  - "Oportunidades - Em Negociação" (filtro estágio = Em Negociação)
  - "Oportunidades - Proposta Enviada" (filtro estágio = Proposta Enviada)
  - etc.
- **Colunas**: Conta, Consultor, Valor Estimado, Ano-Safra, Último Update, Próximo Passo

**T6.1.5 — Validar com stakeholders**
- **Onde**: sessão com Oscar + Majolie (gestão comercial)
- **Critério**: métricas batem com expectativa? Drill-down está útil? Precisa de mais filtros?

**Estimativa**: M
**Prioridade**: Should
**Dependências**: Story 5.2, Story 5.3

---

## Story 6.2: Relatório de Desempenho por Linha de Negócio
**Como** gestor comercial e de produtos, **quero** visualizar o histórico de vendas por linha de negócio com indicadores de crescimento/declínio, **para que** decisões estratégicas de portfolio sejam baseadas em dados.

**Acceptance Criteria:**
- [ ] Dashboard com vendas por linha de negócio (valor e quantidade)
- [ ] Comparativo ano-safra anterior vs. atual
- [ ] Indicadores de crescimento/declínio por linha
- [ ] Filtros: Ano-Safra, Filial, Tipo de Cliente

**Tasks:**

**T6.2.1 — Criar views agregadas de Propostas por Linha de Negócio**
- **Onde**: make.powerapps.com → ftd_proposta → Views → New
- **View "Propostas por Linha de Negócio"**: colunas: Linha de Negócio, SUM Receita Bruta, COUNT propostas, Ano-Safra
- **Nota**: views D365 não suportam aggregate/GROUP BY nativamente. Para dados agregados, duas opções:
  - **Opção A**: criar charts sobre views (chart agrupa automaticamente)
  - **Opção B**: Power BI report com query direta ao Dataverse (recomendado para comparativos)

**T6.2.2 — Criar System Dashboard "Performance por Linha de Negócio"**
- **Onde**: make.powerapps.com → Dashboards → New
- **Layout**:
  - **Chart 1**: Bar chart horizontal — Receita por Linha de Negócio (eixo Y = linha, eixo X = valor)
  - **Chart 2**: Stacked bar — comparativo safra anterior vs. atual (2 séries por linha)
  - **View**: tabela com: Linha de Negócio, Receita Safra Anterior, Receita Safra Atual, Δ%, Trend (↑/↓)
- **Se Power BI**: criar report com medidas DAX: `Crescimento% = (Atual - Anterior) / Anterior * 100`

**T6.2.3 — Implementar gráficos comparativos**
- **Onde**: chart editor do D365 ou Power BI Desktop
- **Desafio**: comparativo entre safras requer 2 queries com filtro diferente no mesmo chart. D365 charts nativos NÃO suportam multi-query. Power BI é a melhor opção para isso.
- **Alternativa D365**: criar 2 charts side-by-side (safra 2025 e safra 2026)

**T6.2.4 — Validar com stakeholders**
- **Onde**: sessão com equipe de Produtos + Comercial
- **Critério**: dados fazem sentido? Linhas de negócio estão corretas (depende da Story 2.1)?

**Estimativa**: M
**Prioridade**: Should
**Dependências**: Story 2.1 (taxonomia correta), Story 5.2

---

## Story 6.3: Relatório de Performance de Aprovação
**Como** gestor comercial, **quero** acompanhar métricas de aprovação (tempo médio por alçada, taxa de recusa por motivo, lead time total), **para que** gargalos no fluxo sejam identificados e resolvidos.

**Acceptance Criteria:**
- [ ] Dashboard com: tempo médio de aprovação total, tempo médio por alçada, taxa de recusa por motivo
- [ ] Ranking de propostas paradas por mais tempo
- [ ] Filtros: Período, Filial, Aprovador, Nível de Alçada
- [ ] Dados capturados automaticamente dos registros de `ftd_aprovacao_historico`

**Tasks:**

**T6.3.1 — Criar campos calculados para métricas de tempo**
- **Onde**: make.powerapps.com → Customization Master → ftd_aprovacao_historico → Fields → New Calculated
- **Campos**:
  - `ftd_tempo_resposta_horas` (Calculated Field, Decimal): = DATEDIFF(hours, `createdon`, `ftd_timestamp_decisao`). Mostra quantas horas o aprovador levou para decidir.
  - `ftd_tempo_resposta_dias` (Calculated Field, Whole Number): = DATEDIFF(days, `createdon`, `ftd_timestamp_decisao`)
- **Na Proposta**: `ftd_lead_time_aprovacao` (Rollup Field): = MAX de `ftd_timestamp_decisao` dos registros de `ftd_aprovacao_historico` vinculados - data de submissão da proposta
- **Nota**: Calculated/Rollup fields do D365 são avaliados async (até 12h de delay). Para real-time, calcular via plugin ou Power BI.

**T6.3.2 — Criar System Dashboard "Performance de Aprovações"**
- **Onde**: make.powerapps.com → Dashboards → New
- **Layout**:
  - **Chart 1**: Bar chart — Tempo Médio por Alçada (eixo X = nível alçada 1-7, eixo Y = horas médias)
  - **Chart 2**: Pie chart — Recusas por Motivo (baseado em `ftd_razao_recusa` das propostas recusadas)
  - **View 1**: "Top 10 Propostas Paradas" — ordenada por tempo em status "Em Aprovação" DESC
  - **KPIs** (se Power BI): cards com: "Tempo Médio Total", "Taxa de Aprovação (%)", "Taxa de Recusa (%)", "Propostas Pendentes"
- **Recomendação**: este dashboard é melhor em **Power BI** pela complexidade das métricas (médias, taxas, rankings). D365 charts nativos são limitados para KPIs.

**T6.3.3 — Criar view "Propostas Pendentes de Aprovação"**
- **Onde**: make.powerapps.com → ftd_proposta → Views → New
- **Filtro**: `ftd_estagio_proposta = Em Aprovação` + `statecode = Active`
- **Colunas**: Nome Proposta, Conta, Consultor, Data Submissão, Dias Pendente (campo calculado ou coluna custom), Nível Atual, Aprovador Atual
- **Ordenação**: por "Dias Pendente" DESC (mais antigos primeiro)
- **Uso**: view útil para gestores identificarem gargalos e cobrar aprovadores

**T6.3.4 — Criar gráfico de recusas por motivo**
- **Onde**: make.powerapps.com → ftd_proposta → Charts → New
- **Tipo**: Pie Chart ou Donut
- **Eixo**: `ftd_razao_recusa` (OptionSet)
- **Valor**: COUNT de propostas
- **Filtro**: apenas propostas com `ftd_estagio_proposta = Recusada`

**T6.3.5 — Validar métricas com stakeholders**
- **Onde**: sessão com Alisson (diretor comercial) + Quintela
- **Critérios**: métricas fazem sentido? Baseline de tempo é razoável? Precisa de alertas automáticos (ex: proposta parada > 48h)?
- **Desdobramento**: se necessário, criar Power Automate flow para alertar gestores sobre propostas paradas por mais de X horas

**Estimativa**: M
**Prioridade**: Should
**Dependências**: Épico 1 (dados de aprovação disponíveis)

**Estimativa**: M
**Prioridade**: Should
**Dependências**: Épico 1 (dados de aprovação disponíveis)

---

# RESUMO QUANTITATIVO

| Métrica | Quantidade |
|---------|-----------|
| **Épicos** | 6 |
| **Stories** | 22 |
| **Tasks detalhadas** | ~150 |

## Distribuição por Prioridade
| Prioridade | Stories |
|-----------|---------|
| Must | 13 (Épico 1) |
| Should | 7 (Épicos 2, 4, 5, 6) |
| Could | 2 (Épico 3) |

## Distribuição por Sizing
| Sizing | Stories |
|--------|---------|
| XS | 0 |
| S | 4 |
| M | 8 |
| L | 8 |
| XL | 2 |

## Sequência Recomendada de Sprint

### Sprint 1-2 (Prioridade Imediata): Épico 1 — Aprovação de Propostas
- **Foundation**: 1.1 (Alçadas), 1.7 (Cadeia Propostas), 1.13 (Status/Estágio)
- **Motor**: 1.2 (Cálculo), 1.3 (Fluxo Sequencial)
- **Notificações**: 1.4 (Email + Teams)
- **Recusa/Reenvio**: 1.5 (Recusa), 1.6 (Atalho)
- **Encerramento**: 1.8 (Perda)
- **Interface**: 1.10 (Etapa 6 Big Numbers)
- **Integração**: 1.9 (DocuSign)
- **Admin**: 1.11 (Usuários), 1.12 (Manutenção Política)

### Sprint 3-4 (Paralelo): Épicos 2, 4, 5 — Cadastro + Higienização
- Dependente de resultado de Discovery (Oscar + ISA/TOTVS + Comercial)
- Story 2.4 (Séries) e Story 5.1 (Diagnóstico) podem iniciar imediatamente

### Sprint 5+: Épicos 3, 6 — Preços + Relatórios
- Dependente de consultoria de pricing e dados limpos

---

> **Nota**: Este backlog foi gerado a partir do refinamento de 16/Mar/2026 e especificações disponíveis. As stories do Épico 1 estão prontas para refinamento técnico com a equipe Avanade. Os demais épicos aguardam resultados de Discovery em andamento (Oscar + FTD). Kevellin é a PO responsável por fatiar em histórias definitivas para o DevOps.
