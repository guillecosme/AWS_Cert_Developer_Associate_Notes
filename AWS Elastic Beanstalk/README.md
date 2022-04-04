# AWS Elastic Beanstalk

Mais conhecido como EB ou BeanStalk, é uma plataforma como serviço (PaaS). Basicamente a plataforma gerencia e provisiona a infraestrutura ao redor de uma arquitetura de software que o usuário provê. É um serviço totalmente focado para os desenvolvedores, onde ele ficam focado totalemnte focado no código, e o BeanStalk cuida da parte do provisionamento da infraestrutura. É um serviço altamente customizado para atender as necessidades de desenvolvimento. A infraestrutura provisionada sempre é provisionada seguindo as melhores práticas e o Well Architected Framwork. O grande destaque do serviço é a sua grande variedade de possíveis configurações.

## Elastic Beanstalk Plataforms

É definição dos tipos de funcionalidades e arquiteturas que o serviço oferece

Built In  Languages: Docker & Custom Plataforms

 - Go, Java, TomCat
 - .Net Core (Linux) & .Net Core & .Net (Windows)
 - Python, NodeJS, PHP, Ruby

 ## Conceitos Fundamentais do Beanstalk

 - Elastic Beanstalk Application: é o container que contém todas as informações referente a aplicação que será gerenciada pelo Beanstalk

 - Application Versions: é um versão de uma application container do Elastic Beanstalk, essas versões e todas as infirmações normalmente são armazenadas em Buckets S3, estes buckets são referenciados como <b>Bundle Source</b>

 - Environments: cada Elastic Beanstalk Application pode ter um ou mais environments, esse environments são basicamente ambientes configuráveis de infraestrutura onde você pode realizar o staging da sua aplicação (Dev, Homol, Prod)

    - <b>Worker Environment:</b>

 - Blue/Green Deployment: 

 - Direcionamento de trafégo para ambiente utilizando o Route 53

 - Importante: todos os banco de dados que servem uma aplicação, devem, como melhores práticas, serem criados fora do Beanstalk, posi caso uma nova versão da aplicação seja criada e o Banco deletado não haverá perda de dados.

- Elastic Beanstalk Deployment Policies