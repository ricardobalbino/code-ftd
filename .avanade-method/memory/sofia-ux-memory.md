### Design System Patterns
_Componentes e padrões reutilizáveis_

**Exemplo**:
```yaml
- design_system: "Fluent UI (Microsoft Design Language)"
  components_used:
    - "Buttons (Primary, Secondary, Icon)"
    - "Forms (TextField, Dropdown, Checkbox)"
    - "Navigation (Nav, Breadcrumb, Pivot)"
    - "Feedback (MessageBar, Dialog, ProgressIndicator)"
  benefits:
    - "Consistência visual (todo app usa mesmos components)"
    - "Acessibilidade built-in (WCAG AA compliance)"
    - "Development speed (Tiago reutiliza components)"
  customization: "Theming (cores brand, typography)"
  documentation: "Storybook (catalog de components com exemplos)"
  
- pattern: "Atomic Design (Atoms ? Molecules ? Organisms)"
  atoms: "Button, Input, Label (building blocks)"
  molecules: "SearchBar (Input + Button), FormField (Label + Input + Error)"
  organisms: "Header (Logo + Nav + SearchBar), Card (Image + Title + Description)"
  benefits: "Reusabilidade, escalabilidade (adicionar features combina components)"
  tool: "Figma (componentes com variants, auto-layout)"
  
- pattern: "Responsive Design (mobile-first)"
  breakpoints:
    - "Mobile: 320-767px"
    - "Tablet: 768-1023px"
    - "Desktop: 1024px+"
  approach: "Design mobile primeiro, progressive enhancement"
  rationale: "62% do tráfego é mobile (analytics)"
  testing: "Browserstack, Chrome DevTools (device emulation)"
```

---

### User Research Insights
_Descobertas de pesquisa com usuários_

**Exemplo**:
```yaml
- research_method: "Usability Testing (moderated)"
  participants: "5 usuários (persona: Sarah, Gerente de Vendas)"
  task: "Cadastrar novo cliente no CRM"
  findings:
    - "100% dos usuários clicaram em 'Save' antes de preencher campos obrigatórios (erro UX)"
    - "80% não viram mensagem de erro (posicionada no topo, fora do viewport)"
    - "60% tentaram usar Ctrl+F para buscar cliente (search box não óbvia)"
  actions:
    - "Disable 'Save' button até campos obrigatórios preenchidos"
    - "Mostrar erros inline (próximo ao campo, não topo)"
    - "Search box mais proeminente (hero position)"
  impact: "Task completion time 3min ? 1min (-66%)"
  
- research_method: "A/B Testing (quantitative)"
  hypothesis: "Botão CTA verde converte mais que azul"
  variants:
    control: "Botão azul (brand color)"
    variant_a: "Botão verde (high contrast)"
  sample_size: "10k visitors (5k cada variante)"
  results:
    control: "Conversion rate 5.2%"
    variant_a: "Conversion rate 6.8% (+30%)"
  decision: "Ship variant A (green button)"
  learning: "Contrast > brand consistency (quando conversion crítica)"
  
- research_method: "Heatmaps & Session Recordings (Hotjar)"
  page: "Homepage"
  findings:
    - "80% scroll até hero section apenas (conteúdo abaixo ignorado)"
    - "20% clicaram em imagem (esperando link, mas não era clicável)"
    - "Exit rate alto (40%) no formulário de signup (campo 'Company Tax ID' confuso)"
  actions:
    - "Above-the-fold optimization (conteúdo crítico no hero)"
    - "Fazer imagens clicáveis (link para product page)"
    - "Simplificar signup (Tax ID opcional, não obrigatório)"
  impact: "Bounce rate 55% ? 40%, signup completion +25%"
```

---

### Accessibility (WCAG) Compliance
_Padrões de acessibilidade validados_

**Exemplo**:
```yaml
- wcag_level: "AA (target)"
  criteria_checklist:
    perceivable:
      - "Contrast ratio =4.5:1 (texto normal) =3:1 (texto grande)" # ?
      - "Alt text para imagens" # ?
      - "Captions para vídeos" # ?
    operable:
      - "Navegação via teclado (Tab, Enter, Esc)" # ?
      - "Skip links (pular para conteúdo principal)" # ?
      - "Focus visível (outline em elementos focados)" # ?
    understandable:
      - "Labels claros em formulários" # ?
      - "Mensagens de erro descritivas" # ?
      - "Linguagem simples (evitar jargão)" # ?
    robust:
      - "HTML semântico (<header>, <nav>, <main>)" # ?
      - "ARIA labels onde necessário (role, aria-label)" # ?
  
- tool: "axe DevTools (Chrome extension)"
  usage: "Scan de cada página antes de ship"
  findings_example:
    - "? Link sem texto descritivo ('Clique aqui' ? 'Saiba mais sobre produto')"
    - "? Formulário sem labels associados (<label for='email'>)"
    - "?? Contrast ratio 3.8:1 (abaixo do 4.5:1 requerido)"
  fix: "Corrigir antes de merge (blocker em code review)"
  
- assistive_tech_testing:
  - "NVDA (screen reader Windows) - testado por Sofia mensalmente"
  - "VoiceOver (screen reader macOS) - testado por Sofia mensalmente"
  - "Keyboard-only navigation - testado em cada feature"
  impact: "Accessibility compliant ? WCAG AA certification (requirement para govt contracts)"
```

---

### UI Component Library
_Componentes customizados documentados_

**Exemplo**:
```yaml
- component: "DataTable (sortable, filterable, paginated)"
  variants:
    - "Simple (read-only)"
    - "Editable (inline editing)"
    - "Selectable (checkboxes, bulk actions)"
  props:
    - "columns: { key, label, sortable, filterable }"
    - "data: Array<object>"
    - "pageSize: number (default 25)"
    - "onRowClick: callback"
  accessibility:
    - "Sortable headers (keyboard: Enter to sort)"
    - "Screen reader announces row count"
    - "Aria-labels em ações (edit, delete)"
  figma_link: "https://figma.com/file/components/datatable"
  storybook_link: "http://localhost:6006/?path=/story/datatable"
  
- component: "Modal Dialog"
  variants:
    - "Confirmation (Yes/No)"
    - "Form (input fields + submit)"
    - "Alert (info, warning, error)"
  props:
    - "isOpen: boolean"
    - "title: string"
    - "content: ReactNode"
    - "onClose: callback"
    - "primaryAction: { label, onClick }"
  accessibility:
    - "Focus trap (Tab navega apenas dentro do modal)"
    - "Esc fecha modal"
    - "Focus retorna ao elemento que abriu (após fechar)"
  figma_link: "https://figma.com/file/components/modal"
```

---

## ?? User Personas & Journeys

### User Personas Recorrentes
_Personas validadas com research_

**Exemplo**:
```yaml
- persona: "Sarah, Gerente de Vendas"
  demographics:
    - "35-50 anos"
    - "Tecnicamente competente (usa CRM diariamente)"
    - "Mobile-first (60% do tempo em smartphone)"
  goals:
    - "Fechar vendas rapidamente"
    - "Acessar dados de clientes on-the-go"
    - "Gerar relatórios para apresentar a diretoria"
  pain_points:
    - "Sistema atual lento em mobile (frustração)"
    - "Dados desatualizados (sincronização ruim)"
    - "Relatórios difíceis de customizar"
  tech_savviness: "Média-Alta (smartphone power user, mas resiste a novos softwares)"
  quote: "'Preciso de um sistema que simplesmente funcione, sem me fazer pensar'"
  projects_referenced: 8
  
- persona: "Carlos, CFO"
  demographics:
    - "45-60 anos"
    - "Uso esporádico de software (assistente faz data entry)"
    - "Desktop primário (Excel power user)"
  goals:
    - "Visualizar KPIs financeiros rapidamente"
    - "Export para Excel (análise adicional)"
    - "Compliance/audit trail (rastreabilidade)"
  pain_points:
    - "Dashboards complexos (quer simplicidade)"
    - "Falta de drill-down (quer detalhes sem abrir múltiplas telas)"
  tech_savviness: "Baixa (não navega bem em interfaces complexas)"
  quote: "'Mostre-me os números que importam, não me faça procurar'"
  projects_referenced: 5
```

---

### User Journey Maps
_Jornadas mapeadas_

**Exemplo**:
```yaml
- journey: "Primeiro uso (onboarding)"
  persona: "Sarah, Gerente de Vendas"
  stages:
    1_signup:
      actions: "Cria conta via email"
      thoughts: "Espero que seja rápido, odeio formulários longos"
      emotions: "?? Neutra (ceticismo inicial)"
      touchpoints: "Landing page ? Signup form"
      pain_points: "Formulário pede Tax ID (não sei de cabeça)"
      opportunities: "Simplificar signup (Tax ID opcional)"
    
    2_onboarding:
      actions: "Tutorial interativo (5 tooltips)"
      thoughts: "Parece intuitivo, gosto do tour guiado"
      emotions: "?? Positiva (first win - adicionou primeiro cliente)"
      touchpoints: "Dashboard ? Interactive tour"
      pain_points: "Tour longo demais (pula após 3 steps)"
      opportunities: "Onboarding progressivo (não tudo de uma vez)"
    
    3_daily_use:
      actions: "Adiciona clientes, atualiza pipeline"
      thoughts: "Rápido em mobile, melhor que sistema anterior"
      emotions: "?? Satisfeita (adoption)"
      touchpoints: "Mobile app (80% do tempo)"
      pain_points: "Busca às vezes lenta (>3s)"
      opportunities: "Otimizar busca (indexing, caching)"
    
    4_reporting:
      actions: "Gera relatório de vendas (mensal)"
      thoughts: "Export to Excel funcionou, perfeito"
      emotions: "?? Encantada (delight moment)"
      touchpoints: "Reports ? Export to Excel"
      pain_points: "Nenhum (funcionalidade matadora)"
      opportunities: "Adicionar templates de relatórios pré-configurados"
  
  overall_sentiment: "Positivo (NPS 9/10)"
  retention_risk: "Baixo (usuária ativa diária)"
```

---

## ??? Wireframing & Prototyping

### Low-Fi to Hi-Fi Progression
_Processo de design iterativo_

**Exemplo**:
```yaml
- stage_1: "Sketches (papel e caneta)"
  duration: "30min - 1h"
  purpose: "Ideação rápida (múltiplas variantes)"
  artifact: "Foto de sketches (Figma import)"
  validation: "Team review (Paula PO, Maria Analyst)"
  
- stage_2: "Low-Fi Wireframes (Figma)"
  duration: "2-4h"
  purpose: "Estrutura de conteúdo, fluxos de navegação"
  fidelity: "Grayscale, placeholders (lorem ipsum, boxes)"
  validation: "Usability test com 3 usuários (paper prototypes)"
  iterations: "2-3 rounds (baseado em feedback)"
  
- stage_3: "Mid-Fi Wireframes (com conteúdo real)"
  duration: "1-2 dias"
  purpose: "Validar copy, hierarquia de informação"
  fidelity: "Conteúdo real, grayscale, spacing definido"
  validation: "Stakeholder review (Paula + João PM)"
  
- stage_4: "Hi-Fi Mockups (design visual)"
  duration: "2-3 dias"
  purpose: "Design final (cores, typography, imagery)"
  fidelity: "Pixel-perfect, brand guidelines aplicados"
  validation: "Design review (accessibility, brand compliance)"
  
- stage_5: "Interactive Prototype (Figma)"
  duration: "1-2 dias"
  purpose: "Simular fluxos (user testing)"
  interactions: "Click-through, hover states, transitions"
  validation: "Usability testing (5 usuários, moderated)"
  handoff: "Design to dev (Figma ? code by Tiago)"
```

---

### Design Handoff Process
_Como Sofia entrega designs para Tiago_

**Exemplo**:
```yaml
- tool: "Figma (design) + Figma Dev Mode (handoff)"
  assets_export:
    - "Icons (SVG, optimized)"
    - "Images (WebP, multiple resolutions)"
    - "Fonts (WOFF2)"
  css_extraction:
    - "Colors (hex codes)"
    - "Typography (font-family, size, weight, line-height)"
    - "Spacing (margins, paddings em px/rem)"
  component_specs:
    - "States (default, hover, active, disabled)"
    - "Variants (primary button, secondary button)"
    - "Responsive behavior (mobile, tablet, desktop)"
  
- documentation: "Design System Wiki (Notion)"
  contents:
    - "Component guidelines (quando usar cada component)"
    - "Accessibility notes (ARIA labels, keyboard navigation)"
    - "Edge cases (long text, empty states, error states)"
  
- collaboration: "Design review meeting (Sofia + Tiago)"
  frequency: "Início de cada sprint"
  agenda:
    - "Sofia: Walkthrough de designs (15min)"
    - "Tiago: Perguntas técnicas (feasibility)"
    - "Ambos: Clarify edge cases, animations, interactions"
  output: "Alignment (evita rework durante dev)"
```

---

## ?? Visual Design Principles

### Color Theory Applications
_Uso estratégico de cores_

**Exemplo**:
```yaml
- principle: "60-30-10 Rule (color balance)"
  primary_60: "Neutral (branco, cinza claro) - backgrounds"
  secondary_30: "Brand color (azul) - headers, accents"
  accent_10: "High contrast (verde, vermelho) - CTAs, alerts"
  benefit: "Visual hierarchy clara, não overwhelming"
  
- principle: "Color Psychology"
  blue: "Confiança, profissionalismo (usado em brand)"
  green: "Sucesso, positivo (confirmações, CTAs positivos)"
  red: "Urgência, erro (alerts, delete actions)"
  yellow: "Atenção, warning (avisos não críticos)"
  
- principle: "Accessibility (color blindness)"
  rule: "Nunca depender APENAS de cor (adicionar ícones/texto)"
  example: "Sucesso = verde + ? ícone, Erro = vermelho + ? ícone"
  testing: "Color blindness simulator (Figma plugin)"
```

---

### Typography Hierarchy
_Sistema tipográfico_

**Exemplo**:
```yaml
- scale: "Type Scale (modular, ratio 1.25)"
  h1: "48px (3rem) - Page titles"
  h2: "38px (2.4rem) - Section headers"
  h3: "30px (1.9rem) - Subsections"
  h4: "24px (1.5rem) - Card titles"
  body: "16px (1rem) - Body text"
  small: "14px (0.875rem) - Captions, footnotes"
  
- font_stack:
  primary: "Segoe UI (Fluent UI default)"
  fallback: "Arial, sans-serif"
  monospace: "Consolas, 'Courier New', monospace (code snippets)"
  
- readability_rules:
  - "Line height: 1.5 (body text)"
  - "Line length: 50-75 chars (optimal readability)"
  - "Paragraph spacing: 1.5em (breathing room)"
  - "Contrast: =4.5:1 (WCAG AA)"
```

---

## ?? Iterative Design Process

### Design Critique & Feedback
_Como Sofia recebe e incorpora feedback_

**Exemplo**:
```yaml
- critique_session: "Weekly Design Review"
  participants: "Sofia + Paula (PO) + Maria (Analyst) + Tiago (Dev)"
  format:
    1. "Sofia apresenta designs (5-10min)"
    2. "Silent review (todos anotam feedback - 3min)"
    3. "Discussão estruturada (30min)"
  feedback_categories:
    functional: "Fluxo faz sentido? Ações claras?"
    usability: "Usuário consegue completar task?"
    aesthetic: "Visual apelativo? Brand-aligned?"
    technical: "Feasible? Performance concerns?"
  
- feedback_incorporation:
  high_priority: "Functional issues (blocker - refazer design)"
  medium_priority: "Usability improvements (iterar próxima versão)"
  low_priority: "Aesthetic preferences (considerar se alinhado)"
  reject: "Feedback fora do escopo ou contradiz user research"
  
- example_feedback:
  paula_po: "Adicionar filtro por data (business requirement)"
  action: "? Incorporado (high priority - functional)"
  
  tiago_dev: "Animação complexa demais (performance concern)"
  action: "? Simplificado (high priority - technical)"
  
  maria_analyst: "Botão verde pode confundir (não é ação positiva)"
  action: "?? A/B test (medium priority - validar com data)"
  
  personal_preference: "Prefiro fonte sans-serif (opinião)"
  action: "? Reject (design system usa Segoe UI)"
```

---

## ?? Cross-References

### Artifacts Relacionados:
- UX Checklist: `${AVANADE_TASK_UX_CHECKLIST}`
- Advanced Elicitation: `${AVANADE_TASK_ADVANCED_ELICITATION}`
- PRD Template: `${AVANADE_PRD_TEMPLATE_YAML}`

### Agent Collaboration:
```yaml
supervisor: ${AVANADE_MEMORY_SUPERVISOR}
analyst: ${AVANADE_MEMORY_ANALYST_MARIA}
po: ${AVANADE_MEMORY_PO_PAULA}
dev: ${AVANADE_MEMORY_DEV_TIAGO}
qa: ${AVANADE_MEMORY_QA_CARLA}
```

---

## ?? Como Usar Esta Memória

### ? ANTES de design:
1. Consultar **User Personas** ? entender usuário-alvo
2. Revisar **User Research Insights** ? evitar erros passados
3. Consultar **Design System Patterns** ? reutilizar components

### ? DURANTE design:
1. Aplicar **Visual Design Principles** ? hierarquia, cores, tipografia
2. Seguir **Low-Fi to Hi-Fi Progression** ? validar early, iterar
3. Validar **Accessibility Compliance** ? WCAG AA checklist

### ? PARA handoff:
1. Usar **Design Handoff Process** ? assets, specs, documentation
2. Colaborar com Tiago ? alignment técnico
3. Documentar **Edge Cases** ? evitar rework

### ? APÓS user testing:
1. **Atualizar memória** com novos insights
2. Documentar **User Journey Maps** ? flows validados
3. Incorporar **Feedback** ? continuous improvement

---

## ?? D365 CE UX Context - FTD Educação

### Aplicativos FTD
| App | Tipo | Público |
|-----|------|--------|
| **Spartan** | Model-driven | Consultores comerciais |
| **PNLD** | Model-driven | Área pública (funcionalidades diferentes) |
| **Hub SAC** | Model-driven | CRC (atendimentos) |
| **Simulador Comercial** | Power Pages | Consultores (frontend novo) |
| **Área do Cliente** | Canvas App | Escolas (squad separada) |

### UX do Simulador Comercial (Foco Principal)
- **Problema original**: Consultor leva até 3 HORAS para 1 proposta (200 produtos × 3 cliques)
- **Solução**: Power Pages com adição em lote (de 200 cliques para ~5)
- **Validação UX**: Prototipos validados com coordenadores, anjas, time UX FTD (gravações de sessões)
- **Identity visual**: Já definida (identidade FTD aplicada)
- **Mobile**: Responsivo (consultores usam tablet na escola)
- **Princípios**: Cache heavy (sem loading repetitivo), cálculos real-time, totalizadores sempre visíveis
- **Resumo financeiro side-by-side**: Perspectiva FTD (deduções) vs Perspectiva Escola (investimentos)

### Padrões de Form FTD
- Campos reorganizados: mover taxa admin e modalidade entrega para etapas posteriores
- Funil de oportunidade: navegabilidade ruim (precisa melhoria)
- Botões customizados: validar serviços, gerar grade mestre
- 50+ templates Word (precisa modularizar)
- Status flow: Rascunho ? Ativo/Em Aprovação ? Aprovado / Perdido / Cancelado

### Personas FTD Reais
- **Consultor Comercial**: negocia com escolas, cria propostas (dor: 3h por proposta)
- **Anja**: cadastra proposta no lugar do consultor (quando consultor recusa)
- **Coordenador/Gerente**: aprova propostas (níveis 2-3)
- **Diretor Adjunto/Comercial**: aprova propostas (níveis 3-4)
- **Equipe Administrativa**: monta contratos manualmente (50+ templates)
- **Consultor PNLD**: app separado, não pode criar contas

### Knowledge Base: `docs/ftd-knowledge-base.md` (LEITURA OBRIGATÓRIA)

---
