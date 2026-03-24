---

## ?? O que é este Artefato?

Este é o **guia de referência CommonMark** para criar Markdown válido e portável. CommonMark é a especificação padrão de Markdown, garantindo que documentos renderizem consistentemente em qualquer plataforma.

**Por que CommonMark?**
- ? **Portabilidade**: Funciona em GitHub, GitLab, VS Code, Docusaurus, etc.
- ? **Consistência**: Sintaxe bem definida (não "flavors" contraditórios)
- ? **Validação**: Linters podem verificar conformidade
- ? **Acessibilidade**: Semântica clara para screen readers

---

## ?? Quando Usar

### ? USE para:
- Toda documentação técnica em Markdown
- README files, guides, tutorials, API docs
- GitHub/GitLab wikis e issues
- Validar Markdown com linters (markdownlint)

### ? NÃO USE para:
- Plataformas com Markdown customizado (se precisa features não-CommonMark)
- Rich text editors (Google Docs, Word)

---

## ?? COMMONMARK QUICK REFERENCE

### 1. Headings (ATX Style)

```markdown
# Heading 1 (H1)
## Heading 2 (H2)
### Heading 3 (H3)
#### Heading 4 (H4)
##### Heading 5 (H5)
###### Heading 6 (H6)

RULES:
- Espaço após # é obrigatório: "# Title" (não "#Title")
- Apenas 1 H1 por documento
- Não pular níveis (H1 ? H2 ? H3, não H1 ? H3)
- Max depth: H4 (deeper = poor information architecture)
```

**Rendered:**
# Heading 1
## Heading 2
### Heading 3

---

### 2. Emphasis & Strong

```markdown
*Italic text* or _italic text_
**Bold text** or __bold text__
***Bold and italic*** or ___bold and italic___

BEST PRACTICE:
- Use * for italic (mais comum)
- Use ** for bold
- Avoid mixing * and _ no mesmo documento
```

**Rendered:**
- *Italic text*
- **Bold text**
- ***Bold and italic***

---

### 3. Lists

#### Unordered Lists
```markdown
- First item
- Second item
  - Nested item (2 spaces indent)
  - Another nested item
- Third item

RULES:
- Use - (hyphen) consistently (pode usar * ou +, mas seja consistente)
- 2 ou 4 spaces para nesting (seja consistente)
- Linha em branco antes/depois de lista para clareza
```

**Rendered:**
- First item
- Second item
  - Nested item
  - Another nested item
- Third item

#### Ordered Lists
```markdown
1. First item
2. Second item
   1. Nested item (3 spaces indent)
   2. Another nested item
3. Third item

RULES:
- Números não precisam ser sequenciais (1., 1., 1. funciona)
- Mas RECOMENDADO: use números corretos (1., 2., 3.)
- 3 spaces para nesting (alinha com texto do item pai)
```

**Rendered:**
1. First item
2. Second item
   1. Nested item
   2. Another nested item
3. Third item

#### Task Lists (GitHub Flavored)
```markdown
- [ ] Unchecked task
- [x] Checked task
- [ ] Another task

?? NÃO É CommonMark PURO (GitHub extension)
Mas amplamente suportado
```

**Rendered:**
- [ ] Unchecked task
- [x] Checked task
- [ ] Another task

---

### 4. Links

#### Inline Links
```markdown
[Link text](https://example.com)
[Link with title](https://example.com "Hover title")

RULES:
- Texto descritivo (não "click here")
- URLs devem ser absolute ou relative válidas
```

**Rendered:**
[Link text](https://example.com)

#### Reference Links
```markdown
[Link text][reference-id]
[Another link][reference-id]

[reference-id]: https://example.com "Optional title"

USEFUL FOR:
- Múltiplos links para mesma URL
- Manter texto limpo (referências no fim do documento)
```

**Rendered:**
[Link text][ref]

[ref]: https://example.com

#### Autolinks
```markdown
<https://example.com>
<email@example.com>

RULES:
- Wrap URL/email em < >
- Renderiza como clicável automaticamente
```

**Rendered:**
<https://example.com>

---

### 5. Images

```markdown
![Alt text](image.png)
![Alt text](image.png "Optional title")

RULES:
- Alt text é OBRIGATÓRIO para acessibilidade
- Alt deve descrever imagem, não apenas "image" ou "screenshot"
- Prefer relative paths para images no mesmo repo
```

**Rendered:**
![Example of a login form with username and password fields](https://via.placeholder.com/300x200)

#### Reference-Style Images
```markdown
![Alt text][image-ref]

[image-ref]: image.png "Optional title"
```

---

### 6. Code

#### Inline Code
```markdown
Use `backticks` para inline code.

Examples:
- Run `npm install` to install dependencies.
- The `getUserById()` function returns a user object.

RULES:
- Use para commands, function names, variables, file names
```

**Rendered:**
Run `npm install` to install dependencies.

#### Code Blocks (Fenced)
````markdown
```javascript
function hello(name) {
    return `Hello, ${name}!`;
}
```

RULES:
- Use ``` (3 backticks) antes e depois
- Especificar linguagem (javascript, python, bash, etc.)
- Linguagem deve ser lowercase
````

**Rendered:**
```javascript
function hello(name) {
    return `Hello, ${name}!`;
}
```

#### Code Blocks (Indented)
```markdown
    Indent 4 spaces para code block
    Sem syntax highlighting
    Menos preferido que fenced blocks

AVOID: Use fenced blocks (```) ao invés de indented
```

---

### 7. Blockquotes

```markdown
> This is a blockquote.
> It can span multiple lines.
>
> And include multiple paragraphs.

NESTING:
> Level 1
>> Level 2
>>> Level 3

RULES:
- Use > no início de cada linha
- Linha em branco (apenas >) para novo parágrafo dentro de quote
```

**Rendered:**
> This is a blockquote.
> It can span multiple lines.

---

### 8. Horizontal Rules

```markdown
---
(3 ou mais hyphens)

***
(3 ou mais asterisks)

___
(3 ou mais underscores)

BEST PRACTICE:
- Use --- (mais visível)
- Linha em branco antes/depois
```

**Rendered:**

---

---

### 9. Tables (GitHub Flavored)

```markdown
| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Row 1 Col 1 | Row 1 Col 2 | Row 1 Col 3 |
| Row 2 Col 1 | Row 2 Col 2 | Row 2 Col 3 |

ALIGNMENT:
| Left | Center | Right |
|:-----|:------:|------:|
| L1   | C1     | R1    |
| L2   | C2     | R2    |

RULES:
- Outer pipes (|) são opcionais mas recomendados
- Alinhamento: :--- (left), :---: (center), ---: (right)
- Cells podem conter inline Markdown (bold, italic, code, links)
```

**Rendered:**

| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Row 1 Col 1 | Row 1 Col 2 | Row 1 Col 3 |
| Row 2 Col 1 | Row 2 Col 2 | Row 2 Col 3 |

?? **NÃO É CommonMark PURO** (GitHub/GitLab extension)
Mas amplamente suportado em platforms de documentação

---

### 10. Line Breaks

```markdown
Método 1: Duas ou mais spaces no fim da linha  
Próxima linha será nova linha (hard break).

Método 2: Backslash no fim da linha\
Próxima linha será nova linha.

BEST PRACTICE:
- Use backslash \ (mais visível)
- Ou simplesmente deixe linha em branco para novo parágrafo
```

**Rendered:**
Line 1 (two spaces after)  
Line 2

Line 1 (backslash after)\
Line 2

---

### 11. Escaping Characters

```markdown
Use backslash \ para escapar caracteres especiais:

\* Asterisk (sem italic)
\_ Underscore (sem italic)
\# Hash (sem heading)
\[ Square bracket (sem link)
\\ Backslash literal

SPECIAL CHARS QUE PRECISAM ESCAPE:
\ ` * _ { } [ ] ( ) # + - . ! | <
```

**Rendered:**
\* Not italic \*
\# Not a heading

---

## ?? TEMPLATE COMPLETO DE DOCUMENTO

```markdown
# Document Title

**Author**: [Nome]  
**Date**: YYYY-MM-DD  
**Status**: Draft | Review | Published

---

## Overview

Brief introduction explaining what this document covers and who it's for.

## Table of Contents

- [Section 1](#section-1)
- [Section 2](#section-2)
- [Section 3](#section-3)

---

## Section 1

### Subsection 1.1

Regular paragraph text with **bold** and *italic* emphasis.

#### Code Example

```python
def example_function(param):
    """Docstring explaining what this does."""
    return param * 2
```

#### Key Points

- Point 1 with inline `code`
- Point 2 with [link](https://example.com)
- Point 3 with **emphasis**

---

## Section 2

### Subsection 2.1

> **Note**: This is a blockquote for important information.

#### Process Steps

1. First step - describe action clearly
2. Second step - include expected result
3. Third step - provide troubleshooting if needed

#### Reference Table

| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Data 1   | Data 2   | Data 3   |
| Data 4   | Data 5   | Data 6   |

---

## Section 3

### Images

![Descriptive alt text for accessibility](./images/example.png)

### Links & Resources

- [External Resource 1](https://example.com/resource1)
- [Internal Link](./other-doc.md)
- [Reference with title][ref-1]

[ref-1]: https://example.com "Hover title"

---

## Troubleshooting

**Issue**: Common problem description  
**Solution**: How to fix it

**Issue**: Another problem  
**Solution**: Another fix

---

## Next Steps

- [ ] Task 1 to complete
- [ ] Task 2 to complete
- [ ] Task 3 to complete

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-02-03 | Initial release |

---

**Last Updated**: 2025-02-03  
**Maintained By**: [Team Name]
```

---

## ? VALIDATION CHECKLIST

### CommonMark Compliance

- [ ] **Headings**: ATX-style (#), espaço após #, não pular níveis
- [ ] **Lists**: Consistente (- ou *, não mixed), indentação correta
- [ ] **Links**: Formato [text](url) válido, URLs corretas
- [ ] **Images**: Alt text presente, formato ![alt](url)
- [ ] **Code blocks**: Fenced (```) com language tag
- [ ] **Emphasis**: Consistente (* ou _, não mixed)
- [ ] **Escaping**: Caracteres especiais escapados quando necessário
- [ ] **Line breaks**: Duas spaces ou \ para hard breaks

### Rendering Test

- [ ] **Preview localmente**: VS Code, Markdown Preview
- [ ] **Test em target platform**: GitHub, GitLab, Docusaurus
- [ ] **Lint**: Run markdownlint ou similar
- [ ] **Links funcionam**: Todos absolute e relative links válidos
- [ ] **Images carregam**: Paths corretos, arquivos existem

---

## ?? TOOLS & LINTERS

### VS Code Extensions

```json
{
  "recommendations": [
    "davidanson.vscode-markdownlint",  // Linter
    "yzhang.markdown-all-in-one",      // Productivity
    "bierner.markdown-preview-github-styles"  // GitHub preview
  ]
}
```

### Markdownlint Configuration

`.markdownlint.json`:
```json
{
  "default": true,
  "MD013": false,  // Line length (disabled - some code examples are long)
  "MD033": false,  // HTML (allow for complex tables)
  "MD041": false,  // First line heading (allow frontmatter)
  
  "heading-style": { "style": "atx" },
  "ul-style": { "style": "dash" },
  "code-fence-style": { "style": "fenced" }
}
```

### CLI Validation

```bash
# Install markdownlint-cli
npm install -g markdownlint-cli

# Lint single file
markdownlint document.md

# Lint all markdown files
markdownlint **/*.md

# Fix auto-fixable issues
markdownlint --fix **/*.md
```

---

## ?? MARKDOWN FLAVORS COMPARISON

| Feature | CommonMark | GitHub | GitLab | Docusaurus |
|---------|-----------|--------|--------|------------|
| Tables | ? | ? | ? | ? |
| Task Lists | ? | ? | ? | ? |
| Strikethrough | ? | ? (~~text~~) | ? | ? |
| Autolinks | ? | ? | ? | ? |
| Emoji | ? | ? (:smile:) | ? | ? |
| Footnotes | ? | ? | ? | ? |
| Mermaid | ? | ? | ? | ? (plugin) |
| Admonitions | ? | ? | ? | ? (:::note) |

**Recommendation**: Stick to CommonMark + Tables (universally supported)

---

## ?? ADVANCED PATTERNS

### Multi-Paragraph List Items

```markdown
1. First item

   This is a continuation of the first item.
   Multiple paragraphs need to be indented.

   ```python
   # Code blocks inside list items
   print("Also indented")
   ```

2. Second item

   Another paragraph here.
```

**Rendered:**
1. First item

   This is a continuation of the first item.
   Multiple paragraphs need to be indented.

2. Second item

---

### Nested Lists (Complex)

```markdown
- Top level
  - Second level
    - Third level
      - Fourth level (avoid - too deep!)
  - Back to second level
- Back to top level
```

**Best Practice**: Max 3 levels deep

---

### Definition Lists (Not CommonMark)

```markdown
Term
: Definition

Another Term
: Another definition
: Multiple definitions for same term

?? NÃO SUPORTADO em todos platforms
Use HTML <dl> se necessário
```

---

## ?? Integração com Outros Artefatos

- **${AVANADE_DOC_STANDARDS_MD}**: Usa CommonMark como base para formatting
- **${AVANADE_MEMORY_TECH_WRITER_PAIGE}**: Code examples seguem CommonMark
- **${AVANADE_TASK_EDITORIAL_REVIEW_STRUCTURE}**: Valida estrutura CommonMark
- **${AVANADE_MERMAID_LIBRARY_MD}**: Mermaid em code blocks (```)

---

## ?? REFERENCES

- **CommonMark Spec**: <https://commonmark.org/>
- **CommonMark Tutorial**: <https://commonmark.org/help/>
- **Markdown Guide**: <https://www.markdownguide.org/>
- **GitHub Flavored Markdown**: <https://github.github.com/gfm/>

---
