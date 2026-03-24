# Framework de Governança - Dynamics 365 & Power Platform com Avanade Code

> 📋 **Versão**: 1.0 (Draft Inicial)  
> 📅 **Última atualização**: 20/Mar/2026  
> 👥 **Público-alvo**: Times de desenvolvimento D365/Power Platform usando Avanade BCA

---

## 🎯 Objetivo

Este framework estabelece padrões, processos e melhores práticas para projetos Dynamics 365 CE e Power Platform que utilizam **Avanade Code (BCA v3)**, garantindo:

- ✅ **Setup consistente** entre todos os desenvolvedores
- ✅ **Governança clara** para trabalho colaborativo
- ✅ **Sincronização eficiente** de artefatos e código
- ✅ **Qualidade** através de padrões obrigatórios
- ✅ **Fluxo de trabalho** otimizado do desenvolvimento ao deploy

---

## 📚 Documentação

### 🚀 Getting Started
1. [Setup Inicial](./docs/01-setup-inicial.md) - Ferramentas, instalação e configuração
2. [Licenças e Permissões](./docs/02-licencas-permissoes.md) - O que você precisa ter
3. [Estrutura de Projeto](./docs/03-estrutura-projeto.md) - Como organizar o projeto

### 👥 Governança Multi-Dev
4. [Trabalho Colaborativo](./docs/04-trabalho-colaborativo.md) - Múltiplos devs no mesmo projeto
5. [Sincronização de Artefatos](./docs/05-sincronizacao-artefatos.md) - Manter todos atualizados
6. [Resolução de Conflitos](./docs/06-resolucao-conflitos.md) - Como lidar com colisões

### 🔄 Workflow de Desenvolvimento
7. [Fluxo de Trabalho Dev](./docs/07-workflow-desenvolvimento.md) - Do código ao deploy
8. [Branching Strategy](./docs/08-branching-strategy.md) - Git flow para D365
9. [Code Review](./docs/09-code-review.md) - Padrões de revisão
10. [CI/CD Pipelines](./docs/10-pipelines-cicd.md) - Automação de build e deploy

### 📐 Padrões Avanade BCA
11. [Padrões de Código](./docs/11-padroes-codigo.md) - Backend (C#) e Frontend (TS)
12. [Testes e Qualidade](./docs/12-testes-qualidade.md) - Unit tests, coverage, quality gates
13. [Documentação](./docs/13-documentacao.md) - O que e como documentar

### 🛠️ Ferramentas e Utilitários
14. [Ferramentas Obrigatórias](./docs/14-ferramentas-obrigatorias.md) - Stack técnico
15. [Scripts Úteis](./docs/15-scripts-uteis.md) - Automações e facilitadores
16. [Troubleshooting](./docs/16-troubleshooting.md) - Problemas comuns e soluções

---

## 🎓 Quick Start (5 min)

```powershell
# 1. Clone o repositório
git clone [url-do-repo]
cd [nome-projeto]

# 2. Execute o script de setup
.\scripts\setup-dev-environment.ps1

# 3. Configure suas credenciais D365
.\scripts\configure-d365-connection.ps1

# 4. Valide a instalação
.\scripts\validate-setup.ps1

# 5. Pronto! Comece a desenvolver
```

---

## 🏗️ Arquitetura do Framework

```
.avanade-method/
├── framework/                    # Este framework
│   ├── docs/                    # Documentação completa (16 docs)
│   ├── templates/               # Templates de código e config
│   ├── scripts/                 # Scripts de automação
│   └── checklists/             # Checklists de validação
│
├── configs/                     # Configurações do projeto
│   ├── d365-config.yaml        # Config D365 (ambientes, conexões)
│   └── pipeline-config.yaml    # Config pipelines
│
└── src/                        # Código-fonte
    ├── Backend/                # Plugins, Custom APIs (C#)
    ├── Frontend/               # Forms, Scripts (TypeScript)
    └── Solutions/              # Solutions CRM
```

---

## ⚡ Princípios do Framework

1. **🔒 Segurança First** - Nunca commitar credenciais ou dados sensíveis
2. **📦 Isolamento** - Cada dev trabalha em sua branch/feature
3. **🔄 Sincronização Frequente** - Pull diário da `develop`
4. **✅ Quality Gates** - Build passa? Code review ok? Só então merge
5. **📝 Documentação Viva** - Atualizar docs junto com código
6. **🤝 Padrões Avanade** - 100% compliance com BCA v3

---

## 🚨 Pré-requisitos Críticos

Antes de começar, você **DEVE** ter:

- [ ] Visual Studio 2022 (17.8+) com workload ".NET desktop development"
- [ ] Power Platform CLI (pac CLI) instalado
- [ ] Git configurado com SSH
- [ ] Acesso ao Azure DevOps do projeto
- [ ] Credenciais D365 válidas (Service Account ou User)
- [ ] Node.js 18+ e npm (para scripts TypeScript)
- [ ] Licença Power Apps Developer Plan ou superior

➡️ **Detalhes completos**: [Setup Inicial](./docs/01-setup-inicial.md)

---

## 👥 Cenários de Uso

### Cenário 1: Novo Dev Entrando no Projeto
1. Leia [Setup Inicial](./docs/01-setup-inicial.md)
2. Execute scripts de configuração
3. Valide sua instalação
4. Faça sua primeira branch e commit

### Cenário 2: Múltiplos Devs na Mesma Solution
1. Siga [Trabalho Colaborativo](./docs/04-trabalho-colaborativo.md)
2. Use [Sincronização de Artefatos](./docs/05-sincronizacao-artefatos.md)
3. Resolva conflitos com [Resolução de Conflitos](./docs/06-resolucao-conflitos.md)

### Cenário 3: Deploy em Produção
1. Certifique-se que [Code Review](./docs/09-code-review.md) foi aprovado
2. Valide [Quality Gates](./docs/12-testes-qualidade.md)
3. Execute [Pipeline de Deploy](./docs/10-pipelines-cicd.md)

---

## 📊 Status do Framework

| Componente | Status | Prioridade |
|------------|--------|-----------|
| Setup Inicial | 🟡 Draft | 🔴 Alta |
| Licenças | 🟡 Draft | 🔴 Alta |
| Trabalho Colaborativo | 🟡 Draft | 🔴 Alta |
| Sincronização | 🟡 Draft | 🔴 Alta |
| Workflow Dev | 🟡 Draft | 🟢 Média |
| Branching | 🟡 Draft | 🟢 Média |
| Code Review | 🟡 Draft | 🟢 Média |
| Pipelines | ⚪ To Do | 🟡 Baixa |
| Padrões BCA | 🟡 Draft | 🔴 Alta |
| Testes | ⚪ To Do | 🟢 Média |
| Scripts | ⚪ To Do | 🟡 Baixa |
| Templates | ⚪ To Do | 🟡 Baixa |

**Legenda**: 🟢 Completo | 🟡 Draft | ⚪ A Fazer

---

## 🤝 Contribuindo

Este framework é **vivo** e deve evoluir com o projeto:

1. Encontrou um problema? Documente em `troubleshooting`
2. Criou um script útil? Adicione em `scripts/`
3. Tem uma melhoria? Abra um PR ou discussão
4. Atualizar documentação faz parte do trabalho!

---

## 📞 Contatos e Suporte

- **Tech Lead**: [Nome] - [email]
- **Architect**: [Nome] - [email]
- **DevOps**: [Nome] - [email]
- **Avanade BCA Docs**: [Link interno Avanade]

---

## 📜 Changelog

### v1.0 - 20/Mar/2026 (Inicial)
- ✨ Criação do framework base
- 📝 Estrutura de 16 documentos
- 🎯 Definição de princípios e objetivos
- 📋 README com quick start

---

**🚀 Vamos começar! Leia o [Setup Inicial](./docs/01-setup-inicial.md) e configure seu ambiente.**
