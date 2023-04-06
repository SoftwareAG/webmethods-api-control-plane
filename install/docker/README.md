# API Control Plane docker compose

The docker compose files in this folder allow for deploying API Control Plane on docker.

## Standard deployment of API Control plane

The standard deployment of API Control plane contains the following 5 microservices.
![img.png](../attachments/img.png)

1. Asset catalog - Assets(Runtimes,Data planes) processing of Control plane.
2. Engine - Metrics processing and aggregation.
3. Ingress - User management and security
4. API Control Plane UI - user interface
5. Elasticsearch - data persistence layer

## Pre-requisites

### 1. Connecting to GitHub registry to pull the API Control plane images

We published docker images for all API Control Plane microservice to this GitHub reporotory. To be able to pull them or have docker compose pull them for you, you'll need:

1. A Personal Access token (PAT) from Software AG.

    Open an issue here in the repo or email your Software AG contact for API Control Plane BETA.

2. To run the following command in your docker environment:

    ```bash
    docker login ghcr.io \
      -u <Your git username> \
      -p <Your PAT>
    ```

### 2. Configuring the Ingress controller

The `.env` file (install/docker/.env) allows for configuring differenc aspects of API Control Plane deplpyment. To be able to access API Contol Plane after it's deployed, you need to edit this file and provide a value for `NGINX_DOMAIN_NAME` that matches the hostname of the machine you're deploying API Conrtol plane on. Make sure this hostname is accessible to whoever will be connecting to APi Control Plane.

## Deploying API Control Plane BETA

To deploy the API Control Plane with default configuration:

- change to install/docker directory:

    ```bash
      cd install/docker
    ```

- execute the deployment script

    ```bash
      docker compose -f control-plane-beta.yaml up -d
    ```

## Stopping API Control Plane BETA

To stop and remove the API Control Plane default configuration:

- change to install/docker directory:

    ```bash
      cd install/docker
    ```

- execute the deployment script

    ```bash
      docker compose -f control-plane-beta.yaml down
    ```

## Additional deployment flavors

### 1. Enabling Open Telemetry using Jaeger

API Control plane can be started in debug mode with Open telemetry enabled and exposed using [Jaeger UI](https://www.jaegertracing.io/). For this, the deployment needs additional image, namely `jaegertracing/all-in-one`.

To start API Control Plane BETA in debug mode:

- change to install/docker directory:

    ```bash
      cd install/docker
    ```

- execute the deployment script

    ```bash
      docker compose -f control-plane-beta.debug.yaml up -d
    ```

> **Note:** The Jaeger UI can be accessed via the `JAEGER_UI_PORT` which will configured in the `.env` file

## 2. Enabling Gainsight integration

If you want to see Gainsight powerd user engagemnets (work in progress) in API Control Plane like bots, articles, feature introduction etc., you can start it with additional configuration set up in `ui/ui-config.gainsight.env` file. Please contact your Software AG API Control Plane BEAT contact or ask us here for the proper confoguration values.

To start API Control Plane BETA with Gainsight enabled:

- change to install/docker directory:

    ```bash
      cd install/docker
    ```

- execute the deployment script

    ```bash
      docker compose -f control-plane-beta.gainsight.yaml up -d
    ```

## 3. Enabling Secure Elastic communication

If you wuld like to run elasticsearch in secure mode, then the following properties needs to be set in the `.env` file to enable secure communication

``` yaml
ELASTICSEARCH_USERNAME=
ELASTICSEARCH_PASSWORD=
ELASTICSEARCH_CERTPATH=
```

To start API Control Plane BETA with secure elasticsearch connectovoty enabled:

- change to install/docker directory:

    ```bash
      cd install/docker
    ```

- execute the deployment script

    ```bash
      docker compose -f control-plane-beta-secure-es.yaml up -d
    ```
