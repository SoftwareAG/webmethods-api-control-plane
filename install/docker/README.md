# API Control Plane docker compose 

The docker compose files in this folder installs Control Plane product 
Refer `.env` for the configurable values that can be passed while running docker
compose command.

## Standard deployment of API Control plane.
The standard deployment of API Control plane contains the following 5 microservices.
![img.png](../attachments/img.png)

1. Asset catalog microservice. - Assets(Runtimes,Data planes) processing of Control plane.
2. Engine microservice. - Metrics processing and aggregation.
3. Ingress microservice. - User management and security
4. API Control plane UI microservice. - Angular based UI
   and
5. `Elastic search 8.1.1` which is the data store.

You can run the below command to install the standard flavour of the  product.

```
docker compose -f control-plane-beta.yaml up -d

```

> **Note:** The `NGINX_DOMAIN_NAME` in the .env file should be added to `/etc/hosts` (for linux) or 
> `C:\Windows\System32\drivers\etc\hosts` (for windows)
pointing to the VM to access the product from the developer machine.

## Enabling Open Telemetry using Jaeger
API Control plane can be started in debug mode  
with Open telemetry enabled using Jaeger(https://www.jaegertracing.io/).
 
This example uses the 'jaegertracing/all-in-one' image.
To start environment with debug mode 
```
 docker compose -f control-plane-beta.yaml up -d
```
> **Note:** The Jaeger UI can be accessed via the `JAEGER_UI_PORT` which will configured
> in the .env 

## Enabling Gainsight
To enable Gainsight proper values needs to be provided in the 
ui/ui-config.gainsight.env
Please contact SoftwareAG product management team for the proper values and the key.

```
 docker compose -f control-plane-beta.gainsight.yaml up -d
```

## Enabling Secure Elastic communication
The following properties needs to be set to enable secure es communication
```
ELASTICSEARCH_USERNAME=
ELASTICSEARCH_PASSWORD=
ELASTICSEARCH_CERTPATH=
```
After setting these execute
```
 docker compose -f control-plane-beta-secure-es.yaml up -d
```
