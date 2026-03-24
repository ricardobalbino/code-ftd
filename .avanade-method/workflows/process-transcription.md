---
description: "Workflow para processar novas transcrições de discovery e atualizar memórias"
---

# /process-transcription

Este workflow orienta o agente na leitura de novos dados de discovery (transcrições) e na atualização da memória do agente para que o aprendizado seja contínuo.

## Protocolo de Execução (Ciclo de Aprendizado Global)

1. **Localizar e Ingerir Nova Transcrição**:
   - Localizar o arquivo `.md` em `docs/discovery/`.
   - Identificar stakeholders, dores, regras e decisões.

2. **Atualizar a "Source of Truth" (Conhecimento Global)**:
   - **Discovery (`docs/discovery/ftd-discovery.md`)**: Adicionar novos itens de Fit/Gap ou pontos de investigação resolvidos.
   - **Knowledge Base (`docs/ftd-knowledge-base.md`)**: Atualizar o glossário ou as seções de processos/integrações se houver mudança.
   - **Arquitetura (`docs/arquitetura/`)**: Se a reunião definiu um padrão técnico novo, o agente deve atualizar o documento correspondente (ex: `padroes-desenvolvimento.md` ou `d365-data-model.md`).
   - **Simulador Comercial (`docs/especificacao-simulador-notion.md`)**: Atualizar regras de cálculo ou fluxos de tela.

3. **Atualizar Memória Individual do Agente**:
   - Somente após atualizar os documentos globais, o agente salva seu aprendizado pessoal em seu `{agent-id}-memory.md`.
   - Registrar: "Dados globais atualizados em [arquivos]. Minha reflexão pessoal: [aprendizado]."

4. **Propagação (Orquestração)**:
   - Se os documentos globais foram alterados, o agente deve informar ao usuário: "Base de conhecimento FTD atualizada. Agora todos os agentes (@supervisor, @wilson-architect, etc.) terão acesso a essa informação no próximo boot."


---

// turbo
**Comando rápido**: `process-transcription`
