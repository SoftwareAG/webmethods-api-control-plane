# API Control Plane deployment with docker compose

The instructions for installing and running the API Control Plane on docker are below.
If you have any questions or find issues, please use the [Issues](https://github.com/SoftwareAG/webmethods-api-control-plane/issues) or [Discussions](https://github.com/SoftwareAG/webmethods-api-control-plane/discussions) tabs in this repository.

The standard deployment of API Control plane contains the following 5 microservices.
![img.png](../attachments/img.png)

1. Asset catalog - Assets(Runtimes,Data planes) processing of Control plane.
2. Engine - Metrics processing and aggregation.
3. Ingress - User management and security
4. API Control Plane UI - user interface
5. Elasticsearch - data persistence layer

Table of contents

1. [How to deploy webMethods API Control Plane using docker compose?](#how-to-deploy-webmethods-api-control-plane-using-docker-compose)
2. [How to stop webMethods API Control Plane using docker compose?](#how-to-stop-webmethods-api-control-plane-using-docker-compose)
3. [Additional deployment flavors](#additional-deployment-flavors)

## How to deploy webMethods API Control Plane using docker compose?

1. Clone this repository
2. Get a Personal Access Token (PAT)

    We published docker images for all API Control Plane microservice to this GitHub reporotory. To be able to pull them or have docker compose pull them for you, you'll need a Personal Access token (PAT). Go to 'settings' in your GitHub profile, then to 'Developer settings' on the bottom of left side navigation bar and finally to 'Personal access tokens' to generate one. Please use classic token type and make sure to select read privilege for 'packages'.

3. Log in to ghcr.io docker repository

    Run the following command in your docker environment:

    ```bash
    docker login ghcr.io -u [Your git username] -p [Your PAT]
    ```

4. Configure your deployment

    The `.env` file in [deployment/docker/.env](deployment/docker/.env) allows for configuring different aspects of API Control Plane deplpyment. To be able to access API Contol Plane after it's deployed, you need to edit this file and provide a value for `NGINX_DOMAIN_NAME` that matches the hostname of the machine you're deploying API Conrtol plane on. Make sure this hostname is accessible to whoever will be connecting to API Control Plane.

5. Execute the deployment sccripts

    To deploy the API Control Plane with default configuration:

    - change to install/docker directory:

        ```bash
          cd deployment/docker
        ```

    - execute the deployment script

        ```bash
          docker compose -f control-plane-beta.yaml up -d
        ```

    If everything goes well, the outpus should be similar to this

    ```bash
    [przemek@somehost docker]$ docker compose -f control-plane-beta.yaml up -d
    [+] Running 8/8
    ⠿ Network softwareag-api-management      Created                         0.2s
    ⠿ Container elasticsearch                Healthy                        22.6s
    ⠿ Container nginx_setup                  Started                         1.5s
    ⠿ Container control-plane-asset-catalog  Healthy                        88.6s
    ⠿ Container control-plane-engine         Healthy                        88.6s
    ⠿ Container control-plane-ui             Healthy                       119.1s
    ⠿ Container control-plane-ingress        Healthy                       150.5s
    ⠿ Container nginx                        Started                       151.2s
    ```

6. Verify it's atarted

    It will take a couple of minutes to start. You can monitor that with solutions like Portainer or Docker Dashboard etc. or simply user Docker CLI like this

    ```bash
    docker ps --format '{"ID":"{{ .ID }}", "Status": "{{ .Status }}", "Name":"{{ .Names }}"}'
    ```

    The output, when everything starts should look similar to this

    ```json
    {"ID":"9b791d1dfa4b", "Status": "Up 2 minutes (healthy)", "Name":"nginx"}
    {"ID":"7504f5e94ecd", "Status": "Up 2 minutes (healthy)", "Name":"control-plane-ingress"}
    {"ID":"88573a9436ff", "Status": "Up 2 minutes (healthy)", "Name":"control-plane-ui"}
    {"ID":"e9c1def23279", "Status": "Up 2 minutes (healthy)", "Name":"control-plane-engine"}
    {"ID":"d1e89d3227de", "Status": "Up 2 minutes (healthy)", "Name":"control-plane-asset-catalog"}
    {"ID":"4b598e3e422c", "Status": "Up 2 minutes (healthy)", "Name":"elasticsearch"}
    ```

###### [Back to Top](#api-control-plane-deployment-with-docker-compose)
***

## How to stop webMethods API Control Plane using docker compose?

To stop and remove the API Control Plane default configuration:

- change to deployment/docker directory:

    ```bash
      cd deployment/docker
    ```

- execute the deployment script

    ```bash
      docker compose -f control-plane-beta.yaml down
    ```

If everything goes well, the outpus should be similar to this

```bash
[przemek@somehost docker]$ docker compose -f control-plane-beta.yaml down
[+] Running 8/8
⠿ Container nginx_setup                  Removed                         0.0s
⠿ Container nginx                        Removed                         0.3s
⠿ Container control-plane-ingress        Removed                        10.3s
⠿ Container control-plane-ui             Removed                        10.4s
⠿ Container control-plane-engine         Removed                        10.3s
⠿ Container control-plane-asset-catalog  Removed                        10.3s
⠿ Container elasticsearch                Removed                         2.6s
⠿ Network softwareag-api-management      Removed                         0.3s
```

###### [Back to Top](#api-control-plane-deployment-with-docker-compose)
***

## Additional deployment flavors

### 1. Enabling Open Telemetry using Jaeger

API Control plane can be started in debug mode with Open telemetry enabled and exposed using [Jaeger UI](https://www.jaegertracing.io/). For this, the deployment needs additional image, namely `jaegertracing/all-in-one`.

To start API Control Plane BETA in debug mode:

- change to deployment/docker directory:

    ```bash
      cd deployment/docker
    ```

- execute the deployment script

    ```bash
      docker compose -f control-plane-beta.debug.yaml up -d
    ```

:wave: The Jaeger UI can be accessed via the `JAEGER_UI_PORT` which will configured in the `.env` file

### 2. Enabling Gainsight integration

If you want to see Gainsight powerd user engagemnets (work in progress) in API Control Plane like bots, articles, feature introduction etc., you can start it with additional configuration set up in `ui/ui-config.gainsight.env` file. Please contact your Software AG API Control Plane BEAT contact or ask us here for the proper confoguration values.

To start API Control Plane BETA with Gainsight enabled:

- change to deployment/docker directory:

    ```bash
      cd deployment/docker
    ```

- execute the deployment script

    ```bash
      docker compose -f control-plane-beta.gainsight.yaml up -d
    ```

### 3. Enabling Secure Elastic communication

If you wuld like to run elasticsearch in secure mode, then the following properties needs to be set in the `.env` file to enable secure communication

``` yaml
ELASTICSEARCH_USERNAME=
ELASTICSEARCH_PASSWORD=
ELASTICSEARCH_CERTPATH=
```

To start API Control Plane BETA with secure elasticsearch connectovoty enabled:

- change to deployment/docker directory:

    ```bash
      cd deployment/docker
    ```

- execute the deployment script

    ```bash
      docker compose -f control-plane-beta-secure-es.yaml up -d
    ```

###### [Back to Top](#api-control-plane-deployment-with-docker-compose)
***
