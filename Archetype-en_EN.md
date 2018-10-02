# Heimdall Middleware Archetype

This is the archetype for an Heimdall Middleware. It provides a simple functional middleware as a base for development.

### How to use

Clone or download this repository and use this command in the root directory

`mvn install`

This will create the archetype in your local repository. Now to create your middleware navigate the the folder you want your middleware project to be located and type:

```
mvn archetype:generate
     -DarchetypeGroupId=br.com.conductor
     -DarchetypeArtifactId=heimdall-middleware-archetype
     -DarchetypeVersion=0.0.1
     -DgroupId=<your.middleware.groupId>
     -DartifactId=<your-middleware-artifactId>
     -DinteractiveMode=false
```

After this you will be able to `mvn install` your middleware and test it.