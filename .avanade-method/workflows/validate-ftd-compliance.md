---
description: "Workflow de validação de compliance e padrões técnicos FTD/Avanade"
---

# /validate-ftd-compliance

Este workflow é o gate final antes de qualquer entrega ou subida em pipeline. Garante que o artefato (código, documento ou configuração) está 100% alinhado com o contexto FTD Educação.

## Protocolo de Execução

1. **Checklist de Naming (Obrigatório)**:
   - [ ] As novas entidades e campos usam o prefixo `ftd_`?
   - [ ] Os plugins estão no namespace `FTD.Plugins`?
   - [ ] Os Power Automates começam com `FTD - [Módulo] -`?

2. **Checklist de Arquitetura (D365)**:
   - [ ] A lógica respeita o **Dataverse Storage Mitigation** (sem anexos no Dataverse)?
   - [ ] A segurança segue o modelo de **Visibilidade por Filial** (BU)?
   - [ ] Nenhuma string de conexão ou segredo está no código (uso de Key Vault)?
   - [ ] As alçadas foram calculadas respeitando os **7 níveis**?

3. **Checklist de Qualidade (Avanade)**:
   - [ ] Solution Checker foi executado e está limpo?
   - [ ] (Se código) FakeXrmEasy foi usado nos testes unitários?
   - [ ] A documentação em `docs/arquitetura/` foi atualizada com a mudança?

4. **Veredicto Final**:
   - Reportar ao usuário (Supervisor):
     - ✅ **Aprovado para OAT** (Tudo em conformidade).
     - ⚠️ **Aprovado com Ressalvas** (Descrever desvios aceitáveis).
     - ❌ **Reprovado** (Descrever falhas críticas de compliance).

---

// turbo
**Comando rápido**: `validate-ftd-compliance`
