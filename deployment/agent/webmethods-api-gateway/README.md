# Connecting API Gateway to API control plane

> **Note:** To configure webMethods API Gateway to connect to API Control Plane, API Gateway must be in version **10.15 Fix 4 or higher**. 

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
| apigw_cp_agentConfig_runtimeConfig_region|    | The region name where the runtime is hosted.Example:`apigw_cp_agentConfig_runtimeConfig_region=AWS-US`|
| apigw_cp_agentConfig_runtimeConfig_capacity_value|    |The number of transaction calls that a runtime can process for the specified duration.|
| apigw_cp_agentConfig_runtimeConfig_capacity_units |    | The duration for which the runtime threshold capacity is defined. Possible values are `per second,per minute,per hour,per day,per week,per month,per year|
| apigw_cp_agentConfig_controlPlaneConfig_controlPlaneURL*|    |The valid URL that is used to access API Control Plane.|
| apigw_cp_agentConfig_controlPlaneConfig_username |    | User name that is used to log in to API Control Plane. Please note that this user should belong to all three groups: API Control Plane administrators, API platform providers, API product managers.|
| apigw_cp_agentConfig_controlPlaneConfig_password |    | Password of the corresponding user name that is used to log into API Control Plane.|

### Sample Docker file
A sample docker compose file that is used to start API Gateway and Elasticsearch can have the following configuration to connect to API Control Plane:

:wave: Please note that any of the above mentioned properties can be configured as environment variables

```yaml
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
      - apigw_cp_agentConfig_runtimeConfig_runtimeName=<name_of_this_api_gateway_runtime>
      - apigw_cp_agentConfig_runtimeConfig_description=<description_for_this_runtime>
      - apigw_cp_agentConfig_runtimeConfig_tags=<comma_separated_list_of_tags>
      - apigw_cp_agentConfig_runtimeConfig_location=<location_where_it_is_deployed>
      - apigw_cp_agentConfig_runtimeConfig_region=<region_where_it_is_deployed>
      - apigw_cp_agentConfig_runtimeConfig_capacity_value=<numerical_value_of_the_expected_transactional_volume>
      - apigw_cp_agentConfig_runtimeConfig_capacity_units=<time_units_of_the_capacity>
      - apigw_cp_agentConfig_controlPlaneConfig_controlPlaneURL=<protocol://host:port-of-your-api-control-plane-instance>
      - apigw_cp_agentConfig_controlPlaneConfig_username=<user-account-of-api-control-plane-in-all-three-groups>
      - apigw_cp_agentConfig_controlPlaneConfig_password=<user-password>
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

Example:
```yaml
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
      - apigw_cp_agentConfig_runtimeConfig_runtimeName=API_Gateway_Development
      - apigw_cp_agentConfig_runtimeConfig_description="A development environment of API Gateway" 
      - apigw_cp_agentConfig_runtimeConfig_tags=on_premises,dev
      - apigw_cp_agentConfig_runtimeConfig_location=Chennai
      - apigw_cp_agentConfig_runtimeConfig_region=APJ
      - apigw_cp_agentConfig_runtimeConfig_capacity_value=1000
      - apigw_cp_agentConfig_runtimeConfig_capacity_units="per second"
      - apigw_cp_agentConfig_controlPlaneConfig_controlPlaneURL="https://control-plane-host:8443"
      - apigw_cp_agentConfig_controlPlaneConfig_username=my_admin_user
      - apigw_cp_agentConfig_controlPlaneConfig_password=MyAdminP@$$w0rd
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

# Control Plane Agent Configuration using YAML file
In API Gateway, create an external file called *cp-agent.yaml* in the *\<SAGInstallDir\>/IntegrationServer/instances/\<instance_name\>/packages/WmAPIGateway/resources/configuration*. Place the below content in the file and make sure to change the configurations appropriately.

> **Note:** It is important to name the file ONLY as *cp-agent.yaml*.

```yaml
---
agentConfig:
  enabled: true
  runtimeConfig:
    runtimeName: "<name_of_this_api_gateway_runtime>"
    deploymentType: "<deployment_type_of_this_runtime>"
    description: "<description_for_this_runtime>"
    heartBeatIntervalInSeconds: <numerical_interval_with_which_the_runtime_status_information_is_sent>
    metricsSynchIntervalInSeconds: <numerical_interval_with_which_the_runtime_metrics_are_sent>
    assetSynchIntervalInSeconds: <numerical_interval_with_which_the_apis_are_synchronized>
    region: "<region_where_it_is_deployed>"
    location: "<location_where_it_is_deployed>"
    host: "<protocol://host:port-of-your-api-gateway-instance>"
    tags:
    - <each_line_represents_one_tag_entry>
    capacity:
      value: <numerical_value_of_the_expected_transactional_volume>
      units: "<time_units_of_the_capacity>"
    controlPlaneConfig:
      controlPlaneURL: "<protocol://host:port-of-your-api-control-plane-instance>"
      username: "<user-account-of-api-control-plane-in-all-three-groups>"
      password: "<user-password>"
```

A sample YAML file with configured values will look like below:

```yaml
---
agentConfig:
  enabled: true
  runtimeConfig:
    runtimeName: "API_Gateway_Development"
    deploymentType: "SAG_SAAS"
    description: "A development environment of SAG API Gateway"
    heartBeatIntervalInSeconds: 60
    metricsSynchIntervalInSeconds: 120
    assetSynchIntervalInSeconds: 180
    region: "Chennai"
    location: "New Delhi"
    host: "https://apigateway-runtime:9072"
    tags:
    - saas_gateway
    - dev_stage
    capacity:
      value: 1000
      units: "per second"
    controlPlaneConfig:
      controlPlaneURL: "https://control-plane-host:8443"
      username: "my_admin_user"
      password: "MyAdminP@$$w0rd"
```

> **Note:** The password provided in the YAML file will be read by the API Gateway on application startup and it will be removed thereafter and placed in the password store for future reference. As the file will be updated to remove the password, ensure that the file has write permissions.