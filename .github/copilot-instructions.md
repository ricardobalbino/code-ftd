# Avanade Method - VSCode Integration

## Contexto do Projeto: FTD Educação
**LEITURA OBRIGATÓRIA antes de qualquer tarefa:**
- `.avanade-method/docs/ftd-knowledge-base.md` - Knowledge base completo da FTD (processos, integrações, ambientes, glossário)
- `.avanade-method/docs/discovery/ftd-discovery.md` - Discovery parcial com fit-gap e gaps de investigação
- `.avanade-method/docs/especificacao-simulador-notion.md` - Especificação completa do Simulador Comercial (Oscar, 501 linhas)
- `.avanade-method/configs/d365-config.yaml` - Configuração D365 com dados reais (pain points, process, environments)
- `.github/instructions/avanade-bca-guidelines.instructions.md` - Diretrizes BCA de desenvolvimento Avanade (padrões obrigatórios)
- `.avanade-method/docs/diretriz-avanade-inventory.md` - Inventário completo 197 docs BCA (referência detalhada)

**Stack**: D365 CE Online (Sales, CS), Power Pages (Simulador), Azure Functions, TOTVS/Datasul, ISA, Adobe Sign
**Tipo**: Brownfield - customizações múltiplas em ambiente existente

## Personas MCP Ativas:
- joao-pm: Gestão de projetos e criação de PRDs
- wilson-architect: Arquitetura técnica e decisões arquiteturais
- maria-analyst: Business analysis e discovery
- roberto-sm: Scrum Master e gestão de sprints
- carla-qa: Quality assurance e code review
- tiago-dev: Desenvolvimento e implementação
- paula-po: Product ownership e priorização
- sofia-ux: UX Design e wireframes
- paige-tech-writer: Documentação técnica

## Comandos Metodológicos:
- #avanade-method create prd
- #avanade-method create architecture
- #avanade-method create story
- #avanade-method validate [doc]

SEMPRE use MCP artifacts via mcp_avanade-metho_search_artifacts() e mcp_avanade-metho_get_artifact().
