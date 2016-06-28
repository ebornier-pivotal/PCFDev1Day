# Developer Basics

## Pushing to Cloud Foundry

### Pushing via CLI

Use `cf help` and/or `cf <command> --help` for details on each of the commands below.

1. Review the docs: http://docs.pivotal.io/pivotalcf/devguide/deploy-apps/deploy-app.html

2. Verify you are logged in with your own userid (*not admin*) and targeted to your PCF instance:

```bash
$ cf target
$ cf login -a <CF API ENDPOINT> --skip-ssl-validation
$ cf target
```

3. Go to cities-service folder. Push the cities-service in the dev space:

```bash
$ cf push <login>-cities-service -i 1 -m 512M
```

**Be sure to name your application ``<login>-cities-service``**

4. Does it work? tips

```bash
$ cf create-service p-mysql 100mb-dev <instance service name>
$ cf env <application name>
```

5. Verify you can access your application via a curl request:

```bash
$ curl -i <your-app>/cities
```
### Using Manifests

Now, look at the manifest to help automate deployment.

1. Review the documentation: http://docs.pivotal.io/pivotalcf/devguide/deploy-apps/manifest.html

2. Update the manifest with the correct application name & Test your manifest by re-pushing your app with no parameters :

```bash
$ cf push
$ curl -i <your-app>/cities
```

References:

* link:http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/context/annotation/Profile.html[Spring Profiles]

### Check Application Logs

1. What do the recent logs tell you?

### Behind the scene : SSH

```bash
$ cf ssh <application name>
```

2. You are now inside the container!! This feature is often enables only in dev.

== Scaling

Apps can be scaled via the CLI or the Console.

=== Vertical Scale

1. Increase your app memory to 1G.

2. QUESTION: What happens?  How long does it take?  Why?

3. Scale your app memory back down to 512M.

=== Horizontal Scale

1. Scale your app to 2 instances.

2. QUESTION: What happens?  How long does it take?  Why?

3. Scale your app to 11 instances.

4. QUESTION: What happens?  How long does it take?  Why?

## User Provided Service Instances & Tags

The main directory also includes a `cities-ui` application which uses the `cities-client` to consume from the `cities-service`.

The goal of this exercise is to use what you have learned to deploy the `cities-ui` application.

### Deploying the Cities UI App

* A `manifest.yml` is included in the cities-ui app.  Edit this manifest with your initials and be sure this manifest works with the service you create below.

* The cities-ui and cities-client can be both built at once by running `gradle assemble` in the parent +cities+ directory.

### Creating a Service Instance & Deploy

You will need to connect the application to a service instance representing your microservice (cities-service).

You can create a User Provided Service Instance and bind this to the ui application.

*Review the documentation on link:http://docs.pivotal.io/pivotalcf/devguide/services/user-provided.html[User Provided Service Instances]
Look for the details by running `cf help`.*

You will need to specify 1 parameter when you create the service instance: The `citiesuri` should point to your deployed microservice

1. Create the user provided service instance

```bash
$ cf cf create-user-provided-service <service instance name> -p '{ "citiesuri": "<URI>" }
```

2.  Deploy using the `manifest.yml` contained in the cities-ui project.

3. Access the cities-ui with your Browser to verify it is connected to your microservice.

### Deploying the Cities Services into Prod space

```bash
$ cf target ... && cf push
```

1. QUESTION: What happens?  How long does it take?  Why?

### Blue green deployment in dev

1. Do a minimal change to cities-ui and redeploy it without application downtime.
