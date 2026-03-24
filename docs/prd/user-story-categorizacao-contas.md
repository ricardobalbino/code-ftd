# User Story: [Cadastro] - Hierarquia de Categorização de Contas (3 Níveis)

**ID**: PBI-002  
**Módulo**: D365 Sales / Cadastro  
**Status**: Refinado (BMAD Pilot)  
**Data**: 20/03/2026

---

## 1. Persona & Valor de Negócio
**Como** Analista Comercial ou Gestor de BI da FTD Educação,  
**Desejo** que o cadastro de Contas (Escolas/Livreiros) siga uma hierarquia rígida de três níveis (Segmento → Tipo de Instituição → Classificação Comercial),  
**Para que** possamos eliminar a poluição de dados no campo atual `tipo_de_instituicao` (que mistura conceitos) e permitir relatórios precisos de market share e segmentação.

---

## 2. Contexto de Negócio (FTD Specific)
- **Problema Atual**: O campo `tipo_de_instituicao` mistura conceitos como "Confessional", "KA" e "Rede Marista" no mesmo nível.
- **Diferenciação**: A FTD precisa separar claramente o que é **Segmento** (Privado/Público) do que é o **Tipo organizacional** (Laica, Confessional) e da **Classificação** (Key Account, Regional).
- **Legado**: O campo `Mantenedora` deve ser exclusivo para o grupo mantenedor, não sendo mais usado para parceiro comercial.

---

## 3. Detalhamento Técnico (D365)
- **Entidade**: `Account` (Conta).
- **Campos a Criar/Atualizar**: 
  - `ftd_segmento`: OptionSet L1 (Privado / Público).
  - `ftd_tipo_organizacional`: OptionSet L2 (Confessional, Laica, Rede, Parceiro, KA).
  - `ftd_classificacao_comercial`: OptionSet L3 (KA, Regional, Especial, Regular).
- **Lógica**: JavaScript no formulário para filtrar as opções de L2 baseado em L1, e L3 baseado em L2 (Cascading Picklists).
- **Dataverse Storage**: Nenhum impacto negativo relevante (campos nativos OptionSet).

---

## 4. Critérios de Aceite (Gherkin)

### Cenário 1: Seleção em Cascata (Privado)
**Dado** que o Analista está criando uma nova Conta,  
**Quando** selecionar o Segmento `Privado`,  
**Então** o campo Tipo Organizacional deve exibir apenas: `Confessional`, `Laica`, `Rede`, `KA`.

### Cenário 2: Seleção em Cascata (Público)
**Dado** que o Analista está criando uma nova Conta,  
**Quando** selecionar o Segmento `Público`,  
**Então** o campo Tipo Organizacional deve exibir apenas: `Municipal`, `Estadual`, `Federal`.

### Cenário 3: Obrigatoriedade de Hierarquia
**Dado** que o Segmento foi preenchido,  
**Quando** o usuário tentar salvar a Conta sem preencher a Classificação Comercial (L3),  
**Então** o sistema deve bloquear o salvamento e emitir um alerta de obrigatoriedade.

---

## 5. Backlog de Tarefas Técnicas (Obrigatório)
| ID Tarefa | Camada | Descrição da Atividade | Executor |
| :--- | :--- | :--- | :--- |
| **TAS-011** | Data Model | Criar os 3 OptionSets Globais e os campos na entidade `Account` conforme especificação. | **Tiago Dev** |
| **TAS-012** | Client Side | Desenvolver JavaScript `ftd_account_categorization.js` para lógica de filtros em cascata (Cascading Picklists). | **Tiago Dev** |
| **TAS-013** | UI/UX | Reorganizar a seção "Resumo do Cliente" no formulário da Conta com os novos campos hierárquicos. | **Paula/UX** |
| **TAS-014** | Data Migration | Criar script (Data Flow ou Data Loader) para mapear o conteúdo do campo antigo `ftd_tipo_instituicao` para a nova hierarquia nos 101k registros. | **Tiago Dev** |
| **TAS-015** | QA/Valid | Validar a integridade dos dados migrados e a funcionalidade de filtro Cross-Browser. | **Carla QA** |

---

## 6. Definition of Done (DoD) - Padrão FTD
- [ ] Codificado com prefixo `ftd_`.
- [ ] Scripts JavaScript minificados e com tratamento de erros.
- [ ] Documentação de Arquitetura em [`docs/arquitetura/d365-data-model.md`](file:///c:/CODE.IA/code-ftd/docs/arquitetura/d365-data-model.md) atualizada.
- [ ] Validado pelo workflow `/validate-ftd-compliance`.

---

**Relacionamento Global**: Refere-se à seção 11 do Knowledge Base FTD e seção 2.1 do Data Model. 🚀
