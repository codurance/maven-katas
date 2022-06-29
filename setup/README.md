# Setup kata

In this exercise, the main idea is to take the role of the developer/architect who starts a project, so it can later be distributed to other developers. 
Once a project is setup and distributed, other developers won't need to install maven at all.

In order to achieve that, you will need to execute the following tasks:

* Install maven from scratch (using SDKMAN)
* Generate a project from a maven archetype
* Build the project with maven command
* Incorporate the maven wrapper to this project 
* Build the project with the maven wrapper command



## Install Maven
In order to complete this task, you'll need to install first SDKMAN (if you haven't already)

Simply run

```
curl -s "https://get.sdkman.io" | bash
```

And there you have, SDKMAN installed and ready for usage

Now, you should be able to run the following command

```
sdk help
```

and explore all available options

In particular, our option of interest will be

```
sdk list maven
```

which will list all maven available version for installation. It can also show you which version you have currently installed.

Now you can install a maven version. Try the following command

```
sdk install maven 3.6.3
```

And it will install maven 3.6.3 in your local machine

You can also try to install other JVM-based stuff, like a specific JDK version or a gradle distribution if you like. For further documentation please visit https://sdkman.io/


## Generate a project from a maven archetype
This step is necessary for this exercise, but typically, you won't need it

```
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart
```

When asked for values, use the following
* groupId: com.codurance.maven
* artifactId: setup-example
* 1.0-SNAPSHOT

This should generate an `setup-example` folder with the project, base code, tests and a pom file

Navigate to the project folder, open the `pom.xml` file and add the following content right before the `<dependencies>` tag

```
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <java.version>1.8</java.version>
        <maven.compiler.source>${java.version}</maven.compiler.source>
        <maven.compiler.target>${java.version}</maven.compiler.target>
    </properties>
```


## Build the artifact with maven

Building and generating an artifact is a crucial task for a build tool. In this case, we will build our artifact by running:

```
mvn clean install
``` 

This will generate a target folder where you'll find the jar (also, install saves the same jar into your local cache).

# Introducing the maven wrapper

Nowadays, distributing applications with some kind of wrapper, so you can guarantee it will always be built with the same build tool version, is considered a best practice.

You'll see distributed apps that comes with an `mvnw` script (available for Windows and *nix systems). When you see them, execute all build tasks with it rather than with the global Maven installation.

But first of all, we need to enable our project to be able to build via maven wrapper. For that, we're going to execute

```
mvn -N wrapper:wrapper -Dmaven=3.5.2
``` 

This will enable this project to be built with the maven wrapper, with the specific `3.5.2` version


Try cleaning the project `target` folder by running:

```
./mvnw clean
```

(*nix based) or 

```
mvnw.cmd clean
```

(Windows based)

You should see no more the `target` folder.

Now try again building the project one more time but now with:

```
./mvnw clean install
```

(*nix based)  or 

```
mvnw.cmd clean install
```

(Windows based)

You should see it succeed and the `target` folder will be generated once more



# Implications

Here you've seen how easy is to start a maven project from scratch and the convenience of using the wrapper.
The wrapper will act as a way of standardazing versions among your team. 
Typically, when someone new joins a team, this is already generated, but if you happen to be the one generating from scratch, this is the way


From now one, we will only rely on the Maven wrapper tool
