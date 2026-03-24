---
description: Como automatizar a triagem e processamento de novas transcrições/documentos
---

# WF-04: Processamento Documental Automático

Este workflow automatiza a organização de arquivos brutos da pasta `docs/transcriçao/` para as pastas de destino corretas e aciona a extração de conhecimento.

## Passo 1: Triagem e Movimentação (Automação Técnica)

Execute o comando abaixo no terminal para mover automaticamente arquivos que contenham termos técnicos para a pasta de Arquitetura:

```powershell
# Localiza arquivos técnicos e os move para docs/arquitetura/
Get-ChildItem "docs/transcriçao/*.txt" | ForEach-Object {
    $content = Get-Content $_.FullName -Raw
    if ($_.Name -match "Técnico|Arquitetura|Onboarding|Pipeline|Ambiente" -or $content -match "Dataverse|CRM|Dynamics|Deploy") {
        Move-Item $_.FullName "docs/arquitetura/$($_.Name)" -Force
        Write-Host "Movido para Arquitetura: $($_.Name)" -ForegroundColor Cyan
    }
}
```

## Passo 2: Extração de Conhecimento (Instrução para Agente)

Após a movimentação, peça ao agente (neste caso, a Maria Analyst):

> "Maria, processe os novos arquivos em `docs/transcriçao/` e `docs/arquitetura/`. Atualize o `docs/ftd-knowledge-base.md` com os processos de negócio novos e o `docs/discovery/ftd-discovery.md` com os novos fits/gaps identificados."

## Passo 3: Atualização de Configuração (Check Final)

Garanta que os novos arquivos importantes (Specs) foram adicionados ao `config.yaml` na seção `devLoadAlwaysFiles`.

---
> [!NOTE]
> Este workflow garante que o conhecimento técnico chegue aos desenvolvedores (na pasta arquitetura) e o conhecimento de negócio chegue aos analistas (na pasta discovery/knowledge-base).
