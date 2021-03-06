# Quarkus JHipster demo project

This is a basic JHipster Angular CRUD application using Quarkus as the backend service.

Checkout also my related [LinkedIn article](https://www.linkedin.com/pulse/jhipster-quarkus-demo-app-stephan-janssen).

The backend code is very straight forward and uses the following Quarkus (extensions) :
 - RESTEasy to expose the REST endpoints
 - Hibernate ORM with Panache to perform the CRUD operations on the database
 - MapStruct for DTO mapping
 - FlyWay version control for the database tables
 - ArC, the CDI inspired dependency injection tool with zero overhead
 - The high performance Agroal connection pool
 - Infinispan based caching
 - All safely coordinated by the Narayana Transaction Manager
 - A PostgreSQL database; see below to run one via Docker
 
This demo application is based on the Quarkus example project  'hibernate-orm-panache-resteasy' provided by the RedHat team @ https://github.com/quarkusio/quarkus-quickstarts

Thanks to the Quarkus (Red Hat), JHipster, GraaVM teams for their amazing work! 


## Requirements

To compile and run this demo you will need:
- GraalVM `1.0 rc16`
- Apache Maven `3.5.3+`

In addition, you will need either a PostgreSQL database, or Docker to run one.

If you don't have GraalVM installed, you can download it here:

<https://github.com/oracle/graal/releases>

Installing GraalVM is very similar to installing any other JDK:
just unpack it, add it to your path, and point the `JAVA_HOME`
and `GRAALVM_HOME` environment variables to it.

You should then use this JDK to run the Maven build.


## Building the Quarkus backend

Double check if you have set GraalVM as your JVM

```
$ export PATH=/Library/Java/JavaVirtualMachines/graalvm-ce-1.0.0-rc16/Contents/Home/bin:$PATH
$ export GRAALVM_HOME=/Library/Java/JavaVirtualMachines/graalvm-ce-1.0.0-rc16/Contents/Home 

$ java -version

openjdk version "1.8.0_202"
OpenJDK Runtime Environment (build 1.8.0_202-20190206132754.buildslave.jdk8u-src-tar--b08)
OpenJDK GraalVM CE 1.0.0-rc16 (build 25.202-b08-jvmci-0.59, mixed mode)
```

Now you can launch the Maven build on the checked out sources of this demo:

> cd backend

> mvn clean package

## Running the Quarkus backend

### Run Quarkus in developer mode

To run the application in interactive mode (developer mode):

>  mvn quarkus:dev

```
Listening for transport dt_socket at address: 5005
2019-05-25 10:42:44,149 INFO  [io.qua.dep.QuarkusAugmentor] (main) Beginning quarkus augmentation
2019-05-25 10:42:44,429 INFO  [io.qua.fly.FlywayProcessor] (build-6) Adding application migrations in path: file:/Users/stephan/java/projects/quarkushipster/backend/target/classes/db/migration/
2019-05-25 10:42:44,431 INFO  [io.qua.fly.FlywayProcessor] (build-6) Adding application migrations in path: file:/Users/stephan/java/projects/quarkushipster/backend/target/classes/db/migration
2019-05-25 10:42:44,852 INFO  [io.qua.dep.QuarkusAugmentor] (main) Quarkus augmentation completed in 703ms
2019-05-25 10:42:45,296 INFO  [org.fly.cor.int.lic.VersionPrinter] (main) Flyway Community Edition 5.2.4 by Boxfuse
2019-05-25 10:42:45,369 INFO  [org.fly.cor.int.dat.DatabaseFactory] (main) Database: jdbc:postgresql:quarkus_hipster (PostgreSQL 10.5)
2019-05-25 10:42:45,428 INFO  [org.fly.cor.int.com.DbValidate] (main) Successfully validated 1 migration (execution time 00:00.013s)
2019-05-25 10:42:45,438 INFO  [org.fly.cor.int.sch.JdbcTableSchemaHistory] (main) Creating Schema History table: "public"."flyway_schema_history"
2019-05-25 10:42:45,462 INFO  [org.fly.cor.int.com.DbMigrate] (main) Current version of schema "public": << Empty Schema >>
2019-05-25 10:42:45,463 INFO  [org.fly.cor.int.com.DbMigrate] (main) Migrating schema "public" to version 1.0.0 - HIPSTER
2019-05-25 10:42:45,487 INFO  [org.fly.cor.int.com.DbMigrate] (main) Successfully applied 1 migration to schema "public" (execution time 00:00.050s)
2019-05-25 10:42:45,789 INFO  [io.quarkus] (main) Quarkus 0.15.0 started in 1.781s. Listening on: http://[::]:8080
2019-05-25 10:42:45,790 INFO  [io.quarkus] (main) Installed features: [agroal, cdi, flyway, hibernate-orm, jdbc-postgresql, narayana-jta, resteasy, resteasy-jsonb]
```

In this mode you can make changes to the code and have the changes immediately applied, by just refreshing your browser.

    Hot reload works even when modifying the JPA entities.


### Run Quarkus as a native application

Before you can do this make sure the native-image tool is create

> gu install native-image


Compilation will take a bit longer, so this step is disabled by default;
let's build again by enabling the `native` profile:

> mvn package -Dnative

After reading a few tweets you'll be able to run this binary:

> ./target/QuarkusHipster-1.0-SNAPSHOT-runner

## Application startup takes longer than 5 seconds?

This happened to me on two Mac computers, it was related to an [incorrect configuration](https://thoeni.io/post/macos-sierra-java/) of /etc/hosts
You can resolve this by adding your hostname to the /etc/hosts localhost, as follows: 

```
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1       localhost   Stephans-MacBook-Pro.local
::1             localhost   Stephans-MacBook-Pro.local
```

## Start JHipster front end (Angular 7)

Now that the backend is running we can start the Angular frontend as follows: 

> cd frontend

> npm start

Navigate to:

<http://localhost:9000>

Use admin / admin to do a fake authentication and click on the top menu "Entities" and select "Event".

---
## ToDo list

- Add JWT authentication
- Add management/* REST endpoints
- Add Metrics REST endpoints
- Add Swagger, details @ https://quarkus.io/guides/openapi-swaggerui-guide
- ... 

