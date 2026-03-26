# Glossário — Termos FTD e Projeto CRM

> [← Voltar ao Início](./Home.md)

---

## Termos de Negócio FTD

| Termo | Significado |
|-------|-------------|
| **Safra** | Ano letivo/comercial. Ex: "Safra 26" = ano 2026. Ciclo de vendas anual. |
| **Alunado** | Quantidade de alunos de uma escola, por série. Base para cálculo de propostas. |
| **Adoção Escolar** | Período de escolha de materiais didáticos pela escola (nov-jan). Pico de propostas. |
| **Linha de Negócio** | Segmento de produtos FTD. Exemplos: Sistema de Ensino, Didático, Literatura, Bilíngue. |
| **Coleção** | Conjunto de produtos de uma linha (ex: Trilhas = 15 materiais por série). |
| **Grade** | Produto customizado para ensino médio, específico por escola. |
| **Produto Prateleira** | Produto padrão do catálogo disponível para qualquer escola. |
| **Produto Customizado Compartilhado** | Alteração leve (ex: retirar capítulo). Qualquer escola pode usar. |
| **Produto Personalizado** | Alteração pesada (capa, mascote). Exclusivo para escola/mantenedora específica. |
| **Código Substituto** | Vínculo entre produto antigo e novo entre safras (p.ex.: versão 2025 → versão 2026). |
| **Série** | Segmento escolar: EI (Ed. Infantil), EFI (Anos Iniciais), EFAF (Anos Finais), EM (Ensino Médio). |
| **Mantenedora** | Grupo/rede que administra escolas. Exemplo: Grupo Marista. |
| **KA (Key Account)** | Conta estratégica — escola/rede de alto volume ou alta importância comercial. |
| **Hunter** | Consultor que prospecta novas contas (escolas nunca atendidas). |
| **Farmer** | Consultor que mantém e expande carteira de contas existentes (renovações). |
| **Anja** | Pessoa que cadastra proposta no lugar do consultor (apoio administrativo da filial). |
| **Carteira / Carteirização** | Conjunto de contas atribuídas a um consultor. Processo de atribuição. |
| **Estabelecimento** | Filial FTD registrada no CRM (28 filiais no Brasil). |
| **Royalty / Markup** | % que a escola adiciona ao preço negociado para cobrar das famílias. Diferença fica com a escola. |
| **Adiantamento** | Antecipação do royalty futuro (fluxo de caixa para escola, não valor extra). |
| **Alçada** | Nível de aprovação necessário para aprovar uma proposta (1 a 7). |
| **PNLD** | Programa Nacional do Livro Didático — programa do governo federal para escolas públicas. |
| **Venda Direta** | Faturamento direto para a escola (nota fiscal para CNPJ da escola). |
| **Smart POS** | Terminal de pagamento para plantão de vendas presencial na escola. |
| **Taxa de Administração** | Condição comercial que afeta o nível de alçada da proposta. |
| **Benefício** | Cupom de desconto para filhos de professores (20% ou 40%). |
| **Doação** | Material com 90% ou 100% de desconto para escola. |
| **Patrocínio** | Investimento em dinheiro na escola (laptops, fachada, jardim). Requer alçada alta. |
| **FTD com Você** | Nome comercial do modelo Lumisfera — escola compartilha link, famílias compram online. |

---

## Termos de Sistemas e Tecnologia

| Termo | Significado |
|-------|-------------|
| **Spartan** | App principal de vendas no D365. Interface dos consultores e gerentes comerciais. |
| **Hub SAC** | App D365 para atendimento ao cliente (CRC — Central de Relacionamento com Cliente). |
| **Lumisfera** | E-commerce da FTD (Adobe Commerce) para venda de materiais às famílias. |
| **ISA** | Sistema de cadastro de produto da FTD. Fonte de verdade para família de produto (SKU). |
| **TOTVS / Datasul** | ERP corporativo da FTD. Faturamento, estoque, financeiro, código de cliente. |
| **Adobe Sign** | Plataforma de assinatura digital de contratos. Em vias de mudança de tecnologia. |
| **Vulcano** | Ferramenta externa de aprovação de propostas. **Será eliminado** — substituído pelo D365. |
| **Pede Livreiro** | Sistema legado para livreiros/parceiros. **Será eliminado** — substituído pelo CRM. |
| **GMUD** | Gestão de Mudança — processo obrigatório para deploy em Produção (aprovado via SMAX). |
| **SMAX** | Sistema de abertura de GMUDs (Service Management). Giselle é responsável. |
| **FTDMaxFlow** | Usuário de serviço proprietário de todos os Power Automate flows. Acesso restrito. |
| **Dataverse** | Plataforma de dados do Microsoft Power Platform / D365 (banco de dados gerenciado). |
| **BCA** | Avanade Best Practice Accelerator v3 — metodologia e padrões de desenvolvimento. |
| **PCF** | PowerApps Component Framework — controles TypeScript/React para formulários D365. |
| **ETL** | Extract, Transform, Load — processo de sincronização de dados entre sistemas. |
| **Early Bound** | Geração de classes tipadas para entidades/campos Dataverse (preferível a Late Bound). |
| **Late Bound** | Acesso a dados via strings (ex: `entity["fieldname"]`). Evitar no BCA. |
| **SPA** | Single Page Application — arquitetura do Simulador Comercial em Power Pages. |
| **Liquid** | Engine de template do Power Pages usada para carregar dados via FetchXML. |
| **OData** | Web API de dados do Dynamics consumida via JavaScript no portal. |

---

## Termos de Processo Comercial

| Termo | Significado |
|-------|-------------|
| **Revisão (Proposta)** | Nova versão de uma proposta (ID Revisão inicia em 0, pode chegar a 27-28). |
| **Aditivo** | Modificação de contrato ativo (encerramento, renovação, ajuste). |
| **Contrato Novo** | Primeira proposta aprovada para uma oportunidade. |
| **Oportunidade** | Registro que vincula conta a proposta. Uma por conta por ano-safra. |
| **Proposta Ativa** | Única proposta com Status = Ativo por oportunidade (a vigente). |
| **Proposta Histórico** | Versão anterior sobreposta por revisão mais recente. |
| **Sublista de Produtos** | Conjunto de produtos disponíveis em um canal de venda específico. |
| **Matriz de Serviços** | Lista de serviços educacionais vinculados aos produtos contratados. |
| **Nível de Alçada** | Valor calculado (1-7) que determina qual aprovador precisa assinar a proposta. |
| **Área do Cliente** | Portal web para escolas: visualizar proposta, fazer comentários, assinar contrato. |

---

## Termos do Projeto/Metodologia

| Termo | Significado |
|-------|-------------|
| **Brownfield** | Projeto em ambiente existente (CRM já em produção). Oposto de Greenfield. |
| **Onda** | Fase de entrega do projeto (MVP, Onda 1, Onda 2...). |
| **Transformação CX** | Nome oficial do projeto de modernização (CX = Customer Experience). |
| **Squad CRM** | Time de desenvolvimento e sustentação do CRM D365 da FTD. |
| **PBS** | Product Breakdown Structure — estrutura de decomposição de produto. |
| **Higienização** | Limpeza de dados incorretos/duplicados/desatualizados no CRM. |
| **Source of Truth** | Fonte de verdade — sistema que tem a versão mais correta e autoritativa do dado. |
| **Shift-Left** | Princípio de testar cedo, junto com o desenvolvimento (não só no final). |

---

## Séries Escolares (Cadastro de Referência)

| Segmento | Código | Séries |
|----------|--------|--------|
| Educação Infantil | EI | 1 Ano, 2 Anos, 3 Anos, 4 Anos, 5 Anos |
| Anos Iniciais | EFAI | 1º ao 5º Ano |
| Anos Finais | EFAF | 6º ao 9º Ano |
| Ensino Médio | EM | 1ª, 2ª e 3ª Série |

---

*Referências: [ftd-knowledge-base.md](../docs/ftd-knowledge-base.md) | [README.md](../README.md)*
