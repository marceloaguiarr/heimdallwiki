# Heimdall Middleware Archetype

Esse projeto é o archetype para um Middleware do Heimdall. Ele provê um projeto de um middleware simples como base para o desenvolvimento.

### Como utilizar

Clone ou faça o download desse repositório e utilize o seguinte comando na raiz: 

`mvn install`

Esse comando vai criar o archetype no seu repositório local. Para criar o projeto do seu middleware navegue até o diretório que você pretende usar para a criação e execute o comando:

```
mvn archetype:generate
     -DarchetypeGroupId=br.com.conductor
     -DarchetypeArtifactId=heimdall-middleware-archetype
     -DarchetypeVersion=0.0.1
     -DgroupId=<your.middleware.groupId>
     -DartifactId=<your-middleware-artifactId>
     -DinteractiveMode=false
```

Esse comando irá criar um middleware simples e funcional, para testar basta executar o comando `mvn install` na raiz do middleware.