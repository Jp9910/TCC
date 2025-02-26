# TCC 2

Perguntar:
    
    -V mudar o nome do orientador? deixar os 2
    
    -V como ficaria o capitulo de analise de viabilidade? considerar no final
    
    - remover capitulo de plano de continuidade?

    - Enviar como artigo?
        https://www.computer.org/digital-library/magazines/ic/cfp-programming-flexible-systems?preview=true

    - Análise de viabilidade poderia ter a ver com os requisitos funcionais e não funcionais do sistema e alguns diagrama UML, relacionando com ferramentas.

    - Sobre o objetivo específico de **Propor** uma combinação de ferramentas:
        Eu reescrevi o objetivo para alinhar com o que eu queria fazer com o trabalho. O que o senhor acha?

        Desenvolver uma aplicação -> exemplar/protótipo? ...
        
        (A arq de microsserviços geralmente são usados em aplicações grandes, mas claro que não tenho condição de construir um sistema assim em poucos meses. 
        Então a aplicação que fiz é pequena (pequena como? em termos de que?). 
        Acontece que "quais ferramentas usar" depende muito do sistema desenvolvido. 
        E quais vão ser usadas se limitam às funcionalidades de infraestrutura que eu desenvolvi. 
        Além disso, também se limitam a ferramentas open-source pq n faria sentido, no contexto academico, pagar pra fazer um projeto de demonstração
        Sendo assim, as ferramentas que usei foram rabbitmq para implementar fila de mensageria... etc)
    
    - Seria válido alterar os objetivos específicos? Dependendo do que eu consegui fazer. Ou se eu achar que o trabalho fica melhor mudando o objetivo mesmo.
           -Pq eu escrevi de um modo que fica parecendo que eu quero estudar as ferramentas, mas eu só quero estudar a arquitetura mesmo (e o desenvolvimento?).

    - Qual tipo de leitor eu devo presumir que está lendo? Alguem com conhecimento técnico? Imagino que seria alguem da área de computaácão que está interessado em aprender sobre microsserviços. Então eu não precisaria, por exemplo, explicar o que é um "commit"

## TO DO:

    #### Prioridade alta:
    - Implementar login de cliente (front)
    - Implementar o front de carrinho
    - Orquestração de serviços (kubernetes ou docker swarm)
    - Implementar mensageria - carrinho e loja ✅
    - Parametrizar configurações dos serviços - Front✅, FrontAdmin✅, Loja✅, Auth✅, Carrinho✅
    - API Gateway (Nginx ou AWS ElasticLoadBalancing)
    - Monitoramento e Logging
    - CI (com GitHub Actions) ✅
    - CD (decidir entre deploy na AWS ou local com imagens de containers e kubernetes)
    
    #### Prioridade média
    - Implementar salvamento de imagens dos produtos no banco de dados e gerenciamento no front de administração (?)
    - Testes na teoria
    - Testes na prática
    - Proteger todos os endpoints do serviços - FrontAdmin ✅, Auth ✅, Loja ❌, Carrinho? ❌, Front? ❌
    - Implementar gerenciamento de usuários no front de administracao ✅
    - Implementar login de admin (front-admin) ✅

    #### Prioridade baixa:
    - Caching na Loja (com Memcached)
    - Serviço de Financeiro
    - Serviço de Recomendação
    - Clustering no rabbitmq
        https://www.rabbitmq.com/docs/distributed
        https://www.rabbitmq.com/docs/clustering

    - Sobre CI/CD na parte escrita: 
        -Compilação vem antes ou depois do merge? Ou os dois?? Para testar é preciso compilar o código primeiro..? Talvez seja melhor remover essa etapa e colocá-la na mesma etapa de testes: "Testes (após compilação, caso não seja linguagem interpretada)"
        -Então essa ilustração pode mudar um pouco, dependendo da linguagem (compilada ou interpretada).
        - Em linguagens compiladas, o artefato geralmente é o executável gerado pela compilação. Em linguagens interpretadas, o artefato depende do que é o ambiente de produção. Se for um servidor web, por exemplo, o artefato é o próprio código-fonte. Se for num servidor de aplicação usando java e TomCat, por exemplo, o artefato é o .war (ou .jar?) compilado, e o deploy é mandar esse .war para o servidor e executá-lo. Se o ambiente de produção está em containers docker, o artefato é a geração de uma nova versão da imagem sendo usada, e o deploy seria usar buildar um container com essa nova versão. Fonte: curso de integraçao continua do vinicius dias
        -A etapa "Testes de unidade e integração" na imagem que retrata CI/CD deveria ser "Esteira (pipeline) de CI", que pode englobar diversas etapas, tal como lint, testes, formatação, etc
        -Revisão (mesma coisa que versão?) deveria ser antes do merge??

    - Reorganizar os tópicos no 4.1



### pra lembrar:
- Sobre o script de entrypoint nos dockerfiles do projeto do curso:
    https://docs.docker.com/reference/compose-file/services/#command
    (Pode ser usado para rodar um shell, por exemplo `sh ./entrypoint.sh`.)

- Tipos de autenticação (https://www.alura.com.br/artigos/tipos-de-autenticacao):
    Autenticação em APIs é _stateless_, ou seja, o server não guarda uma sessão do usuário. Não lembra das requisições passadas; cada requisição é independente.
    Autenticação em apps web tradicionais é _stateful_ - o servidor guarda o estado como uma sessão, e nas próximas requisições consegue identificar o usuário e se já foi autenticado. 

- no linux, `echo $?` mostra o retorno do ultimo comando usado. 0 significa sucesso, qualquer outra coisa é não sucesso. então pode ser usado após cada etapa de testes para verificar o resultado do teste que acabou de ser executado.

- O docker-compose dentro de cada repositório dos microsserviços deve rodar todos, e apenas todos, os serviços necessários para aquele microsserviço operar. No front-admin por exemplo, vai ter o próprio front-admin, a api de usuarios e a api de produtos. No carrinho, vai ter o próprio carrinho, o mongo, e o rabbitmq.
- O docker-compose no multi-repo principal é que vai conter tudo da aplicação. E poderá servir como base para o deploy..? Ou será que vai ser melhor ver isso com Kubernetes


## Passos gerais
    Fazer a comunicação entre os projetos/serviços
        apis, mensageria e talvez RPC

    Aplicar as práticas/ferramentas estudadas
    
    Esquematizar cada funcionalidade do site, e separar em (micro)serviços. (Usar Domain-Driven Design?)
        - site de compras?
            Possíveis microsserviços
            * Frontend - (micro)serviço de ponta: website ou aplicativo mobile
            * catálogo
            * carrinho
            * pedidos
            * pagamento/financeiro
            * recomendações/marketing (usar IA? AM?)
            * cadastro - feito em um (micro)serviço de negócio: envia os dados necessários para o microsserviço de pagamento, de gamificação, e de cursos, que seriam 3 inserts and 3 bancos de dados diferentes
            * recompensas, gamificação
            * microsserviço de tradução - acesso à uma API externa (de pagamento por exemplo), um serviço de logs ou de notificação

    Desenvolver o site
        - Aproveitar o site que eu fiz com php e html? quebrar em microsserviços? (isolando os dados ou os domínios) (sidecar pattern - usar pacote de código)
        (haverá diferentes projetos (projeto laravel, node, python, etc) para diferentes microserviços)

## Planejamento:
    1. Apresentar ferramentas que frequentemente são usadas em aplicativos com arquitetura de microsserviços
    ~Propor uma combinação de ferramentas para o desenvolvimento de aplicações com arquitetura de microsserviços~

    2. Contextualizar essas ferramentas e apontar os problemas que resolvem e necessidades que suprem

    3. --> Esquematizar cada funcionalidade do site, separando em (micro)serviços. (Usar Domain-Driven Design? Aplicando as camadas do DDD na arq. de microserv)
    
    4. Desenvolver uma aplicação de exemplo usando as práticas/ferramentas propostas
    
    5. Discutir pontos positivos e negativos das ferramentas usadas

    6. Preparar apresentação do trabalho

## Ideias:
    - AMQP 1.0 não é necessariamente melhor do que AMQP 0.9. E 0.9 ainda é bem mais usado
        https://www.rabbitmq.com/blog/2024/08/05/native-amqp#the-pervasiveness-of-amqp-091

    - Usar alguma referencia para o livro Distributed Systems

    - Comentar sobre segurança em camadas (defense in depth)

    - Comparar quais fatores dos 12-factors a aplicação segue
    
    - Etapas de build (pipeline): 
            lint - (eslint);
            formatação de código - (prettier https://prettier.io/docs/install#git-hooks);
            analise estatica;
            testes
            (um projeto real provavelmente teria bem mais etapas)
    
    - adicionar réplicas a cada microserviços (orquestradores)
    
    - monitoramento: prometheus, grafana, telegraph, 
    
    - Nginx: api gateway e geração de logs?
        - (reverse) proxying: redirecionar requisições para o serviço apropriado
        - forward apenas as requisições de certo ip
        - adicionar informações na request ou na resposta
        - ter informações de cache no api gateway
        - Load balancing rústico: usar upstreams do nginx
        - plugin para gerar logs e mandar para ferramenta de monitoramento. (formato de logs deve ser igual entre os microsservicos)

    - Ponto importante para a apresentação ou em alguma parte do texto escrito: 
        * Vinicius Dias: As soluções para os requisitos/problemas de **negócio** podem ser iguais tanto na arquitetura monolítica quanto na de microsserviços, ou >seja, podem ser satisfeitos/solucionados nas duas arquiteturas. E esses problemas naturalmente tem maior prioridade de serem solucionados do >que os técnicos. E conhecendo o domínio, é mais fácil dividir a aplicação em microsserviços. É a solução para requisitos/problemas técnicos que >geralmente mudam, como a escalabilidade por exemplo. E esse é um dos motivos para que seja recomendado a abordagem monolítica primeiro.
        Fonte: Vídeo-aula do vinicius dias

        * EU: E apesar de ser recomendado começar na monolitica, também é válido ir direto para os microserviços se o domínio da aplicação for bem >conhecido, e soubermos que a aplicação é grande e precisará de uma infra mais flexível. O aplicativo aqui demonstrado é uma aplicação pequena, >e por isso faria mais sentido usar a abordagem monolitica. Mas esse trabalho trata da arquitetura de microsserviços, então é claro, foi usada >esta arquitetura, mesmo que para algumas questões usá-la não faça muito sentido.

        * EU: Vai muito da questão de conhecer o domínio da aplicação. Como essa divisão do domínio e serviços é feita define como a comunicação no sistema e o armazenamento de dados acontece, e certas divisões vão acarretar em mais ou menos comunicação/duplicação de dados. Por exemplo, onde será que deveria ser armazenados os pedidos feitos na loja? No serviço de catálogo (passaria a ser loja), ou serviço de carrinho (passaria a ser pedidos)?

        *There's no single primary data store with which all services interact. Instead, coordination and communication between the services is done on an as-needed basis and by using a message bus. Fonte: <https://learn.microsoft.com/en-us/dotnet/architecture/cloud-native/introduce-eshoponcontainers-reference-app>

    - Substituir alguma chamada HTTP no sistema por uma RPC (tem RPC no rabbitmq)
        - ler: https://www.geeksforgeeks.org/remote-procedure-call-rpc-in-operating-system/
                https://medium.com/@karthikheyan.mgn/remote-procedure-calls-rpc-in-javascript-enhancing-inter-service-communication-7bc45517c00e
        - ver frameworks de RPC no node: https://www.reddit.com/r/node/comments/zz6h8z/ive_made_a_big_comparison_table_of_nodejs_rpc/
        - ou tentar usar o gRPC (projeto .net?)
            https://learn.microsoft.com/en-us/aspnet/core/grpc/?view=aspnetcore-9.0
        - ou tentar usar no node com rabbitmq?
            https://github.com/toa-io/comq
            https://www.rabbitmq.com/client-libraries/devtools#node-dev

    - API gateway pode ser um ponto massivo de falha
        - tentar usar alguma outra técnica no lugar (gRPC)

    - Replicação de databases:
        - https://stackoverflow.com/questions/58399450/keeping-databases-in-sync-after-write-update-across-regions-zones
        - ler: https://atlan.com/what-is/database-replication/
        - ler: https://release.com/blog/syncing-databases-how-to-do-it-and-best-practices
        - Um servidor para escrita e outro para leitura?
        - ver se tem algum serviço assim na AWS
    
    - Sobre o objetivo específico de **Propor** uma combinação de ferramentas:
        A arq de microsserviços geralmente são usados em aplicações grandes, mas claro que não tenho condição de construir um sistema assim em poucos meses. 
        Então a aplicação que fiz é pequena (pequena como? em termos de que?). 
        Acontece que "quais ferramentas usar" depende muito do sistema desenvolvido. 
        E quais vão ser usadas se limitam às funcionalidades de infraestrutura que eu desenvolvi. 
        Além disso, também se limitam a ferramentas open-source pq n faria sentido, no contexto academico, pagar pra fazer um projeto de demonstração
        Sendo assim, as ferramentas que usei foram rabbitmq para implementar fila de mensageria... etc

        - Eu poderia também analisar as ferramentas usadas em uma aplicação grande já pronta, 
        talvez algum produto transparente de uma empresa, ou então o eShopOnContainers.

    - Certificado https
        único que emite gratuitamente: https://letsencrypt.org/
        openssl
        dotnet enforce https: https://learn.microsoft.com/en-us/aspnet/core/security/enforcing-ssl?view=aspnetcore-5.0&tabs=visual-studio%2Clinux-sles

    - hoje em dia é mais comum armazenar imagens em um sistema de arquivos na núvem (como o S3 da amazon), mas pra manter o projeto mais independente de serviços pagos, decidi armazenar no banco de dados mesmo

    - Requerimentos não funcionais:
        The application also has the following non-functional requirements:
            It needs to be highly available and it must scale automatically to meet increased traffic (and scale back down once traffic subsides).
            It should provide easy-to-use monitoring of its health and diagnostic logs to help troubleshoot any issues it encounters.
            It should support an agile development process, including support for continuous integration and deployment (CI/CD).
            In addition to the two web front ends (traditional and Single Page Application), the application must also support mobile client apps running different kinds of operating systems.
            It should support cross-platform hosting and cross-platform development.

    - Microsserviços podem se comunicar de forma síncrona?
        Provavelmente não
        https://stackoverflow.com/questions/64166853/is-synchronous-communication-between-services-anti-pattern-in-microservices

    - Os microsserviços devem estar em redes internas exclusivamente com os microsserviços que precisam estar lá. O frontend é a interface entre a internet e essa rede interna. Toda requisição passaria pelo api gateway, então só precisaria existir autenticação e autorização alí (?)
    >>> Como fazer autentição no api gateway: 
                https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/
                https://stackoverflow.com/questions/72434853/ways-of-authentication-with-nginx

    - Parametrizar configurações de desenvolvimento e de release em todos os serviços (aumenta a portabilidade dos serviços)

    - Fazer algum caching em algum serviço. no spring é muito fácil: https://spring.io/guides/gs/caching

    - Parte escrita: Sobre os problemas de alguns frameworks aplicados a microsserviços:
            https://python-microservices.github.io/
            https://microservices.io/patterns/observability/distributed-tracing.html
            https://microservices.io/patterns/microservice-chassis.html --> Ver o spring initializr, que parece seguir esse padrão

    - Parte escrita: Containers, e Docker especificamente, é uma enorme mão na roda para o desenvolvimento de microsserviços

    - Parte escrita: Sobre bancos de dados - Qual banco de dados escolher? SQL, NoSQL, newSQL? Discutir 
            Palavras-chave: integridade (de dados), disponibilidade, tolerancia, ACID
            Teorema CAP (Consistency, Availabilty and Partition Tolerance) - https://www.ibm.com/think/topics/cap-theorem
            https://www.ibm.com/think/insights/choosing-the-right-databases-for-microservices
            https://www.reddit.com/r/learnprogramming/comments/lo5kpt/can_someone_explain_with_example_when_to_choose/
            https://stackoverflow.com/questions/3713313/when-should-i-use-a-nosql-database-instead-of-a-relational-database-is-it-okay
            https://www.mongodb.com/resources/basics/databases/nosql-explained/nosql-vs-sql
            O site do MongoDB possui um artigo bastante rico sobre as diversas formas que os dados de um banco de dados podem ser modelados em uma relação 1:N. Artigo https://www.mongodb.com/blog/post/6-rules-of-thumb-for-mongodb-schema-design

            Comparação de bancos de dados: https://www.altexsoft.com/blog/comparing-database-management-systems-mysql-postgresql-mssql-server-mongodb-elasticsearch-and-others/
                Aparentemente postgresql é comum entre instituições financeiras?

    - Parte escrita: Discutir aplicações serverless e a abordagem cloud native
            https://cursos.alura.com.br/extra/alura-mais/o-que-e-cloud-native--c874
    
    - Parte escrita: Considerar comunicação com graphql. Discutir sobre APIs com GraphQL, e como é uma alternativa ao REST.

    - Análise de viabilidade poderia ter a ver com os requisitos funcionais e não funcionais do sistema e alguns diagrama UML, relacionando com ferramentas.

    - Documentação das APIs para demonstração de contratos de dados e versionamento (como é discutido em um dos tópicos do tcc)


## Propor uma combinação de ferramentas e contextualiza-las (problema/solução):
    - Linguagens/Framework:
        - Backend:
            ExpressJS - minimalista, unopinionated
            Django/PyMS (python) https://python-microservices.github.io/
            Dotnet <-- mais alto nível
            Spring (https://spring.io/microservices) <-- ecosistema maduro, de alto nível e com muita biblioteca. facilita a padronização de serviços e redução de código boilerplate. já tem muita coisa pronta, e facilita a externalização da configurabilidade. por outro lado, reduz a clareza do código (ex classe para tratar qualquer erro na api. o programador pode ficar perdido sem saber de onde vem esse tratamento) e a liberdade do programador (é um framework >>opinionated<<). da pra compilar o app todo em um jar, o que melhora a portabilidade do app. documentação não concisa, incompleta e desatualizada. ver sobre spring cloud?
            Laravel
            Gin, Micro (https://github.com/micro/micro) - (golang)
            (PHP, JavaScript, Python, C#, Java, Golang)
            Comparação frameworks: https://www.youtube.com/watch?v=FQPlEnKav48
        - Frontend:
            HTML/CSS/JS
            Angular, >React< (muito usado, muito suporte pra infinitos plugins), Vue (apropriado para projetos de pequeno a médio porte e com requisitos flexíveis)
            Javascript, >Typescript<,
            Vite (inicialização e configuração da aplicação bem fácil), create-react-app (abandonado), Snowpack (abandonado), webpack, rollup, parcel
            - CSS:
                ref: https://www.alura.com.br/artigos/tailwind-framework-bootstrap-tailwind
                TailwindCSS (baseado em utilidades. reutilizar componenetes feitos por outros. estilo mais customizável. amplamente usado e bem avaliado em enquetes, mas é muito baixo-nível), 
                react-bootstrap (baseado em componentes. praticidade e facilidade mas falta de originalidade), 
                materialize (já conheço mas não parece mais ter muito suporte),
                PureCSS (leve e responsivo)
                SASS (?)
                CSSModules (para organização de css em formato de módulos)
                DaisyUI -> framework (plugin) para tailwind
    - Mensageria:
        RabbitMQ
        Apache Kafka
    - Banco de Dados:
        PostgreSQL*, MongoDB*, ... Sobre escolher BDs: https://www.ibm.com/think/insights/choosing-the-right-databases-for-microservices
    - Controle de versão:
        Git, Svn
    - Gerenciamento de repositórios:
        GitHub<, GitLab, bitbucket, amazon s3
    - Organização de código:
        Multirepo<, Monorepo
    - Containers:
        >Docker<, LXC (linux containers)
    - Orquestração de containers:
            Docker Swarm ou Kubernetes
    - CI/CD:
        >GitHub Actions<, Jenkins, CircleCI, GitLab (both ci/cd), travisCI
    - Comunicação:
        HTTP, APIs (REST ou GraphQL), RPC com gRPC?, fastCGI?
    - Testes:
        Selenium, jest (javascript), contract cesting with Pact?, Playwright, cypress, postman/thunderclient, aws device farm
        Ler: https://www.atlassian.com/continuous-delivery/software-testing/types-of-software-testing
    - Logging:
        nginx (com plugins), 


>"Here’s some other lessons we’ve learned along the way:
>
>    Networking/debugging now becomes a Pain In The Ass.
>
>    Try to separate microservices by the business domain they cover and have one service handle one action completely (I.E, signing up a new user). If you start having to reach out to several services for one action, you’re going down a dark path with a lot of failure points.
>
>    Using something like Apache Kafka as an event source that is central source of truth in your system. You are eventually going to have failures which will cause discrepancies in your data. You need someplace to go to rebuild a service when that happens.
>
>    Copying data is okay. Every service should be able to query its own materialized view of the global data in its own database. This means that some pieces of data (I.E., user data) might be saved in a lot of places which may lead to different results from different services.
>
>    “No service should know anything about the other” is a hard concept to grasp. This tripped us up in the beginning as we were trying to follow this mantra too closely. It should read “no service should know about the internal workings of another.” We found though that exposing a global REST API that’s a conglomerate of an individual REST APIs per service is a good practice for command based actions that require validation.
>
>    Change data capture is your friend for events."
>    https://www.reddit.com/r/node/comments/106qfbb/first_time_building_microservicebased_application/




# TCC 1

\citeonline{WASEEM2020110798} diz que ...
[...] \cite{WASEEM2020110798}

\textit{testeteste}
\emph{123 teste teste}

- pode usar a imagem do mapeamento de problemas e soluções no anexo? sim, citação comum
imagem
fonte: ()

Separar em
1- introduçao
2- fundamentacao
3- padrões
4- soluçoes

- tem que explicar em algum lugar as palavras que eu deixar em ingles? por exemplo pipeline - fluxo? usar um parentese mesmo

- procurar uma outra referência sobre começar primeiro com o monolito e não com microserviços.

Escolher alguns tópicos para se aprofundar?
Enumerar os problemas, e aprofundar melhor em alguns?
Abordar em maior profundidade (opções):
- infra como código
- observabilidade (monitoramento)
- testes
- ci/cd

- como citar parágrafos seguidos? cita em cada ou pode ser apenas no ultimo?

- falar sobre REST?
- melhorar introdução .. falar sobre?
- adicionar mais trabalhos relacionados?
- usar imagem do eShopOnContainers?
- ler o resto do artigo 'microservices' de martin fowler (infrastructure automation)

- 4.7 monorepo ou multirepo
- soluções: cd
