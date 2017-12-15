# Spring Cloud Data Flow on Pivotal Cloud Foundry

These instructions are for getting started with Spring Cloud Data Flow when deployed as a service in Pivotal Cloud Foundry marketplace

Pre-Req:
- Spring Cloud Data Flow installed as a tile on Pivotal Cloud Foundry. If installed correctly, it should show up as a service in PCF Marketplace.
- Java Installed on local workstation

```bash
cf marketplace

service
p-dataflow      standard      Deploys Spring Cloud Data Flow servers to orchestrate data pipelines
```

1. Let's create a service instance
```bash
cf create-service p-dataflow standard dataflow-server
```

2. Check the service is fully initialized
```bash
cf service dataflow-server

Service instance: dataflow
Service: p-dataflow
Bound apps:
Tags:
Plan: standard
Description: Deploys Spring Cloud Data Flow servers to orchestrate data pipelines
Documentation url: http://cloud.spring.io/spring-cloud-dataflow/
**Dashboard**: https://p-dataflow.apps.cloud/instances/32d68866-9cd5-4607-8d42-25936b3fced7/dashboard

Last Operation
Status: *create succeeded*
Message: Created
Started: 2017-12-15T04:14:55Z
Updated: 2017-12-15T04:17:59Z
```

3. Visit the **`Dashboard`** and login with your Cloud Foundry credentials

`https://p-dataflow.apps.cloud/instances/32d68866-9cd5-4607-8d42-25936b3fced7/dashboard`

4. This is the homepage for Spring Cloud Data Flow server. Note down the url from your browser now.

`https://dataflow-32d68866-9cd5-4607-8d42-25936b3fced7.apps.cloud/dashboard/index.html#/apps/apps`.

We will use part of this domain to login with the spring cloud data flow shell below.

    From the homepage link above, the **`dataflow.uri`** will be

    `https://dataflow-32d68866-9cd5-4607-8d42-25936b3fced7.apps.cloud`

5. Open a terminal and download the spring cloud data flow shell.
```bash
mkdir ~/scdf && cd ~/scdf
wget http://repo.spring.io/release/org/springframework/cloud/spring-cloud-dataflow-shell/1.2.3.RELEASE/spring-cloud-dataflow-shell-1.2.3.RELEASE.jar
```

6. Login to the Spring Cloud Data Flow with your shell
```bash
java -jar spring-cloud-dataflow-shell-1.2.3.RELEASE.jar \
--dataflow.skip-ssl-validation=true \
--dataflow.uri=https://dataflow-32d68866-9cd5-4607-8d42-25936b3fced7.apps.cloud \
--dataflow.credentials-provider-command="cf oauth-token"
```

7. Verify connection by running the command in dataflow shell
```bash
dataflow:>dataflow config info
```
You should see lots of information about Cloud Foundry & Spring Cloud Data Flow server

8. Register all out-of-the box applications built with RabbitMq binder
```bash
dataflow:>app import --uri http://bit.ly/Bacon-RELEASE-stream-applications-rabbit-maven
```

Have fun building highly-scalable, resilient data pipeline!
Reference: [Spring Cloud Data Flow Docs](https://docs.spring.io/spring-cloud-dataflow/docs/1.2.3.RELEASE/reference/htmlsingle/#spring-cloud-dataflow-register-stream-apps)
