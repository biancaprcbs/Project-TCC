## Aquisição Automática de Domínios de Planejamento para Gerenciamento de Agentes de Diálogos a partir de Dados Não-Estruturados

  Projeto de pesquisa de conclusão do curso de Ciência da Computação.

### :white_check_mark: Objetivos:

  - Planejamento Automatizado é a área da Inteligência Artificial (IA) que estuda o processo de planejar nossas ações e adaptá-las aos nossos passos futuros de forma computacional. Normalmente, as ações podem ter características não-determinísticas, apresentando incerteza em seus efeitos causada por eventos que alteram o ambiente.
  
  - A aquisição de um domínio de planejamento é uma tarefa difícil, já que necessita de conhecimentos específicos sobre o domínio a ser modelado.

  - Os sistemas de diálogo representam um meio de comunicação entre humanos e máquinas. O gerenciador de diálogo é parte fundamental da arquitetura desses sistemas, caracterizando um processo realizado através de técnicas como árvores de diálogo e algoritmos de aprendizado de máquina. Ambas são complexas e possuem dificuldades para garantir o completo esclarecimento sobre as respostas forncecidas ao usuário.
    
  - Surge a necessidade de alternativas de gerenciamento de diálogo que produzam resultados mais confiáveis e com processos de aquisição menos complexos. É preciso realizar um processo eficiente de aquisição de domínio para projetar um bom gerenciamento do chatbot pretendido.
  
  - O objetivo do trabalho é empregar as técnicas de aquisição automática de domínios de planejamento a partir de dados de diálogo em Linguagem Natural, visando construir um domínio de planejamento capaz de gerenciar as ações  do chatbot Plantão Coronavírus, utilizado pela Plataforma de Atendimento ao Cidadão do governo do estado do Ceará.
  
  - Buscamos investigar técnicas de aquisição automática de domínios de planejamento e aplicá-las em dados de conversa em Linguagem Natural do chatbot, avaliando os planos gerados de acordo com as características do domínio.


### :paperclip: Conceitos importantes:
  
  - Um **domínio de planejamento** é um conjunto de esquemas de **ações** que caracterizam o ambiente em que um agente inteligente irá atuar. Além do nome da ação, o esquema consiste de um conjunto de **predicados** descritivos do ambiente, das **pré-condições**, que representam as condições que devem ser verdadeiras para que a ação possa ser executada, e dos **efeitos**, que apresentam o resultado da execução da ação.
  
  - Um **problema de planejamento** é definido para um domínio em termos de um estado inicial e de uma meta que o agente deve alcançar. Assim, o problema é solucionado quando um plano é encontrado como solução, consistindo de uma sequência de ações que terminam em um estado que satisfaz a meta.
    
  - Em um diálogo, a **declaração do usuário** representa o texto propriamente digitado por ele na interface do sistema. Para o processamento dessas informações, as declarações são classificadas em **intenções**, que determinam as categorias das expressões utilizadas pelo usuário. Por exemplo, uma declaração como "Gostaria de reservar passagens aéreas para o Rio de Janeiro" pode ser classificada na intenção "PEDIDO-DE-RESERVA-DE-PASSAGENS".
    
  - Com isso, as **entidades** são definidas através das variáveis representativas do domínio presentes nas declarações do usuário, podendo ser de uso geral (lugares, datas, pessoas) ou de características específicas do domínio em questão. Na declaração utilizada como exemplo, a expressão "Rio de Janeiro" poderia representar uma atribuição da entidade "LUGAR".
  
  - Essas informações (intenções e entidades), juntamente com o histórico do diálogo, permitem construir o **contexto** da conversa, que contém os dados relevantes até um determinado ponto do diálogo.
  
  - As **ações**, nos diálogos, representam as respostas do chatbot, cada mensagem enviada de acordo com a intenção do usuário.

### :dependabot: Chatbot Plantão Coronavírus

  - Os dados utilizados correspondem aos diálogos do chatbot Plantão Coronavírus, utilizado pela Plataforma de Atendimento ao Coronavírus do estado do Ceará. O chatbot foi desenvolvido com o objetivo de auxiliar o atendimento ao cidadão durante a pandemia da COVID-19, informando sobre a doença e reforçando a adoção de medidas de saúde confiáveis em um cenário composto por poucos profissionais de saúde disponíveis para atendimento e uma quantidade alarmante dos casos da doença.
  
  - Por meio do Plantão Coronavírus é possível que um cidadão busque informações sobre o vírus ou sobre locais físicos de atendimento, assim como informe possíveis sintomas ao sistema para que, caso necessário, seja encaminhado a um profissional de saúde (médico, enfermeiro, psicólogo, dentre outros). Neste cenário, o domínio considerado para este trabalho baseia-se nas informações dos diálogos sobre a COVID-19.
  
  - A estrutura da conversa baseia-se em menus de opções com o objetivo de auxiliar o usuário em suas dúvidas sobre a doença, contando com respostas padrões que relacionam-se a um determinado cenário.
  
  - Os diálogos acessados apresentam problemas de formatação, como a presença de emoticons e espaçamentos exagerados entre as frases. Assim, a etapa de pré-processamento é importante para estruturar os dados e determinar a qualidade final das análises.

### :bookmark_tabs: Procedimentos Metodológicos:

- #### Fase de Pré-Processamento dos Dados

  Como ambiente de desenvolvimento foi escolhida a ferramenta Google Colaboratory, que fornece ambiente de notebook interativo e colaborativo, e permite a criação e execução de código diretamente no navegador. A linguagem de programação Python foi utilizada para a construção dos algoritmos referentes ao acesso ao conjunto de dados e ao processo de formatação dos textos. Para o processo de pré-processamento, foram removidas colunas consideradas dispensáveis para o objetivo deste trabalho. Assim, o conjunto de dados resultante conta com as seguintes colunas:
    - _id_chat, representando o id geral da mensagem;
    - _id, representando o id com um índice específico de cada mensagem;
    - ori, que especifca a origem da mensagem, podendo ser do paciente, do bot ou do profissional de saúde; e
    - txt, que refere-se ao texto da mensagem.

  Para garantir esse processo, as bibliotecas **pandas**, **re** (para operações com expressões regulares) e **demoji** (para acesso aos emoticons) foram utilizadas para a implementação da função `format_text()`, que tem como objetivo formatar o texto selecionado removendo emoticons, espaços desnecessários e pequenas cadeias de texto. Cada linha do conjunto de dados passa por esse processo, resultando em um conjunto de dados devidamente formatado e salvo em um arquivo JSON para posterior uso no processo de aquisição do caminho dos diálogos.

- #### Extração das Entidades a partir das Declarações do Usuário

  Para extrair as entidades representadas nas declarações do usuário, foi utilizada principalmente a **spaCy**, uma biblioteca de software de código aberto muito usada para processamento avançado de Linguagem Natural. O modelo de **Reconhecimento de Entidade Nomeada (NER)** da spaCy, considerando a língua portuguesa, abrange as entidades referentes à pessoa, local, organizações e entidades diversas. Para abranger o escopo deste trabalho, foram desconsideradas as entidades referentes à organização e diversas e o modelo do spaCy foi atualizado com nossas **próprias entidades**, como telefone, CPF, gênero e data de nascimento, caracterizando o processo percebido pelo fluxo dos diálogos do chatbot Plantão Coronavírus. Essa atualização ocorreu através de expressões regulares para garantir o formato dos dados, funções extras de validação para dados como CPF e da classe **PhraseMatcher** do spaCy, que reconhece padrões de correspondência no texto.

- #### Extração dos Predicados do Usuário a partir das Entidades e Intenções

  Os predicados do usuário são obtidos analisando as entidades e as intenções contidas nas declarações do usuário, de modo que para cada intenção e entidade temos um predicado relacionado. Por exemplo, a declaração do usuário **"Eu sou mulher."** corresponde à intenção **"send-info-gender"** e o texto **"mulher"** é uma entidade **"$have-info-gender"**. Esses predicados são verdadeiros quando a entidade e a intenção são encontradas no fluxo do diálogo. No processo de extração, esse mapeamento é feito utilizando um **dicionário** em Python, uma estrutura de dados que consiste em chaves e valores correspondentes. Assim, cada chave no dicionário representa um predicado do usuário, tendo seu valor booleano correspondente. A função `predicates_extraction()` ilustra a verificação de cada texto do usuário a partir das entidades encontradas, resultando em um dicionário de predicados do usuário que representam o momento exato do diálogo.
  
- #### Mapeamento das Ações a partir dos Textos do Chatbot

  As ações do chatbot, e seus respectivos nomes, são extraídas dos textos do bot. O processo de mapeamento do texto inicia com a atribuição de rótulos para as ações que representam cada frase. Por exemplo, o texto **"Qual seu número de telefone?"** é rotulado como o nome da ação **"ask-user-phone-number"**. Com a classificação dos textos em ações temos um mapeamento de um para um. Esse processo foi feito acessando o mapeamento manual realizado anteriormente e fazendo os ajustes necessários. Para isso foi utilizado um **dicionário** em Python onde cada chave representa um nome designado à ação, tendo como valor o texto referente à resposta fornecida pelo chatbot ao usuário. A construção da estrutura foi um processo manual, considerando os resultados e as análises próprias dos dados.
  
- #### Obtenção das Sequências de Ações a partir dos Diálogos

  Para obter as sequências de ações, ao percorrer o diálogo cada linha referente às mensagens do chatbot passa por um processo de verificação, a função `find_actions()`. Esse processo certifica qual ação corresponde ao texto enviado, apoiando-se no mapeamento feito anteriormente. Com isso, o caminho percorrido pelo diálogo é retornado, caracterizando todas as ações executadas pelo chatbot durante o fluxo do diálogo


> [!NOTE]
> Trabalho ainda em desenvolvimento e em produção de escrita.

