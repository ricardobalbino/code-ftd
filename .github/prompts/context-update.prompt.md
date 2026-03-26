---
---

> **⚠️ FTD EDUCAÇÃO - CONTEXTO OBRIGATÓRIO**
> Este prompt deve ser usado para manter a "Camada 5 (Conhecimento)" e "Camada 6 (Arquitetura)" do Motor BMAD sempre sincronizadas com as decisões de negócio e técnicas mais recentes.
> Documentos-alvo: `docs/`, `wiki/`, `.github/agents/` (Personas) e `.avanade-method/agents/` (Customizações de Regras).

## 📋 O que é este Prompt?

Este prompt orienta a IA a processar uma **nova transcrição** (reunião, discovery, refinamento) e atualizar sistematicamente os artefatos de contextualização do projeto FTD Educação. Ele garante que:
- O **Glossário** contenha termos novos (ex: Safra, Anja, Alçada).
- Os **Pain Points** do Discovery reflitam a realidade atual.
- O **Modelo de Dados** documente novos campos ou entidades.
- As **Decisões Arquiteturais** fiquem registradas.

---

## 🎯 Quando Usar

### ✅ USE para:
- Processar transcrições de reuniões de Refinamento ou Discovery.
- Atualizar o Knowledge Base após discussões com Stakeholders (Julio, Oscar, Fernando).
- Refletir mudanças no Roadmap ou prioridades de Ondas.
- Documentar novos padrões ou dores identificadas.

---

## 📄 Instruções para o Agente (@paige-tech-writer ou @maria-analyst)

Ao receber uma nova transcrição, siga estes passos:

### 1. Leitura e Triagem
- Leia a **Nova Transcrição** integralmente.
- Identifique:
    - **Participantes** (nome e papel).
    - **Termos novos** para o Glossário.
    - **Regras de Negócio** alteradas ou novas.
    - **Dores/Pain Points** mencionados.
    - **Decisões Técnicas** tomadas (tecnologia, estágio de plugin, etc).
    - **Mudanças no Roadmap** (Deadlines, MVP, Ondas).

### 2. Cruzamento de Dados (Gap Analysis)
- Compare com os arquivos existentes:
    - `docs/ftd-knowledge-base.md`
    - `docs/discovery/ftd-discovery.md`
    - `docs/arquitetura/d365-data-model.md`
- Identifique o que é **novo**, o que **mudou** e o que foi **confirmado**.

### 3. Atualização de Artefatos (MÃO NA MASSA)
Atualize os arquivos seguindo as seções específicas:

#### A. In `docs/ftd-knowledge-base.md`:
- Adicione novos participantes na seção de cabeçalho.
- Atualize a seção correspondente ao assunto (ex: 3. Processo Comercial).
- Adicione termos ao **Glossário** (Seção 10).
- Adicione notícias ou projeções na Seção 11.

#### B. In `docs/discovery/ftd-discovery.md`:
- Atualize a lista de **Critical Pain Points**.
- Ajuste o **Fit-Gap Analysis** se necessário.
- Atualize o **Roadmap / Ondas de Entrega**.

#### C. In `docs/arquitetura/d365-data-model.md`:
- Adicione descrições de novos campos ou entidades Dataverse discutidos.
- Documente gaps de dados identificados.

### 4. Tom de Voz e Formatação
- Use o tom **Profissional, Técnico e Objetivo** da Avanade.
- Mantenha a formatação Markdown (tabelas, negrito, listas).
- Use **Emojis** (⚠️, ✅, 🔴, 🟠) para indicar severidade e status conforme os padrões existentes.
- Inclua a data da atualização no rodapé/cabeçalho dos arquivos.

---

## 🛠️ Prompt "Master" para Copiar e Colar

```markdown
@paige-tech-writer / @maria-analyst

Recebi uma nova transcrição no arquivo: [CAMINHO_DA_TRANSCRICAO]

Sua tarefa é atualizar toda a referência de contextualização FTD (BMAD Engine Layer 5 & 6).

PASSOS:
1. Leia a nova transcrição e extraia: Participantes, Regras, Glossário, Dores e Decisões.
2. Analise os arquivos atuais em docs/, wiki/, .github/agents/ e .avanade-method/agents/ e identifique o que precisa ser atualizado ou criado.
3. Realize as edições nos arquivos:
   - docs/ftd-knowledge-base.md (Base de conhecimento e Glossário)
   - docs/discovery/ftd-discovery.md (Pain points e Roadmap)
   - docs/arquitetura/d365-data-model.md (Se houver novas entidades/campos)
   - wiki/*.md (Replique as mudanças modulares na Wiki)
   - .github/agents/*.agent.md (Destaque novas atribuições ou mudanças na persona do agente)
   - .avanade-method/agents/*.customize.yaml (Insira novas CORE RULES técnicas ou princípios baseados na reunião)
4. Mantenha os padrões estéticos e de cores (emojis) originais.
5. Ao concluir, me dê um resumo das principais mudanças realizadas em cada arquivo.

Documentação de referência para o seu "Style Guide": docs/arquitetura/bmad-engine-flow.md
```
