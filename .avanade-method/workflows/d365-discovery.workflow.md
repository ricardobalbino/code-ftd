# ============================================================
# D365 Discovery Workflow - Avanade Method v4.29.0
# ============================================================
# Rota para: Maria Analyst
# Trigger: Comando "d365-discovery" ou início de projeto D365

---
id: d365-discovery
name: "D365 Discovery & Fit-Gap Analysis"
version: "1.0"
owner: maria-analyst
template: d365-discovery-template.md
output: "docs/discovery/d365-discovery.md"

## Workflow Steps

### Step 1: Context Gathering (Maria Analyst)

**Objetivo**: Coletar contexto completo do ambiente D365 existente.

**Perguntas Obrigatórias**:

1. **Módulos D365 em uso**: Quais módulos estão ativos? (Sales, Service, Marketing, Field Service)
2. **Volumes**: Quantos usuários? Quantos registros nas tabelas principais?
3. **Customizações existentes**: Plugins, web resources, flows, PCF controls?
4. **Integrações**: Quais sistemas externos estão conectados? Como?
5. **Problemas atuais**: O que não funciona bem? O que é manual e deveria ser automático?
6. **Requisitos novos**: O que precisa ser construído/mudado?
7. **Licenciamento**: Quais licenças estão disponíveis?
8. **Restrições**: Prazo, orçamento, aprovações necessárias?

**Output**: Seções 1-2 do template preenchidas.

---

### Step 2: Requirements Elicitation (Maria Analyst)

**Objetivo**: Detalhar cada requisito de negócio.

**Técnicas**:
- **5 Whys**: Para cada requisito, entender o "por quê" raiz
- **User Journey Mapping**: Mapear jornada do usuário no CRM
- **Process Flow**: Documentar processos atuais vs desejados Empathy Mapping**: Entender dores dos usuários CRM

**Para cada requisito**:
1. Descrever cenário de uso (Given/When/Then)
2. Identificar entidades Dataverse envolvidas
3. Estimar frequência de uso
4. Classificar prioridade (Must/Should/Could/Won't)

**Output**: Seção 3 (Estado Desejado) e Seção 5 (Requisitos por Módulo).

---

### Step 3: Fit-Gap Classification (Maria Analyst)

**Objetivo**: Classificar cada requisito no framework Fit-Gap.

**Framework de Classificação**:

```
Para cada requisito, pergunte em sequência:

1. D365 faz isso OOB?
   └─ SIM → FIT ✅

2. Pode ser resolvido com configuração (views, forms, BRs, dashboards)?
   └─ SIM → CONFIG 🔵

3. Pode ser resolvido com Power Automate ou Canvas App?
   └─ SIM → LOW-CODE 🟡

4. Requer código customizado (Plugin, PCF, Azure Function, Web Resource)?
   └─ SIM → PRO-CODE 🟠

5. Existe solução no AppSource que atende?
   └─ SIM → ISV 🟣

6. Nenhuma opção viável sem análise adicional?
   └─ SIM → GAP 🔴
```

**Output**: Seção 4 (Fit-Gap Matrix completa).

---

### Step 4: Impact & Risk Analysis (Maria Analyst)

**Objetivo**: Avaliar impacto das customizações no ambiente existente.

**Avaliar**:
- Impacto em tabelas existentes (forms, views, plugins existentes afetados)
- Impacto em integrações existentes
- Impacto em segurança (novos roles, mudanças em permissions)
- Riscos de licenciamento (limites de API, storage)
- Riscos técnicos (performance, upgrade compatibility)

**Output**: Seções 6-8 (Impacto, Licenciamento, Riscos).

---

### Step 5: Recommendations & Roadmap (Maria Analyst)

**Objetivo**: Gerar recomendações priorizadas e roadmap.

**Critérios de priorização**:
- Valor de negócio (alto = first)
- Dependências técnicas (foundation antes de features)
- Risco (mitigar riscos early)
- Quick wins primeiro (moral da equipe)

**Output**: Seção 9-10 (Recomendações e Próximos Passos).

---

### Step 6: Quality Gate - Discovery Review

**Checklist de validação**:
- [ ] Todos os módulos D365 relevantes foram documentados
- [ ] Estado atual (as-is) completo
- [ ] Cada requisito tem classificação Fit-Gap
- [ ] Impacto em customizações existentes avaliado
- [ ] Licenciamento analisado
- [ ] Riscos e dependências identificados
- [ ] Roadmap de fases definido
- [ ] Próximos passos com responsáveis

**Handoff**: Discovery aprovada → João PM (Create PRD usando d365-prd-template.md)
