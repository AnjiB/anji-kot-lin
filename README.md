# ![RealWorld Example App](logo.png)

This repo is a copy of [Real-World-App(https://github.com/alisabzevari/kotlin-http4k-realworld-example-app.git) which helps us to launch an application to be used for API Testing.

For more information on how to this works with other frontends/backends, head over to the [RealWorld](https://github.com/gothinkster/realworld) repo.

# How it works

The application was made mainly to demo the functionality of http4k framework together with exposed.

## Tech stack
The application was built with:

* [Kotlin](https://kotlinlang.org) as programming language.
* [http4k](https://http4k.org) as web framework.
* [h2](https://www.h2database.com/html/main.html), an embedded lightweight database, as data storage. Although the application can support all the databases supported by exposed.
* [exposed](https://github.com/JetBrains/Exposed) to access database and build typesafe SQL queries.
* [jsonwebtoken](https://github.com/jwtk/jjwt) to handle JSON Web Tokens for request authorization.
* [log4j](https://logging.apache.org/log4j/2.x/) for proper logging in the application.
* [kotest](https://github.com/kotest/kotest/) as testing framework for kotlin.
* [mockk](https://mockk.io/) as mocking library for Kotlin.

## Application structure

Basically, the application has four main parts:
* `main` function which instantiates all the services and handlers and connects them together, then starts the server.
* `Router` class which handles the translation of 1. http requests to request handler calls and 2. handler results to http responses. Authorization logic is also implemented in this class.
* `handler` package which contains a class for every request handler. Handlers are classes with one method (`invoke`) which handles the request. This package contains the whole business logic of the application. Handlers have access to data layer.
* `ConduitRepository` class which is responsible for building database queries. In order to communicate to database, a class called `ConduitTransactionManagerImpl` is needed. this class has one method (`tx`) which connects to database and opens a transaction to communicate with database. `tx` accepts an anonymous function while provides an instance of `ConduitRepository` as a receiver.

```
+ config/
    Application config as simple data classes
+ handler/
    All handler classes which describe business logic of the application
+ model/
    domain model and dtos
+ repository/
    table definitions, typesafe database creation scripts and classes to communicate with database
+ util/
    utility classes such as request filters, serialization/deserialization functions and jwt utility functions
+ Main.kt
    File containing main function
+ Router.kt
    File containing Router class responsible for handling http server communication
```

## Database

The application currently uses H2 embedded database. The connection is defined in `config/local.kt`. If you want to change the database you need to provide the correct dependency for the driver and change the configuration. 

## Tests

You can run `./gradlew test` to run all the tests. A test logger gradle plugin (`com.adarshr.test-logger`) has used to render the test beautifully in console.
There are a couple of unit tests for handlers but not for all of them (contributions welcome!).
There are integration tests to cover all the cases of the postman test file.

# Getting started

You need Java 11 installed.

**Build and run tests:**
```
./gradlew clean build
```

**Start the server:**
```
./gradlew run
```
The server will be available on `http://localhost:9000`
