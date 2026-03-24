---
description: Como automatizar a geração de User Stories a partir de documentos de negócio
---

# WF-06: Gerenciador de Backlog Automático

Este workflow automatiza a criação de histórias de usuário (User Stories) baseadas nos requisitos extraídos dos documentos em `docs/discovery/` ou `docs/prd/`.

## Passo 1: Leitura de Requisito (Gatilho)

Sempre que um novo PRD for adicionado em `docs/prd/`, peça ao agente:

> "Paula (PO), processe o PRD `docs/prd/{seu_arquivo}.md` e gere o arquivo de stories em `docs/stories/{seu_arquivo}-stories.md` usando o template padrão."

## Passo 2: Geração de Estrutura de Story

A Paula (PO) utilizará este prompt automático embutido:
1. Extraia Épicos do PRD.
2. Crie Stories (Título, Descrição, Critérios de Aceitação).
3. Adicione um campo de [Referência] linkando de volta para a linha exata do PRD.

## Passo 3: Sincronização de Backlog

O agente deve adicionar o novo arquivo de stories ao `config.yaml` se necessário e atualizar o `README.md` na seção de entrega.

---
> [!TIP]
> Com este workflow, você nunca mais precisará escrever uma Story do zero. A Paula apenas refinará o rascunho.
