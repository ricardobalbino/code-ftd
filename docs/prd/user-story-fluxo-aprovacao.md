# User Story: [Simulador] - Fluxo de Aprovação de Proposta (7 Níveis)

**ID**: PBI-001  
**Módulo**: Simulador Comercial / D365 Sales  
**Status**: Refinado (BMAD Pilot)  
**Data**: 20/03/2026

---

## 1. Persona & Valor de Negócio
**Como** Coordenador, Gerente ou Diretor da FTD Educação,  
**Desejo** que o sistema CRM calcule automaticamente o nível de alçada e orquestre as aprovações de propostas comerciais de forma sequencial dentro do Dynamics 365,  
**Para que** possamos eliminar o uso da ferramenta externa Vulcano, reduzir o tempo médio de 3 horas de criação/aprovação e garantir que as diretrizes comercial do Grupo Marista sejam seguidas rigorosamente.

---

## 2. Contexto de Negócio (FTD Specific)
- **Eliminação do Vulcano**: O consultor não deve mais anexar PDFs em sistemas externos. A aprovação ocorre no botão "Solicitar Aprovação" da proposta.
- **Pico Sazonal**: O motor de aprovação deve suportar o volume de até 5.000 contratos/dia entre Novembro e Janeiro com baixa latência.
- **Transparência**: O consultor deve visualizar em tempo real em qual nível de alçada (1 a 7) a proposta está parada.

---

## 3. Detalhamento Técnico (D365)
- **Entidades**: `ftd_proposta` (entidade principal), `Account` (para hierarquia de aprovação), `User`.
- **Campo de Controle**: `ftd_nivel_alcada` (Integer, 1-7) e `ftd_estagio_proposta` (Rascunho → Em Aprovação).
- **Lógica de Localização**: 
  - Cálculo de Nível: **Plugin C#** (Pre-Operation) para garantir integridade dos dados.
  - Orquestração: **Power Automate Approvals** (para notificações Teams/Email).
- **Dataverse Storage**: O histórico de aprovação deve ser mantido em entidade dedicada `ftd_historico_aprovacao`, evitando o uso de `Notes` com anexos pesados.

---

## 4. Critérios de Aceite (Gherkin)

### Cenário 1: Auto-aprovação (Nível 1)
**Dado** uma Proposta comercial cujas condições (royalty, desconto, adiantamento) estão dentro da política comercial padrão,  
**Quando** o consultor clicar em "Solicitar Aprovação",  
**Então** o sistema deve marcar o estágio como `Aprovada sem assinatura` automaticamente, pular o fluxo de emails e registrar o log de auto-aprovação.

### Cenário 2: Aprovação Crítica (Nível 7 - Presidência)
**Dado** uma Proposta comercial com Adiantamento Bruto acima do limite do Diretor Superintendente,  
**Quando** a proposta for submetida,  
**Então** o sistema deve enviar notificação sequencial do Nível 2 ao 6, terminando na alçada do Presidente do Grupo Marista.

### Cenário 3: Resubmissão com Pulo de Nível
**Dado** uma Proposta que foi recusada no Nível 4 (Gerente Regional),  
**Quando** o consultor fizer um ajuste técnico que NÃO altere os valores financeiros (ex: correção de endereço),  
**Então** ao resubmeter, o sistema deve "pular" as aprovações dos Níveis 2 e 3 (já concedidas) e retornar diretamente para o Nível 4.

---

## 5. Backlog de Tarefas Técnicas (Obrigatório)
| ID Tarefa | Camada | Descrição da Atividade | Executor |
| :--- | :--- | :--- | :--- |
| **TAS-001** | Data Model | Criar OptionSet `ftd_estagio_proposta` e campo `ftd_nivel_alcada` (1-7) em `ftd_proposta`. | **Tiago Dev** |
| **TAS-002** | C# Plugin | Implementar Stage 20 (Pre-Op) para cálculo dos 7 níveis no namespace `FTD.Plugins`. | **Tiago Dev** |
| **TAS-003** | Logic | Power Automate `FTD - Sales - Alçadas` para orquestração Teams/Email. | **Tiago Dev** |
| **TAS-004** | Security | Definir visibilidade por Business Unit (Filial) p/ Gerentes. | **Wilson Arch** |
| **TAS-005** | UI/UX | Customizar BPF da Proposta com os 7 níveis e estágios. | **Paula/UX** |
| **TAS-006** | QA/Unit | Testes com FakeXrmEasy (cobertura 80%). | **Carla QA** |

## 6. Definition of Done (DoD) - Padrão FTD
- [ ] Codificado usando prefixo `ftd_`.
- [ ] Plugins no namespace `FTD.Plugins`.
- [ ] Testes unitários com FakeXrmEasy (80% coverage).
- [ ] Documentação de Arquitetura atualizada.
- [ ] Validado pelo workflow `/validate-ftd-compliance`.

---

**Relacionamento Global**: Referencia a Knowledge Base FTD e o Data Model v1.0. 🚀
