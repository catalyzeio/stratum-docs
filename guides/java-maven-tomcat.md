---
title: Java + Maven Guide
category: guide
---

# Getting Started with Java and Maven

This guide is targeted for readers with an existing Java application that runs in [Tomcat](https://tomcat.apache.org/). The instructions assume that the reader has Java installed locally.

## Dependency Management

Most Java-based web applications pull in external dependencies. To manage these (so that they don't need to be included in the repository), a common solution is to use [Maven](https://maven.apache.org/). The rest of this guide will assume Maven is being used.

Another popular choice is [Gradle](https://gradle.org/).

## Buildpack Choice

All builds on the Platform are done using [buildpacks](../buildpacks). If your repository contains a `pom.xml` (Maven configuration) file at its root, [Heroku's Java + Maven buildpack](https://github.com/heroku/heroku-buildpack-java) will be used (for Gradle, [Heroku's Java + Gradle buildpack](https://github.com/heroku/heroku-buildpack-gradle) is used). The buildpack's repository includes advanced configuration options - these won't be needed right now, but likely will be needed to tune a production application, so they're worth a look.

If your application doesn't have either a `pom.xml` or a `build.gradle`, you can use the [Maven CLI](http://maven.apache.org/install.html) to generate a sample application from an archetype:

```
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-webapp
```

This will generate some project scaffolding as well. If you already have a project and just need the `pom.xml`, you can use this scaffolding structure as a base to rearrange your project's files to work with Maven's [Standard Directory Layout](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html).

## Running the Application Without Tomcat

Neither of the above buildpacks contain a Tomcat installation. To enable your application to run inside the [container](../concepts/containers) the buildpack will build, we use an open-source tool called [webapp-runner](https://github.com/jsimone/webapp-runner), which maintains compatibility with Tomcat. webapp-runner can be brought into your project by adding a dependency to your `pom.xml`:

```
<build>
  <!-- ... -->
  <plugins>
    <!-- ... -->
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-dependency-plugin</artifactId>
      <version>2.3</version>
      <executions>
        <execution>
          <phase>package</phase>
          <goals><goal>copy</goal></goals>
          <configuration>
            <artifactItems>
              <artifactItem>
                <groupId>com.github.jsimone</groupId>
                <artifactId>webapp-runner</artifactId>
                <version>8.5.11.3</version>
                <destFileName>webapp-runner.jar</destFileName>
              </artifactItem>
            </artifactItems>
          </configuration>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>
```

To target a different version of Tomcat, change the version above to use the corresponding release of webapp-runner ([releases](https://github.com/jsimone/webapp-runner/releases)).

Typically, IDEs can import projects using `pom.xml` and pull in dependencies automatically for you. To pull the webapp-runner jar down and package your application into a `war` without an IDE, however, you can use the [Maven CLI](http://maven.apache.org/install.html):

```
mvn package
```

> Note: if your `pom.xml` wasn't generated from an archetype, you'll want to make sure `packaging` is set to `war`:
>
> ```
> <project ...>
>   ...
>   <packaging>war</packaging>
>   ...
> </project>
> ```

Then, to run your packaged application using webapp-runner locally:

```
java -jar target/dependency/webapp-runner.jar target/*.war
```

## Procfile

To tell the Platform what command to run when a container is started, the `web` target must be set in a file called `Procfile` (no extension) in the root of your project's repository:

```
web: java $JAVA_OPTS -jar target/dependency/webapp-runner.jar --port $PORT target/*.war
```

The `PORT` [variable](../environment-variables) will be provided at runtime to your container, telling it what port it needs to bind to locally so that your [environment](../concepts/environments)'s [service proxy](../concepts/service-proxy) can connect it to the outside world.

The `JAVA_OPTS` variable is optional, but useful - it'll pass through any Java options you set later, so that you don't need to rebuild to tweak settings.

For more on Procfiles and building your application to run well on the Platform, see the [Writing Your Application](../writing-your-application) article.

## Deploying Your Application

After completing [initial setup](../initial-setup) and having the CLI installed and configured for your code service and repository, you should be able to push code and see your application running on the configured [site](../concepts/sites):

```
git push datica master
```

## Example Application

_TODO: public repo link_
