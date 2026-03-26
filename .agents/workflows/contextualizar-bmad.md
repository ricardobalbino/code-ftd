---
description: Como atualizar toda a referencia de contextualização BMAD via nova transcrição
---

# Fluxo de Atualização de Contexto BMAD (FTD Educação)

Quando uma nova transcrição (discovery, reunião ou refinamento) é adicionada ao repositório, siga este fluxo para garantir que o motor @bmad esteja sempre atualizado com a verdade mais recente.

## 🛠 Ativar Agentes Necessários
Você deve atuar nas personas de **@paige-tech-writer** (para redação técnica) ou **@maria-analyst** (para extração de requisitos).

## 📋 Passo a Passo

### 1. Preparação
Certifique-se de que a nova transcrição está salva em `docs/transcricoes/` ou forneça o caminho do arquivo diretamente.

### 2. Leitura Obrigatória
O agente deve carregar os seguintes arquivos para ter o contexto base:
- `docs/ftd-knowledge-base.md`
- `docs/discovery/ftd-discovery.md`
- `docs/arquitetura/bmad-engine-flow.md`

### 3. Execução do Prompt de Atualização
Use o prompt oficial localizado em `.github/prompts/context-update.prompt.md`.

// turbo
### 4. Extração e Comparação
O agente deve realizar uma varredura na transcrição procurando por:
- **Novas Regras**: "A aprovação do nível 4 agora requer o CPF do sócio".
- **Mudanças de Termos**: "Não chamamos mais de 'Pedido', agora é 'Venda Consultiva'".
- **Dores Críticas**: "O tempo de faturamento no TOTVS subiu para 24h".

### 5. Edição dos Artefatos
O agente deve aplicar as mudanças nos arquivos correspondentes (docs/, wiki/ e /agents).
- **Importante (Arquitetura)**: Se a transcrição trouxer uma nova regra técnica (ex: "Sempre usar FakeXrmEasy em testes"), isso deve ser inserido nos arquivos `.customize.yaml` de todos os agentes relevantes (Tiago, Wilson, etc.).

### 6. Validação (Checklist)
Ao finalizar, verifique se:
- [ ] O glossário foi atualizado.
- [ ] Os participantes da reunião foram adicionados.
- [ ] O roadmap reflete os novos prazos discutidos.
- [ ] Emojis de status (🔴, 🟠, ✅) foram aplicados corretamente.

## 🚀 Comando Rápido (Prompt Sugerido)

"Utilize o workflow `/contextualizar-bmad` para processar o arquivo `docs/transcricoes/[NOME_DO_ARQUIVO].txt` e atualizar toda a referência de contextualização do projeto."
