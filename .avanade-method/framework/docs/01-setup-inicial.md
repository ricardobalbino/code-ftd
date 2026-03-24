# Setup Inicial - Ambiente de Desenvolvimento D365/Power Platform

> 📋 **Documento**: 01 - Setup Inicial  
> 🎯 **Objetivo**: Configurar ambiente de desenvolvimento completo  
> ⏱️ **Tempo estimado**: 45-60 minutos  
> 👤 **Público**: Todos os desenvolvedores

---

## 📦 O Que Você Vai Instalar

Este guia cobre a instalação e configuração de **TODAS** as ferramentas necessárias para desenvolvimento em projetos Dynamics 365 CE / Power Platform com Avanade BCA v3.

---

## 🔧 Ferramentas Obrigatórias

### 1. Visual Studio 2022 (17.8 ou superior)

**Por quê**: IDE principal para desenvolvimento backend (Plugins, Custom APIs, Early Bound)

**Download**: https://visualstudio.microsoft.com/downloads/

**Workloads necessários**:
- ✅ `.NET desktop development`
- ✅ `ASP.NET and web development`
- ✅ `Azure development` (opcional, mas recomendado)

**Extensões VS obrigatórias**:
```powershell
# Execute no Package Manager Console do VS
Install-Package Microsoft.CrmSdk.CoreAssemblies
Install-Package Microsoft.CrmSdk.Workflow
Install-Package Microsoft.CrmSdk.XrmTooling.CoreAssembly
```

**Extensões VS recomendadas** (instalar via Extensions → Manage Extensions):
- ☑️ Power Platform Tools
- ☑️ XrmToolBox Extension
- ☑️ Resharper ou SonarLint (análise de código)

---

### 2. Power Platform CLI (pac CLI)

**Por quê**: Ferramenta CLI oficial Microsoft para Power Platform

**Instalação**:
```powershell
# Método 1: Via winget (recomendado)
winget install Microsoft.PowerPlatformCLI

# Método 2: Via MSI direto
# Download: https://aka.ms/PowerPlatformCLI
```

**Validação**:
```powershell
pac --version
# Deve mostrar versão 1.30+ ou superior
```

**Configuração inicial**:
```powershell
# Autenticar no ambiente
pac auth create --url https://[seu-ambiente].crm2.dynamics.com

# Listar ambientes disponíveis
pac org list

# Selecionar ambiente padrão
pac org select --environment [environment-id]
```

---

### 3. Git + SSH

**Por quê**: Controle de versão e colaboração

**Instalação Git**:
```powershell
winget install Git.Git
```

**Configuração SSH** (para Azure DevOps):
```powershell
# Gerar chave SSH
ssh-keygen -t rsa -b 4096 -C "seu.email@avanade.com"

# Copiar chave pública
cat ~/.ssh/id_rsa.pub | clip

# Adicionar em Azure DevOps: User Settings → SSH public keys → Add
```

**Configuração Git global**:
```powershell
git config --global user.name "Seu Nome"
git config --global user.email "seu.email@avanade.com"
git config --global core.autocrlf true
git config --global init.defaultBranch main
```

---

### 4. Node.js + npm

**Por quê**: Para scripts TypeScript, ferramentas frontend e automação

**Instalação**:
```powershell
# Versão LTS (18.x ou superior)
winget install OpenJS.NodeJS.LTS
```

**Validação**:
```powershell
node --version  # v18.x.x ou superior
npm --version   # 9.x.x ou superior
```

**Configuração npm**:
```powershell
# Configurar registry corporativo Avanade (se houver)
npm config set registry https://[registry-avanade-interno]

# Instalar ferramentas globais úteis
npm install -g typescript
npm install -g eslint
npm install -g prettier
```

---

### 5. XrmToolBox

**Por quê**: Suite de ferramentas essenciais para trabalhar com CRM

**Download**: https://www.xrmtoolbox.com/

**Plugins obrigatórios** (instalar via Tool Library no XrmToolBox):
- ✅ **Plugin Registration Tool** - Registrar plugins e steps
- ✅ **Early Bound Generator** - Gerar classes early bound
- ✅ **FetchXML Builder** - Construir queries FetchXML
- ✅ **Metadata Browser** - Explorar metadata do CRM
- ✅ **Solution Packager** - Unpack/pack solutions

**Plugins recomendados**:
- ☑️ Ribbon Workbench
- ☑️ User Settings Utility
- ☑️ View Designer
- ☑️ Workflow Elements Builder

---

### 6. Spkl (Avanade BCA Tool)

**Por quê**: Ferramenta CLI Avanade para deploy de plugins e Early Bound

**Instalação**:
```powershell
# Via dotnet tool
dotnet tool install --global spkl

# Validar
spkl --version
```

**Configuração no projeto**:
```powershell
# Criar arquivo spkl.json na raiz do projeto Backend
# Ver template em: .avanade-method/framework/templates/spkl.json
```

---

## 🔐 Credenciais e Acessos Necessários

### Azure DevOps
- [ ] Acesso ao projeto Azure DevOps
- [ ] Permissões de **Contributor** no repositório
- [ ] Acesso aos **Pipelines**
- [ ] Acesso à **Library** (para secrets)

### Dynamics 365 / Power Platform
- [ ] **Service Account** ou **User Account** com permissões para:
  - ✅ Importar/Exportar solutions
  - ✅ Registrar plugins e custom APIs
  - ✅ Criar/modificar entidades (em DEV)
  - ✅ Acesso ao ambiente DEV
  - ✅ (Opcional) Acesso ao ambiente OAT para testes

### Licenças Necessárias
- [ ] **Microsoft 365 E3/E5** (conta corporativa Avanade)
- [ ] **Power Apps Developer Plan** ou superior
  - Solicitar em: https://powerapps.microsoft.com/developerplan/
- [ ] **Visual Studio Subscription** (fornecido pela Avanade)

---

## 🚀 Setup do Projeto (First Time)

### Passo 1: Clonar Repositório
```powershell
# 1. Navegar para pasta de projetos
cd C:\Users\[seu-user]\source\repos

# 2. Clonar projeto
git clone git@ssh.dev.azure.com:v3/[org]/[project]/[repo]

# 3. Entrar na pasta
cd [nome-do-repo]

# 4. Verificar branch
git branch -a
```

### Passo 2: Criar Arquivo de Configuração Local

**Criar `.env.local`** na raiz (NÃO versionar este arquivo):
```ini
# .env.local (NUNCA COMMITAR ESTE ARQUIVO!)
D365_URL=https://[seu-ambiente].crm2.dynamics.com
D365_USERNAME=seu.usuario@avanade.com
D365_PASSWORD=[use credential manager, NÃO salve aqui]
D365_DOMAIN=
D365_AUTHTYPE=Office365
```

**Adicionar ao `.gitignore`**:
```
# Arquivos locais de configuração
.env.local
*.user
appsettings.Development.json
```

### Passo 3: Restaurar Dependências
```powershell
# Backend (C#)
cd src/Backend
dotnet restore

# Frontend (TypeScript) - se houver
cd ../Frontend
npm install
```

### Passo 4: Baixar Early Bound Classes
```powershell
# Via pac CLI
pac modelbuilder build --outdirectory "src/Backend/EarlyBound"

# OU via XrmToolBox → Early Bound Generator
# Configurar: CrmSvcUtil → Generate
```

### Passo 5: Validar Build
```powershell
# Build de tudo
dotnet build

# Deve compilar sem erros
# Warnings podem existir, mas erros = problema de setup
```

---

## ✅ Checklist de Validação

Execute este checklist para garantir que está tudo OK:

```powershell
# 1. Validar Visual Studio
# Abra o .sln e compile (Ctrl+Shift+B)

# 2. Validar pac CLI
pac org list

# 3. Validar Git
git status

# 4. Validar Node
node --version

# 5. Validar spkl
spkl --version

# 6. Validar conexão D365
pac solution list
# Deve listar solutions do ambiente
```

**Se TODOS passarem ✅ → Você está pronto!**

---

## 🛠️ Scripts de Automação

O projeto contém scripts PowerShell para automatizar o setup:

```powershell
# Setup completo (executa todos os passos)
.\scripts\setup-dev-environment.ps1

# Apenas configurar conexão D365
.\scripts\configure-d365-connection.ps1

# Validar setup
.\scripts\validate-setup.ps1

# Atualizar dependências
.\scripts\update-dependencies.ps1
```

---

## 🚨 Problemas Comuns

### "pac not recognized"
**Causa**: PATH não atualizado após instalação  
**Solução**: Feche e reabra o PowerShell/Terminal

### "Unable to connect to Dynamics 365"
**Causa**: Credenciais inválidas ou ambiente offline  
**Solução**: 
1. Verificar URL do ambiente
2. Testar login via browser
3. Verificar se service account está desbloqueado

### "Build failed - missing references"
**Causa**: Packages NuGet não restaurados  
**Solução**: `dotnet restore` na pasta do projeto

### "Git permission denied"
**Causa**: SSH não configurado  
**Solução**: Voltar na seção "Git + SSH" e configurar chave

---

## 📚 Próximos Passos

Após concluir o setup:

1. ✅ Leia [Estrutura de Projeto](./03-estrutura-projeto.md)
2. ✅ Entenda [Trabalho Colaborativo](./04-trabalho-colaborativo.md)
3. ✅ Familiarize-se com [Workflow de Desenvolvimento](./07-workflow-desenvolvimento.md)
4. ✅ Revise [Padrões de Código BCA](./11-padroes-codigo.md)

---

## 🤝 Precisa de Ajuda?

- **Tech Lead do Projeto**: [nome/email]
- **Avanade BCA Docs**: [link interno]
- **Troubleshooting**: [16-troubleshooting.md](./16-troubleshooting.md)

---

**📝 Última atualização**: 20/Mar/2026 | **Versão**: 1.0 (Draft)
