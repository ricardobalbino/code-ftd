# ADR-001: Consolidação da Estrutura Documental (Single Source of Truth)

**Data**: 24/Mar/2026  
**Status**: 🟢 Aceito  
**Autor**: Antigravity (Supervisor)  
**Participantes**: Ricardo / Equipe FTD

---

## 📅 Contexto
O repositório possuía documentação fragmentada entre as pastas raiz `docs/`, `.avanade-method/docs/` e `.avanade-method/framework/docs/`. Isso gerava duplicidade de informações (`onboarding.txt` em múltiplos locais), confusão de caminhos para os agentes de IA e dificuldade de navegação para o usuário final.

## 💡 Decisão
Decidimos consolidar **toda** a documentação de projeto (Negócio, Arquitetura, Onboarding e Método) sob uma única pasta de primeiro nível: **`docs/` na raiz**.

### Estrutura de Destino:
- `docs/arquitetura/`: Toda spec técnica e onboarding de engenharia.
- `docs/discovery/`: Todo o conhecimento extraído de negócio.
- `docs/framework/`: Padrões de metodologia do time.
- `docs/transcricoes/`: Arquivos brutos de reuniões (Saneado: sem caracteres especiais).
- `docs/prd/`: Especificações de negócio.
- `docs/stories/`: Histórias de usuário e tarefas de desenvolvimento.

## ✅ Consequências (Positivas)
- **SSOT (Single Source of Truth)**: Elimina arquivos duplicados com versões diferentes.
- **Eficiência de IA**: Redução de erros de leitura de contexto pelos agentes MCP.
- **Onboarding Facilitado**: Caminho único para o desenvolvedor entender o projeto.

## ⚠️ Consequências (Atencao)
- **Links Remotos**: Todos os links existentes na Wiki e no Azure DevOps que apontavam para `.avanade-method/docs/` foram atualizados para a nova raiz.

---
> [!IMPORTANT]
> Esta estrutura é a regra de ouro do repositório. Nenhuma nova documentação de projeto deve ser criada fora da pasta `docs/`.
