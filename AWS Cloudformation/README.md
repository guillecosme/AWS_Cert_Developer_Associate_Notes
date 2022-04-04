# AWS Cloud Formation 

Basicamente o Cloud Formation é um serviço para prver recursos físicos ou lógicos através da escrita de código. Normalmente os templates do Cloud Formation são escritos em JSON ou YAML, sendo YAML a maneira mais utilizada para criar os recursos da AWS.

No template do Cloud Formation você lida com os recursos lógicos, ou seja o que você desja provisonar e não como você quer provisionar. Existem esta diferença entre os conceitos. Cada template cria uma <b>Stack<b> que pode ser utilizada em várias regiões diferentes.

Quando você altera um template a stack é alterada, ou seja, você altera os recursos lógicos ne template e os recursos físicos no ambiente da AWS também são alterados. Se a stack é deletada os recursos físicos também são deletados.

Um exemplo simples de um template do Cloud Formation seria este


``` yaml
Resources:
s3Bucket:
  Type: AWS::S3::Bucket
  Properties: 
    AccessControl: Private
    BucketName: Simple_S3_Bucket_Example
    Tags:
      - Key: Owner
        Value: GuilhermeCosme
      - Key: Objetivo
        Value: TestStackCloudFormation
```

O template acima pode criar vários Bucket idênticos em várias regiões distintas de maneira automática sem a preocupação de ter que provisioná-los de maneira automática 

- Cloud Formation - Physical Resource:

        É a representação lógica dos recursos  no template YAML ou JSON. 

- Cloud Formation - Logical Resource:

        São de fato os recursos físicos no ambienta da AWS criados a partir de um template YAML ou JSON.

O Cloud Formation é uma ferramenta muito versátil e poderosa pois permite o provisionamento de infraestrutra de maneira rápida, segura e eficaz.

<b>Importante:<b> A AWS oferece um guia de usuário específico com toda a documentação e informações necessárias sobre as propriedades de cada serviço suportado pelo CloudFormation:

- [AWS Cloud Formation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)

- [Procurar por <b>Template Reference:<b>](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-reference.html)


# Example Básico de um primeiro template para provisionar um Bucket S3 e uma Instância EC2

``` yaml
# Stack Name = CloudFormation-Stack-Example-First-Template

Resources:  
  Bucket: 
    Type: 'AWS::S3::Bucket'
    Properties:   
      BucketName: 's3-bucket-loudformation-example'
      Tags:
        - Key: 'Owner'
          Value: 'GuilhermeCosme'
  
  Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      KeyName: 'DevOpsLabs'
      InstanceType: 't2.micro'
      ImageId: 'ami-09344f463b780bd4c'
      Tags:
        - Key: 'Owner'
          Value: 'GuilhermeCosme'
        - Key: 'Name'
          Value: 'EC2 Instance CloudFormation Lab'
```

Após criar a Stack é possível identificar os recursos lógicos e os recursos físicos recentmente criados pelo template na aba de detalhamento da Stack. O cloud formation é um serviço regional, portanto se deseja provisionar os recursos de sua stack em outra região, é necessário criar a stack na outra região.

- Parameters: Os parâmetros são mecanismos do template do CloudFormation onde é posséivel definir campos de Input para o usuário do template digitar informações que serão colocadas no template. Quando um parâmetro é definido ele pode ser 'inputado' via console, AWS CLI e API.

- Pseudo Parameters: são parâmetros definidos pela própria AWS e podem ser utilizados a qualquer momento dentro do template do CloudFormation sem serem declarados como um parâmetro padrão. Exemplos de pseudo parâmetros:

```yaml 
    
AWS::Region

AWS::StackId
 
AWS::StackName

AWS::AccountId

```

- Intrinsic Funtions: 

```yaml
# Capaz de referenciar um recurso ao outro dentro do template do terraform
Ref & Fn::GetAtt

# Com essas funções é possível concatenar strings e separá-las
Fn::Join & Fn::Split

# Com essas funções é possível listar as AZs de uma determinada região e também selecionar uma item de uma determinada lista.
Fn::GetAzs & Fn::Select

# Capaz de realizar operações lógicas
Fn::If | Fn::And | Fn::Not | Fn::Equals | Fn::Or

# Capaz de construir blocos CIDR para a parte de Networking
Fn::Cidr


```

- Mappings: com o mapping é possível mapear objetos através de buscas de valores baseados em parâmetros [Praticar mais o conceito]. É muito utilizado para parametrizar valores dependo da região em que o template do CoudFormation será utilzado.

- Output: É o mecanismo que permite exibir valores de saída após a stack ser criada com sucesso. Ele é opcional.

- Conditions: Permite a stack reagir a situações específicas a partir de determinadas condições ter diferentes comportamentos.

- Depends On: Estabelece uma relação de dependência entre recursos do template. Por padrão o CloudFormation tem a premissa de ser uma ferramenta eficiente, portanto ao criar uma stack o comportamento padrão do CloudFormation é realizar a criação dos recursos de maneira paralela, no entanto há cenários específicos onde um recurso necessita aguardar a status de Creation Complete de outro recurso para ser iniciado. O mecanismo do Depends On atende está  necessidade. O Depends On é um jeito de declarar explicitamente a dependência entre os recursos.

- Wait Conditionals & cfn-signal: Basicamente é o recurso que permite a stack saber se de fato todos os recursos físicos representados pelos recursos lógicos nos templates foram ou não criados no ambiente da AWS. é possível manipular explicitamente os Signals, realizar timeouts e etc.

- Nested Stacks: Como melhor prática de arquitetura é sempre recomendável dividir os recursos da sua stack em domínios (sempre levar em consideração também que cada stack só permite a criação de 500 recursos). Com isto é possível criar uma Nested Stack, ou uma stakc com várias Stacks.

- Cross Stack References: é um recurso do cloudo formation onde você pode por exemplo passar um Output de uma Stack para outra Stack como parâmetro. Neste sentido, por padrão uma Stack não pode ver os outputs de outra (isso acontece somente com Nested Stacks). Então esse processo é realizado através do Export Value e depois o Import Value. Este recurso é regional, e cada recurso n lista de Export deve ser único. O ponto cahave para entender o conceito é que com o Exmport/Import você pode compartilhar e utilizar recursos entre diversos template/stack.

- Stack Sets: Possibilita criar recursos em diversas contas da AWS de uma maneira mais simples, utilizando o mesmo template. Cada Stack Set coontém vários Stack Instances, que na verdade são versões da Stack Set rodando em diferentes contas. Uma Stack Instance sempre referencia um StackSet. O template para criar uma StackSet é um template padrão, nada muda em relação a isso.

- Deletion Policy: É uma feature da Cloudformation para estabelecer a ordem a de deleção de recursos físicos representados por um template. A deletion policy permite estabelecer algumas políticase comportamentos para determinados tipos de recurso. ISto pode evitar a perda de dados sensíveis caso uma Stack seja deletada indevidamente. Por exemplo no Bucket S3 é possível colocar o valor Retain nesta propriedade, isto significa que mesmo que a stack seja deletaada o recurso, Bucket S3, não será deletado. Não são todos os recursos que suportam a Deletion Policy.

- Stack Roles: Como todo template de fato cria recursos físicos, para o CloudFormation funcionar você sempre precisára de permissões IAM para Criar/Atualizar/Deletar esses recursos. Normalmente é garantido pelo CloudFormation Roles, onde basicamente o template assume este role temporariamente, somente aos recursos lógicos/físicos descritos no template da Stack. É uma ferramenta poderosa que garante o mínimo privilégio possível para o gerenciamento dos recursos.

- Cloud Formation INIT: Esta é uma feature muito interessante que possibilita passar configurações de incializações (Init Config) para instâncias EC2. Você basicamente define o estado que você quer que a instância seja entregue. Ele só roda uma vez, na primeira vez que a instância é criada, caso ela seja alterada via template do Cloudformation o cfn-init não roda novamente.

- Cloud Formation HUP: Possibilita re-executar scripts toda vez que o recurso é alterado via template do Cloud Fomration, ao identificar uma mudança você pode indicar que, por exemplo, o Cloud Formation Init seja executado novamente.

- Change Sets: Feature que garante o estado dos recursos físicos criados a partir de um template do Cloud Formation. Toda vez que um recurso do template é alterado podemos ter três estados diferentes:

  - No interruption
  - Some interruption
  - Replacement

  O change set permite a riaçaõ de várias versões de um mesmo template e aplicar as configurações de acordo com as versão que você quer (se assimila muito com o Git). A change Set basicamente simplesmente cria uma visão com  a diferença dos recursos alterados de um template para o outro e deixa você revisar e seguir em frente com a alteração.

- Custom Resources: A custom resource permite você criar um recurso que propriedades que nativamente o Cloud Formation ainda não suporta. Normalmente é utilizado em conjunto com lambda para construir os recursos. Sempre pensar no exemplo onde você tenta deletar uma Stack de um buckt criado, onde este bucket já possui objetos. A satck irá falhar. Olhar o template do VPC Endpoints (Itaú. Ver bastante exemplos disso)


### Dicas interessantes

Para criar um alias para um chave KMS existe um padrão para a propriedade <b>AliasName<b>, o campo sempre deverá ser escrito como: alias/"Seu_Nome_Aqui".

[Documentação Oficial](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-kms-alias.html#cfn-kms-alias-targetkeyid)

[Igual este exemplo no template da criação da pipeline](./simple_codepipeline_s3_example.ymlsimple_codepipeline_s3_example.yml)

[Documentação nteressante para padrões de Bucket S3](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-s3.html)

[Exemplo útil de criação de Bucket Criptografado](https://github.com/getcft/aws-encrypted-s3-cf-template/blob/master/encrypted-s3-cf-template.yml)