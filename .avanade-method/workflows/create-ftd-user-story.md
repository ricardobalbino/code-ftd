---
description: "Workflow para gerar User Stories automatizadas no padrão FTD Educação"
---

# /create-user-story

Este workflow orienta o agente na geração de histórias de usuário (PBIs) de alta qualidade, garantindo que o contexto D365, as dores da FTD e os padrões técnicos da Avanade estejam presentes.

## Entrada Necessária
- O agente deve ter acesso à transcrição de discovery (ex: `docs/discovery/...`) ou à especificação de negócio.

## Protocolo de Execução

1. **Título (Padrão FTD)**:
   - `[Módulo] - Título Descritivo` (Ex: `[Simulador] - Adição de produtos em lote`)

2. **Persona & Valor**:
   - Descrição no formato: "Como [Persona], desejo [funcionalidade], para que [valor de negócio]."
   - **IMPORTANTE**: Referenciar as dores FTD (ex: "para reduzir o tempo de 3 horas de criação de proposta").

3. **Contexto Técnico (D365)**:
   - Entidades envolvidas (Account, Proposta, etc.).
   - Impacto no **Dataverse Storage** (propor alternativas caso envolva arquivos).
   - Localização da lógica (Plugin, Power Automate, Azure Function).

4. **Critérios de Aceite (Gherkin)**:
   - No mínimo 3 cenários (Caminho Feliz, Erro de Validação, Limite de Sistema).
   - Referenciar o `d365-data-model.md` para campos obrigatórios.

5. **Backlog de Tarefas Técnicas (Obrigatório)**:
   - Quebrar a Story em tarefas granulares.
   - Categorias: Data Model, Plugins, Power Automate, UI/UX, Segurança, QA.
   - Atribuir executores: **Tiago Dev**, **Wilson Arch**, **Carla QA**.

6. **Definição de Pronto (DoD) FTD**:
   - [ ] Codificado com prefixo `ftd_`.
   - [ ] Unit Test (FakeXrmEasy) com min 80% cobertura.
   - [ ] Solution Checker sem erros críticos.
   - [ ] Documentação de Arquitetura em `docs/arquitetura/` atualizada.
   - [ ] Evidência de execução em ambiente DEV.
   - [ ] Validado pelo workflow `/validate-ftd-compliance`.

---

// turbo
**Comando rápido**: `create-user-story`
