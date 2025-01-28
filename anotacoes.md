# TCC 2

Perguntar:
    - mudar o nome do orientador? deixar os 2
    - como ficaria o capitulo de analise de viabilidade? considerar no final
    - remover capitulo de plano de continuidade?
    - Seria válido alterar os objetivos específicos? Dependendo do que eu consegui fazer. Ou se eu achar que o trabalho fica melhor mudando o objetivo mesmo.
        - Pq eu escrevi de um modo que fica parecendo que eu quero estudar as ferramentas, mas eu só quero estudar a arquitetura mesmo (e o desenvolvimento?).

### pra lembrar:
Sobre o script de entrypoint nos dockerfiles do projeto do curso:
https://docs.docker.com/reference/compose-file/services/#command
(Pode ser usado para rodar um shell, por exemplo `sh ./entrypoint.sh`.)

## TO DO:
    > Esquematizar cada funcionalidade do site, e separar em (micro)serviços. (Usar Domain-Driven Design?)
        - site de compras?
            Possíveis microsserviços
            * Frontend - (micro)serviço de ponta: website ou aplicativo mobile
            * catálogo
            * carrinho
            * pagamento
            * recomendações (usar IA? AM?), marketing
            * cadastro - feito em um (micro)serviço de negócio: envia os dados necessários para o microsserviço de pagamento, de gamificação, e de cursos, que seriam 3 inserts and 3 bancos de dados diferentes
            * recompensas, gamificação
            * microsserviço de tradução - acesso à uma API externa (de pagamento por exemplo), um serviço de logs ou de notificação

    > Criar o projeto inicial do site
        - Aproveitar o site que eu fiz com php e html? quebrar em microsserviços? (isolando os dados ou os domínios) (sidecar pattern - usar pacote de código)
        (haverá diferentes projetos (projeto laravel, node, python, etc) para diferentes microserviços)

    Fazer a comunicação entre os projetos/serviços
        api gateway
        mensageria rabbitmq
        rpc?

    Aplicar as práticas/ferramentas estudadas

## Planejamento:
    1. Propor uma combinação de ferramentas para o desenvolvimento de aplicações com arquitetura de microsserviços

    2. Contextualizar essas ferramentas e apontar os problemas que elas resolvem

    3. --> Esquematizar cada funcionalidade do site, separando em (micro)serviços. (Usar Domain-Driven Design? Aplicando as camadas do DDD na arq. de microserv)
    
    4. Desenvolver uma aplicação de exemplo usando as práticas/ferramentas propostas
    
    5. Discutir pontos positivos e negativos das ferramentas usadas

    6. Preparar apresentação do trabalho

## Ideias:
    - Comparar quais fatores dos 12-factors a aplicação segue
    
    - Etapas de build: lint, verificar estilo de codigo, analise estatica, testes...
    
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
        * As soluções para os requisitos/problemas de **negócio** podem ser iguais tanto na arquitetura monolítica quanto na de microsserviços, ou >seja, podem ser satisfeitos/solucionados nas duas arquiteturas. E esses problemas naturalmente tem maior prioridade de serem solucionados do >que os técnicos. E conhecendo o domínio, é mais fácil dividir a aplicação em microsserviços. É a solução para requisitos/problemas técnicos que >geralmente mudam, como a escalabilidade por exemplo. E esse é um dos motivos para que seja recomendado a abordagem monolítica primeiro.
        * E apesar de ser recomendado começar na monolitica, também é válido ir direto para os microserviços se o domínio da aplicação for bem >conhecido, e soubermos que a aplicação é grande e precisará de uma infra mais flexível. O aplicativo aqui demonstrado é uma aplicação pequena, >e por isso faria mais sentido usar a abordagem monolitica. Mas esse trabalho trata da arquitetura de microsserviços, então é claro, foi usada >esta arquitetura, mesmo que para algumas questões usá-la não faça muito sentido.
        * Vai muito da questão de conhecer o domínio da aplicação

    - Substituir alguma chamada HTTP no sistema por uma RPC
        - ler: https://www.geeksforgeeks.org/remote-procedure-call-rpc-in-operating-system/
        - ver frameworks de RPC no node: https://www.reddit.com/r/node/comments/zz6h8z/ive_made_a_big_comparison_table_of_nodejs_rpc/
        - ou tentar usar o gRPC

    - API gateway pode ser um ponto massivo de falha
        - tentar usar alguma outra técnica no lugar

    - Replicação de databases:
        - https://stackoverflow.com/questions/58399450/keeping-databases-in-sync-after-write-update-across-regions-zones
        - ler: https://atlan.com/what-is/database-replication/
        - ler: https://release.com/blog/syncing-databases-how-to-do-it-and-best-practices
    
    - Sobre o objetivo específico de Propor uma combinação de ferramentas:
        A arq de microsserviços geralmente são usados em aplicações grandes, mas claro que não tenho condição de construir um sistema assim em poucos meses. 
        Então a aplicação que fiz é pequena (pequena como? em termos de que?). 
        Acontece que "quais ferramentas usar" depende muito do sistema desenvolvido. 
        E quais vão ser usadas se limitam às funcionalidades de infraestrutura que eu desenvolvidas. 
        Além disso, também se limitam a ferramentas open-source pq n faria sentido, no contexto academico, pagar pra fazer um projeto de demonstração
        Sendo assim, as ferramentas que usei foram rabbitmq para implementar fila de mensageria... etc

        - Eu poderia também analisar as ferramentas usadas em uma aplicação grande já pronta, 
        talvez algum produto transparente de uma empresa, ou então o eShopOnContainers.

## Propor uma combinação de ferramentas e contextualiza-las (problema/solução):
    - Linguagens/Framework:
        HTML/CSS
        PHP
        JS: Node+ExpressJS 
        Python: PyMS, Nameko, Django
        C#: Dotnet?
    - Mensageria:
        RabbitMQ
    - Banco de Dados:
        PostgreSQL*, MongoDB*, Couchbase*, Redis*, Amazon DynamoDB
    - Controle de versão:
        Git
    - Gerenciamento de repositórios:
        GitHub
    - Organização de código:
        Monorepo
    - Containers:
        Docker
    - Orquestração de containers:
            Docker Swarm ou Kubernetes
    - CI/CD:
        GitHub Actions ou Jenkins
    - Testes:
        Selenium, contract cesting with Pact?
    - Comunicação:
        HTTP, APIs e REST, RPC com gRPC?
    - Testes:
        ?


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
