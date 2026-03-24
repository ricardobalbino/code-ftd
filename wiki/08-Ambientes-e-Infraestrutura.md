# Ambientes e Infraestrutura

> [← Voltar ao Início](./Home.md)

---

## 1. Ambientes D365

| Ambiente | Status | Pipeline | Uso |
|----------|--------|---------|-----|
| **Dev** | ✅ Ativo | Plugin deploy **manual** (pipeline não configurado) | Desenvolvimento e testes locais |
| **OAT (UAT)** | ✅ Ativo | Pipeline automático | Homologação / UAT |
| **Produção** | ✅ Ativo | Pipeline + GMUD via SMAX | Ambiente live |
| **QA** | 🔧 Criado, não configurado | Pendente no pipeline | Testes de qualidade (futuro) |
| **Release Candidate** | 🔧 Criado, não configurado | Pendente no pipeline | RC antes de PROD (futuro) |

---

## 2. Processo de Deploy

### Dev (Desenvolvimento)
```
Dev faz mudança → CLI LocalDeployment → D365 Dev (manual)
```
- Deploy de plugins: **manual** via CLI `LocalDeployment Backend`
- Pipeline de CI/CD para Dev: ainda não configurado

### OAT (Homologação)
```
PR merge → deploy-code pipeline (auto) → OAT
```
- Pipeline automático após merge
- Aprovadores obrigatórios no pipeline

### Produção
```
QA Sign-off → GMUD no SMAX → deploy pipeline (manual) → PROD
```
- Deploy é **SEMPRE MANUAL** — nunca automatizar
- **GMUD**: Giselle monta a estrutura de Gestão de Mudança no SMAX
- **Branch control**: apenas deploys de `refs/heads/main`
- **Subidas manuais**: SOMENTE em casos de urgência/hotfix com liberação específica
- **Exclusive lock**: evitar deploys concorrentes

---

## 3. Azure DevOps

### Organização e Repositório
- **Projeto**: FTD Dynamics (dentro da org FTD Educação)
- **Repo**: `FTD Dynamics` (em migração para nova org)
- **Branches principais**:
  - `dev` → **Source of truth** (não é `master`!)
  - `master` → não usado para deploys atualmente
  - Feature branches: criadas a partir de `dev`

### Problema Atual
> ⚠️ O repositório **NÃO está no mesmo projeto dos PBIs** → impossível vincular commits a features  
> **Migração em andamento**: novo org para unificar repo + PBIs

### Usuário de Serviço: FTDMaxFlow
- Proprietário de todos os Power Automate flows e conexões
- Acesso restrito a: Julio, Fernando e Thiago (sustentação)
- **NUNCA** transferir ownership de flows para usuário pessoal

---

## 4. Pipelines CI/CD (4 Pipelines YAML)

### 4.1 build-guard (CI)
- **Trigger**: Automático — cada commit ou abertura de PR
- **Função**: Build frontend (TypeScript → Webpack) + backend (C# → NuGet) + TODOS os unit tests
- **Resultado**: PR só pode ser mergeado se guard PASSAR

### 4.2 deploy-code
- **Trigger**: Automático — após merge de PR para `main`/`dev`
- **Função**: Deploy dos artifacts para o ambiente Customization Master

### 4.3 deploy (MANUAL)
- **Trigger**: **SEMPRE MANUAL** — aprovação humana obrigatória
- **Função**: Full deploy para upper environments (solutions + data + pre/post steps)
- **Inclui**: Substituição de placeholders `{SecretVariable}` de CSV via Key Vault

### 4.4 export-unpack
- **Trigger**: Opcional (pode ser automático ou manual)
- **Função**: Exporta solutions do Customization Master → desempacota → cria PR automático

---

## 5. Azure Key Vault

- Armazena: tokens, URLs, connection strings para Azure Functions
- **Power Automates**: usam **variáveis de ambiente** (não Key Vault — diferença importante!)
- Integrado ao pipeline via "linked Library" (secrets nunca no repositório)

---

## 6. Soluções D365 (9 Soluções)

Deploy obrigatoriamente em **ordem sequencial** (dependências):

| Ordem | Solution | Conteúdo | Deploy tipo |
|-------|----------|----------|-------------|
| 1 | FTDCore | Data model | Managed (upper) |
| 2 | FTDSecurity | Security roles | Managed |
| 3 | FTDPlugins | Plugins C# assemblies | Managed |
| 4 | FTDFlows | Power Automate flows | Managed |
| 5 | FTDClientExtensions | PCF controls, Web resources | Managed |
| 6 | FTDSiteMap | Navigation, App Modules | Managed |
| 7-9 | Módulos específicos | PNLD, SAC, etc. | Managed |

**Regras**:
- Upper environments: sempre soluções **managed** (nunca unmanaged)
- Usar **patches** para releases incrementais
- **NUNCA** editar solução Default

---

## 7. Monitoramento e Observabilidade

| Ferramenta | Uso |
|-----------|-----|
| **Datadog** | Monitoramento de SLA, erros de integração, volume de pico |
| **Grafana** | Dashboards de performance |
| **Azure Application Insights** | Telemetria de Azure Functions |
| **ITracingService** | Logs dentro de Plugins D365 |
| **Logging Configuration** | Entidade D365 — controla nível de log por plugin |

### Configurações de Logging (Plugin)
- Configurável via entidade `Logging Configuration` no Dataverse
- Suporta: CRM Tracing Service, Windows Debug Output, Azure Application Insights
- **Regra**: Nunca logar PII (dados pessoais) em produção

---

## 8. Azure Functions

| Aspecto | Detalhe |
|---------|---------|
| **Runtime** | .NET 8 Isolated Process |
| **Triggers** | Service Bus (processamento assíncrono), HTTP (chamadas do Simulador) |
| **Secrets** | Azure Key Vault (via DefaultAzureCredential) |
| **Scale** | Auto-scale (consumption plan) |
| **Deploy** | Via pipeline `deploy` (manual para upper envs) |

### Uso no Projeto
- Cálculos de proposta com >50 produtos (sem risco de timeout de 2 min)
- ETL TOTVS ↔ CRM
- Geração de documentos de contrato
- Validações pesadas pós-MVP

---

## 9. Pipeline de Concorrência

> ⚠️ **Pipeline é concorrido com outras áreas** da FTD — pode haver fila  
> Planejamento de deploys deve considerar janelas disponíveis

### Aprovadores Obrigatórios
- Cada ambiente tem aprovadores cadastrados no pipeline Azure DevOps
- PROD: requer GMUD no SMAX + aprovação técnica

---

*Referências: [ftd-knowledge-base.md](../docs/ftd-knowledge-base.md) | [docs/arquitetura/d365-solution-architecture.md](../docs/arquitetura/d365-solution-architecture.md)*
