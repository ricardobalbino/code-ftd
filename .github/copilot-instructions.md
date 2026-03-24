# Avanade Method - VSCode Integration

## Contexto do Projeto: FTD Educação
**LEITURA OBRIGATÓRIA antes de qualquer tarefa:**
- `docs/ftd-knowledge-base.md` - Knowledge base completo da FTD
- `docs/discovery/ftd-discovery.md` - Discovery parcial
- `docs/especificacao-simulador-notion.md` - Spec Simulador
- `.avanade-method/configs/d365-config.yaml` - Config D365
- `.github/instructions/avanade-bca-guidelines.instructions.md` - Diretrizes BCA
- `docs/diretriz-avanade-inventory.md` - Inventário BCA

**Stack**: D365, Power Pages, Azure, TOTVS, ISA, Adobe Sign
**Tipo**: Brownfield

## Personas MCP Ativas:
- joao-pm: Gestão/PRDs
- wilson-architect: Arquitetura
- maria-analyst: Análise/Discovery
- roberto-sm: Scrum Master
- carla-qa: Qualidade/QA
- tiago-dev: Desenvolvimento
- paula-po: Product Ownership
- sofia-ux: UX Design
- paige-tech-writer: Documentação

## Comandos Metodológicos:
- #avanade-method create prd
- #avanade-method create architecture
- #avanade-method create story
- #avanade-method validate [doc]

## 🤖 Governança Automática e Fluxo de Transcrição (Gatilhos):

Sempre que um novo documento for detectado ou o contexto for alterado, o agente deve agir proativamente:

1. **Gatilho de Transcrição (FLOW-DOC)**: Ao detectar nova entrada em `docs/transcriçao/`, o agente deve:
   - **Triagem**: Mover arquivos técnicos para `docs/arquitetura/`.
   - **Extração**: Atualizar `ftd-knowledge-base.md` (Processos) e `ftd-discovery.md` (Gaps/Fits).
   - **Aviso**: Notificar o usuário sobre os documentos atualizados.
   - **Stories**: Se houver novos requisitos claros, sugerir o acionamento do **WF-06** para gerar Stories.

2. **Consolidação**: NUNCA crie pastas `docs/` dentro de `.avanade-method/`. Documentos de projeto vão sempre para a raiz `docs/`.
3. **Higiene de Links**: Corrigir links antigos de `.avanade-method/docs/` para `docs/` assim que encontrados.
4. **Sincronização**: Manter o `config.yaml` atualizado com as novas specs criadas.

SEMPRE use MCP artifacts via mcp_avanade-metho_search_artifacts() e mcp_avanade-metho_get_artifact().
