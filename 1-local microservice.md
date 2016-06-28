# Running the Spring Boot Microservice Locally

In this exercise, you will setup the `cities` microservice and run it locally.

## Set Up

### Running the application locally with Spring Boot

1. Build the application:

```bash
$ ./gradlew assemble
```

2. Run the application locally to be sure it works:

```bash
$ cd cities-service
$ java -jar build/libs/cities-service.jar
```

3. Access the application using 'curl'
```bash
$ curl -i http://localhost:8080
```

You'll see that the primary endpoint automatically exposes the ability to page, size, and sort the response JSON.

4. 'curl' for results
```bash
$ curl -i http://localhost:8080/cities
```
You'll see that we have already loaded a large data set upon startup.  The application is currently using an in memory hypersonic database. We will change this to use an external mysql database soon.

5. Try the following +curl+ statements to see how the application behaves:

```bash
$ curl -i localhost:8080/cities?size=5
$ curl -i localhost:8080/cities?size=5&page=3
$ curl -i localhost:8080/cities?sort=postalCode,desc
```

### FYI: Using Spring Boot Actuator

Try out the following endpoints. The output is omitted here because it can be quite large:

http://localhost:8080/beans:: Dumps all of the beans in the Spring context.
http://localhost:8080/autoconfig:: Dumps all of the auto-configuration performed as part of application bootstrapping.
http://localhost:8080/env:: Dumps the application's shell environment as well as all Java system properties.
http://localhost:8080/metrics:: Dumps all metrics currently being collected by Actuator, primarily response time and access counts for endpoints.
http://localhost:8080/mappings:: Dumps all URI request mappings and the controller methods to which they are mapped.

### Shutdown

1. Shutdown the application
