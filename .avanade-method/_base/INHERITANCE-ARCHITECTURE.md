# Avanade Method Agent Inheritance Architecture

## 📚 Overview

O Avanade Method usa uma arquitetura de herança inspirada no BMAD, onde o **avanade-master.md** serve como template base para todos os agentes especializados.

## 🏗️ Estrutura de Arquivos

```
.avanade-method/
├── _base/
│   ├── avanade-master.md        # 🎯 Template base - herança completa
│   └── config.yaml              # Configuração global
│
├── agents/                      # 📁 Customizações por agente
│   ├── supervisor.customize.yaml
│   ├── tiago-dev.customize.yaml
│   ├── maria-analyst.customize.yaml
│   ├── joao-pm.customize.yaml
│   ├── wilson-architect.customize.yaml
│   ├── paula-po.customize.yaml
│   ├── roberto-sm.customize.yaml
│   ├── carla-qa.customize.yaml
│   ├── sofia-ux.customize.yaml
│   └── paige-tech-writer.customize.yaml
│
├── workflows/
├── tasks/
├── checklists/
├── guides/
├── memory/
├── standards/
└── templates/

.github/
└── agents/                       # 📁 Arquivos de ativação por agente
    ├── avanade-supervisor.agent.md
    ├── tiago-dev.agent.md
    ├── maria-analyst.agent.md
    └── ... (um por agente)
```

## 🔄 Modelo de Herança

### Camadas de Definição

```
┌─────────────────────────────────────────────────────────────┐
│                    PRIORITY 1 (HIGHEST)                      │
│              .agent.md (Agent File)                        │
│         Definição primária do agente no VSCode               │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                       PRIORITY 2                             │
│           agents/*.customize.yaml                            │
│         Customizações específicas opcionais                  │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                  PRIORITY 3 (BASE)                           │
│            _base/avanade-master.md                           │
│         Template base com padrões compartilhados             │
└─────────────────────────────────────────────────────────────┘
```

### O que é Herdado vs Override

| Componente | Comportamento | Descrição |
|------------|---------------|-----------|
| **Activation Protocol** | APPEND | Steps do agente são adicionados após steps base |
| **Menu Handlers** | INHERIT | Totalmente herdado, não modificado |
| **Cognitive Framework** | INHERIT | Elicitação e análise de contexto compartilhados |
| **Base Menu Items** | INHERIT | MH, CH, PM, DA sempre disponíveis |
| **Rules** | APPEND | Regras do agente adicionadas às base |
| **Persona** | REPLACE | Agente substitui completamente persona base |
| **Dependencies** | MERGE | Dependências do agente + base compartilhadas |
