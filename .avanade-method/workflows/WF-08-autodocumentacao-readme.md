---
description: Como manter o README.md 100% atualizado com a estrutura real das pastas
---

# WF-08: Sincronização Automática de README (Árvore)

Este workflow garante que a visualização da árvore de diretórios no `README.md` seja gerada a partir do estado real do sistema, evitando documentação "mentirosa".

## Passo 1: Geração de Árvore Limpa (PowerShell)

Execute este comando para gerar o novo mapa de diretórios (filtrando o que não importa):

```powershell
# Gera árvore ignorando pastas de compilação/temp (.git, bin, obj)
tree /a /f | Select-String -NotMatch "\.git|node_modules|bin|obj|\.vs" | select -first 60
```

## Passo 2: Atualização do README.md

Peça ao agente (ou Supervisor):

> "Atualize a seção 'Estrutura do Repositório' no `README.md` com a saída do comando tree gerado no Passo 1, preservando as anotações explicativas."

## Passo 3: Verificação de Links na Wiki

O agente deve passar um "scanner" simples para ver se os links da Wiki ainda batem com a nova árvore gerada.

---
> [!TIP]
> Um repositório auto-documentado é um sinal de maturidade técnica e facilita imensamente o onboarding de novos desenvolvedores.
