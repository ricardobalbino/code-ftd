---

## 📋 O que é este Artefato?

Este guia documenta os princípios, componentes e práticas do **Microsoft Fluent Design System** adaptado para uso em projetos Avanade Method. Serve como referência central para designers e desenvolvedores garantirem consistência visual e experiência de usuário.

---

## 🎯 Quando Usar

Use este guia quando:
- ✅ Iniciar design de uma nova interface
- ✅ Escolher componentes UI para implementação
- ✅ Definir paleta de cores e design tokens
- ✅ Validar acessibilidade e responsividade
- ✅ Criar wireframes ou protótipos de alta fidelidade
- ✅ Fazer code review de componentes UI

---

## 🌟 Princípios do Fluent Design

### 1. **Light** (Iluminação)
Criar foco e hierarquia através de luz e sombra.

**Implementação**:
- **Elevation (Profundidade)**: Usar sombras para indicar hierarquia
  - `depth_4`: Componentes no nível da superfície (cards)
  - `depth_8`: Componentes ligeiramente elevados (dropdowns, tooltips)
  - `depth_16`: Modais e overlays
  - `depth_64`: Elementos em primeiro plano (menus contextuais)

**Exemplo CSS**:
```css
/* Depth 8 - Dropdown menu */
.dropdown {
  box-shadow: 0 3.2px 7.2px rgba(0,0,0,0.13), 
              0 0.6px 1.8px rgba(0,0,0,0.11);
}
```

---

### 2. **Depth** (Profundidade)
Usar camadas e paralaxe para criar sensação de espaço tridimensional.

**Implementação**:
- Z-index consistente
- Overlays semi-transparentes
- Transições suaves entre camadas

**Hierarquia de Z-index**:
- Base content: `z-index: 0`
- Cards/Panels: `z-index: 1`
- Sticky headers: `z-index: 100`
- Dropdowns: `z-index: 1000`
- Tooltips: `z-index: 2000`
- Modals: `z-index: 10000`
- Toast notifications: `z-index: 20000`

---

### 3. **Motion** (Movimento)
Animações intencionais que guiam atenção e facilitam compreensão.

**Princípios**:
- **Propósito**: Toda animação deve ter uma razão (feedback, transição, atenção)
- **Duração**: Rápida mas perceptível (150-300ms)
- **Easing**: Natural (ease-in-out)

**Timings Padrão**:
```css
/* Fast - Micro-interactions */
--duration-fast: 150ms;

/* Normal - Transições padrão */
--duration-normal: 300ms;

/* Slow - Transições complexas */
--duration-slow: 500ms;

/* Easing */
--easing-standard: cubic-bezier(0.4, 0.0, 0.2, 1);
--easing-decelerate: cubic-bezier(0.0, 0.0, 0.2, 1);
--easing-accelerate: cubic-bezier(0.4, 0.0, 1, 1);
```

**Exemplos de Uso**:
- **Hover**: 150ms
- **Focus**: 150ms
- **Dropdown open**: 300ms
- **Modal fade-in**: 300ms
- **Page transition**: 500ms

---

### 4. **Material** (Material)
Superfícies e texturas que conectam o digital ao físico.

**Implementação**:
- **Acrylic**: Efeito de vidro translúcido (backgrounds)
- **Reveal**: Destacar elementos ao passar o mouse
- **Blur**: Fundos desfocados para modais

**Acrylic Background**:
```css
.acrylic-background {
  background: rgba(243, 242, 241, 0.7);
  backdrop-filter: blur(30px);
  -webkit-backdrop-filter: blur(30px);
}
```

---

### 5. **Scale** (Escala)
Design responsivo que se adapta a múltiplos dispositivos e contextos.

**Breakpoints Padrão**:
- **Mobile**: 0 - 767px
- **Tablet**: 768px - 1023px
- **Desktop**: 1024px - 1365px
- **Large Desktop**: 1366px+

**Grid System**:
- **Colunas**: 12 (desktop), 8 (tablet), 4 (mobile)
- **Gutter**: 24px (desktop), 16px (mobile)
- **Margin**: 32px (desktop), 16px (mobile)

---

## 🎨 Design Tokens

### Cores (Color Palette)

#### Primary Colors (Azure Theme)
```yaml
themePrimary: "#0078D4"           # Ações primárias, links, botões
themeLighterAlt: "#EFF6FC"
themeLighter: "#DEECF9"
themeLight: "#C7E0F4"
themeTertiary: "#71AFE5"
themeSecondary: "#2B88D8"
themeDarkAlt: "#106EBE"
themeDark: "#005A9E"
themeDarker: "#004578"
```

#### Neutral Colors
```yaml
# Texto
neutralPrimary: "#323130"        # Texto principal
neutralSecondary: "#605E5C"      # Texto secundário
neutralTertiary: "#A19F9D"       # Texto desabilitado

# Backgrounds
white: "#FFFFFF"
neutralLighterAlt: "#FAF9F8"
neutralLighter: "#F3F2F1"
neutralLight: "#EDEBE9"
neutralQuaternaryAlt: "#E1DFDD"
neutralQuaternary: "#D2D0CE"
neutralTertiaryAlt: "#C8C6C4"

# Bordas
neutralTertiary: "#A19F9D"
neutralSecondary: "#605E5C"
neutralPrimaryAlt: "#3B3A39"
neutralPrimary: "#323130"
neutralDark: "#201F1E"
black: "#000000"
```

#### Semantic Colors
```yaml
# Success (verde)
successText: "#107C10"
successBackground: "#DFF6DD"
successIcon: "#107C10"

# Error (vermelho)
errorText: "#A4262C"
errorBackground: "#FDE7E9"
errorIcon: "#A80000"

# Warning (amarelo/laranja)
warningText: "#797673"
warningBackground: "#FFF4CE"
warningIcon: "#F7630C"

# Info (azul)
infoText: "#0078D4"
infoBackground: "#F3F2F1"
infoIcon: "#0078D4"
```

#### Uso de Cores

**Texto**:
- **Primário**: `neutralPrimary` (#323130) - Títulos, corpo de texto
- **Secundário**: `neutralSecondary` (#605E5C) - Subtítulos, texto auxiliar
- **Desabilitado**: `neutralTertiary` (#A19F9D) - Campos desabilitados

**Backgrounds**:
- **Página**: `white` (#FFFFFF)
- **Card/Panel**: `white` com `depth_4` shadow
- **Hover**: `neutralLighter` (#F3F2F1)
- **Pressed**: `neutralLight` (#EDEBE9)

**Bordas**:
- **Default**: `neutralLight` (#EDEBE9)
- **Focus**: `themePrimary` (#0078D4), 2px
- **Error**: `errorText` (#A4262C), 2px

---

### Tipografia (Typography)

#### Font Family
```css
font-family: 'Segoe UI', -apple-system, BlinkMacSystemFont, 'Roboto', 'Oxygen', 
             'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue', 
             sans-serif;
```

#### Type Ramp (Escala Tipográfica)

| Variant | Size | Weight | Line Height | Uso |
|---------|------|--------|-------------|-----|
| **mega** | 68px | 600 | 92px | Hero headlines (raro) |
| **xLargePlus** | 32px | 600 | 40px | Page titles |
| **xLarge** | 24px | 600 | 32px | Section headers |
| **large** | 20px | 600 | 28px | Sub-section headers |
| **mediumPlus** | 18px | 600 | 24px | Card titles |
| **medium** | 14px | 600 | 20px | Labels, small headings |
| **small** | 12px | 400 | 16px | Captions, metadata |
| **xSmall** | 10px | 400 | 14px | Micro text (timestamps) |

#### Body Text
```css
/* Body 1 - Texto padrão */
font-size: 14px;
font-weight: 400;
line-height: 20px;
color: var(--neutralPrimary);

/* Body 2 - Texto secundário */
font-size: 14px;
font-weight: 400;
line-height: 20px;
color: var(--neutralSecondary);
```

#### Font Weights
- **Regular**: 400 (body text)
- **Semibold**: 600 (headings, labels)
- **Bold**: 700 (raramente usado, apenas ênfase)

---

### Espaçamento (Spacing)

Sistema de espaçamento baseado em múltiplos de 4px:

```yaml
spacing-xs: 4px       # Espaçamento mínimo
spacing-sm: 8px       # Pequeno (entre elementos relacionados)
spacing-md: 16px      # Médio (padrão entre componentes)
spacing-lg: 24px      # Grande (entre seções)
spacing-xl: 32px      # Extra grande (margens externas)
spacing-xxl: 48px     # Espaçamento generoso (hero sections)
```

**Aplicação**:
- **Gap entre elementos inline**: 8px
- **Padding interno de botão**: 16px horizontal, 8px vertical
- **Margin entre parágrafos**: 16px
- **Padding de cards**: 24px
- **Margin entre seções**: 32px ou 48px

---

### Border Radius (Arredondamento)

```yaml
radius-none: 0px
radius-small: 2px      # Botões, inputs
radius-medium: 4px     # Cards, panels (padrão)
radius-large: 8px      # Modais, grandes containers
radius-circular: 50%   # Avatares, badges circulares
```

---

## 🧩 Componentes Fluent UI

### Button (Botões)

#### Variantes

**PrimaryButton** (Ação principal):
```jsx
<PrimaryButton text="Salvar" onClick={handleSave} />
```
- Background: `themePrimary`
- Texto: `white`
- Hover: `themeDarkAlt`
- Uso: 1 por tela (ação mais importante)

**DefaultButton** (Ação secundária):
```jsx
<DefaultButton text="Cancelar" onClick={handleCancel} />
```
- Background: `white`
- Borda: `neutralTertiary`
- Texto: `neutralPrimary`
- Hover: `neutralLighter`

**CompoundButton** (Botão com descrição):
```jsx
<CompoundButton 
  text="Upload Arquivo" 
  secondaryText="Selecione um arquivo do seu computador"
  icon={<UploadIcon />}
/>
```

**IconButton** (Apenas ícone):
```jsx
<IconButton iconProps={{ iconName: 'Delete' }} title="Excluir" />
```
- Usar para ações secundárias/toolbar
- Sempre fornecer `title` (tooltip)

#### Estados
- **Default**: Estado inicial
- **Hover**: Background mais escuro (themeDarkAlt para Primary)
- **Pressed**: Background ainda mais escuro (themeDark)
- **Disabled**: Opacidade 0.6, `cursor: not-allowed`
- **Focus**: Borda azul 2px (themePrimary), outline offset 2px

#### Tamanhos
- **Small**: height 24px, padding 0 8px
- **Medium** (default): height 32px, padding 0 16px
- **Large**: height 40px, padding 0 20px

---

### TextField (Campos de Texto)

```jsx
<TextField
  label="Email"
  placeholder="seu.email@exemplo.com"
  required
  errorMessage="Email inválido"
  description="Usaremos para recuperação de senha"
/>
```

#### Propriedades
- **label**: Texto acima do campo
- **placeholder**: Hint dentro do campo
- **required**: Marca campo como obrigatório (asterisco)
- **errorMessage**: Exibe erro abaixo do campo
- **description**: Texto de ajuda (abaixo do campo)
- **multiline**: Transforma em textarea
- **disabled**: Desabilita campo

#### Estados
- **Default**: Borda `neutralLight`
- **Focus**: Borda `themePrimary` (2px), label anima para cima
- **Error**: Borda `errorText`, exibe errorMessage
- **Disabled**: Background `neutralLighter`, opacidade reduzida

#### Variantes
- **TextField**: Input padrão
- **MaskedTextField**: Com máscara (CPF, telefone)
- **SearchBox**: Input de busca com ícone

---

### Dropdown (Seleção)

```jsx
<Dropdown
  label="País"
  placeholder="Selecione um país"
  options={[
    { key: 'br', text: 'Brasil' },
    { key: 'us', text: 'Estados Unidos' },
    { key: 'uk', text: 'Reino Unido' }
  ]}
  selectedKey={selectedCountry}
  onChange={handleCountryChange}
/>
```

#### Tipos
- **Dropdown**: Seleção única
- **ComboBox**: Seleção com busca/autocomplete
- **DropdownMultiselect**: Seleção múltipla

---

### Checkbox & Toggle

**Checkbox** (Seleções múltiplas):
```jsx
<Checkbox 
  label="Aceito os termos de uso" 
  checked={agreed}
  onChange={handleAgree}
/>
```

**Toggle** (Estados binários on/off):
```jsx
<Toggle 
  label="Notificações" 
  checked={notificationsEnabled}
  onText="Ativado"
  offText="Desativado"
  onChange={handleToggle}
/>
```

**Quando usar**:
- **Checkbox**: Aceitar termos, selecionar múltiplos itens
- **Toggle**: Ativar/desativar features (configurações)

---

### MessageBar (Notificações)

```jsx
<MessageBar messageBarType={MessageBarType.success}>
  Alterações salvas com sucesso!
</MessageBar>
```

#### Tipos
- **info** (azul): Informações gerais
- **success** (verde): Ação completada
- **warning** (amarelo): Atenção necessária
- **error** (vermelho): Erro crítico
- **severeWarning** (vermelho escuro): Erro muito crítico

#### Uso
- Exibir no topo do formulário/página
- Auto-dismiss para success/info após 5s
- Manter visível para errors (requer ação do usuário)

---

### Modal (Modais/Diálogos)

```jsx
<Modal
  isOpen={isModalOpen}
  onDismiss={handleClose}
  isBlocking={false}
>
  <Stack tokens={{ childrenGap: 16 }} styles={{ root: { padding: 24 } }}>
    <Text variant="xLarge">Confirmar Exclusão</Text>
    <Text>Tem certeza que deseja excluir este item?</Text>
    <Stack horizontal tokens={{ childrenGap: 8 }}>
      <PrimaryButton text="Confirmar" onClick={handleConfirm} />
      <DefaultButton text="Cancelar" onClick={handleClose} />
    </Stack>
  </Stack>
</Modal>
```

#### Propriedades
- **isOpen**: Controla visibilidade
- **onDismiss**: Callback ao fechar (Esc ou click fora)
- **isBlocking**: Se true, não fecha ao clicar fora
- **isModeless**: Modal não-bloqueante (pode interagir com background)

#### Overlay
- Background: `rgba(0,0,0,0.4)` (semi-transparente)
- Blur no background (opcional, usa backdrop-filter)

---

### Panel (Painel Lateral)

```jsx
<Panel
  isOpen={isPanelOpen}
  onDismiss={handlePanelClose}
  type={PanelType.medium}
  headerText="Filtros"
>
  {/* Conteúdo do painel */}
</Panel>
```

#### Tamanhos
- **smallFixedFar**: 272px
- **medium**: 340px
- **large**: 592px
- **extraLarge**: 940px
- **custom**: Largura customizada

#### Uso
- Filtros avançados
- Formulários de criação/edição
- Detalhes de item selecionado

---

### CommandBar (Barra de Comandos)

```jsx
<CommandBar
  items={[
    { key: 'new', text: 'Novo', iconProps: { iconName: 'Add' }, onClick: handleNew },
    { key: 'upload', text: 'Upload', iconProps: { iconName: 'Upload' } }
  ]}
  farItems={[
    { key: 'settings', iconProps: { iconName: 'Settings' }, onClick: handleSettings }
  ]}
/>
```

#### Uso
- Ações principais de uma página
- Toolbar de aplicação
- Ações de seleção (bulk actions)

---

### DetailsList (Tabela/Lista)

```jsx
<DetailsList
  items={items}
  columns={columns}
  selectionMode={SelectionMode.multiple}
  onItemInvoked={handleItemClick}
/>
```

#### Features
- Ordenação por coluna (sortable)
- Seleção (single/multiple)
- Paginação
- Filtros inline
- Grouping (agrupamento)

#### Colunas
```jsx
const columns = [
  { key: 'name', name: 'Nome', fieldName: 'name', minWidth: 100, maxWidth: 200, isResizable: true },
  { key: 'email', name: 'Email', fieldName: 'email', minWidth: 150, isResizable: true }
];
```

---

### Persona (Avatar + Informações)

```jsx
<Persona
  text="Maria Silva"
  secondaryText="Analista de Negócios"
  imageUrl="/avatars/maria.jpg"
  size={PersonaSize.size48}
/>
```

#### Tamanhos
- **size24**: 24x24px (inline, listas)
- **size32**: 32x32px (padrão)
- **size48**: 48x48px (cards)
- **size72**: 72x72px (perfil)
- **size100**: 100x100px (hero/destaque)

#### Initiials
Se `imageUrl` não fornecida, exibe iniciais do nome com background colorido.

---

### Spinner (Loading)

```jsx
<Spinner label="Carregando dados..." size={SpinnerSize.large} />
```

#### Tamanhos
- **xSmall**: 12px
- **small**: 16px
- **medium**: 20px
- **large**: 28px

#### Uso
- Carregamento de página: Large, centralizado
- Carregamento de componente: Medium/Small, inline
- Botão em loading: Small, dentro do botão

---

### ProgressIndicator (Barra de Progresso)

```jsx
<ProgressIndicator 
  label="Upload em progresso" 
  description="45% concluído"
  percentComplete={0.45}
/>
```

**Uso**:
- Upload de arquivos
- Processamento em lote
- Wizards/formulários multi-step

---

### Dialog (Diálogo Simples)

```jsx
<Dialog
  hidden={!isDialogOpen}
  onDismiss={handleClose}
  dialogContentProps={{
    type: DialogType.normal,
    title: 'Confirmar Ação',
    subText: 'Esta ação não pode ser desfeita.'
  }}
>
  <DialogFooter>
    <PrimaryButton onClick={handleConfirm} text="Confirmar" />
    <DefaultButton onClick={handleClose} text="Cancelar" />
  </DialogFooter>
</Dialog>
```

#### Tipos
- **normal**: Diálogo padrão
- **largeHeader**: Com header visual destacado
- **close**: Com X para fechar

---

## ♿ Acessibilidade (WCAG 2.1 AA)

### Contraste de Cores

**Requisitos WCAG 2.1 - 1.4.3**:
- **Texto normal**: Mínimo 4.5:1
- **Texto grande** (18pt/24px+): Mínimo 3:1
- **UI Components**: Mínimo 3:1

**Validação**:
```
✅ Texto preto (#323130) em branco (#FFFFFF): 12.63:1
✅ themePrimary (#0078D4) em branco: 4.54:1
❌ neutralTertiary (#A19F9D) em branco: 2.63:1 - NÃO usar para texto
```

**Ferramentas**:
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- axe DevTools (extensão Chrome)
- WAVE (extensão Chrome)

---

### Navegação por Teclado

**Requisitos WCAG 2.1 - 2.1.1**:

**Tab Order**:
- Seguir ordem visual (top-to-bottom, left-to-right)
- Elementos interativos acessíveis via Tab
- Skip links para conteúdo principal

**Atalhos**:
- **Tab**: Próximo elemento
- **Shift + Tab**: Elemento anterior
- **Enter/Space**: Ativar botão/link
- **Esc**: Fechar modal/dropdown
- **Arrow keys**: Navegar em listas/menus
- **/**: Focar campo de busca (comum em apps)

**Focus Indicators**:
```css
:focus-visible {
  outline: 2px solid var(--themePrimary);
  outline-offset: 2px;
  border-radius: 2px;
}
```

---

### Screen Readers

**ARIA Labels**:
```jsx
{/* Botão apenas com ícone */}
<IconButton 
  iconProps={{ iconName: 'Delete' }} 
  ariaLabel="Excluir item"
  title="Excluir"
/>

{/* Input com descrição */}
<TextField
  label="Email"
  ariaDescribedBy="email-description"
/>
<Text id="email-description">Usaremos para recuperação de senha</Text>

{/* Notificação dinâmica */}
<div role="alert" aria-live="polite">
  Item adicionado ao carrinho
</div>
```

**Landmarks**:
```jsx
<header role="banner">...</header>
<nav role="navigation">...</nav>
<main role="main">...</main>
<aside role="complementary">...</aside>
<footer role="contentinfo">...</footer>
```

**Testes**:
- **NVDA** (Windows, grátis)
- **JAWS** (Windows, pago)
- **VoiceOver** (macOS, built-in)
- **TalkBack** (Android)

---

### Forms Acessíveis

```jsx
<Label htmlFor="email" required>Email</Label>
<TextField
  id="email"
  ariaRequired={true}
  ariaDescribedBy="email-error"
  errorMessage="Email inválido"
/>
<Text id="email-error" role="alert">
  {errorMessage}
</Text>
```

**Validação**:
- Labels associados (htmlFor)
- Campos required marcados
- Erros anunciados (role="alert")
- Instruções antes do formulário

---

## 📱 Responsividade

### Mobile-First Approach

**Estratégia**:
1. Design para mobile primeiro
2. Progressivamente adicionar features para telas maiores (progressive enhancement)

### Adaptações por Breakpoint

**Mobile (<768px)**:
- Navigation: Hamburger menu
- Forms: Full-width inputs
- Buttons: Stack verticalmente
- Tables: Cards ou horizontal scroll
- Grid: 4 colunas
- Typography: Reduzir tamanhos (H1: 24px)

**Tablet (768px - 1023px)**:
- Navigation: Collapsed horizontal ou side nav
- Grid: 8 colunas
- Forms: Max-width 600px, centered

**Desktop (>=1024px)**:
- Full navigation
- Grid: 12 colunas
- Hover states ativos
- Tooltips (não disponíveis em touch)

### Touch Targets

**WCAG 2.1 - 2.5.5**:
- **Mínimo**: 44x44px (iOS), 48x48dp (Android)
- **Recomendado**: 48x48px ou maior

```css
.touch-target {
  min-width: 48px;
  min-height: 48px;
  /* Se conteúdo menor, adicionar padding */
}
```

---

## 🎭 Padrões de Interação

### Empty States

```jsx
<Stack 
  horizontalAlign="center" 
  verticalAlign="center" 
  tokens={{ childrenGap: 16 }}
  styles={{ root: { minHeight: 400 } }}
>
  <Icon iconName="EmptyRecycleBin" style={{ fontSize: 64, color: '#A19F9D' }} />
  <Text variant="xLarge">Nenhum item encontrado</Text>
  <Text variant="medium" style={{ color: '#605E5C' }}>
    Comece adicionando seu primeiro item
  </Text>
  <PrimaryButton text="Adicionar Item" iconProps={{ iconName: 'Add' }} />
</Stack>
```

---

### Loading States

**Skeleton Screens** (preferido para listas/cards):
```jsx
<Shimmer />
<ShimmerElementsGroup
  shimmerElements={[
    { type: ShimmerElementType.circle, height: 40 },
    { type: ShimmerElementType.gap, width: 16 },
    { type: ShimmerElementType.line, height: 16, width: '40%' }
  ]}
/>
```

**Spinners** (para carregamento inicial):
```jsx
<Spinner label="Carregando dados..." size={SpinnerSize.large} />
```

---

### Error States

```jsx
<MessageBar messageBarType={MessageBarType.error} isMultiline>
  <Text><strong>Erro ao salvar</strong></Text>
  <Text>Não foi possível conectar ao servidor. Tente novamente.</Text>
  <Link onClick={handleRetry}>Tentar novamente</Link>
</MessageBar>
```

---

### Success States

```jsx
<MessageBar 
  messageBarType={MessageBarType.success}
  onDismiss={handleDismiss}
  dismissButtonAriaLabel="Fechar"
>
  Alterações salvas com sucesso!
</MessageBar>
```

---

## 📐 Layout Patterns

### App Shell

```
┌─────────────────────────────────┐
│          Header (64px)          │ ← Fixed
├─────────────────────────────────┤
│         │                       │
│   Nav   │    Main Content      │
│  (240px)│    (fluid)           │
│         │                       │
│         │                       │
└─────────┴───────────────────────┘
```

---

### Card Layout

```jsx
<Stack 
  horizontal 
  wrap 
  tokens={{ childrenGap: 16 }}
>
  <Card styles={{ root: { width: 300 } }}>
    <Card.Item>
      <Image src="/..." />
    </Card.Item>
    <Card.Section>
      <Text variant="large">Card Title</Text>
      <Text variant="small">Card description...</Text>
    </Card.Section>
    <Card.Item>
      <PrimaryButton text="Ação" />
    </Card.Item>
  </Card>
</Stack>
```

---

### Master-Detail Pattern

```
┌──────────────┬────────────────────┐
│              │                    │
│   Master     │      Detail        │
│   (List)     │   (Selecionado)    │
│              │                    │
│  - Item 1    │  Título            │
│  ▶ Item 2 ◀  │  Descrição...      │
│  - Item 3    │  [Ações]           │
│              │                    │
└──────────────┴────────────────────┘
```

---

## 🔗 Integração com Outros Artefatos

- **${AVANADE_WIREFRAME_TEMPLATE}**: Usar este guia como referência ao criar wireframes
- **${AVANADE_MEMORY_UX_SOFIA}**: Documentar patterns validados baseados neste guia
- **${AVANADE_TASK_UX_CHECKLIST}**: Validar implementação contra este guia
- **${AVANADE_TASK_ACCESSIBILITY_WCAG}**: Aplicar requisitos de acessibilidade deste guia

---

## 📚 Recursos Adicionais

### Documentação Oficial
- [Fluent UI React Documentation](https://developer.microsoft.com/en-us/fluentui#/controls/web)
- [Fluent Design System](https://www.microsoft.com/design/fluent/)
- [Fluent UI Icons](https://uifabricicons.azurewebsites.net/)

### Ferramentas
- [Figma Fluent UI Kit](https://www.figma.com/@microsoft)
- [Theme Designer](https://fabricweb.z5.web.core.windows.net/pr-deploy-site/refs/heads/master/theming-designer/)
- [Color Contrast Analyzer](https://www.tpgi.com/color-contrast-checker/)

### Exemplos
- [Fluent UI React Northstar](https://fluentsite.z22.web.core.windows.net/)
- [Office UI Fabric Samples](https://github.com/OfficeDev/office-ui-fabric-react)

---