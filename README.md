# Documentação do Projeto DevOps Course

## Introdução
Este documento descreve a implementação de um projeto de infraestrutura e microserviços aplicando práticas DevOps utilizando Kubernetes, Docker, Prometheus, Grafana, entre outras tecnologias. O sistema foi projetado para garantir alta disponibilidade, resiliência e monitoramento contínuo.

## Arquitetura
- Microserviços Utilizados:
  - ProdutoService: Gerencia informações de produtos.
  - VendaService: Lida com operações de venda de produtos.
- Discovery Service: Eureka Server para descoberta dinâmica de serviços.
- API Gateway: Utilizando Spring Cloud Gateway para roteamento e filtragem de tráfego.
- Client-side Load Balancer: Integração com Eureka para balanceamento de carga.
- Monitoramento e Rastreamento: Uso do Zipkin para rastreamento distribuído.

<img src=" capturas-de-telas /Eureka.png" width="100%" height="100%">

*Figura 1: Registro de serviço Eureka mostrando o status dos microserviços*

## Tecnologias e Ferramentas
- Spring Boot para a criação dos microserviços.
- Docker para conteinerização dos serviços e banco de dados.
- Kubernetes para orquestração dos containers.
- Eureka Server: Serviço de descoberta para gerenciamento de instâncias de microserviços.
- Resilience4j: Implementação de padrões de resiliência como Circuit Breaker e Retry.
- OpenFeign: Para comunicação declarativa entre microserviços.
- Swagger/OpenAPI: Para documentação dos endpoints dos serviços.
- PostgreSQL: Banco de dados para persistência de dados.
- Spring Cloud Gateway: Como gateway de API para roteamento e filtragem.
- Zipkin: Para rastreamento distribuído das chamadas entre serviços.
- Maven: Para gerenciamento de dependências e build do projeto.
- Prometheus e Grafana: Para monitoramento de métricas e visualização de dados.

## Docker Hub Images
As imagens Docker foram construídas com as configurações necessárias para cada serviço do nosso sistema de microserviços e publicadas no Docker Hub. Estas imagens são então usadas pelo Kubernetes para implantar cada serviço no cluster.

<img src=" capturas-de-telas /dockerhub.png" width="100%" height="100%">

*Figura 2: Exemplo das imagens disponíveis no Docker Hub que são usadas pelo Kubernetes*

## Implementação
### Criação do Namespace
- O namespace devops-course foi criado para isolar os recursos do projeto dentro do cluster Kubernetes.

<img src=" capturas-de-telas /yaml.png" width="100%" height="100%">

*Figura 3: Exemplo do arquivo .yaml com todas as configurações de deployments e services disponíveis usadas pelo Kubernetes*

### Deployments e Services
- Foram implementados Deployments e Services para cada componente do sistema, garantindo sua comunicação e funcionamento correto. O Api-Gateway foi configurado com quatro réplicas para assegurar a alta disponibilidade.

<img src=" capturas-de-telas /replica.png" width="100%" height="100%">

*Figura 4: Exemplo de configuração do api-gateway deployment usando 4 replicas*

### Probes de Saúde
- Readiness e Liveness probes foram adicionadas aos microserviços para assegurar que o tráfego seja direcionado apenas para instâncias saudáveis e que instâncias com falhas sejam reiniciadas automaticamente.

<img src=" capturas-de-telas /probe.png" width="100%" height="100%">

*Figura 5: Exemplo de configuração do Readiness e Liveness*

### Monitoramento e Visualização
- ConfigMaps foram utilizados para definir as dashboards do Grafana, permitindo visualizar métricas importantes como contagem de threads, uso de CPU e memória.

<img src=" capturas-de-telas /mapgrafana.png" width="100%" height="100%">

*Figura 6: Exemplo de configuração do configMap para o Grafana*

### Banco de Dados
- Um deployment do PostgreSQL foi configurado juntamente com um serviço NodePort, permitindo acesso externo para fins de administração e desenvolvimento.

<img src=" capturas-de-telas /postgres.png" width="100%" height="100%">

*Figura 7: Exemplo de configuração do deployment e service para o Postgres*

### Rastreamento Distribuído
- O Zipkin foi implantado para permitir o rastreamento das requisições entre os serviços, facilitando o diagnóstico de problemas.

<img src=" capturas-de-telas /zipkin.png" width="100%" height="100%">

*Figura 8: Rastreamento do Zipkin para sistemas distribuídos*

### Coleta de Métricas
- O Prometheus foi configurado para coletar métricas dos microserviços, utilizando o modelo de push para a coleta.

<img src=" capturas-de-telas /prometheus.png" width="100%" height="100%">

*Figura 9: Interface do Prometheus para monitoramento de métricas dos microserviços*

### Testes de Carga com JMeter

- Como parte do processo de garantir a qualidade e a performance dos microserviços, foram realizados testes de carga usando Apache JMeter. Um conjunto de testes foi criado para simular diferentes cargas de usuários acessando o ProdutoService para avaliar o desempenho e a estabilidade do serviço sob condições variadas.

### Configuração do Teste
O teste GetAllProdutos foi configurado para fazer requisições GET para o endpoint de listagem de todos os produtos. A configuração especificou o host como localhost e a porta 30080, indicando que o serviço foi exposto externamente para fins de teste.

### Execução e Resultados
O teste foi executado com sucesso, e os resultados foram analisados utilizando os listeners internos do JMeter, como a Árvore de Resultados e o Relatório de Resumo, para verificar as respostas e os tempos de resposta. Além disso, os resultados foram visualizados no Grafana para monitorar as métricas em tempo real.

<img src=" capturas-de-telas /jmeter.png" width="100%" height="100%">
<img src=" capturas-de-telas /jmeter1.png" width="100%" height="100%">
<img src=" capturas-de-telas /jmeter2.png" width="100%" height="100%">

*Figuras 10, 11 e 12: Testes com JMeter*

### Integração Contínua com Jenkins

- Para automatizar o processo de build e deploy dos microserviços, foi utilizado o Jenkins, uma ferramenta de integração contínua. Uma pipeline de CI/CD simples foi criada para automatizar a compilação, os testes e o deployment dos serviços quando novos commits são feitos no repositório de código.

#### Configuração da Pipeline
- A pipeline foi configurada com os seguintes passos:

Checkout: Recuperar o código mais recente do repositório Git.
Build: Construir os artefatos usando Maven.
Test: Executar testes automatizados.
Docker Build e Push: Construir a imagem Docker para o serviço e enviar para o Docker Hub.
Deploy no Kubernetes: Atualizar o serviço no Kubernetes com a nova imagem.
A pipeline foi definida como código usando o Jenkinsfile e armazenada no repositório de código para versionamento e fácil manutenção.

## Resiliência e Segurança
- Implementação de Circuit Breaker e Retry com Resilience4j.

## Monitoramento
- Monitoramento de métricas com Prometheus e visualização no Grafana.

<img src=" capturas-de-telas /grafana.png" width="100%" height="100%">

*Figura 7: Dashboard do Grafana visualizando métricas dos serviços*

## Conclusão
Este projeto demonstrou a eficácia da utilização de contêineres e orquestração para gerenciar um sistema complexo de microserviços. Com o uso do Kubernetes, foi possível garantir a disponibilidade e escalabilidade dos serviços. O Prometheus e o Grafana forneceram as ferramentas necessárias para monitorar a saúde e o desempenho do sistema, enquanto o Zipkin permitiu o rastreamento distribuído eficiente.
