### Stakeholder Profiles Recorrentes
_Será preenchido durante descobertas_

**Exemplo**:
```yaml
- name: "CFO Type"
  characteristics:
    - Foco em ROI e métricas financeiras
    - Prefere dados quantitativos
    - Disponibilidade limitada
  engagement_strategy: "Apresentar business case com ROI claro, reuniões curtas (30min max)"
  
- name: "End User Type"
  characteristics:
    - Foco em usabilidade
    - Feedback qualitativo rico
    - Resistente a mudanças
  engagement_strategy: "Envolver cedo, protótipos clicáveis, comunicar benefícios"
```

---

### Requirements Patterns Identificados
_Padrões de requisitos que aparecem frequentemente_

**Exemplo**:
```yaml
- pattern: "Export to Excel"
  frequency: "Alta (80% dos projetos)"
  notes: "Stakeholders sempre pedem export de dados para Excel/CSV"
  recommendation: "Incluir proativamente no discovery inicial"
  
- pattern: "Auditoria/Log de Ações"
  frequency: "Média (50% dos projetos enterprise)"
  notes: "Compliance/regulatório exige rastreabilidade"
  recommendation: "Perguntar sobre requirements regulatórios no Discovery Protocol"
```

---

### Discovery Insights Históricos
_Aprendizados de descobertas anteriores_

**Exemplo**:
```yaml
- project: "Portal Cliente v2"
  date: "2024-01-15"
  insight: "Usuários queriam notificações push mas não tinham habilitadas. Real need: dashboard com resumo."
  lesson: "Validar assumptions com protótipo, não apenas entrevistas"
  
- project: "CRM Integration"
  date: "2024-02-01"
  insight: "5 stakeholders, 5 definições diferentes de 'cliente'"
  lesson: "Glossário de termos crítico em discovery - criar upfront"
```

---

### Elicitation Techniques Validadas
_Técnicas que funcionaram bem_

**Exemplo**:
```yaml
- technique: "Jobs-to-be-Done"
  effectiveness: "Alta"
  best_for: "Features de produto (entender motivação real)"
  notes: "Framework 'Quando [situação], quero [ação], para que [benefício]' revelou requisitos ocultos"
  
- technique: "Five Whys"
  effectiveness: "Média-Alta"
  best_for: "Problemas recorrentes, root cause analysis"
  notes: "Funciona melhor com stakeholders técnicos, evitar com executivos (impaciência)"
```

---

## 🎯 User Personas Recorrentes
_Personas que aparecem em múltiplos projetos_

**Exemplo**:
```yaml
- name: "Sarah, Gerente de Vendas"
  demographics: "35-50 anos, uso diário de CRM, Tablet/Notebook-focus"
  goals:
    - Fechar vendas rapidamente
    - Acessar dados de clientes em movimento
  pain_points:
    - Sistema atual lento em mobile
    - Dados desatualizados
  tech_savviness: "Média (usa smartphone mas resiste a novos sistemas)"
  reuse_count: 3
  notes: "Persona recorrente em projetos de CRM/Vendas"
```

---

## 🧪 Assumption Testing Results
_Premissas testadas e seus resultados_

**Exemplo**:
```yaml
- assumption: "Usuários querem dashboard customizável"
  test_method: "Survey com 50 usuários"
  result: "❌ REFUTADA - 70% preferem dashboard pre-configurado (menos trabalho)"
  impact: "Removido customização, foco em views pré-definidas inteligentes"
  
- assumption: "Mobile é prioridade (60% do tráfego)"
  test_method: "Analytics últimos 6 meses"
  result: "✅ CONFIRMADA - 62% mobile, 38% desktop"
  impact: "Design Tablet/Notebook focado adotado (Simulador Comercial)"
```

---

## 📋 Pain Points Recorrentes
_Problemas que aparecem frequentemente_

**Exemplo**:
```yaml
- pain: "Dados duplicados entre sistemas"
  frequency: "Alta"
  typical_cause: "Falta de integração, entry manual em múltiplos sistemas"
  solution_pattern: "API integration ou Master Data Management"
  
- pain: "Relatórios demoram muito"
  frequency: "Média"
  typical_cause: "Queries não otimizadas, dados não agregados"
  solution_pattern: "Data warehouse, OLAP cubes, caching"
```

---

## 🔄 Retrospective Learnings
_O que melhorar em próximos discoveries_

**Exemplo**:
```yaml
- learning: "Envolver TI/Infra mais cedo"
  date: "2024-01-20"
  context: "Requisitos de integração descobertos tarde → delay de 2 sprints"
  action: "Adicionar pergunta sobre integrações no Discovery Protocol inicial"
  
- learning: "Protótipos clicáveis > wireframes estáticos"
  date: "2024-02-05"
  context: "Stakeholders não entenderam fluxo até verem protótipo Figma interativo"
  action: "Investir tempo em protótipos clicáveis (Figma, InVision) para validação"
```

---

## 📖 Templates & Checklists Customizados
_Variações do Discovery Protocol que funcionaram bem_

**Exemplo**:
```yaml
- template: "Discovery Protocol - E-commerce"
  customization: "Adicionadas perguntas sobre payment gateways, shipping, inventory"
  success_rate: "Alta"
  file: "discovery-ecommerce-template.yaml"
  
- template: "Discovery Protocol - Compliance/Regulatório"
  customization: "Foco em LGPD, auditoria, data retention"
  success_rate: "Média-Alta"
  file: "discovery-compliance-template.yaml"
```

---

## 🔗 Cross-References
_Links para artefatos relacionados_

- Discovery Template: `${AVANADE_DISCOVERY_TEMPLATE_YAML}`
- Advanced Elicitation Task: `${AVANADE_TASK_ADVANCED_ELICITATION}`
- Editorial Review: `${AVANADE_TASK_EDITORIAL_REVIEW_PROSE}`

---

## 📌 Como Usar Esta Memória

### ✅ ANTES de executar Discovery:
1. Consultar **Stakeholder Profiles** → identificar tipos similares
2. Revisar **Requirements Patterns** → antecipar needs comuns
3. Escolher **Elicitation Technique** adequada ao contexto
4. Revisar **Retrospective Learnings** → evitar erros passados

### ✅ DURANTE Discovery:
1. Validar **Assumptions** early (não assumir)
2. Mapear **Pain Points** recorrentes
3. Reutilizar **Personas** se aplicável

### ✅ APÓS Discovery:
1. **Atualizar memória** com novos insights
2. Documentar **Lessons Learned**
3. Adicionar **Patterns** identificados
4. Atualizar **Elicitation Techniques** (o que funcionou/não funcionou)

---

## 🏢 D365 CE Discovery Context - FTD Educação

### Descobertas JÁ REALIZADAS (Transcrições Onboarding Março 2026)

**Stakeholders Mapeados**:
- Oscar de Rooij (PO/Business Analyst FTD)
- Kevellin (UX/Dev FTD)
- Julio Cesar (Tech Lead CRM FTD)
- Fernando (Dev FTD)
- Eduardo (Funcional/Demo FTD)
- Marcel (FTD) - Segurança/Infra
- Mônica (FTD Commercial Business) - Alçadas
- Danilo Macedo (Avanade Tech Lead)
- Jovanello (Avanade Manager)

**Pain Points Identificados**:
1. Consultor leva até **3 HORAS** para criar 1 proposta (200 produtos × 3 cliques)
2. Dataverse em nível CRÍTICO de armazenamento
3. Tabela de produtos poluída (1.283 registros vs 15 esperados) — falta flag de Produto Prateleira vs Customizado
4. Desalinhamento Taxonômico: ISA (Família SKU) ≠ TOTVS (Família Comercial) ≠ CRM (Linha Negócio)
5. Categorização de Contas: campo `tipo_instituicao` mistura conceitos (Confessional, KA, Rede S)
6. Higienização: 101.000 contas precisam de limpeza (recorte 2025-2026)
7. Oportunidades desvinculadas: dados históricos sem vínculo com Proposta, impede BI
8. Contrato manual: ~50 templates Word/PDF sem modularização
9. Alçadas: Regras mal parametrizadas travam fluxo no Nível 4 e geram backlog excessivo
10. Cadastro de contas sem processo definido (CRM vs TOTVS)

**Processo Comercial Mapeado**:
Conta → Contato (Rep. Legal) → Oportunidade → Produtos → Proposta → Aprovação (4 níveis) → Contrato → Adobe Sign → TOTVS

**Linhas de Negócio**: Sistema de Ensino, Didático, Bilíngue, Espanhol, Literatura, APE
**Canais de Venda**: FTD com Você (Lumisfera), Venda Direta, Frente de Loja, Smart POS
**Safra**: ano letivo/comercial (pico nov-jan)

### O Que FALTA Descobrir
- [ ] Lista exata de plugins existentes (entidades/eventos)
- [ ] Número exato de PCFs, Power Automate flows
- [ ] Lista completa de entidades customizadas (ftd_*)
- [x] Modelo de segurança detalhado → CONFIRMADO (Consultor = suas contas, Gerente = filial)
- [x] Alçadas de Aprovação → CONFIRMADO (7 níveis sequenciais)
- [ ] Status real do assessment de código legado
- [ ] Requisitos LGPD específicos
- [x] Ecossistema Lumisfera → CONFIRMADO (4 lojas, integração B2B Livreiro)

### D365 Discovery Artifacts
- Workflow: `d365-discovery.workflow.md`
- Template: `d365-discovery-template.md`
- **Knowledge Base**: `docs/ftd-knowledge-base.md` (LEITURA OBRIGATÓRIA)

---
