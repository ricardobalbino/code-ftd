---
description: Como garantir a integridade e saúde do repositório
---

# WF-05: Health Check & Manutenção do Repositório

Este workflow automatiza a verificação de integridade dos links e da estrutura do projeto FTD Educação.

## Passo 1: Verificador de Links e Caminhos (PowerShell)

Execute o comando abaixo para encontrar qualquer referência "zumbi" aos caminhos antigos que ainda possam existir:

```powershell
# Busca por referências antigas em todos os arquivos
Get-ChildItem -Recurse -Include *.md,*.yaml,*.json | Select-String -Pattern "avanade-method/docs", "avanade-method\\docs" | Select-Object FileName, LineNumber, Line
```

## Passo 2: Sincronizador de Documentos Mandatórios

Use este script para listar arquivos na pasta `docs/` que talvez você tenha esquecido de colocar no `config.yaml`:

```powershell
# Compara arquivos em docs/ com a lista no config.yaml
$configFiles = Get-Content ".avanade-method/config.yaml" | Select-String "docs/"
Get-ChildItem "docs/*.md" | ForEach-Object {
    if (!($configFiles -match $_.Name)) {
        Write-Host "ALERTA: Documento [ $($_.Name) ] não está no config.yaml" -ForegroundColor Yellow
    }
}
```

## Passo 3: Atualização Automática do README (Estrutura)

Ao rodar este script, a árvore de diretórios do no README.md será atualizada com o estado real das pastas:

```powershell
# Gera a árvore e você pode copiar para o README
tree /f /a | select -first 50 
```

## Passo 4: Limpeza de Memória (Agentes)

Se os agentes começarem a ficar confusos com contextos antigos, rode a tarefa de limpeza da pasta `.avanade-method/memory/`.

---
> [!IMPORTANT]
> Um repositório saudável é o segredo para que os agentes de IA (Maria, Tiago, Paula) trabalhem com 100% de precisão.
