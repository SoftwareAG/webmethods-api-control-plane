# Connecting API Gateway to API control plane

In API Control Plane, each API Gateway deployment is referred as
runtime. The runtimes get auto populated in API Control Plane when
the connectivity is established between API Gateway and API Control Plane.
The connectivity is established by externalizing the control plane agent configuration through a set of properties.
When you enable this configuration and start up the runtime (API Gateway),
the runtime reads the control plane agent configuration, registers the runtime in API Control Plane, starts the schedulers, and sends the health check status, asset, and runtime metrics (such as transaction count, error rate, avaerage latency, and average response time at API level) to API Control Plane at regular intervals.

# Control Plane Agent Properties
The following set of properties are used to establish the 
connectivity between the runtime and API Control Plane

| Property        | Default | Description              |
| ------------- |:-------------:|:------------------------------|
| apigw_cp_agentConfig_enabled* | false | This property decides whether the runtime should start communicating with API Control Plane. |
| apigw_cp_agentConfig_runtimeConfig_runtimeName*     |       |   The name of the runtime is used as ID in API Control Plane. This property defines how you want to identify the runtime in API Control Plane. |
| apigw_cp_agentConfig_runtimeConfig_deploymentType | SAG_SAAS      |   The Runtime deployment type. Permitted values are ON_PREMISE,SAG_SAAS,PRIVATE_CLOUD |
| apigw_cp_agentConfig_runtimeConfig_heartBeatIntervalInSeconds | 60 | The duration in seconds in which the runtime has to send health check status to API Control Plane.|
| apigw_cp_agentConfig_runtimeConfig_metricsSynchIntervalInSeconds | 60 | The duration in seconds in which the runtime has to send metrics to API Control Plane.|
| apigw_cp_agentConfig_runtimeConfig_assetSynchIntervalInSeconds | 120 |  The duration in seconds in which the runtime has to synchronize the changes made to the assets to API Control Plane.|
| apigw_cp_agentConfig_runtimeConfig_location |     | The location where the runtime is deployed.  Example: Oregon|
| apigw_cp_agentConfig_runtimeConfig_tags |     | The tag name of the runtime. Tags are used to organize and categorize the runtimes. Multiple tags can be specified by adding comma.Example: `apigw_cp_agentConfig_runtimeConfig_tags= test,local,dev`|
| apigw_cp_agentConfig_runtimeConfig_host|    | The host name of the runtime.Example:`apigw_cp_agentConfig_runtimeConfig_host =demo.apigw-aw-us.webmethods.in`|
| apigw_cp_agentConfig__runtimeConfig_region|    | The region name where the runtime is hosted.Example:`apigw_cp_agentConfig__runtimeConfig_region= AWS-US`|
| apigw_cp_agentConfig_runtimeConfig_capacity_value|    |The number of transaction calls that a runtime can process for the specified duration.|
| apigw_cp_agentConfig_runtimeConfig_capacity_units |    | The duration for which the runtime threshold capacity is defined. Possible values are `per second,per minute,per hour,per day,per week,per month,per year|
| apigw_cp_agentConfig_controlPlaneConfig_controlPlaneURL*|    |The valid URL that is used to access API Control Plane.|
| apigw_cp_agentConfig_controlPlaneConfig_username |    | User name that is used to log in to API Control Plane.|
| apigw_cp_agentConfig_controlPlaneConfig_password |    | Password of the corresponding user name that is used to log into API Control Plane.|

### Sample Docker file
A sample docker compose file that is used to start API Gateway and Elasticsearch is as follows
```
version: "3.5"
services:
  apigateway:
    depends_on:
      - elasticsearch
    image: ${GATEWAY_IMAGE}
    container_name: apigw01
    environment:
      - apigw_elasticsearch_hosts=elasticsearch:9200
      - apigw_elasticsearch_http_username=
      - apigw_elasticsearch_http_password=
      - apigw_cp_agentConfig_enabled=true
      - apigw_cp_agentConfig_runtimeConfig_runtimeName=API_Gateway_CHN
      - apigw_cp_agentConfig_runtimeConfig_description=My test description apigw_cp_agentConfig_runtimeConfig_tags= test,local,dev
      - apigw_cp_agentConfig_runtimeConfig_location=Chennai
      - apigw_cp_agentConfig__runtimeConfig_region=Chennai
      - apigw_cp_agentConfig_controlPlaneConfig_controlPlaneURL=<valid API Control Plane URL>
      - apigw_cp_agentConfig_controlPlaneConfig_username=<put username here>
      - apigw_cp_agentConfig_controlPlaneConfig_password=<put password here>
    ports:
      - 5555:5555
      - 9072:9072
    networks:
      - cp-gateway
    
   elasticsearch:
    ...............
    ..............
    ...............

```

#Control Plane Agent Configuration using YAML file
In API Gateway, create an external file called cp-agent.yaml in the \IntegrationServer\instances\default\packages\WmAPIGateway\resources\configuration.

```
---
agentConfig:
enabled: true
runtimeConfig:
runtimeName: "API_Gateway_CHN"
deploymentType: "SAG_SAAS"
description: "A demo deployment of SAG API Gateway"
heartBeatIntervalInSeconds: 30
metricsSynchIntervalInSeconds: 30
assetSynchIntervalInSeconds: 30
region: "Chennai"
location: "New Delhi"
host: "<valid API Gateway URL>"
tags:
- mki
- local
capacity:
value:
units:
controlPlaneConfig:
controlPlaneURL: "<valid API Control Plane URL>"
username: "<user name>"
password: "<password>"
```