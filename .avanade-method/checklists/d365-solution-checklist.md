# ============================================================
# D365 Solution Checklist - Avanade Method v4.29.0
# ============================================================
# Owner: Carla QA
# When: Antes de exportar/transportar solution entre environments

---

# ✅ D365 Solution Quality Checklist

## 1. Solution Checker (Obrigatório)

- [ ] Solution Checker executado na solution
- [ ] **0 Critical issues**
- [ ] **0 High issues**
- [ ] Medium issues ≤ 10 (documentar exceções)
- [ ] Low issues revisados (ação se critical path)

## 2. Plugin Quality

### Code Standards
- [ ] Todos plugins seguem pattern: Validate → Extract → Execute → Handle
- [ ] `ITracingService` usado para logging (não Console.Write)
- [ ] `InvalidPluginExecutionException` para erros de negócio (mensagem amigável)
- [ ] Depth check implementado (evitar loops infinitos)
- [ ] Sem hard-coded GUIDs ou magic strings
- [ ] Sem `async/await` ou `Thread` em plugins
- [ ] Sem acesso a filesystem, rede ou recursos externos
- [ ] Sem state estático (plugins são stateless)
- [ ] Early-bound entities usados (não strings mágicas)
- [ ] `IOrganizationService` obtido via context (não criado manualmente)

### Testing
- [ ] Unit tests escritos com FakeXrmEasy + xUnit
- [ ] Cobertura ≥ 80%
- [ ] Cenários de sucesso testados
- [ ] Cenários de erro testados (validações, exceções)
- [ ] Edge cases testados (null values, empty collections)

### Registration
- [ ] Plugin registration documentado (entity, message, stage, mode)
- [ ] Filtering attributes configurados (não processar desnecessariamente)
- [ ] Pre/Post images configurados apenas com atributos necessários
- [ ] Async vs Sync definido corretamente

## 3. PCF Control Quality

- [ ] Manifest.xml válido e propriedades tipadas
- [ ] `init/updateView/getOutputs/destroy` implementados
- [ ] Jest tests escritos (cobertura ≥ 70%)
- [ ] Bundle size otimizado (< 5MB)
- [ ] Responsive design (Web + Tablet + Phone)
- [ ] Acessibilidade (keyboard navigation, aria labels)
- [ ] Sem memory leaks (cleanup em destroy)
- [ ] `pac pcf push` testado em environment de dev

## 4. Azure Functions Quality

- [ ] Authentication: Managed Identity ou App Registration (nunca hardcoded)
- [ ] Secrets em Azure Key Vault (nunca em appsettings)
- [ ] Retry policy configurada
- [ ] Application Insights configurado
- [ ] Error handling com logging estruturado
- [ ] Throttling Dataverse API respeitado (6000 req/5min)
- [ ] Unit tests ≥ 80% (xUnit + Moq)
- [ ] Integration tests escritos
- [ ] Health check endpoint (se HTTP trigger)

## 5. Power Automate Flow Quality

- [ ] Nome segue convenção: "FTD - [Module] - [Action] - [Trigger]"
- [ ] Scope Try-Catch-Finally implementado
- [ ] Run-after configurado para failure paths
- [ ] Concurrency control configurado
- [ ] Timeout configurado em actions longas
- [ ] Variables inicializadas no topo do flow
- [ ] Connection references usadas (não connections diretas)
- [ ] Error notification configurado (email/Teams)
- [ ] Testado em Test mode com dados reais

## 6. Dataverse Model Quality

- [ ] Publisher prefix consistente ("ftd")
- [ ] Nomes de tabelas seguem convenção (ftd_[nome])
- [ ] Nomes de colunas seguem convenção (ftd_[nome])
- [ ] Tipos de dados corretos para cada coluna
- [ ] Required/Optional definido corretamente
- [ ] Relationships configuradas (cascade behavior revisado)
- [ ] Views criadas com colunas úteis e filtros
- [ ] Forms organizados (tabs, sections, sub-grids)
- [ ] Business rules funcionando corretamente

## 7. Security Quality

- [ ] Security roles definidos com least privilege
- [ ] Field security profiles para dados sensíveis
- [ ] Testado com cada role (matrix role × operation × entity)
- [ ] Business units configurados se segregação necessária
- [ ] Column-level security para PII (LGPD/GDPR)
- [ ] Audit logging habilitado para tabelas sensíveis

## 8. Solution Transport Quality

- [ ] Solution unmanaged exportada com sucesso do Dev
- [ ] Solution Checker clean no export
- [ ] Managed solution buildada via pipeline
- [ ] Managed solution importada com sucesso no Test
- [ ] Sem erros de dependências ausentes
- [ ] Sem warnings críticos no import
- [ ] Dados de referência migrados (se necessário)
- [ ] Connection references atualizadas no target environment

## 9. Performance Quality

- [ ] Plugin execution < 2s em cenários normais
- [ ] Queries usando colunas indexadas
- [ ] FetchXML com top/count (sem retornar tudo)
- [ ] Batch operations usando ExecuteMultipleRequest (max 1000)
- [ ] Sub-grids com lazy loading habilitado
- [ ] Sem N+1 queries em plugins ou functions

## 10. Documentation Quality

- [ ] README atualizado no repositório
- [ ] Plugin registration guide atualizado
- [ ] Environment-specific configurations documentadas
- [ ] Runbook de deployment atualizado
- [ ] Known issues documentados
- [ ] Rollback procedure documentado

---

## Summary

| Área | Status | Issues |
|------|--------|--------|
| Solution Checker | ⬜ Pass / ⬜ Fail | |
| Plugin Quality | ⬜ Pass / ⬜ Fail | |
| PCF Quality | ⬜ Pass / ⬜ Fail / ⬜ N/A | |
| Azure Functions | ⬜ Pass / ⬜ Fail / ⬜ N/A | |
| Power Automate | ⬜ Pass / ⬜ Fail / ⬜ N/A | |
| Dataverse Model | ⬜ Pass / ⬜ Fail | |
| Security | ⬜ Pass / ⬜ Fail | |
| Solution Transport | ⬜ Pass / ⬜ Fail | |
| Performance | ⬜ Pass / ⬜ Fail | |
| Documentation | ⬜ Pass / ⬜ Fail | |

**Overall**: ⬜ APPROVED / ⬜ REJECTED (requires fixes)

**Reviewer**: Carla QA
**Date**: YYYY-MM-DD
**Notes**: [Observações adicionais]
