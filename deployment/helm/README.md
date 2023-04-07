# API Control Plane deployment with helm

The instructions for installing and running the API Control Plane on kubernetes using helm are below.
If you have any questions or find issues, please use the [Issues](https://github.com/SoftwareAG/webmethods-api-control-plane/issues) or [Discussions](https://github.com/SoftwareAG/webmethods-api-control-plane/discussions) tabs in this repository.

The standard deployment of API Control plane contains the following 5 microservices.
![img.png](../attachments/img.png)

1. Asset catalog - Assets(Runtimes,Data planes) processing of Control plane.
2. Engine - Metrics processing and aggregation.
3. Ingress - User management and security
4. API Control Plane UI - user interface
5. Elasticsearch - data persistence layer

***

Table of contents

1. [How to deploy webMethods API Control Plane using helm?](#how-to-deploy-webmethods-api-control-plane-using-helm)
2. [How to stop webMethods API Control Plane using helm?](#how-to-stop-webmethods-api-control-plane-using-helm)
3. [How to access the newly deployed webMethods API Control Plane?](#how-to-access-the-newly-deployed-webmethods-api-control-plane)
4. [Additional deployment flavors](#additional-deployment-flavors)

***

## How to deploy webMethods API Control Plane using helm?

1. Clone this repository
2. Get a Personal Access Token (PAT)

    We published docker images for all API Control Plane microservice to this GitHub reporotory. To be able to pull them or have docker compose pull them for you, you'll need a Personal Access token (PAT). Go to 'settings' in your GitHub profile, then to 'Developer settings' on the bottom of left side navigation bar and finally to 'Personal access tokens' to generate one. Please use classic token type and make sure to select read privilege for 'packages'.

3. Create a kubernetes namespace for your deployment

    Execute the following command

    ```bash
    kubectl create namespace control-plane
    ```

    If everything goes well the output should be similar to

    ```bash
    [przemek@somehost helm]$ kubectl create namespace control-plane
    namespace/control-plane created
    ```

4. Create a kubernetes secret to be able to pull docker images

    Execute the following command

    ```bash
    kubectl create secret docker-registry regcred -n control-plane --docker-server=ghcr.io --docker-username=[Your git username] --docker-password=[Your PAT]
    ```

    If everything goes well the output should be similar to

    ```bash
    [przemek@somehost helm]$ kubectl create secret docker-registry regcred -n control-plane --docker-server=ghcr.io --docker-username=przemekuliok --docker-password=*secret*
    secret/regcred created created
    ```

5. Configure your deployment

    The values.yaml file in [deployment/helm/values.yaml](deployment/helm/values.yaml) allows for configuring different aspects of API Control Plane deplpyment. To be able to access API Contol Plane after it's deployed, you need to edit this file and provide a value for `domainName` that matches the hostname of the machine you're deploying API Conrtol plane on. Make sure this hostname is accessible to whoever will be connecting to API Control Plane.

    Default configuration is set up to deploy 2 replicas of API Control Plane containers and 1 replica for others. Edit the values.yaml file to change that as needed.

6. Execute the deployment script

    To deploy the API Control Plane with default configuration:

    - change to deployment/docker directory:

        ```bash
        cd deployment/helm
        ```

    - execute the deployment script

        ```bash
        helm upgrade --install --create-namespace --namespace control-plane --wait --timeout 5m0s control-plane .
        ```

    If everything goes well, the output should be similar to this

    ```bash
    [przemek@somehost helm]$ helm upgrade --install --create-namespace --namespace control-plane --wait --timeout 5m0s control-plane .
    Release "control-plane" does not exist. Installing it now.
    NAME: control-plane
    LAST DEPLOYED: Fri Apr  7 11:37:53 2023
    NAMESPACE: control-plane
    STATUS: deployed
    REVISION: 1
    TEST SUITE: None
    ```
  
7. Verify it's started

    It will take a couple of minutes to start. You can monitor that with solutions like Portainer or Docker\Kubernetes Dashboard etc. or simply user Docker\Kubernetes CLI like this

    ```bash
    [przemek@somehost helm]$ kubectl get all -n control-plane
    NAME                               READY   STATUS    RESTARTS   AGE
    pod/assetcatalog-75d94cff5-7bmkk   1/1     Running   0          5m43s
    pod/assetcatalog-75d94cff5-vrsxm   1/1     Running   0          5m43s
    pod/elasticsearch-0                1/1     Running   0          5m43s
    pod/engine-57967b46c4-5b5kg        1/1     Running   0          5m43s
    pod/engine-57967b46c4-92pnz        1/1     Running   0          5m43s
    pod/ingress-7d4567dcf8-grrrp       1/1     Running   0          5m43s
    pod/ingress-7d4567dcf8-jnrwt       1/1     Running   0          5m43s
    pod/ui-6c8d58d754-bc7s5            1/1     Running   0          5m43s
    pod/ui-6c8d58d754-l5qcs            1/1     Running   0          5m43s

    NAME                             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
    service/assetcatalog-svc         ClusterIP   10.43.72.67     <none>        8080/TCP   5m43s
    service/elasticsearch-headless   ClusterIP   None            <none>        9300/TCP   5m43s
    service/elasticsearch-lb         ClusterIP   10.43.31.70     <none>        9200/TCP   5m43s
    service/engine-svc               ClusterIP   10.43.19.194    <none>        8080/TCP   5m43s
    service/ingress-svc              ClusterIP   10.43.111.144   <none>        8080/TCP   5m43s
    service/ui-svc                   ClusterIP   10.43.66.124    <none>        8080/TCP   5m43s

    NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/assetcatalog   2/2     2            2           5m43s
    deployment.apps/engine         2/2     2            2           5m43s
    deployment.apps/ingress        2/2     2            2           5m43s
    deployment.apps/ui             2/2     2            2           5m43s

    NAME                                     DESIRED   CURRENT   READY   AGE
    replicaset.apps/assetcatalog-75d94cff5   2         2         2       5m43s
    replicaset.apps/engine-57967b46c4        2         2         2       5m43s
    replicaset.apps/ingress-7d4567dcf8       2         2         2       5m43s
    replicaset.apps/ui-6c8d58d754            2         2         2       5m43s

    NAME                             READY   AGE
    statefulset.apps/elasticsearch   1/1     5m43s
    ```

###### [Back to Top](#api-control-plane-deployment-with-helm)
***

## How to stop webMethods API Control Plane using helm?

To stop and remove the API Control Plane default configuration:

- change to deployment/helm directory:

    ```bash
    cd deployment/helm
    ```

- execute the deployment script

    ```bash
    helm uninstall control-plane -n control-plane
    ```

If everything goes well, the output should be similar to this

```bash
[przemek@somehost helm]$ sudo /usr/local/bin/helm uninstall control-plane -n control-plane
release "control-plane" uninstalled
```

###### [Back to Top](#api-control-plane-deployment-with-helm)
***

## How to access the newly deployed webMethods API Control Plane?

1. Open your browser and go to https://[the-host-you-configured]:443/
2. You should see the login screen. Log in using Administrator usename and the default password.

###### [Back to Top](#api-control-plane-deployment-with-helm)
***

## Additional deployment flavors

### 1. Enabling Open Telemetry using Jaeger

API Control plane can be started in debug mode with Open telemetry enabled and exposed using [Jaeger UI](https://www.jaegertracing.io/). For this, the deployment needs additional image, namely `jaegertracing/all-in-one`.

To enable it set the property `applications.jaegertracing.enabled` to `true` in `values.yaml` file (line 111).

```yaml
  jaegertracing:
    enabled: true
    name: jaeger-tracing
    #--- Jaeger Tracer image name
    imageName: jaegertracing/all-in-one
    #--- Jaeger Tracer image tag
    imageTag: latest
    #--- Jaeger Tracer image replicas number
    replicas: 1
    #--- Port to run Jaeger.
    port : 4317
    #--- UI Port in which Jaeger is exposed
    uiPort: 16686
     #--- Node Port to access JaegarUI outside the cluster 
    extPort: 30007
    storage: 2Gi
    resources:
      # This is just an example. This should be a consious choice of the user. Refer
      # https://raw.githubusercontent.com/hansehe/jaeger-all-in-one/master/helm/jaeger-all-in-one/values.yaml
      limits:
        cpu: 100m
        memory: 128Mi                                                     
      requests:
        cpu: 100m
        memory: 128Mi
    # - Enable to use volume mapping for Jaeger    
    volume:
      enabled: false
      className: ""
      size: 3Gi

```

:wave: The Jaeger UI can be accessed via the `applications.jaegertracing.extPort` port (NodePort type) configured in the `values.ymal` file.

:wave: The Jaeger deplpoyment here is a stateless deployment. Please refer to https://github.com/hansehe/jaeger-all-in-one/tree/master/helm for the complete production level deployment of Jaeger.

### 2. Enabling Gainsight integration

If you want to see [Gainsight](https://www.gainsight.com/product-experience/) powered user engagemnets (work in progress) in API Control Plane like bots, articles, feature introduction etc., you need to provide additional configuration set up in `values.yaml` file. Please contact your Software AG API Control Plane BETA contact or ask us here for the proper confoguration values.

To enable Gainsight integration set `applications.gainsight.enabled` to `true` in `values.yaml` file (line 142). Values for the remaining attributes will be provided to you by Software AG.

```yaml
  gainsight:
    enabled : true
    tenant:
      name: "ACME Inc"
      cloudProvider: "Azure"
      region: "US West Oregon"
      plan: "Free"
      stage: "Staging"
      key: "secret"
```

### 3. Enabling secure Elasticsearch communication

To enable secure communication to Elastic, proper certificates needs to created as kuberentes secrets and provided to the microservices.
Please refer to
https://github.com/elastic/helm-charts/blob/main/elasticsearch/examples/security/values.yaml
https://github.com/lisenet/kubernetes-homelab/tree/master/logging 
to enable Elasticsearch over SSL.

###### [Back to Top](#api-control-plane-deployment-with-helm)
***

## Known issues

1. If your Kubernetes server's version is lower than 1.19, then edit the `templates\nginx_ingress.yaml` file to:

- change the `apiVersion` from 'networking.k8s.io/v1' to 'networking.k8s.io/v1beta1'.
- edit the 'backend' part fo the specification to look like this:

  ```yaml
  backend:
    # service: 
    #   name: {{ .Values.applications.ingress.name }}-svc
    #   port: 
    #     number: 8080            
    serviceName: {{ .Values.applications.ingress.name }}-svc
    servicePort: 8080
  ```

###### [Back to Top](#api-control-plane-deployment-with-helm)
***
