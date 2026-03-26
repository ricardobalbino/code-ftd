# ADR-003: Análise Comparativa de Tecnologia - Simulador Comercial

| Campo | Valor |
|-------|-------|
| **Status** | 🟡 EM ANÁLISE |
| **Data** | 24 Março 2026 |
| **Decisores** | Danilo Macedo (Avanade TL), João Carlos Figueirôa (Architect), Fernando Costa Filho (FTD TL) |
| **Stakeholders** | Oscar de Rooij (PO), Kevellin (UX/Dev), Marcio Jovanello (Manager) |
| **Última Atualização** | 24 Março 2026 |

---

## 📋 SUMÁRIO EXECUTIVO

Este documento apresenta análise comparativa de **5 opções tecnológicas** para o Simulador Comercial FTD, considerando contexto de **deadline crítica (MVP 31/Mar/26)** e licenciamento **Dynamics 365 Sales Enterprise** (todos usuários já cobertos).

### 🎯 Recomendação Estratégica

| Fase | Tecnologia | Status | Justificativa |
|------|-----------|--------|---------------|
| **MVP (Mar-Abr/26)** | ✅ **Power Pages** | Em produção UAT | Deadline não-negociável, aplicação funcional |
| **Onda 1+ (Ago/26)** | 🎯 **Power Apps Code** | Proposto (POC Maio/26) | Stack BCA, offline robusto, DX superior, licença $0 |

---

## 1. CONTEXTO DO PROBLEMA

### 1.1 Visão Geral do Simulador Comercial

**Objetivo**: Automatizar criação de propostas comerciais, reduzindo tempo de **3 horas → <30 minutos**.

**Pain Point Principal**:
- Consultor faz até **600 cliques** para adicionar 200 produtos (3 cliques/produto)
- Lentidão do Dynamics 365 (carregamento de telas, grids, salvamento)
- Falta de totalizadores em tempo real

### 1.2 Requisitos Críticos

| Requisito | Criticidade | Descrição |
|-----------|-------------|-----------|
| **Cálculos Real-Time** | 🔴 ALTA | 10+ totalizadores devem reagir instantaneamente a mudanças em até 200 produtos |
| **Mobile-First** | 🔴 ALTA | Consultores em trânsito, celular = ferramenta principal de trabalho |
| **Licenciamento Zero-Cost** | 🔴 ALTA | Usuários Dynamics já licenciados (D365 Sales Enterprise) |
| **Single Codebase** | 🟡 MÉDIA | Evitar manutenção de desktop + mobile separados |
| **Offline Capability** | 🟡 MÉDIA | Regiões com conectividade ruim (leitura mínima offline) |
| **Integração Dataverse** | 🔴 ALTA | Dados persistem no Dataverse (backend CRM) |
| **Deadline MVP** | 🔴 **CRÍTICA** | 31/Março/2026 (aplicação UAT já existe) |
| **State Management** | 🔴 ALTA | Salvar em memória, sync para Dataverse sob demanda (Dataverse storage 100% crítico) |

### 1.3 Contexto de Licenciamento

**Todos os usuários possuem**: **Dynamics 365 Sales Enterprise**

**O que está incluído (sem custo adicional)**:
- ✅ Model-Driven Apps (ilimitados, baseados em D365)
- ✅ Custom Pages (PCF components React/TS)
- ✅ Canvas Apps (até 3 apps "seeded" por usuário que se conectam ao Dataverse)
- ✅ Power Pages (internal users com Entra ID)
- ✅ Power Apps Code (apps code-first React/TS hospedados na Power Platform)
- ✅ Power Automate (cloud flows dentro do contexto D365)
- ✅ Dataverse capacity (10GB + 20GB + incrementos por usuário)

**Fonte**: [Microsoft Licensing Guide - Dynamics 365](https://www.microsoft.com/en-us/licensing/product-licensing/dynamics365)

### 1.4 Estado Atual (24/Mar/26)

- **Power Pages em UAT** (screenshots confirmam aplicação funcional)
- **MVP deadline**: 31/Março/2026 (HOJE/AMANHÃ) 🚨
- **Time treinado**: Fernando (Dev) + Kevellin (UX/Dev) conhecem Power Pages
- **Investimento sunk cost**: ~2 meses de desenvolvimento

---

## 2. OPÇÕES TECNOLÓGICAS ANALISADAS

### 2.1 Opção 1: Power Pages (Decisão Atual)

**Descrição**: Low-code web portal hospedado na Power Platform, autenticação Entra ID, HTML/CSS/JS customizável.

#### 🎯 Avaliação Dimensional

| Dimensão | Avaliação | Detalhes |
|----------|-----------|----------|
| **Performance Cálculos Real-Time** | 🟡 **MODERADA** | ✅ JavaScript custom sem limites<br>⚠️ Latência post-back se usar Liquid (evitável)<br>✅ State management client-side possível |
| **Mobile-First** | 🟢 **EXCELENTE** | ✅ Responsivo nativamente (Bootstrap/Fluent)<br>✅ Touch-friendly<br>⚠️ Não é PWA nativo (instalável) |
| **Offline Capability** | 🔴 **LIMITADA** | ❌ Não possui offline-first nativo<br>⚠️ Workaround via Service Workers (complexo, manual)<br>⚠️ Read-only offline possível com cache strategy |
| **Licenciamento (D365 Sales Ent)** | 🟢 **EXCELENTE** | ✅ Confirmado Microsoft: Entra ID (internal users) = **$0 extra**<br>✅ Usuários Dynamics já cobertos |
| **Single Codebase** | 🟢 **EXCELENTE** | ✅ Uma aplicação para desktop + mobile |
| **Integração Dataverse** | 🟢 **EXCELENTE** | ✅ Web API FetchXML/OData nativo<br>✅ Portals Web API (autenticada)<br>✅ Liquid tags (server-side) |
| **Complexidade UI Rica** | 🟡 **MODERADA** | ✅ HTML/CSS/JS livre (sem limites de Canvas)<br>⚠️ Grids complexos = desenvolvimento manual<br>⚠️ UX rica exige expertise frontend |
| **Time to Market** | 🟢 **EXCELENTE** | ✅ Ambiente UAT JÁ existe (screenshots)<br>✅ MVP deadline HOJE = **não dá tempo mudar**<br>✅ Fernando + Kevellin já treinados |
| **Manutenção/Handoff** | 🟡 **MODERADA** | ✅ Avanade tem expertise Power Pages<br>⚠️ Stack não-padrão (não é React puro)<br>⚠️ Onboarding moderado |
| **Capacidade Estado em Memória** | 🟢 **EXCELENTE** | ✅ JavaScript puro = controle total<br>✅ LocalStorage, SessionStorage, IndexedDB<br>✅ Redux/Zustand se necessário |
| **Dataverse Storage Impact** | 🟢 **POSITIVO** | ✅ State em memória = menos writes no Dataverse (**crítico - storage 100%!**) |
| **Developer Experience (DX)** | 🟡 **MODERADA** | ⚠️ Power Pages Studio (web-based)<br>⚠️ Não é VS Code nativo<br>⚠️ Debugging limitado |
| **Testing** | 🔴 **LIMITADA** | ❌ Testes unitários difíceis<br>⚠️ Somente E2E (Playwright/Selenium) |

#### ✅ Pontos Fortes
- MVP já implementado (sunk cost)
- Licenciamento $0
- Mobile responsivo
- State management controla writes Dataverse

#### ❌ Pontos Fracos
- Offline limitada
- DX não é VS Code padrão
- Testing unitário difícil
- Stack não-padrão Avanade BCA

#### 💰 Custo Total
**$0** (incluído no D365 Sales Enterprise)

---

### 2.2 Opção 2: Canvas App

**Descrição**: Low-code app Power Apps com PowerFx, mobile nativo, offline robusto.

#### 🎯 Avaliação Dimensional

| Dimensão | Avaliação | Detalhes |
|----------|-----------|----------|
| **Performance Cálculos Real-Time** | 🟢 **BOA** | ✅ PowerFx rápido para cálculos<br>✅ Collections em memória<br>⚠️ Delegação pode falhar com 200 produtos (limite 2000 registros locais) |
| **Mobile-First** | 🟢 **EXCELENTE** | ✅ Mobile nativo (inclusive app download)<br>✅ Offline robusto nativo |
| **Offline Capability** | 🟢 **EXCELENTE** | ✅ Offline-first nativo (SaveData/LoadData)<br>✅ Sync automático ao reconectar |
| **Licenciamento (D365 Sales Ent)** | 🟢 **BOA (REVISADO)** | ✅ "Seeded apps" cobertos por D365 Sales Enterprise<br>✅ **$0 extra** (até 3 Canvas Apps por usuário conectados ao Dataverse) |
| **Single Codebase** | 🔴 **PROBLEMA** | ❌ Desktop + Mobile = **2 apps separados** (Canvas não é responsivo nativamente)<br>⚠️ **Dobro de manutenção** (argumento original time FTD) |
| **Integração Dataverse** | 🟢 **EXCELENTE** | ✅ Connector nativo otimizado<br>✅ Delegação automática |
| **Complexidade UI Rica** | 🟡 **MODERADA** | ⚠️ Grids complexos (9 colunas) = galleries aninhados (performance hit)<br>⚠️ Filtros dinâmicos múltiplos = fórmulas gigantes<br>✅ UX rica possível mas trabalhosa |
| **Time to Market** | 🔴 **LENTA** | ❌ Precisa começar do zero (screenshots mostram Power Pages já avançado)<br>❌ MVP deadline HOJE = **IMPOSSÍVEL migrar agora** |
| **Manutenção/Handoff** | 🟡 **MODERADA** | ✅ Avanade tem expertise Canvas<br>⚠️ PowerFx complexo dificulta onboarding devs |
| **Capacidade Estado em Memória** | 🟢 **BOA** | ✅ Collections (até 2000 registros)<br>⚠️ 200 produtos × colunas pode estourar limites |
| **Dataverse Storage Impact** | 🟢 **POSITIVO** | ✅ Collections = menos writes |
| **Developer Experience (DX)** | 🔴 **FRACA** | ⚠️ Maker Portal (browser-based)<br>❌ Não é VS Code<br>⚠️ PowerFx = curva aprendizado |
| **Testing** | 🟡 **MODERADA** | ⚠️ Power Apps Test Studio (limitado)<br>⚠️ E2E via Power Automate |

#### ✅ Pontos Fortes
- Offline excelente
- Mobile nativo
- Licenciamento $0 (revisado)
- PowerFx para cálculos

#### ❌ Pontos Fracos
- **2 apps separados** (desktop + mobile)
- Começar do zero (deadline impossível)
- PowerFx não é stack Avanade BCA
- DX não é pro-code

#### 💰 Custo Total
**$0** (seeded apps no D365 Sales Enterprise, até 3 apps/usuário)

---

### 2.3 Opção 3: Custom Pages (PCF Components)

**Descrição**: React/TypeScript components (PCF) rodando dentro de Model-Driven App (Spartan).

#### 🎯 Avaliação Dimensional

| Dimensão | Avaliação | Detalhes |
|----------|-----------|----------|
| **Performance Cálculos Real-Time** | 🟢 **EXCELENTE** | ✅ React/TypeScript custom = controle total<br>✅ State management (Redux, MobX, Zustand) |
| **Mobile-First** | 🟡 **MODERADA** | ⚠️ Custom Pages rodando **DENTRO do Spartan** (Model-Driven App)<br>⚠️ Mobile D365 app = experiência limitada (**pain point conhecido FTD**)<br>⚠️ Não funciona standalone mobile |
| **Offline Capability** | 🔴 **LIMITADA** | ❌ Depende do D365 Mobile app (offline limitado)<br>⚠️ Custom Pages herdam limitações da shell |
| **Licenciamento (D365 Sales Ent)** | 🟢 **EXCELENTE** | ✅ Incluído na licença Dynamics (usuários já cobertos)<br>✅ Sem custo adicional |
| **Single Codebase** | 🟢 **BOA** | ✅ Uma aplicação (PCF component)<br>⚠️ Mas rodando dentro do Spartan app (navegação travada) |
| **Integração Dataverse** | 🟢 **EXCELENTE** | ✅ WebAPI nativo TypeScript<br>✅ Context do framework |
| **Complexidade UI Rica** | 🟢 **EXCELENTE** | ✅ React completo (libs terceiras: AG-Grid, Material-UI)<br>✅ UX rica sem limites |
| **Time to Market** | 🔴 **LENTA** | ❌ Começar do zero<br>⚠️ Deploy via solution (pipeline)<br>❌ **Deadline impossível** |
| **Manutenção/Handoff** | 🟢 **EXCELENTE** | ✅ React/TypeScript = stack standard Avanade BCA<br>✅ Fácil onboarding devs |
| **Capacidade Estado em Memória** | 🟢 **EXCELENTE** | ✅ Redux/MobX = state management robusto |
| **Dataverse Storage Impact** | 🟢 **POSITIVO** | ✅ State em memória |
| **Developer Experience (DX)** | 🟢 **EXCELENTE** | ✅ VS Code nativo<br>✅ npm/yarn<br>✅ React DevTools |
| **Testing** | 🟢 **EXCELENTE** | ✅ Jest + React Testing Library<br>✅ E2E (Playwright) |

#### ✅ Pontos Fortes
- React/TypeScript (stack BCA)
- VS Code + npm (DX profissional)
- Licenciamento $0
- Testing completo

#### ❌ Pontos Fracos
- **Roda DENTRO do Spartan** (navegação travada)
- **Mobile D365 app ruim** (pain point conhecido)
- Começar do zero (deadline impossível)

#### 💰 Custo Total
**$0** (incluído no D365 Sales Enterprise)

---

### 2.4 Opção 4: Power Apps Code 🎯

**Descrição**: Apps code-first (React/TypeScript) desenvolvidos em VS Code, hospedados na Power Platform, standalone (não embedded).

**Referência**: [Microsoft Learn - Power Apps Code](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/overview)

#### 🎯 Avaliação Dimensional

| Dimensão | Avaliação | Detalhes |
|----------|-----------|----------|
| **Performance Cálculos Real-Time** | 🟢 **EXCELENTE** | ✅ React otimizado<br>✅ State management profissional (Redux Toolkit, Zustand)<br>✅ Memoização e virtualização (TanStack Virtual) |
| **Mobile-First** | 🟢 **EXCELENTE** | ✅ **Standalone app** (não preso ao Spartan)<br>✅ PWA (Progressive Web App) com Service Workers<br>✅ Responsivo total (Fluent UI, Tailwind)<br>✅ Touch-friendly, instalável |
| **Offline Capability** | 🟢 **EXCELENTE** | ✅ Service Workers + IndexedDB = offline-first robusto<br>✅ Background sync<br>✅ Controle total sobre sync strategy<br>✅ Suporte nativo Workbox |
| **Licenciamento (D365 Sales Ent)** | 🟢 **EXCELENTE** | ✅ **$0 extra** (incluído no D365 Sales Enterprise)<br>✅ "Seeded apps" Power Platform<br>✅ Sem custo hosting (hospedado na Power Platform) |
| **Single Codebase** | 🟢 **EXCELENTE** | ✅ Uma aplicação React responsiva |
| **Integração Dataverse** | 🟢 **EXCELENTE** | ✅ Dataverse SDK otimizado (TypeScript)<br>✅ Autenticação Entra ID nativa<br>✅ API calls otimizados |
| **Complexidade UI Rica** | 🟢 **EXCELENTE** | ✅ Ecossistema React completo (AG-Grid, TanStack Table, MUI DataGrid)<br>✅ Total liberdade de UX<br>✅ Componentes reutilizáveis |
| **Time to Market** | 🔴 **LENTA** | ❌ Começar do ZERO (infra + frontend + auth + deploy)<br>⚠️ Estimativa: **3-4 meses** mínimo<br>❌ **Deadline MVP impossível** |
| **Manutenção/Handoff** | 🟢 **EXCELENTE** | ✅ Stack standard Avanade BCA (React + TypeScript)<br>✅ Qualquer dev consegue manter<br>✅ CI/CD Azure DevOps |
| **Capacidade Estado em Memória** | 🟢 **EXCELENTE** | ✅ Redux Toolkit + RTK Query = cache + state sofisticado<br>✅ React Query = server state management |
| **Dataverse Storage Impact** | 🟢 **POSITIVO** | ✅ State em memória = menos writes |
| **Developer Experience (DX)** | 🟢 **EXCELENTE** | ✅ VS Code nativo<br>✅ npm/yarn para dependencies<br>✅ React DevTools + Redux DevTools<br>✅ Hot reload (Vite/Webpack) |
| **Testing** | 🟢 **EXCELENTE** | ✅ Jest/Vitest + React Testing Library<br>✅ E2E (Playwright)<br>✅ Coverage completo |

#### ✅ Pontos Fortes
- **Melhor performance** (React otimizado)
- **Offline robusto** (Service Workers nativos)
- **Stack Avanade BCA** (React/TypeScript)
- **VS Code + npm** (DX profissional)
- **PWA instalável** (mobile UX superior)
- **Licenciamento $0** (incluído D365)
- **Standalone** (não preso ao Spartan)
- **Hosting Power Platform** (sem Azure App Service)

#### ❌ Pontos Fracos
- **Começar do zero** (3-4 meses)
- **POC necessário** (tecnologia relativamente nova)
- Time FTD precisaria treinar em React (ou Avanade mantém)

#### 💰 Custo Total
**$0** (incluído no D365 Sales Enterprise, hospedagem na Power Platform)

---

### 2.5 Opção 5: Custom SPA (React + Azure)

**Descrição**: React SPA custom hospedado em Azure (App Service ou Static Web Apps), autenticação Entra ID, Web API para Dataverse.

#### 🎯 Avaliação Dimensional

| Dimensão | Avaliação | Detalhes |
|----------|-----------|----------|
| **Performance Cálculos Real-Time** | 🟢 **EXCELENTE** | ✅ React/TypeScript otimizado<br>✅ State management profissional<br>✅ Virtualização (react-window, TanStack Virtual) |
| **Mobile-First** | 🟢 **EXCELENTE** | ✅ PWA (Progressive Web App) com Service Workers<br>✅ Responsivo total<br>✅ Touch-friendly |
| **Offline Capability** | 🟢 **EXCELENTE** | ✅ Service Workers + IndexedDB = offline-first robusto<br>✅ Background sync<br>✅ Controle total |
| **Licenciamento (D365 Sales Ent)** | 🟡 **VARIÁVEL** | ⚠️ **Autenticação Entra ID** (Azure AD B2C/B2E) = pode ter custo incremental<br>⚠️ **Hosting Azure** (App Service $50-200/mês ou Static Web Apps $10-50/mês)<br>✅ Usuários não pagam licença app adicional<br>⚠️ Precisa validar com Microsoft se "external app" gera cobrança Dataverse API calls |
| **Single Codebase** | 🟢 **EXCELENTE** | ✅ Uma aplicação React responsiva |
| **Integração Dataverse** | 🟡 **MODERADA** | ⚠️ Web API OData/FetchXML via HTTPS (S2S auth via App Registration)<br>⚠️ API calls contam para limites Dataverse (24h rolling window)<br>⚠️ Sem Portals Web API (externa ao ecossistema) |
| **Complexidade UI Rica** | 🟢 **EXCELENTE** | ✅ Ecossistema React completo<br>✅ Total liberdade de UX |
| **Time to Market** | 🔴 **MUITO LENTA** | ❌ Começar do ZERO (infra + frontend + backend + auth + deploy)<br>⚠️ Estimativa: **4-6 meses** mínimo<br>❌ **Deadline não negociável** |
| **Manutenção/Handoff** | 🟢 **EXCELENTE** | ✅ Stack standard industry (React + Node/C# API)<br>✅ Qualquer dev consegue manter |
| **Capacidade Estado em Memória** | 🟢 **EXCELENTE** | ✅ Redux Toolkit + RTK Query<br>✅ React Query |
| **Dataverse Storage Impact** | 🟢 **POSITIVO** | ✅ State em memória |
| **Developer Experience (DX)** | 🟢 **EXCELENTE** | ✅ VS Code nativo<br>✅ npm/yarn<br>✅ Qualquer framework (Vite, Next.js, etc) |
| **Testing** | 🟢 **EXCELENTE** | ✅ Jest/Vitest + React Testing Library<br>✅ E2E (Playwright) |

#### ✅ Pontos Fortes
- **Melhor performance** possível
- **Offline robusto**
- **UX ilimitada** (total liberdade)
- **Stack standard** industry
- **Manutenção fácil**

#### ❌ Pontos Fracos
- **Custo hosting** ($50-200/mês Azure)
- **Começar do zero** (4-6 meses)
- **API limits Dataverse** (external calls)
- **Complexidade infra** (CI/CD, auth, networking)

#### 💰 Custo Total
**~$600-2.400/ano** (hosting Azure) + **custo desenvolvimento** 4-6 meses

---

## 3. MATRIZ COMPARATIVA CONSOLIDADA

| **Critério** | **Power Pages** | **Canvas App** | **Custom Pages** | **🎯 Power Apps Code** | **Custom SPA** |
|--------------|----------------|----------------|------------------|---------------------|----------------|
| ⚡ **Performance Real-Time** | 🟡 Moderada | 🟢 Boa | 🟢 Excelente | 🟢 **Excelente** | 🟢 Excelente |
| 📱 **Mobile-First** | 🟡 Responsivo web | 🟢 Mobile nativo | 🔴 Preso Spartan | 🟢 **PWA standalone** | 🟢 PWA |
| 📴 **Offline** | 🔴 Limitado | 🟢 Nativo robusto | 🔴 Limitado | 🟢 **Service Workers** | 🟢 Service Workers |
| 💰 **Licenciamento** | 🟢 $0 | 🟢 $0 (revisado) | 🟢 $0 | 🟢 **$0** | 🟡 $600-2.4k/ano |
| 🔧 **Single Codebase** | 🟢 Sim | 🔴 2 apps | 🟢 Sim | 🟢 **Sim** | 🟢 Sim |
| 🔌 **Integração Dataverse** | 🟢 Nativa | 🟢 Nativa | 🟢 Nativa | 🟢 **SDK otimizado** | 🟡 OData HTTPS |
| 🎨 **UI Rica** | 🟡 HTML/CSS manual | 🔴 Galleries | 🟢 React libs | 🟢 **React ecossistema** | 🟢 React |
| ⏱️ **Time to Market MVP** | 🟢 **UAT pronto** | 🔴 Zero | 🔴 Zero | 🔴 **Zero** | 🔴 Zero |
| 👨‍💻 **Stack BCA** | 🟡 JS/HTML | 🔴 PowerFx | 🟢 React/TS | 🟢 **React/TS** | 🟢 React/TS |
| 🔧 **Manutenção** | 🟡 Moderada | 🔴 PowerFx | 🟢 Standard | 🟢 **Standard** | 🟢 Standard |
| 💾 **Estado em Memória** | 🟢 LocalStorage | 🟢 Collections | 🟢 Redux | 🟢 **Redux Toolkit** | 🟢 Redux |
| 🚀 **Deployment** | Power Pages CLI | Power Platform | Solution | 🟢 **Power Platform CLI** | Azure DevOps |
| 👩‍💻 **Developer Experience** | 🟡 Power Pages Studio | 🔴 Maker Portal | 🟢 VS Code | 🟢 **VS Code + npm** | 🟢 VS Code |
| 🧪 **Testing** | 🔴 E2E apenas | 🟡 Test Studio | 🟢 Jest + E2E | 🟢 **Jest/Vitest + E2E** | 🟢 Jest + E2E |
| ♻️ **Reutilização** | 🔴 Baixa | 🔴 Baixa | 🟢 PCF reusável | 🟡 **Moderada** | 🟡 Moderada |
| 💾 **Dataverse Storage** | 🟢 Reduz writes | 🟢 Reduz writes | 🟢 Reduz writes | 🟢 **Reduz writes** | 🟢 Reduz writes |

**Legenda**: 🟢 Excelente | 🟡 Moderada/Boa | 🔴 Fraca/Problema

---

## 4. ANÁLISE DE RISCOS E MITIGAÇÕES

### 4.1 Power Pages (MVP Atual)

| Risco | Probabilidade | Impacto | Mitigação |
|-------|--------------|---------|-----------|
| **Offline insuficiente** | 🟡 Média | 🟡 Médio | Service Workers manual (Phase 2), feedback usuários Onda 1 |
| **Performance com 200 produtos** | 🟡 Média | 🔴 Alto | Virtualização de grid, debounce cálculos, testes carga |
| **Manutenção stack não-padrão** | 🟡 Média | 🟡 Médio | Documentação extensiva, handoff time Avanade |
| **Dataverse storage crítico** | 🔴 Alta | 🔴 Alto | ✅ **State em memória JÁ resolve** (menos writes) |

### 4.2 Power Apps Code (Onda 1 Proposto)

| Risco | Probabilidade | Impacto | Mitigação |
|-------|--------------|---------|-----------|
| **POC falha (limitações não documentadas)** | 🟡 Média | 🔴 Alto | **POC obrigatório Maio/26** antes de commit |
| **Timeline Onda 1 atrasa** | 🟡 Média | 🟡 Médio | Buffer 1 mês, começar POC imediatamente |
| **Time FTD não consegue manter React** | 🟡 Média | 🟡 Médio | Avanade assume manutenção OU treinamento React |
| **Licenciamento muda (Microsoft altera política)** | 🟢 Baixa | 🔴 Alto | Validar com Microsoft por escrito, cláusula contratual |

---

## 5. DECISION FRAMEWORK (Como Decidimos)

### 5.1 Critérios de Decisão (Pesos)

| Critério | Peso | Justificativa |
|----------|------|---------------|
| **Time to Market MVP** | 🔴 **35%** | Deadline não-negociável (31/Mar/26) |
| **Licenciamento** | 🔴 **25%** | Budget zero adicional (todos usuários D365 já licenciados) |
| **Stack BCA / Manutenção** | 🟡 **15%** | Handoff para Avanade, onboarding devs |
| **Mobile + Offline** | 🟡 **10%** | Requisito médio (spec diz "mesmo que limitado") |
| **Performance** | 🟡 **10%** | Crítico mas todas opções atendem |
| **Developer Experience** | 🟢 **5%** | Nice-to-have (não blocker) |

### 5.2 Scoring (0-10 por critério)

| Opção | Time Market | Licenciamento | Stack BCA | Mobile+Offline | Performance | DX | **TOTAL** |
|-------|------------|---------------|-----------|----------------|-------------|----|-----------|
| **Power Pages** | **10** (UAT pronto) | **10** ($0) | 6 | 4 | 7 | 6 | **8.15** 🥇 |
| Canvas App | 0 (zero) | 10 ($0) | 3 (PowerFx) | 10 | 8 | 4 | **4.70** |
| Custom Pages | 0 (zero) | 10 ($0) | 10 | 2 (Spartan) | 10 | 10 | **5.35** |
| **Power Apps Code** | 0 (zero) | **10** ($0) | **10** | **10** | **10** | **10** | **6.00** 🥈 |
| Custom SPA | 0 (zero) | 5 (custo) | 10 | 10 | 10 | 10 | **5.20** |

**Interpretação**:
- **Power Pages vence para MVP** (peso 35% no Time to Market = decisivo)
- **Power Apps Code é 2º lugar** e venceria se Time to Market não fosse crítico

---

## 6. RECOMENDAÇÃO FINAL

### 🎯 Estratégia em 2 Fases

#### **FASE 1 - MVP (Mar-Abr/26)**: ✅ **MANTER POWER PAGES**

**Decisão**: ✅ Entregar MVP em Power Pages (não mudar tecnologia agora)

**Justificativa**:
1. 🔴 **Deadline não-negociável** (31/Mar/26 = HOJE)
2. 🟢 **UAT funcional** (screenshots confirmam aplicação rodando)
3. 🟢 **Time treinado** (Fernando + Kevellin)
4. 🟢 **Investimento sunk cost** (~2 meses desenvolvimento)
5. 🟢 **Licenciamento $0**
6. 🟢 **State em memória** resolve Dataverse storage crítico

**Ações Imediatas**:
- ✅ Confirmar entrega MVP 31/Mar
- ✅ Validar state management implementado (menos writes Dataverse)
- ✅ Planejar testes performance com 200 produtos

---

#### **FASE 2 - ONDA 1+ (Ago/26)**: 🎯 **MIGRAR PARA POWER APPS CODE**

**Decisão**: 🎯 **Recomendar fortemente migração para Power Apps Code**

**Justificativa**:
1. 🟢 **Stack Avanade BCA** (React/TypeScript = padrão guidelines BCA)
2. 🟢 **Developer Experience superior** (VS Code + npm = DX profissional)
3. 🟢 **Offline robusto** (Service Workers nativos, resolve limitação Power Pages)
4. 🟢 **PWA instalável** (mobile UX superior a web responsivo)
5. 🟢 **Testing completo** (Jest + React Testing Library + E2E)
6. 🟢 **Componentização React** (reusável, manutenível)
7. 🟢 **Licenciamento $0** (D365 Sales Enterprise cobre)
8. 🟢 **Handoff facilitado** (qualquer dev React consegue manter)
9. 🟢 **Performance otimizada** (React + state management profissional)

**Pré-requisitos para Decisão GO**:
- ✅ **POC obrigatório** (Maio/26) - implementar 1 tela completa
- ✅ **Validar performance** (<300ms para cálculos 200 produtos)
- ✅ **Validar offline** (Service Workers funcionam)
- ✅ **Validar deployment** (Power Platform CLI)
- ✅ **Validar DX** (time consegue desenvolver)

**Timeline**:
- **Mai/26**: POC (2 semanas - 1 tela "Produtos da Proposta")
- **Jun/26**: Decisão GO/NO-GO (baseada em POC)
- **Jul-Ago/26**: Desenvolvimento Onda 1 (3 meses)
- **Set/26**: Deploy UAT + testes
- **Out/26**: Deploy PROD (substituir Power Pages)

---

### ❌ Opções NÃO Recomendadas

| Opção | Razão |
|-------|-------|
| ❌ **Canvas App** | 2 apps separados (desktop + mobile) = dobro manutenção, PowerFx não é stack BCA |
| ❌ **Custom Pages** | Preso ao Spartan (mobile D365 app é pain point conhecido FTD) |
| ❌ **Custom SPA** | Custo hosting Azure desnecessário (Power Apps Code tem hosting incluso) |

---

## 7. PRÓXIMOS PASSOS (Action Items)

### 📅 Imediato (Semana 25-29/Mar/26)

| # | Ação | Responsável | Deadline | Status |
|---|------|-------------|----------|--------|
| 1 | Confirmar entrega MVP Power Pages | Fernando + Oscar | 27/Mar | 🟡 EM VALIDAÇÃO |
| 2 | Documentar este ADR | João Carlos (Architect) | 28/Mar | ✅ **CONCLUÍDO** |
| 3 | Validar state management implementado | Danilo + Fernando | 29/Mar | 🔴 PENDENTE |
| 4 | Planejar testes performance (200 produtos) | Carla QA | 31/Mar | 🔴 PENDENTE |

### 📅 Maio/26 (POC Power Apps Code)

| # | Ação | Responsável | Deadline |
|---|------|-------------|----------|
| 5 | Criar repositório POC Power Apps Code | Tiago Dev (Avanade) | 01/Mai |
| 6 | Implementar tela "Produtos da Proposta" | Tiago + Fernando | 15/Mai |
| 7 | Validar critérios sucesso POC | João Carlos + Danilo | 20/Mai |
| 8 | Apresentar POC para stakeholders | Tiago Dev | 22/Mai |

### 📅 Junho/26 (Decisão GO/NO-GO)

| # | Ação | Responsável | Deadline |
|---|------|-------------|----------|
| 9 | Reunião decisão GO/NO-GO | João Carlos + Danilo + Fernando + Oscar + Jovanello | 05/Jun |
| 10 | SE GO: Planejar Onda 1 em Power Apps Code | Paula PO + Roberto SM | 10/Jun |
| 11 | SE NO-GO: Identificar mitigações Power Pages | Wilson Architect | 10/Jun |

---

## 8. STAKEHOLDER SIGN-OFF

| Papel | Nome | Assinatura | Data |
|-------|------|------------|------|
| **Tech Lead Avanade** | Danilo Macedo | _______________ | ___/___/2026 |
| **Architect Avanade** | João Carlos Figueirôa | _______________ | ___/___/2026 |
| **Tech Lead FTD** | Fernando Costa Filho | _______________ | ___/___/2026 |
| **Product Owner** | Oscar de Rooij | _______________ | ___/___/2026 |
| **Manager Avanade** | Marcio Jovanello | _______________ | ___/___/2026 |

---

## 9. REFERÊNCIAS

1. [Microsoft Learn - Power Apps Code](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/overview)
2. [Microsoft Licensing - Dynamics 365 Sales](https://www.microsoft.com/en-us/licensing/product-licensing/dynamics365)
3. [FTD Knowledge Base](./../ftd-knowledge-base.md)
4. [Especificação Simulador Comercial](./../especificacao-simulador-notion.md)
5. [Avanade BCA Guidelines](./../../.github/instructions/avanade-bca-guidelines.instructions.md)
6. [FTD Discovery](./../../docs/discovery/ftd-discovery.md)
7. [Power Pages Documentation](https://learn.microsoft.com/en-us/power-pages/)
8. [PCF Components](https://learn.microsoft.com/en-us/power-apps/developer/component-framework/overview)

---

## 10. HISTÓRICO DE REVISÕES

| Versão | Data | Autor | Mudanças |
|--------|------|-------|----------|
| 1.0 | 24/Mar/2026 | Danilo Macedo | Versão inicial - análise comparativa completa |

---

**FIM DO DOCUMENTO**
