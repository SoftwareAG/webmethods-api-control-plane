# API Control Plane helm chart

This helm chart installs Control Plane product in a k8s cluster 
Refer `values.yaml` for the configurable values that can be passed while running the helm installation command.

## Standard deployment of API Control plane.
The standard deployment of API Control plane contains the following 5 microservices.

2 replicas each of 
1. Asset catalog microservice. - Assets(Runtimes,Data planes) processing of Control plane.
2. Engine microservice. - Metrics processing and aggregation.
3. Ingress microservice. - User management and security 
4. API Control plane UI microservice. - Angular based UI
   and 
5. 1 replica of `Elastic search 8.1.1` which is the data store.

You can run the below command to install the standard flavour of the  product.

```
$ helm upgrade \
    --install \
    --create-namespace \
    --namespace control-plane \
    --wait \
    --timeout 5m0s \
    control-plane \
    . \
```
This creates a deployment in the namespace `control-plane`

> **Note:** The `domainName` should be added to `/etc/hosts` (for linux) or `C:\Windows\System32\drivers\etc\hosts` (for windows)
pointing to the VM where the k8s cluster is running to access the product from the developer machine.

## Enabling Open Telemetry using Jaeger
API Control plane can be started in debug mode  
with Open telemetry enabled using Jaeger(https://www.jaegertracing.io/).

To enable jaegar tracing set the property `applications.jaegertracing.enabled=true`.
The Jaegar configurations can be found in the values.yaml that contains the port details and other resource
configurations. 
This example uses the 'jaegertracing/all-in-one' image. 

```
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
> **Note:** The Jaeger UI can be accessed via the `applications.jaegertracing.extPort` which will be a
> Node port in the deployment.

The Jaeger provided in this sample stateless deployment.
Please refer to  https://github.com/hansehe/jaeger-all-in-one/tree/master/helm for the complete production level 
deployment of Jaegar.

## Enabling Gainsight 

To enable Gainsight (https://www.gainsight.com/product-experience/) set `applications.gainsight.enabled=true`.
After enabling Gainsight proper values needs to be provided in the values.yaml. Please contact SoftwareAG product 
management team for the proper values and the key.
```
  gainsight:
    enabled : true
    tenant:
      name: "ACME Inc"
      cloudProvider: "Azure"
      region: "US West Oregon"
      plan: "Free"
      stage: "Staging"
      key: "abracadabra"
```

### Enabling Secure communication to ES

To enable secure communication to ES , proper certificates needs to created as K8s secrets 
and provided to the microservices
Please refer to
https://github.com/elastic/helm-charts/blob/main/elasticsearch/examples/security/values.yaml
https://github.com/lisenet/kubernetes-homelab/tree/master/logging 
to enable Elastic search over SSL.