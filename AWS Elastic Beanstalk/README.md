# AWS Elastic Beanstalk

Mais conhecido como EB ou BeanStalk, é uma plataforma como serviço (PaaS). Basicamente a plataforma gerencia e provisiona a infraestrutura ao redor de uma arquitetura de software que o usuário provê. É um serviço totalmente focado para os desenvolvedores, onde ele ficam focado totalmente focado no código, e o BeanStalk cuida da parte do provisionamento da infraestrutura. É um serviço altamente customizado para atender as necessidades de desenvolvimento. A infraestrutura provisionada sempre é provisionada seguindo as melhores práticas e o Well Architected Framwork. O grande destaque do serviço é a sua grande variedade de possíveis configurações.

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

- Elastic Beanstalk Deployment Policies: também é conhecido como <b>Deployments Types</b>, basicamente é definição das estratégias e métodos de como a versão de uma aplicação (Application Version) será deployada para um o ambiente (Elastic BeanStalk Environment). Existem alguns tipos de deployment, cada um possui uma vantagem específica e dependerá inteiramente da estraégia de arquitetura de cada aplicação.

   - <b>All at once</b>: Esse método é o mais rápido e mais inseguro caso haja a necessidade rollout. Neste método, todas as instâncias no ambiete do BS serão atualizadas de uma única vez com a nova versão da aplicação.

   - <b>Rolling Deployment</b>: Esse método é super eficaz e seguro e não apresenta custos adicionais para a aestratégia de deploy. Nele as instâncias uma a uma são retiradas do LoadBalancer e ficam fora de serviço até recebrem a nova versão da aplicação, após passar nos HealthChecks estabelecidos as instâncias são recolocadas no Elastic Load Balancer e voltam ao serviço.

   - <b>Rolling with an additional batch</b>: Essse método é muito similar ao rollign deployment a diferença é que o número original de instãncias é acrescido por uma nova batch de instâncias que estarão fora de serviços e vão receber as novas versões da aplicação, uma vez que essa batch de novas instância passarem pelo Health Check elas estarão aptas para serem registradas no ELB e subsituirem  as intâncias originais.

   - <b>Immutable</b>: Neste método, uma nova qauntidade instâncias (nova infraestrutura) será criada para receber a nova aplicação, uma vez que receberem a nova versão as instâncias originais serão desativadas e as novas entrarão em seu lugar.

   - <b>Traffic Splitting</b>: Neste método novas instâncias sã criadas e registradas ao mesmo tempo no ELB para trabalharem em conjunto com as instâncias originais, então ao mesmo tempo teremos duas versões da mesma aplicação sendo executada, pemrtindo utilizar o Route 53 para realizar o controle de tráfego e relaizar testes no modelo A/B.

   <b>Ponto interessante:</b> Como toda Application Version do Elastic Beanstalk é armazenada no S3 é possível configurar LifeCicle policies para essas versões, alguns casos de uso onde o BS é utilizado em conjunto com uma esteira de CI/CD podem surgir muitos registros de versões, e o life cicle policies resolve este problema de maneira muito fácil.

- Elastic Beanstalk e o Relational Database Service (RDS): Se você quiser utilizar um RDS integrado a um ambiente do BS você tem duas opções:

   - Criar o RDS dentro do environment do Elastic Beanstalk, mas isso pode gerar perda de dados caso o environment seja deletado ou modifcado.

   - Criar o RDS instance fora do environmento do Elastic Beanstalk (melhor opção)

   - <b>Importante:</b> Caso você queira desacolplar uma instância RDS que tenha sido criada diretamente dentro do environment do Elastic Beanstalk, existe um processo a ser seguido:

      - Tirar um snapshot da instância do RDS.
      - Habilitar a opção "Enable Delete Protection" na instância do RDS.
      - Criar uma nova aplicação no Beanstalk com a mesma versão.
      - Se assegurar que a nova aplicação possa se comunicar com o RDS.
      - Terminate o velho ambiente que estava com o RDS acoplado.
      - Ir no cloud formation e deletar a stack do antigo ambiente.


- Ebextensions: é uma feature do elastic beanstalk que permite customizar os environments e permite o cloudformation a provisionar recursos adicionais que normalmente não são provisionados via console.

- Elastic Beanstalk com HTTPS: é possível utilizar o HTTPS na aplicação que você irá subir no BS, para conseguir isto basta adicionar um certificado SSL ao Elastic LoadBalancer provisionado, diretamente via console da AWS, ou utilizar o .ebextensions para o cloudformation adicione o certficado ao ambiente.

- Elastic Beanstalk Environment Cloning: essa feature permite clonar um abiente que já foi provisionado e já está em execuação (não nenhum tipo de downtime ao relaizar esta ação.)

- Elastic Beanstalk com Docker: é possível utilizar o Docker no BS, ao invés de realizar o deploy da sua aplicação em instãncias EC2, basicamente o deploy será realizado no Docker.