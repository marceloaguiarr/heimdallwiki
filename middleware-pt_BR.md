# Middleware

O Heimdall oferece um tipo de Interceptor específico chamado Middleware. Nesse documento vamos explicar o que é um Middleware e em quais situações deve ou não deve ser utilizado.

### O que é um Middleware

Middleware é um projeto Java Maven que pode ser adicionado como um Interceptor para um endpoint específico cadastrado no Heimdall. Para facilitar a criação de um Middleware existe um archetype padrão que será mostrado mais afrente.

### Quando utilizar um Middleware

Quando apenas interceptar a requisição e redirecionar não for suficiente um Interceptador do tipo Middleware pode ser adicionado ao endpoint.

O Middleware irá interceptar a requisição feita ao endpoint e fará a orquestração dessa requisição. Isso significa que um Middleware é capaz de modificar a requisição e fazer multiplas requisições a outras Api's, modificar esses resultados de forma que esteja de acordo com o desejado e gerar o retorno.

Caso seja necessário algum tipo de persistência de dados pelo Middleware, o Heimdall oferece um banco de dados NoSQL (MongoDB) para essa finalidade.

### Quando não utilizar um Middleware

Um Middleware não é uma Api, é um interceptador de requisições. Caso o seu middleware tenha a possibilidade de evoluir para algo maior, isso é um indicativo você está fugindo do escopo da utilização de um Middleware.

Exemplos de algumas situações em que um Middleware **não** é recomendado:

* Geração de arquivos
* Acesso a bancos de dados relacionais

# Criação de um Middleware

Esta seção pretende mostrar como criar um Middleware para ser utilizado pelo Heimdall. Os objetivos principais são:

* Criar um middleware com auxílio do archetype
* Executar uma chamada pelo middleware
* Fazer o debug do middleware

### Download do archetype

//TODO - adicionar archetype ao maven central

### Criar projeto pelo archetype

Para criar o projeto pelo archetype utilize o seguinte comando.

```bash
mvn archetype:generate
    -DarchetypeGroupId=br.com.conductor
    -DarchetypeArtifactId=heimdall-middleware-archetype
    -DarchetypeVersion=1.1.0
    -DgroupId=<your.middleware.groupId>
    -DartifactId=<your-middleware-artifactId>
    -DinteractiveMode=false
```

### Build do seu projeto

Para criar o artefato do middleware utilize o comando `mvn install` na raiz da pasta do projeto. Isso vai gerar a pasta `target/` na raiz com os artefatos que queremos.
O artefato que queremos é o que contém `jar-with-dependencies` no nome.

# Testar seu middleware

Para tester seu Middleware você vai precisar do Heimdall executando localmente, para isso siga o tutorial em [Executar Heimdall localmente](../Hemidall.md) até o ponto de adicionar um Interceptor. No nosso caso vamos adicionar um Interceptor de um tipo diferente do descrito no link.

### Adicionar middleware ao heimdall

Para adicionar o seu middleware ao Heimdall que está executando localmente acesse tenha certeza que o frontend do Heimdall está executando e navegue para: `http://localhost:3000`

### Adicionado Interceptor

Clique na sua Api cadastrada anteriormente. Na barra superior escolha a opção `Interceptors`.
Nessa tela escolha o Resource e o Operation que você quer adicionar o interceptador. Nesse caso escolha o interceptador do tipo `Middleware`.

Na janela de configuração do interceptador preencha com as seguintes informações:

* Name: MiddlewareExampleGET
* Life Cycle: OPERATION
* Content: `<your.middleware.groupId>.MiddlewareExampleGET`

Salve as alterações.

### Adicionando o Middleware

Clique na aba `Middleware`.

Para fazer o upload do arquivo do Middleware preencha:

* Version: 1.0.0 (número único da versão do middleware que está sendo enviado)
* Escolha o arquivo `jar-with-dependencies` que foi criado anteriormente e adicione

### Testando seu trabalho

Utilizando alguma ferramente da REST (Postman) faça uma requisição GET no endereço: `http://localhost:800/<api_basepath>/<operation>` .
Nesse exemplo você deve receber como resposta a front page da Conductor.

### Debug de um Middleware

Caso durante o desenvolvimento do seu middleware você precise fazer o debug do seu código. Para fazer isso é preciso tomar alguns cuidados.
    
1. **Eclipse IDE**
    
    1. Nas opções de execução do projeto, adicione a seguinte opção da VM:
        * `-Dspring.profiles.active=dev`
    
    2. Execute o heimdall-gateway em modo **debug**

2. **Intellij IDEA**

    1. Nas opções de execução do heimdall-gateway adicione a seguinte opção em Environment -> VM Options:
        * `-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005`
    
    2. No projeto do seu middleware selecione Run -> Edit Configurations
    
    3. Clique no `+` e escolha a opção Remote.
        * Adicione um nome
        * Transport: Socket
        * Debug Mode: Attach
        * Host: localhost
        * Port: 5005
    
    4. Execute o heimdall-gateway normalmente

Após feitas essas configurações, basta adicionar os breakpoints no código do seu middleware e fazer a chamada em alguma ferramenta de REST.