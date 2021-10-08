# Testname"

This project uses Quarkus, the Supersonic Subatomic Java Framework.

If you want to learn more about Quarkus, please visit its website: https://quarkus.io/ .

## Next Steps
Atm, every time this project is rebuilt and pushed to docker, you will receive an email.
This may be annoying, note that it will only be rebuilt if you push to it.

To stop it from sending emails, go to the Jenkinsfile and erase the last part.

    post{
        always{
            script{
          generatedBody = emailTemplate([
            "name"   :   env.name,
            "dockerHub"    :   env.dockerImage,
            "gitUrl" :   env.gitUrl
        ]);
            }
            emailext body: generatedBody,
                      mimeType: 'text/html',
                    subject: "Your Project is now on DockerHub!",
                    to: "${providedEmail}"
        }
    }
    
However, should you want it to keep sending you emails, you could also simply change the subject to something more meaningful.

Maybe update the email-html.template file.

I have provided a simple example in the email-html-v2.template file.

Since I know this may be annoying, why didn't I just solve it?
To show one of the core tenets of good CICD.

Provide solutions that can be easily extended, it would have been easy to hide the email sending, but why?
I could have hidden the jenkinsfile, but why?

By making it available, I provide you, the developer, with the option of choice, while also providing a sensible default option.

It's like going to an ice cream store, and not having to tell him that you don't want anthrax in your ice cream, while still having the option of adding banana ice cream.

After all, the best person to predict what kind of ice cream you like, is you, CICD is just about making it easy for you to select it.

## Running the application in dev mode

You can run your application in dev mode that enables live coding using:
```shell script
./gradlew quarkusDev
```

## Packaging and running the application

The application can be packaged using:
```shell script
./gradlew build
```
It produces the `Testname-1.0.0-SNAPSHOT-runner.jar` file in the `/build` directory.
Be aware that it’s not an _über-jar_ as the dependencies are copied into the `build/lib` directory.

If you want to build an _über-jar_, execute the following command:
```shell script
./gradlew build -Dquarkus.package.type=uber-jar
```

The application is now runnable using `java -jar build/Testname-1.0.0-SNAPSHOT-runner.jar`.

## Creating a native executable

You can create a native executable using: 
```shell script
./gradlew build -Dquarkus.package.type=native
```

Or, if you don't have GraalVM installed, you can run the native executable build in a container using: 
```shell script
./gradlew build -Dquarkus.package.type=native -Dquarkus.native.container-build=true
```

You can then execute your native executable with: `./build/Testname-1.0.0-SNAPSHOT-runner`

If you want to learn more about building native executables, please consult https://quarkus.io/guides/gradle-tooling.

# RESTEasy JAX-RS

<p>A Hello World RESTEasy resource</p>

Guide: https://quarkus.io/guides/rest-json

# RESTEasy JSON serialisation using Jackson

<p>This example demonstrate RESTEasy JSON serialisation by letting you list, add and remove quark types from a list.</p>
<p><b>Quarked!</b></p>

Guide: https://quarkus.io/guides/rest-json


## Jaeger Tracing
This application comes as standard with Jaeger Tracing enabled.

To test this, run the following docker command.

    docker run -p 5775:5775/udp -p 6831:6831/udp -p 6832:6832/udp -p 5778:5778 -p 16686:16686 -p 14268:14268 jaegertracing/all-in-one:latest

Now, when you run your application and curl the restEasy endpoint, the trace will be available at localhost:16686.

Bear in mind, this will not work if both are run as docker images.