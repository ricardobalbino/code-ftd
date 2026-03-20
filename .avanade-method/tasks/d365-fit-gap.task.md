# ============================================================
# D365 Fit-Gap Analysis Task - Avanade Method v4.29.0
# ============================================================
# Owner: Maria Analyst
# Atomic task for classifying a single requirement

---
id: d365-fit-gap
name: "D365 Fit-Gap Classification"
owner: maria-analyst

## Purpose

Classificar UM requisito de negócio no framework Fit-Gap D365 para determinar
a abordagem técnica mais adequada.

## Input Required

- Descrição clara do requisito
- Módulo D365 envolvido (Sales, Service, Marketing, Field Service)
- Prioridade (Must, Should, Could, Won't)

## Decision Tree

```
1. O D365 já faz isso nativamente (OOB)?
   ├── SIM → FIT 🟢 (apenas configurar/habilitar)
   └── NÃO → continuar

2. Pode ser resolvido com configuração sem código?
   (Business Rules, Views, Forms, Dashboards, Option Sets)
   ├── SIM → CONFIGURATION 🔵
   └── NÃO → continuar

3. Pode ser resolvido com Power Platform low-code?
   (Power Automate flows, Canvas Apps, Business Process Flows)
   ├── SIM → LOW-CODE 🟡
   └── NÃO → continuar

4. Requer código customizado?
   (Plugins C#, PCF TypeScript, Azure Functions, Web Resources JS)
   ├── SIM → PRO-CODE 🟠
   └── NÃO → continuar

5. Existe solução ISV no AppSource?
   ├── SIM → ISV 🟣 (avaliar custo/benefício)
   └── NÃO → continuar

6. Nenhuma opção viável sem análise adicional
   └── GAP 🔴 (requer investigação)
```

## Output Format

```yaml
requirement:
  id: "R###"
  description: "[Descrição]"
  module: "[Sales/Service/Marketing/FieldService]"
  classification: "[FIT/CONFIG/LOW-CODE/PRO-CODE/ISV/GAP]"
  approach: "[Detalhe técnico da abordagem]"
  effort: "[Baixo/Médio/Alto]"
  complexity: "[Baixa/Média/Alta]"
  dependencies: ["[dep1]", "[dep2]"]
  notes: "[Observações]"
```
