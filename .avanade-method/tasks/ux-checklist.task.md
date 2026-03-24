## Objetivo
Validar design de interfaces contra princípios de UX, acessibilidade e design systems.

---

## ?? User Experience (UX) Principles

### 1. User-Centered Design
**Critério**: Design serve necessidades reais de usuários, não preferências pessoais

**Checklist**:
- [ ] **User Research**: Personas validadas ou user interviews realizadas
- [ ] **User Journey**: Fluxo mapeado (início ao fim)
- [ ] **Pain Points**: Problemas atuais identificados e endereçados
- [ ] **Jobs-to-be-Done**: "Quando [situação], quero [ação], para que [benefício]"
- [ ] **Success Metrics**: Como mediremos sucesso? (conversion, time-on-task, NPS)

**Exemplo**:
```
? Ruim: "Botão azul porque eu gosto de azul"
? Bom: "Botão azul porque teste A/B mostrou 12% mais conversão vs verde"
```

---

### 2. Accessibility (Acessibilidade) - WCAG 2.1 AA
**Critério**: Design utilizável por todos, incluindo pessoas com deficiências

#### ? Checklist WCAG AA

**Visual**:
- [ ] **Contraste**: Texto 4.5:1, UI components 3:1 (usar WebAIM Contrast Checker)
- [ ] **Tamanho de fonte**: Mínimo 16px (body text)
- [ ] **Zoom**: Conteúdo legível até 200% zoom sem scroll horizontal
- [ ] **Cor não é única**: Informação não depende apenas de cor (adicionar ícones, texto)

**Keyboard Navigation**:
- [ ] **Tab order**: Lógico e sequencial
- [ ] **Focus indicator**: Visível (outline, highlight)
- [ ] **Shortcuts**: Todos controles acessíveis via teclado
- [ ] **Skip links**: "Pular para conteúdo principal"

**Screen Readers**:
- [ ] **Alt text**: Todas imagens têm alt descritivo (ou alt="" se decorativa)
- [ ] **ARIA labels**: Botões/links têm labels claros (`aria-label`, `aria-labelledby`)
- [ ] **Landmarks**: `<header>`, `<nav>`, `<main>`, `<footer>`
- [ ] **Headings**: Hierarquia lógica (H1 ? H2 ? H3, sem pular níveis)

**Forms**:
- [ ] **Labels**: Todo input tem `<label>` associado
- [ ] **Error messages**: Específicas ("Email inválido" não "Erro no campo 3")
- [ ] **Required fields**: Marcados visualmente e com `aria-required`

**Ferramentas de Validação**:
- axe DevTools (browser extension)
- WAVE (webaim.org/wave)
- Screen reader: NVDA (Windows), VoiceOver (Mac)

**Artifact completo**: ${AVANADE_TASK_ACCESSIBILITY_WCAG}

---

### 3. Usability (Usabilidade) - Nielsen's 10 Heuristics
**Critério**: Interface é intuitiva, eficiente, e satisfatória

#### ?? 10 Heurísticas

**1. Visibility of System Status**
- [ ] Loading indicators em operações >1s
- [ ] Feedback visual em ações (botão clicado, item salvo)
- [ ] Progress bars em processos longos

**2. Match Between System and Real World**
- [ ] Linguagem familiar (não jargão técnico)
- [ ] Metáforas do mundo real (ícones reconhecíveis)
- [ ] Ordem lógica (formulário segue fluxo mental do usuário)

**3. User Control and Freedom**
- [ ] Undo/Redo
- [ ] "Cancelar" em processos longos
- [ ] Fácil sair de estados indesejados

**4. Consistency and Standards**
- [ ] Botões primários sempre mesma cor/posição
- [ ] Terminologia consistente ("Salvar" vs "Gravar" ? escolher 1)
- [ ] Seguir padrões de plataforma (iOS HIG, Material Design, Fluent)

**5. Error Prevention**
- [ ] Validação inline (não só no submit)
- [ ] Confirmação em ações destrutivas ("Tem certeza que deseja deletar?")
- [ ] Desabilitar ações inválidas (botão "Enviar" desabilitado se formulário incompleto)

**6. Recognition Rather than Recall**
- [ ] Opções visíveis (não escondidas em menus profundos)
- [ ] Autocomplete em inputs
- [ ] Tooltips em ícones
- [ ] Recently used items

**7. Flexibility and Efficiency of Use**
- [ ] Keyboard shortcuts (Ctrl+S para salvar)
- [ ] Ações em massa (selecionar múltiplos itens)
- [ ] Templates/presets para usuários avançados

**8. Aesthetic and Minimalist Design**
- [ ] Foco no essencial (remove elementos desnecessários)
- [ ] Whitespace adequado (evita sobrecarga visual)
- [ ] Hierarquia visual clara (tamanhos, cores, espaçamento)

**9. Help Users Recognize, Diagnose, and Recover from Errors**
- [ ] Mensagens de erro claras ("Email inválido: formato esperado exemplo@dominio.com")
- [ ] Sugestões de correção ("Você quis dizer: usuario@gmail.com?")
- [ ] Highlight do campo com erro

**10. Help and Documentation**
- [ ] Contextual (ajuda onde usuário precisa, não página separada)
- [ ] Buscável
- [ ] Exemplos visuais (screenshots, vídeos)
- [ ] Concisa

**Artifact completo**: ${AVANADE_TASK_USABILITY_HEURISTICS}

---

### 4. Fluent Design System (Microsoft)
**Critério**: Segue padrões do Fluent Design System

**Checklist**:
- [ ] **Typography**: Fluent typefaces (Segoe UI, Roboto)
- [ ] **Colors**: Palette Fluent (Primary, Neutral, Semantic)
- [ ] **Spacing**: 4px/8px grid system
- [ ] **Components**: Usar Fluent UI components (Button, Input, Card, etc.)
- [ ] **Icons**: Fluent Icons library
- [ ] **Motion**: Subtle animations (duração 200-300ms)

**Resources**:
- Fluent UI: fluent2.microsoft.design
- Fluent Icons: aka.ms/fluentui-icons
- Color tokens: ${AVANADE_FLUENT_DESIGN_GUIDELINES}

**Componentes Obrigatórios**:
- **Buttons**: Primary, Secondary, Tertiary hierarchy
- **Forms**: Input, Select, Checkbox, Radio
- **Feedback**: Toast, Dialog, Progress
- **Navigation**: Navbar, Tabs, Breadcrumb

---

### 5. Responsive Design
**Critério**: Design funciona em todos dispositivos/tamanhos de tela

**Breakpoints** (Fluent):
- Mobile: <640px
- Tablet: 640px - 1024px
- Desktop: >1024px

**Checklist**:
- [ ] **Mobile-first**: Design começa em mobile, expande para desktop
- [ ] **Touch targets**: Mínimo 44x44px (iOS) ou 48x48px (Android)
- [ ] **Readable text**: Sem zoom necessário
- [ ] **Navigation**: Hamburger menu em mobile, navbar em desktop
- [ ] **Tables**: Responsivas (scroll horizontal ou cards em mobile)
- [ ] **Images**: Responsive (`max-width: 100%`)

**Teste**:
- Chrome DevTools (device emulation)
- Dispositivos reais (iPhone, Android, iPad)

---

### 6. Performance (Perceived Performance)
**Critério**: Interface **parece** rápida mesmo se backend é lento

**Checklist**:
- [ ] **Skeleton screens**: Placeholders enquanto carrega
- [ ] **Optimistic UI**: Atualizar UI imediatamente (assumindo sucesso)
- [ ] **Progressive loading**: Mostrar conteúdo à medida que carrega
- [ ] **Lazy loading**: Imagens/componentes fora da viewport carregam depois
- [ ] **Debounce**: Search input não dispara request a cada tecla

**Exemplos**:
```
? Ruim: Loading spinner por 5s
? Bom: Skeleton screen mostra estrutura enquanto carrega dados

? Ruim: Botão "Curtir" espera resposta da API para mudar
? Bom: Botão muda imediatamente (optimistic), reverte se API falhar
```

---

## ?? Scoring System

**Pontuação por categoria** (0-5):
- User-Centered Design: /5
- Accessibility (WCAG AA): /5
- Usability (Nielsen): /5
- Fluent Design System: /5
- Responsive Design: /5
- Performance: /5

**Total: /30**

**Interpretação**:
- **25-30**: ? Excelente UX - Production-Ready
- **20-24**: ?? Boa UX - Melhorias menores
- **15-19**: ?? UX Adequada - Revisar gaps
- **10-14**: ?? UX Problemática - Refazer partes
- **0-9**: ?? UX Inadequada - Redesign necessário

---

## ?? Deliverables (Artefatos de Design)

### Wireframes
- [ ] Low-fi (sketches, papel)
- [ ] Mid-fi (Figma/Sketch, estrutura clara)
- [ ] High-fi (mockups finais com visual design)

### Prototypes
- [ ] Clickable prototype (InVision, Figma)
- [ ] Interactions documentadas (hover, click, transitions)

### Design Specs
- [ ] Component library (Storybook, Figma)
- [ ] Spacing/sizing (margins, paddings, font sizes)
- [ ] Colors/typography
- [ ] Responsive breakpoints

### User Flows
- [ ] User journey maps
- [ ] Task flows (diagrama de fluxo)
- [ ] Edge cases documentados

### Accessibility Report
- [ ] WCAG checklist completo
- [ ] Screen reader testing notes
- [ ] Keyboard navigation validation

---

## ?? Integração com Metodologia Avanade

- **Pré-requisito**: Requirements/User Stories definidas
- **Validação UX**: ${AVANADE_TASK_USABILITY_HEURISTICS}
- **Validação Acessibilidade**: ${AVANADE_TASK_ACCESSIBILITY_WCAG}
- **Validação Adversarial**: ${AVANADE_TASK_ADVERSARIAL_REVIEW}
- **Template**: ${AVANADE_WIREFRAME_TEMPLATE}
- **Memória**: ${AVANADE_MEMORY_UX_SOFIA}

