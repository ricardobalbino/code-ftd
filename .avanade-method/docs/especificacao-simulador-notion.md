# Documento: Especificação - Simulador Comercial

| Campo | Valor |
|-------|-------|
| Attendees | Oscar |
| Data Início | 15 de dezembro de 2025 → 16 de janeiro de 2026 |
| Status | Doing |
| Type | Execution |

> **Objetivo do Template**: Garantir linguagem de negócio acessível, estrutura lógica para decomposição ágil e alinhamento direto com valor entregue, permitindo que cada funcionalidade seja facilmente entendida pelo negócio e refinada pelo PO em histórias de usuário claras e priorizadas.

# Identificação

| Campo | Descrição |
|-------|----------|
| Status | Em desenvolvimento |
| Iniciativa Relacionada | Transformação CX |
| Nome da Funcionalidade | Simulador Comercial |
| Área / Jornada Impactada | Jornada Comercial |
| Áreas Impactadas | Comercial, Coordenação Comercial, Inteligência Comercial |
| Responsável de Negócio | Majolie Galdino |
| Responsável de CX | Oscar de Rooij |
| Product Owner | Kevellin |
| Responsável Tecnologia | Fernando Costa Filho |
| Dependências | CRM FTD (Contas, Oportunidades, Propostas) |

# Contexto e Problema (AS-IS)
O processo de negociação comercial da FTD envolve propostas complexas, com múltiplos produtos, modelos de parceria, regras de royalties, adiantamentos e comissões. Atualmente, grande parte dessa negociação ocorre fora dos sistemas oficiais, utilizando planilhas, documentos paralelos e conhecimento tácito dos consultores.
Como consequência, o CRM recebe propostas já praticamente finalizadas devido ao processo inteiro ter sido executado fora dele. Além disso, a ausência de um mecanismo estruturado de versionamento e comparação de propostas dificulta tanto a condução da negociação quanto a rastreabilidade histórica exigida pelas áreas de suporte.
# Objetivo e Valor para o Negócio (TO-BE)
O Simulador Comercial será a interface de negociação consultiva da FTD, posicionada como camada anterior e complementar ao CRM. Seu objetivo é permitir que o consultor construa, simule, compare e refine propostas comerciais de forma estruturada ao passo que o simulador lida com a complexidade de registrar tudo no CRM (cada versão de proposta com todas as suas respectivas informações).
Valor esperado:
- Mais eficiência e velocidade no dia-a-dia do consultor ao gerenciar múltiplas versões de propostas e a interação com os clientes
- Simulador 100% aderente às instruções e boas práticas da liderança Comercial
- Aumento da qualidade das propostas submetidas ao CRM
- Redução de retrabalho causado por recusas e ajustes manuais
- Maior previsibilidade de aprovações e impactos financeiros
- Padronização da narrativa comercial sem engessar a negociação
- Relatórios de gestão completos para liderança comercial com histórico detalhado de cada versão
- Relatórios de performance por cliente, consultor ou filial
- Aumento da adoção e confiança no CRM como sistema central
# Escopo da Funcionalidade – Entregáveis
ℹ️ O escopo abaixo descreve uma versão completa do Simulador Comercial. A priorização e faseamento deverão ser definidos em conjunto com negócio e tecnologia.
## Performance e Usabilidade
  Algumas particularidades do comportamento do simulador são fundamentais para seu sucesso de engajamento e também de rotorno para FTD. São eles:
  - Mobile First: Consultores estão constantemente em trânsito e o celular é sua ferramenta de trabalho.
  - Performance da Aplicação: Um dos grandes problemas reportados atualmente é a lentidão do CRM em carregar telas, carregar listas de dropdowns, salvar registros, apresentar modais, etc. O simulador deve se aproveitar de tecnologias mais modernas para que essas informações sejam apresentadas com velocidade.
  - Campos Dinâmicos: Diversos campos do simulador são relacionados, filtrados ou recalculados a partir da mudança de um outro parâmetro. Essa reação do sistema deve ser instantânea e não depender de tempos de carregamento significativos (ex. listagem de produtos relacionados apenas à uma linha de negócio ou recálculo de toda a proposta ao mudar o valor da taxa de administração).
  - Disponibilidade Offline: Mesmo que de forma limitada, a solução deve permitir navegação off-line devido a dificuldade de conexão em algumas regiões onde os consultores atuam (funcionalidades principalmente de leitura).
## Base Estruturante para o Simulador
### Integração umbilical com CRM
    O Simulador deve interagir com os dados mestres do CRM em duas mãos, leitura e escrita. As principais entidades são: Contas, Oportunidades, Propostas, Pedidos, Contratos, Produtos e Listas de Preços.
    O Simulador não será sistema de registro oficial, mas sim uma interface de trabalho vinculada à Oportunidade que cria e gerencia versões de Propostas dentro dela.
### Cadastro-base de Linhas de Negócio e Coleção
    As linhas de negócio são “slots” que cada conta/cliente pode preencher com produtos da FTD. Consultores são incentivados a maximixar a quantidade de linhas de negócios “preenchidas” com produtos.
    - As linhas de negócio precisam ser revisadas. As que estão atualmente cadastradas no CRM são
      - Agenda
      - Dicionário
      - Didático Venda Escola
      - Didáticos
      - Didáticos Idiomas
      - Digital
      - Evolution
      - FC. Zoom
      - Literatura
      - Oficina de Financás
      - OPEE
      - Outros
      - SE PUBLICO
      - Serviços Educacionais
      - Sistema de Ensino
    Requisitos
    - Cadastro de Produto deve ser inteiramente revisado e sincronizado entre ISA, Totvs e CRM. Esse trabalho não será detalhadamente especificado aqui, mas o resultado dele é mandatório para construção do Simulador Comercial.
### Cadastro-base de Séries
    As séries são padrão, porém o cadastro no CRM atual não está consistente. Será necessário uma revisão desse campo, ou criação de um novo para adequação dos cadastros de produtos. Séries:
    - Eduação Infantil (EI)
      - 1 Ano
      - 2 Anos
      - 3 Anos
      - 4 Anos
      - 5 Anos
    - Anos Iniciais (EFAI)
      - 1o Ano
      - 2o Ano
      - 3o Ano
      - 4o Ano
      - 5o Ano
    - Anos Finais (EFAF)
      - 6o Ano
      - 7o Ano
      - 8o Ano
      - 9o Ano
    - Ensino Médio (EM)
      - 1a Série
      - 2a Série
      - 3a Série
    Requisitos
    - Correção: O campo está sendo preenchido por integração com duplicação de valores. Os valores também não estão padronizados.
    - Limpeza de Base: O campo Série precisa apresentar apenas os valores fixos acima. Necessário correção de base ou criação de novo campo.
### Cadastro-base de Produtos
    A principal complexidade do simulador está na dificuldade dos consultores em lidar com a grande quantidade de produtos que existe no acervo da FTD. No entanto, poucas informações são suficientes para agilizar a criação de uma proposta muito rapidamente e todas elas dependem de um cadastro de Produto correto e completo.
    - Informações críticas:
      - Linha(s) de Negócio
      - Componente Curricular
      - Coleção
      - Variação
      - Série(s)
    - Esses dados serão utilizados para
      - Filtrar os produtos que serão disponibilizados para o usuário
      - Para uma ferramenta de adição de produtos em massa automaticamente atribuir os produtos de uma Linha de Negócio x Coleção à todas as séries escolhidas pelo usuário.
    Requisitos
    - Limpeza de Base: Revisar cadastro de produtos e garantir que estão corretos com relação as informações descritas acima.
### Cadastro-base de Listas de Preços
    A lista de produtos não é suficiente para o funcionamento do simulador uma vez que os produtos e preços mudam de um ano para o outro. A estrutura da lista de preços deve ser revisada para que permita o simulador saber quais preços e produtos utilizar para o cliente e ano-safra correspondente à proposta.
    Requisitos
    - Regra: Atualmente não existe um padrão que define como identificar o preço de um produto para uma proposta. Precisamos definir.
### Cadastro-base de Estabelecimentos (Filiais)
    Trata-se do cadastro das Filiais no CRM. Esse cadastro de Filial tem diversas funções a exemplo das que estão descritas abaixo:
    - Vínculo de Consultores e Coordenadores (users) comerciais de cada filial
    - Vínculo de Contas/Escolas (Account) com cada filial
    - Utilizado para definição do fluxo de aprovação de proposta para identificação do Coordenador da filial
    - Utilizado para definição da tabela de preço a ser utilizada quando existem tabelas regionais de preço
    - Utilizado também no processo de carteirização
    Requisitos
    - Necessário revisão para garantia da qualidade desses registros para funcionamento da tabela de preço e fluxo de aprovação no Simulador
### Cadastro-base de Contatos e Conta x Contato
    Para que o Simulador ofereça uma experiência completa para o Consultor e seus clientes, o Simulador deve se utilizar dos cadastros de contatos do CRM para definir quem são “Destinatários da proposta” e “Sócios da Escola”. Os primeiros serão utilizados para conceder acesso e visualização online das versões de propostas criadas pelo consultor enquanto os Sócios serão utilizados para futura automação de criação de contratos que precisa dos sócios para definir “As partes interessadas”
    Requisitos
    - Sócios: O campo já existe “É responsável social?”
    - Destinatário: O campo Perfil já existe e identifica “Decisor”, que será o filtro para essa lista de contatos que o Consultor terá de selecionar um ou mais contatos.
### Cadastro-base de Escolas e Mantenedoras
    Para que o Simulador permita aos consultores criar propostas e automatizar os processos seguintes de criação e assinatura de contratos, pagamento de adiantamentos e royalties, o cadastro de Escolas e Mantenedoras deve ser revisto de forma que reflitam a realidade de todas as parcerias.
    Requisitos
    - Limpeza de Base: Equipe Comercial deve apoiar na higienização da base garantindo a qualidade dos cadastros de escolas e mantenedoras.
    - Correção: Existem atualmente 2 campos de Mantenedora em uma Conta, precisamos de uma decisão de arquitetura e correção.
    - Limpeza de Base: Após correção acima, necessário garantir que todas escolas com Mantenedoras estão devidamente cadastradas no CRM.
### Cadastro-base de Oportunidades
    Cada proposta, e todo seu histórico de versões, está relacionado a uma Oportunidade de negócio que foi identificada e registrada no CRM. Para que o simulador funcione, a base de Clientes, Oportunidades e Propostas deverá ser higienizada de modo que toda proposta esteja relacionada a uma Oportunidade.
    Requisitos
    - Regra: contas podem ter uma Oportunidade aberta por ano-safra. Não podem ter mais que uma aberta por ano safra.
    - Limpeza de Base: Será necessário definir um corte (ex. ano-safra 2026) e garantir que todas propostas do ano-safra tenha Oportunidades vinculadas. Essas oportunidades terão de ser criadas e preenchidas.
    - Campos da Oportunidade: Será necessário revisar todos campos da Oportunidade para eliminar sobreposição entre Oportunidade e Proposta.
## Criação de Propostas
  A criação de uma proposta deve seguir algumas etapas que estão claras para o Consultor comercial, mas que não estão refletidas na experiência atual do CRM e do Excel. O simulador permite essa navegação intuitiva reorganizando as informações de maneira lógica e sequencial.
### Casos de Uso
    Este módulo do Simulador Comercial deve ser utilizado para os seguintes casos de uso:
    - Consultor quer iniciar uma nova proposta
    - Consultor recebe uma proposta recusada pelo cliente ou fluxo de aprovação interno
      - Neste caso, o simulador visualmente apresenta quem e quando recusou e a justificativa
      - O simulador traz todos os dados da proposta recusada já pré-preenchidos, permitindo alterações rápidas por parte do consultor
    - Consultor recebe uma nova proposta criada por outros usuário (Coordenador ou administrativo da filial)
      - Neste caso trata-se de uma nova proposta em rascunho, não existe diferença visual com relação a uma nova proposta a não ser o pato de que ela já está preenchida
### Status e Estágios da Proposta
    Atualmente existem 3 campos diferentes para controle dos registros de Proposta. Esses registros tem sido apontados como confusos e dificultam a interpretação e navegação entre eles. São eles:
    A distribuição não é ideal devido a confusão de conceitos, e falta de clareza sequencial, gerando confusão entre o Proposta e Oportunidade (ex. Ganho), a indicação se a proposta está em uso (Ativo) e status do registro (Fechado ou Rascunho).
    Para nova fase do CRM, propomos uma simplificação dessa estrutura.
    > Status da Proposta
      - Na nova estrutura de categorização de propostas, o Status da Proposta indica se ele é um registro que está sendo trabalhado, se reflete a operação atual do cliente ou se faz parte do histórico do cliente.
      - Aberto: Registro em que consultores estão de fato trabalhando. Enquanto o consultor negocia com o cliente esse registro continua nesse status
      - Ativo: Registro que esta ativo e representa a proposta que gerou a última proposta/aditivo que foi assinada pelo cliente.
      - Histórico: Registro que não está mais válido operacionalmente pois foi sobreposto por um novo;
      - Preenchimento sempre é automático:
    > Estágio da Proposta
      Estágio de Proposta não existe atualmente, impossibilitando um acompanhamento sobre o avanço da negociação e uma visão analítica de “funil”, entre outros relatórios importantes. O Estágio tem esse propósito.
      > Estágios
      > Regras para preenchimento automático do Estágio
    > Exemplo Real
      Cliente com 1 Contrato e 2 Aditivos assinados
    > Propensão do cliente a fechar o negócio
      - Entender como isso pode ser padronizado e as regras
### Etapa 1: Contexto da Negociação
### Contexto
    A primeira etapa traz dados básicos relacionados a proposta, são eles que vinculam a proposta à Conta, Cliente e proposta anterior, se existir. Define quem são os destinatários da escola que participarão da negociação. Define o tipo de proposta, um Contrato ou um Aditivo, entre outras informações básicas.
### Comportamento
    - Consultor vincula à Conta, Oportunidade e Proposta anterior, se aplicável
    - Consultor categoriza a proposta como Contrato/Aditivo e tipo de Aditivo se aplicável
    - Consultor preenche informações ao tempo de contrato e reajuste
    - Consultor preenche período letivo da escola que guiará esforços logísticos
    - Consultor define responsável pela assinatura do contrato
    - Consultor define o ano-safra
    - Consultor define o Alunado potencial dessa proposta
### Funcionalidade
    Essa etapa é composta basicamente por um formulário que deve ser preenchido com base em informações padrão ou que já existem no CRM.
    > Formulário
      > Escola e Contatos
      > Tipo de Negociação
      > Informações da Safra
      > Modelos de Venda
      > Alunado
      - Lista de Preços Base
    > Metadado
      - ID Proposta
      - ID Revisão
      - Status da Proposta [Aberto, Ativo, Histórico]
      - Estágio da Proposta [Rascunho, Avaliação Cliente, Em aprovação, Aprovado não assinado, Aprovado e assinado]
      - Responsável Comercial
      - Data criação
      - Criado por
      - Data de modificação
      - Modificado por
### Etapa 2: Produtos e Serviços
### Contexto
      Existem centenas de produtos oferecidos pela FTD, o que naturalmente dificulta a venda devido a sua complexidade de negócio (conhecimento necessário pelo consultor sobre cada produto) e complexidade operacional (dificuldade de lidar com todos esses produtos nos sistemas).
      A adição de produtos deve ser simplificada por meio do uso inteligente de uma base de Produtos cadastrada e categorizada. Informações como Alunado, Linha de Negócio, Disciplina, Coleção e Séries devem ser utilizadas para permitir inclusão rápida e massiva de produtos na proposta.
      Neste simulador separamos a lista de produtos para venda “padrão” (essa etapa) daquela que é a lista de descontos para filhos de professores e doações.
      Importante: Atualmente a FTD cria 1 Proposta para 1 Modelo de Venda para 1 Lista de Produtos. Este simulador altera essa lógica para 1 Proposta para 1 Lista de Produtos para N Modelos de Venda. Essa mudança é significativa, ela permite que uma única negociação seja compartilhada entre todos modelos de venda, resolvendo problemas atuais da FTD (ex. contratos que começam com FTD Com Você e adicionam outros modelos de venda em seguida). Além disso, essa estrutura permite uma conciliação única de vendas, permitindo acompanhamento de performance comercial independente do canal de venda utilizado pelos clientes.
### Comportamento
      - Atenção: Negociações com Modelo de Venda = Livreiro (livreiro apenas) não precisam passar pelas etapas 2 e 3. Se a proposta tiver Livreiro e outro modelo, essa etapa continua válida.
      - A premissa da Etapa 2 é permitir que o consultor adicione grandes lotes de produtos rapidamente e, depois, refina, editando cada produto individualmente apenas quando necessário.
      - O consultor deve poder adicionar produtos em lote ou individualmente.
      - Ao adicionar produtos em lote, cada Linha de Negócio pode um comportamento diferente (ex. Sistema de Ensino precisa da Coleção, Didático precisa de Disciplina e também de Coleção, Literatura deve sempre ser adicionado individualmente).
      - Apesar de criarmos uma única lista de produtos negociada, na etapa de Modelos de Venda, mais adiante, o consultor poderá limitar alguns produtos para serem visualizados em alguns canais de venda (ex. limitar os produtos que estarão disponíveis na Lumisfera)
      - Adição de produtos é dividida entre:
      - As mudanças de parâmetros nessa interface será constante e o consultor deve imediatamente ser capaz de visualizar os recálculos dos totalizadores da proposta para que ele consiga chegar nos negócio desejado.
      - Atenção para Aditivos: A Proposta vigente, seja Contrato ou Aditivo, sempre representa a negociação atual do cliente, ou seja, ela é cumulativa. Sempre o último aditivo representará o que está negociado com o cliente.
### Funcionalidades
      > Ferramenta Adicionar Produtos em Lote
      > Itens da Proposta
      > Taxa de Administração
      > Totalizador da Proposta
      > Alçadas de Aprovação
### Etapa 3: Benefícios, Doações, Patrocínio e Adiantamento
### Contexto
      Benefícios, Doações, Patrocínio e Adiantamento são ferramentas de negociação que podem ser utilizadas pelos Consultores a fim fechar negócios. Por outro lado, elas reduzem a margem de lucro da FTD.
      Nessa etapa o Consultor lista todos esses benefícios e o simulador deixa claro o impacto dessas concessões na negociação.
### Terminologia
      - Benefícios - São descontos para filhos de professores, normalmente oferecidos por meio de cupons de desconto de 20% a 40%.
      - Doações - São materiais doados para a escola diretamente.
      - Patrocínio - Investimento direto na escola, seja em dinheiro ou em categorias específicas como tecnologia (ex. quadros digitais, TVs, notebooks, etc) e infraestrutura (ex. móveis).
      - Adiantamento - O adiantamento é uma ferramenta de negociação em que a FTD concorda em adiantar uma parte do Royalties que será originado a partir da venda de materiais que a escola fará para os seus alunos. (ex. escola estima receber em Royalties R$100.000 e a escola solicita adiantamento de 20% desse valor).
### Comportamento
      - Atenção: Negociações com Modelo de Venda = Livreiro (livreiro apenas) não precisam passar pelas etapas 2 e 3. Se a proposta tiver Livreiro e outro modelo, essa etapa continua válida.
      - A Etapa 3 deve apresentar o totalizador da Etapa 2 (todos os campos, mesmo visual) e, a medida que adiciona Benefícios, Doações, Patrocínio e Adiantamento, deve apresentar como que a Receita Líquida (Perspectiva FTD) e Royalty Estimado p/ Escola (Perspectiva Escola) são reduzidos em termos brutos e percentuais.
      - Os descontos e doações são oferecidos por série, ou seja, o Consultor não tem permissão de oferecer descontos isolados.
      - As únicas possibilidades de desconto são:
      - Para cada série e para cada uma dessa opções o consultor pode definir a quantidade de alunos que terão esses benefícios.
      - Alçadas de Aprovação
### Funcionalidade
      Os Descontos e Doações são definidos com base nos kits por série já definidos na etapa anterior. O que falta é identificar a quantidade de kits que terão descontos. A medida que o consultor preenche os campos abaixo os totalizadores devem ser recalculados.
      > Grid de Benefícios
      > Grid de Patrocínios
      > Adiantamento
      > Totalizadores Descontos e Doações
### Etapa 4: Configuração de Vendas (antigo Modelos de Venda)
### Contexto
      As condições comerciais negociadas nas etapas 3 e 4 precisam, agora, ser disponibilizadas nos modelos de venda da forma como o cliente espera realizar suas vendas. Nessa etapa apresentamos apenas os modelos de venda selecionados na etapa 1.
### Comportamento
      - Dependendo dos modelos de venda disponíveis na proposta, campos específicos são apresentados para o consultor preencher.
### Funcionalidade
    > Configurações logísticas
      Sessão visível se Modelo de Venda = FTD Com você ou Parceiro Comercial - Lumisfera Empresas ou Presencial/plantão de vendas, Venda Escola - Lumisfera Empresas ou Livreiro B2B - Lumisfera Empresas.
      - Modalidade de Operaçao [Dropdown, Operador logístico, Operação];
      - Campos abaixo visíveis apenas se Modelo de Venda = FTD Com você ou Presencial/plantão de vendas.
    > Sublistas de produtos por modelo de venda
      Alguns modelos de venda podem ser configurados de modo que apenas parte da lista de produtos negociada na Etapa 2 seja apresentada para aquisição do cliente correspondente (ex. Família no Lumisfera B2C, escola ou parceiro comercial no Lumisfera B2B). Abaixo segue as sublistas possíveis de serem criadas e quais são os modelos de venda aplicáveis.
      - Sublista de produtos para canais B2C
      - Sublista de produtos para canais Venda Escola - Lumisfera Empresas
      - Sublista de produtos para Parceiro Comercial - Lumisfera Empresas
      - Sublista de produtos para Serviços
      > Comportamento da Sublista
    > Kits por Série
      Aplicável para os Modelo de Venda = FTD Com você, Parceiro Comercial - Lumisfera Empresas, Presencial/plantão de vendas, Venda para Área Pública, Venda para Área Pública Complementar
      Kits são grupos de produtos que não podem ser vendidos separadamente uma vez que, isoladamente, não garantem a qualidade do ensino que a escola espera. Na prática, são todos os livros necessários para o ensino em uma série específica.
      Isso garante que todos os alunos vão chegar em sala com todos materiais fundamentais para as aulas acontecerem. A configuração de kits é compartilhada entre as Lumisfera`s se a escola tiver B2C e B2B habilitada.
      - Por padrão o simulador deve trazer os Kits como um recurso ativado.
      - Quando o consultor ativa o recurso o simulador deve automaticamente apresentar um Kit para cada série, permitindo que ele remova itens.
      - Consultor também pode adicionar um produto, nesse caso, devem ser apresentados apenas produtos daquela série e que não estão presentes no kit ainda.
    > Nome das Séries
      - Escolas podem customizar o nome de cada uma das séries. Para isso, uma tabela com o nome da série padrão FTD e o nome desejado correspondente pode ser preenchida pelo Consultor.
    Frente de Loja
    Sobre o modelo de venda Frente de Loja: trata-se de um canal de venda backup dos demais. Sempre disponível, mas não elegível para uma negociação. Por essa razão, o canal Frente de Loja sempre recebe toda a lista de produtos da PRP.
### Etapa 5: Matriz de Serviços
    - Não será especificado nesse momento.
    - Menos prioritário. CRM atualmente já cria o documento de Matriz automaticamente para inclusão no contrato. Essa lista precisa ser otimizada, mas pode ser feito depois.
### Etapa 6: Revisão e Compartilhamento
    A tabela baixo consolida, em sua maioria, campos que já foram apresentados nos totalizadores das Etapas 2 e 3. Existem, no entanto, algumas diferenças. Segue abaixo o formato tabela desejado.
    Regras visuais de desconto → Marcel enviar
    Big Numbers
    Análise das Condições Comerciais
    Regra específica para Sistema de Ensino
    Quando é um aditivo de renovação os preços de Tabela para o cálculo do Diferença Tabela devem ser recalculados para ser Preço Anterior + IPCA. Com esse novo Preço Tabela pode fazer o calculo da Diferença
    Comparativo vs. Proposta Anterior
    - Justificativa [Parágrafo]
      - Consultor descreve contexto que suporta o processo de aprovação interno. Esse texto será disponibilizado para todos usuários na alçada de aprovação.
### Configuração do Contrato/Aditivo
    - Essa etapa consolida campos que fundamentalmente são necessários para criação dos contratos que serão digitalmente assinados.
    - Esses campos não influenciam a aprovação ou recusa de uma proposta. Na prática, o que isso significa é que a alteração de um campo nessa sessão não precisa da criação de uma nova proposta e, consequentemente, reaprovação do cliente e todas alçadas.
    - A proposta só pode avançar do estágio Aprovação Cliente para Em Aprovação (alçadas de aprovação) quando o cliente aprovar a proposta E essa sessão estiver completamente preenchida.
    > Formulário
      - Vigência do Contrato (anos) [numéro]
      - Responsável Jurídico
      - Sócios (Contrato Social)
      - Responsável Financeiro
      - Início da Vigência
      - Fim da Vigência
      - Atual Fornecedor [Dropdown de concorrentes]
      - Assina contrato? [Sim/Não]
### Fluxo de aprovação
    Durante o fluxo de aprovação as recusas serão baseadas nas informações das etapas 1 a 4, sumarizadas na etapa 6. No entanto, importante mencionar que alguns ajustes não demandarão reinício do fluxo de aprovação. Abaixo segue lista de campos que podem ser modificados pelo Consultor mesmo se a proposta estiver com status Aprovada não assinada
    - Listar campos como Modalidade de Entrega, Concorrente atual e os demais de Configuração de contrato
## Versionamento de Propostas
  Uma negociação, especialmente as mais complexas, envolve uma série de tentativas - ou seja - uma série de versões diferentes de propostas antes de Consultor e Cliente chegarem a um acordo. É fundamental, para histórico e análises que essas versões diferentes de propostas sejam devidamente registradas e encadeadas, sempre mantendo a rastreabilidade entre propostas que vieram antes/depois de outras.
### Comportamento
    - Uma Oportunidade de Mercado Privado pode ter inúmeras propostas relacionadas respeitando as condições abaixo.
    - Apenas uma proposta pode estar Status de Proposta = Ativa, ou seja, é a proposta que está atualmente em vigor, assinada pelo cliente.
    - Essa proposta que está com status Ativa, que pode ser do tipo Contrato ou Aditivo, é necessariamente a proposta mais recente.
    - Pode haver uma proposta mais recente que a Ativa apenas se o Status da Proposta em questão for Aberta, ou seja, o consultor está negociando com esse cliente um novo Aditivo que, quando assinado, irá substituir a proposta ativa atual.
    - A não ser que seja a primeira proposta de uma Oportunidade, todas as outras devem ter o vinculo com a proposta anterior que à originou.
    - A não ser que seja a última proposta de uma Oportunidade, todas as outras devem ter o vínculo com a proposta posterior que a substituiu.
## Navegação pelas Propostas
  A dinâmica do Consulto exige que ele possa navegar facilmente pelas propostas e que o próprio Simulador o ajude a identificar que proposta precisa de atenção imediata. Por essa razão, um dashboard, um sistema de notificações, uma busca e um quadro Kanban são necessários para atender todos os casos navegabilidade.
### Dashboard
    - As informações abaixo atendem as necessidades básicas do consultor.
    image.png
### Kanban
### Contexto
    A visualização Kanban é tradicionar nas organizações comerciais e atende bem a necessidade de navegação que os consultores precisam para encontrar suas propostas. A interface Kanban deve ser acompanhada de filtros que permitem identificar aquela(s) que ele busca, independente do estágio (coluna) em que estão.
### Comportamento
    - Colunas separadas por Estágio de Proposta
    - Click na proposta deve levá-lo a visualização detalhada da Proposta
    - Consultor não deve poder arrastar propostas de uma coluna para outra, isso é feito automaticamente pelas aprovações de cada usuário responsável por cada etapa
    - Filtro padrão por propostas com Status = Aberta
    - Possibilidade de filtrar por outras informações
### Funcionalidade
    - Visualização em KANBAN agrupado por campo Estágio de Proposta
    - Filtros de busca:
      - Texto livre (Razão Social da escola, Nome fantasia da escola, CNPJ com/sem mascara)
      - Tipo de Proposta [Dropdown]
      - Tipo de Aditivo [Multiselect]
      - Ano Safra [Multiselect]
      - Modelo de Venda [Multiselect, propostas que possuem algum dos modelos]
      - Linha de Negócio presente em proposta [Multiselect, propostas que possuem produtos das linhas de negócio escolhidas pelo usuário]
      - Com Adiantamento [Checkbox, com ou sem adiantamento]
    - Ordenação [Crescente/Decrescente]
      - Total Negociado
      - Data de criação da proposta
      - Data de última atualização
    image.png
### Busca
    - Razão social
    - Nome fantasia
    - CNPJ (Com mascara ou sem mascara)
    - Nome de um contato presente na proposta como sócio ou destinatário (ex. João)
### Notificações
### Contexto
    Trata-se da melhor forma de apresentar ao consultor aquilo que teve uma atualização relevante desde sua última visita ou enquanto atua em outra proposta.
### Comportamento
    Consultor deve receber notificações pela plataforma sobre:
    - Proposta recusada
    - Cliente aprovou proposta
    - Alguma alçada de aprovação aprovou/recusou a proposta
    - Todas aprovações foram finalizadas
    - Proposta foi assinada
### Funcionalidade
    - Acessar as notificações de qualquer lugar da plataforma
    - Facilmente identificar quantas notificações novas ele recebeu desde a última vez que usou a plataforma (notificações não lidas)
    - Ao clicar em uma notificação, deve ser levado diretamente a página detalhada do registro relacionado
    - Consultor deve poder visualizar todo seu histórico de notificações em uma página dedicada para essa informação.
    - Filtros disponíveis: Data, Período, Texto (razão social, nome fantasia, cnpj com ou sem mascara)
### Detalhe da Proposta
### Contexto
    A visualização de uma proposta deve ser um espelho das etapas de construção de uma proposta, descrito mais adiante neste documento. A diferença está no fato de que na visualização todos campos são somente leitura.
    Algumas diferenças para o simulador estão listadas abaixo.
### Comportamento
    - Consultor deve poder facilmente navegar entre versões diferentes da mesma proposta na mesma tela (ex. se o Consultor está visualizando a Etapa 3 de uma versão de proposta e clica para visualizar a versão anterior, o sistema deve apresentar a Etapa 3 da versão clicada). Esse comportamento permite rápida comparação pontual daquilo que lhe chama a atenção.
    - Consultor deve poder visualizar todas as versões em modo comparativo, lado a lado, com seus parâmetros em linha, como são as tradicionais tabelas de comparação de funcionalidades.
    - Consultor deve poder visualizar status do fluxo de aprovações da proposta: quem já aprovou, com quem está nesse momento e quem serão os próximos.
    - No caso de recusas, o consultor deve poder identificar a recusa no mesmo fluxo descrito acima, mas também a descrição dada junto da recusa deve ser apresentada de maneira fácil para que ele a considere antes da criação da próxima versão.
    - Dependendo do Estágio de Proposta, os botões de ação são diferentes
      - Rascunho: Nesse estágio o consultor está ainda trabalhando na proposta antes de enviar para o cliente. Ele pode ter salvo como rascunho antes de finalizar e precisa continuar o trabalho. Os botões são: “Editar no simulador” e “Visualizar proposta online”
      - No cliente: Nesse estágio o cliente já consegue visualizar a proposta e estamos aguardando os comentários dele pela própria ferramenta (página de visualização de proposta que ele tem acesso e será descrita abaixo). Caso o cliente envie seu feedback diretamente para o consultor (Whatsapp, reunião, etc) o consultor pode fazer o registro ele mesmo. Os botões são: “Criar nova versão” e “Visualizar proposta online”
      - Em aprovação: Nesse estágio a proposta foi aprovada pelo cliente e passa pelas alçadas internas de aprovação. Não há nenhuma ação possível. O botão disponível é: “Visualizar proposta online” caso o Consultor deseje acessar essa visualização ou compartilhar o link com o cliente novamente.
      - Recusada: Nesse estágio o cliente recusou a proposta e descreveu seus comentários. A açõa imediata do consultor depende do próximo passo da negociação. Os botões são: “Visualizar proposta online”, “Perder oportunidade” ou “Criar nova versão”
      - Aprovada e não assinada: Nesse estágio a proposta foi aprovada por todas alçadas e está pendente apenas do envio do contrato formal e assinatura. Botão: “Visualizar proposta online”
### Funcionalidade
    - Interface deve ser parecida com a do simulador já descrito neste documento, a diferença é que os campos estão em modo somente leitura.
    - Visualização do progresso das aprovações da proposta
    - Visualização das versões relacionadas à proposta em questão
    - Interface dedicada à comparação de todo histórico de negociação lado-a-lado
    - Visualização da razão de recusa se aplicável
    > Botões de ação conforme descrito na sessão Comportamento, acima
      - Editar no Simulador
      - Criar nova versão
      - Perder oportunidade
      - Visualizar proposta online
    image.png
## Visualização de Propostas pelo Cliente
### Contexto
  Atualmente toda comunicação entre FTD e Cliente é feita por e-mail, Whatsapp e/ou telefone, com documentos PDF, PPT e/ou Word anexados em formatos que não são inteiramente padronizados entre todas as filiais.
  A gestão dessa comunicação não é sustentável e prejudica a capacidade de gestão da Comercial e a produtividade dos consultores que devem lidar com várias propostas ao mesmo tempo.
  O simulador, portanto, oferece ao cliente uma visualização online dessa proposta com a possibilidade dele comentar a proposta recebida e recusar, ou aprovar com um clique.
### Comportamento
  - O consultor deve poder visualizar a “versão do cliente” a partir de qualquer Estágio de Proposta (exceção: Rascunho). A diferença entre todas elas é quão completa a proposta estará uma vez que propostas não finalizadas não terão todos campos preenchidos.
  - O consultor pode escrever um texto pessoal que ficará disponível no topo da proposta para criar a conexão entre a última conversa com o cliente e a versão da proposta que o cliente está visualizando.
  - A interface do cliente consumira a maior parte dos dados da proposta criada pelo Consultor, mas não todas. Algumas informações são da FTD apenas.
  - A interface do cliente permitirá que ele comente em cada sessão, enriquecendo sua experiência e também facilitando para o Consultor identificar o que o cliente gostaria de ajustar na proposta.
  - O cliente poderá navegar pelas diferentes versões de proposta, assim como o Consultor
  - O cliente pode Recusar a proposta com comentários (mandatório);
  - O cliente pode Aprovar a proposta sem comentários (também mandatório não ter comentários);
### Funcionalidade
  > Link de Visualização
    - O link de visualização permite que o Consultor se coloque na posição do cliente e valide todo conteúdo da proposta que está montando. Também permite a identificação de melhorias para que a experiência do cliente seja ainda melhor.
    - O link de visualização de uma proposta nasce com sua criação. A página de visualização deve ser capaz de renderizar uma proposta vazia e se preencher a medida que o consultor avança em seu trabalho.
    - O link é de visualização será disponibilizado dentro da Área do Cliente.
  > Acesso às Propostas
    Existem 3 diferentes acessos possíveis à visualização das propostas
    - O consultor (que tem acesso a área do cliente) consegue acessar as propostas que ele criou dentro da área do cliente.
    - O cliente só consegue visualizar a proposta quando o Estágio da Proposta ≠ Rascunho
    - O link da versão da proposta é sempre o mesmo, independente do Estágio
    - O cliente continua podendo visualizar propostas recusadas.
    - Futuro: O cliente, ao logar na plataforma “Área do Cliente”, com suas credenciais e identificarmos que ele é um dos destinatários da proposta (necessário que o login no Portal do Cliente seja identificado pelo CRM)
  > Destinatários
    Na primeira etapa da proposta o Consultor deve identificar os destinatários da proposta. Essas são as pessoas que receberão o link para acesso à proposta por e-mail e, no futuro, no Portal do Cliente.
  > Template de Proposta
    A interface deve deve ser estruturada nas seguintes sessões, sendo que cada uma delas deve permitir que o cliente “anexe” um comentário para uma eventual recusa com ajuste necessário.
    > Cabeçalho
      - Apresentação da mensagem que o Consultor deixou para o cliente ler ao acessar essa proposta
      - Botão para adicionar comentário no Cabeçalho, texto livre
      - Sessão: Detalhes do Contrato
      - Sessão: Detalhes Negociados
      - Identificação do Consultor
    > Resumo da Proposta
      - O resumo da proposta muda dependendo de ela ter ou não markup. Veja abaixo como isso se desdobra.
      > Valores da Proposta
      > Investimentos na sua escola
      - Adiantamento Estimado = Adiantamento Estimado (calculado acima, se aplpicável)
    > Lista de Produtos
      - Estrutura Geral
      - Colunas
    > Patrocínio
      - Botão para adicionar comentário na sessão patrocínio, texto livre
      - Listagem com todos itens de patrocínio identificados (o campo justificativa é para uso interno, não deve ser apresentado aqui)
    > Benefícios
      - Botão para adicionar comentário na sessão benefícios, texto livre
      - Apresentar os cupons de cada tipo (quando tiver no CRM)
    > Ações Possíveis
      - Se Estágio Proposta = Aprovação Cliente
      - Recusar
      - Aprovar
      - Se Estágio da Proposta ≠ Aprovação Cliente
## Gestão Data-Driven
  Toda nova estrutura e jornada comercial especificada não retornará valor se não entregarmos informações úteis para decisões comerciais e administrativas por todos os participantes. Abaixo estão as necessidades mapeadas em tempo de especificação.
### Gestão Comercial
    > Funil de Oportunidades (Novo Cliente, Ampliação de Contrato, Renovação de Contrato Estratégica, Renovação de Manutenção)
      - Filtros padrão:
      - Funil (baseado no Business Flow das Oportunidades)
    > Histórico Comercial por Cliente
      - Visualização por Linha de Negócio, crescimento ou declínio
      - Definir com Comercial
    > Histórico Comercial por Linha de Negócio
      - Visualização por Linha de Negócio, somatório de todos clientes, crescimento ou declínio
      - Definir com Comercial
### Gestão Administrativa Comercial
### Gestão Controladoria
### Gestão Financeira
### Gestão de Produtos e Logística
    - Relatório de Customizações
      - Atenção: Necessário relatório para equipe de Demandas/PCP para acompanhamento dos pedidos e consequente planejamento de produção. Hoje isso é feito manualmente em planilha excel que junta as demandas de customização de cada filial (Identificado por: Claudia Ferreira Saraiva Souza, Cristiane Sanguino Munhoz).
## Captura de Dados
  O simulador foi priorizado com base nas dores listadas no início desse documento. Para mensurar seu sucesso capturaremos dados enquanto o consultor usa a plataforma.
  - Tempo de manuseio de propostas
    - Soma de todas sessões onde o usuário investiu tempo em uma proposta
    - O tempo deve ser acumulado pois o usuário pode salvar, mudar de aba, navegar para outra página do simulador.
    - O tempo total investido em uma proposta é dado desde o Status da Proposta = Aberto até Status da Proposta = Histórico ou Status da Proposta = Ativo.
    - O tempo total investido em uma negociação é a soma de todos os tempos desde a primeira versão da proposta até a última que foi aprovada e assinada.
  - Timestamp de cada início e fim de Estágio de Proposta e Status de Proposta
    - O timestamp de início e fim de cada Estágio e Status de proposta permitirá cálculos de performance e esforço no pipeline de vendas
# Fluxograma Funcional

| Processo | Link | Objetivo |
|----------|------|----------|
| Fluxo de Negociação com o Cliente | Miro: Fluxo de Negociação com o Cliente | Apresenta o fluxo de negociação utilizando o Simulador como ferramenta |

# Métricas de Sucesso
## Growth
Métricas que provam impacto comercial (ganho de conversão, velocidade de fechamento, retenção e qualidade do funil).
- Tempo médio total até assinatura (dias)
  Cálculo: data 1ª proposta enviada ao cliente → data “Aprovada e assinada”.
- Tempo médio de resposta para o cliente (horas)
  Ex.: tempo entre “recusa” e “nova versão enviada” ou entre “comentário do cliente” e “resposta do consultor”.
- Tempo médio de resposta do cliente (horas)
  Ex.: tempo entre a proposta enviada para o cliente e a resposta dele com aprovação ou recusa.
- Tempo médio de aprovação de propostas (horas)
Ex.: tempo médio do início do fluxo de aprovação até a recusa/aprovação da proposta.
- Tempo médio de aprovação de cada alçada (horas)
Ex.: tempo médio de aprovação de cada alçada no fluxo de aprovação.
- Taxa de avanço entre estágios (por etapa)
  Ex.: % que vai de Rascunho → Em negociação → Em aprovação → Aprovada sem assinatura → Aprovada e assinada.
- Número médio de versões por contrato ganho (versões)
  Ajuda a entender complexidade da negociação e evolução ao longo do tempo.
- Taxa de recusa por motivo (% por categoria)
  Base: justificativas de recusa (cliente e interno). Útil para priorizar melhorias de política, produto e proposta.
- Acurácia de forecast (pipeline) (% de diferença)
  Ex.: variação entre receita prevista em Oportunidade e a receita real assinada.
## Cost Optimization
Métricas que provam redução de esforço, retrabalho e custo de servir no fluxo de proposta.
- Tempo de elaboração de proposta (ativo) (minutos)
  Cálculo: tempo em tela no modo edição + tempo entre criação e “enviar ao cliente”, descontando inatividade.
- Horas economizadas por proposta (horas)
  Baseline As-Is (ex.: 3h) vs To-Be. Pode ser convertido em R$ com custo/hora.
- Taxa de retrabalho (% de propostas que retornam para correção)
  Ex.: % de propostas recusadas no fluxo interno por inconsistência, falta de dado ou erro de cálculo.
- Custo por proposta processada (R$)
  Fórmula prática: (tempo total de áreas envolvidas * custo/hora) + custo de retrabalho + custo de suporte.
## Organizational Effectiveness
Métricas que provam coordenação ponta a ponta, governança, accountability e previsibilidade do processo.
- SLA de aprovação interna por nível (% dentro do SLA e fora)
  Percentual de aprovações acontecendo dentro do SLA previso de cada aprovação
- Throughput do time (propostas concluídas por semana)
  Por consultor, por filial e total.
- WIP (Work in Progress) por consultor (propostas abertas simultâneas)
  Ajuda a entender sobrecarga e necessidade de priorização.
## Digital Enablement
Métricas que provam capacidade digital real: performance, automação, integração e qualidade de dados para escala
- Tempo de carregamento de telas críticas (p95 em segundos)
  Ex.: abrir proposta, abrir lista de produtos, abrir comparação de versões, salvar.
- Tempo de recálculo instantâneo (p95 em ms)
  Ex.: mudança de taxa, desconto, royalty, alunado, modelo de venda.
- Taxa de sucesso de integrações com CRM (% e erros por tipo)
  Ex.: leitura de cadastros, escrita de versões, atualização de status, sincronização de contatos.
- Latência de sincronização (segundos/minutos)
  Tempo para uma alteração no simulador refletir no CRM e vice versa.
- Uso de funcionalidades chave (% de uso)
  Ex.: % que usa comparação lado a lado, % que usa adicionar em lote, % que usa link de visualização do cliente, % que registra motivo de recusa.
# Fora de Escopo
- Geração de contratos ou aditivos
- Execução de pagamentos
# Observações Finais
O Simulador Comercial é um habilitador estratégico da transformação comercial da FTD. Ele não substitui o CRM, mas potencializa seu uso ao garantir que as propostas que entram no sistema já estejam maduras, defendidas e alinhadas às políticas comerciais e financeiras da organização.
  AI Instructions (do not edit or change)
  This register belongs to the Project Management portfolio from ‣ a register in Projects table. AI requests performed within this project and inside table entries related to this project (identified by the relation attribute Project ) should only be based on content related to this project. Never should it use information from other projects inside ‣.
Full guidelines for the AI Assistent are here: ‣
