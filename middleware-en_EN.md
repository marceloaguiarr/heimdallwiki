# Middleware

Heimdall offers a special type of Interceptor called Middleware. In this document we will explain what is a Middleware and which situations it should or should not be used.

### What is a Middleware

A Middleware is a maven java project that can be added as a Interceptor for a specific endpoint. There is a maven archetype to provide an easy way to create a middleware. 

### When should you use a Middleware

A Middleware interceptor should be used when the simple act of redirect a request is not enough or when none of the Heimdall provided interceptors have the effect you want.

That way the middleware is capable of modify a request, call multiple Api's, and modify the response in order to achieve the desired result. 

If some sort of data persistence is needed inside the Middleware, Heimdall offers a NoSQL database (MongoDB) for that purpose.

### When should you NOT use a Middleware

A Middleware is not an Api, it is a request interceptor. In cases where you have some indication that your middleware can evolve to something bigger, that is a warning that you might need to think about a new Api.

A few more situations where a Middleware is **not** recommended:

* File generation
* Relational database connections

# Creating a Middleware

This tutorial shows how to create a Heimdall middleware. The main goals are:

* Create a Middleware from the archetype
* Make a request that is intercepted by the middleware
* Debug the Middleware

### Download do archetype

//TODO - adicionar archetype ao maven central

### Project creation

To create the project using the archetype use the following command:

```bash
mvn archetype:generate
    -DarchetypeGroupId=br.com.conductor
    -DarchetypeArtifactId=heimdall-middleware-archetype
    -DarchetypeVersion=1.1.0
    -DgroupId=<your.middleware.groupId>
    -DartifactId=<your-middleware-artifactId>
    -DinteractiveMode=false
```

### Project build

To create the Middleware artifact use the `mvn install` command. All artifacts will be generated inside the `target/` folder. The one that we want is the `*-jar-with-dependencies.jar` file.

# Testing your Middleware

To test your Middleware you are going to need Heimdall running locally. To achive that follow [this tutorial](link.to.heimdall.run). The only diference from that is that we will use a differente type of Interceptor.

### Adding a Middleware to Heimdall

To add your Middleware to Heimdall you will need access to the front-end. Try the following link `http://localhost:3000`

### Adding a Interceptor

Choose your Api, then the Interceptor tab. In this page choose the Resource and Operation you the Middleware to intercept and add the `Middleware` interceptor.

In the pop-up window fill the fields as shown below.

* Name: MiddlewareExampleGET
* Life Cycle: OPERATION
* Content: `<your.middleware.groupId>.MiddlewareExampleGET`

Hit save.

### Adding the Middleware

Go to the `Middleware` tab.

To upload the Middleware file you need to add a middleware version. 

* Version: 1.0.0 (this is a unique number)
* Choose the `*jar-with-dependencies.jar` file previously created

### Testing your work

Using some sort of REST tool or the browser make a GET request to:
 `http://localhost:800/<api_basepath>/<operation>` .
In this example you should get as a return the Conductor front page.

### Debugging a Middleware

In order to debug your middleware there are some steps you need to do first that are directly related to the IDE that you are using.
    
1. **Eclipse IDE**
    
    1. In the project execution options, add the following under `VM options`:
        * `-Dspring.profiles.active=dev`
    
    2. Run heimdall-gateway in **debug** mode

2. **Intellij IDEA**

    1. In the run options for heimdall-gateway add the following to `Environment -> VM options`:
        * `-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005`
    
    2. In your middleware project select `Run -> Edit Configurations`
    
    3. Click `+` and choose the `Remote` option.
        * Add a name
        * Transport: Socket
        * Debug Mode: Attach
        * Host: localhost
        * Port: 5005
    
    4. Run heimdall-gateway normally

After this configurations all you are ready to add breakpoints on your middleware code and start debugging.