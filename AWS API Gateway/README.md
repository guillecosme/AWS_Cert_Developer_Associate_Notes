# AWS API Gateway

O API gateway é o serviço que permite realizar a criação de API na estrutura da AWS, expor essas API's publicamente e integrar essas API's com outros serviços da AWS.

- REST/ RESTFul API (representaional state transfer)
- Socket API:
- SOAP (Simple Object Access Protocol) API: define uma maneira/linguagem de comunicação para a API, normalmente baseada em XML

Os tipos de recurso normalmente são os mesmos métodOs utilizados no protocolo HTTP:

    - POST
    - GET
    - DELETE
    - PUT

- URL de invcação: é o ponto de entrada para acessar os enpoints/recursos da API criada e publicada através do API Gateway. Esta URL é composta de algumas informações importantes;

    - O Enpoint do API Gateway
    - O estágio da API em questão
    - O resource do API Gateway.


- API Gateway Integrations: As integrações é uma featura do API Gateway que basicamente como as chamadas atrvés da URL de invocação será integrada com a API, ou outros recursos dentro do ambiente da AWS.

- API Gateway Stages e Deployments: Uma alteração na API só têm efeito caso o deply seja realizado em algum estágio.

- Open API e Swagger
