---
description: Como auditar o código para estar 100% conforme o Avanade BCA
---

# WF-07: Guardião de Conformidade BCA

Este workflow automatiza a auditoria técnica de código (Code Review Assistido) conforme as regras do projeto FTD Educação.

## Passo 1: Auditoria de Padrões (Tiago Dev)

Peça ao Tiago para realizar o seu próprio self-check antes de um push:

> "Tiago, audite minhas alterações recentes usando o manual de diretrizes `.github/instructions/avanade-bca-guidelines.instructions.md`. Verifique especificamente se as camadas (Service/Plugin) estão respeitadas."

## Passo 2: Script de Nomenclatura (PowerShell)

Execute este script para encontrar arquivos que fogem do prefixo `ftd_` (conforme manual):

```powershell
# Lista arquivos C#, TS ou JS que não começam com ftd_ (se for o padrão)
Get-ChildItem -Recurse -Include *.cs,*.ts,*.js | Where-Object { $_.Name -notmatch "^ftd_" } | Select-Object Name, FullName
```

## Passo 3: Check de Documentação Técnica

Verifique se as novas classes ou métodos têm documentação técnica básica:

```powershell
# Busca por classes sem comentários XML/Doc
Select-String -Path "**/*.cs" -Pattern "public class" -NotMatch "/// <summary>"
```

---
> [!IMPORTANT]
> Este workflow reduz as rodadas de code review manual da Carla (QA), pois o Tiago (Dev) já faz o pré-check automaticamente.
