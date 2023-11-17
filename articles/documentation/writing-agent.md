# Writing Runtime agent for connecting to API Control Plane
This article describes the details about writing an agent for connecting an API runtime to API Control Plane.

## Introduction

The article explains the details about writing an agent for API runtimes to connect to API Control Plane using the REST APIs provided by Control Plane. 

> **Note:** The existing Control Plane REST APIs and data models are closed coupled with webMethods API Gateway. In future releases, they will be made more generic such that it will seamless for the other API runtimes to connect to API Control Plane in a standardized way.

## Required agent functionalities

For an agent to connect to API Control Plane successfully and be part of the API Control Plane landscape, the following functionalities should be supported by it.

- The agent should register the API runtime to API Control Plane at API runtime startup.
- The agent should send the API runtime status as heartbeats to API Control Plane to know about runtime status.
- The agent should publish the assets/resources from the API runtime to API Control Plane in a compatible/required format.
- The agent should be responsible for keeping the runtime assets in API Control Plane to be in sync.
- The agent should send the API usage metrics to API Control Plane in a compatible/required format.
- The agent should be responsible for resending the metrics which are lost when communication to API Control Plane or API runtime is failed due to server unavailability or license expiration.

## Steps to write the agent implementation

Follow the below steps to write the agent to make the API runtime to be part of API Control Plane landscape. API Control Plane provides REST APIs for its assets management and receiving metrics. The agent needs to make use of those APIs for accomplishing all the required functionalities. Refer the Open API documentation present [here](../../apis/openapi-specifications) for detailed information.

### 1. Registering the API runtime

As a first step, the agent should register the API runtime to API Control Plane for making it as member in the API Control Plane runtimes landscape. Use the below REST API for registering the API runtime. Please refer [this](../../apis/openapi-specifications/runtime-management.yaml) Open API spec for detailed explanation of the API.

e.g.

```
POST /api/assetcatalog/v1/runtimes
{
    "id": "AmazonAPIGatewayWestUS"
    "name": "AmazonAPIGatewayWestUS",
    "description": "Amazon API Gateway deployed in West zone in US region. It hosts US Government military APIs",
    "region": "us",
    "location": "denver",
    "host": "eu.west.aws.dms",
    "type": "WEBMETHODS_GATEWAY",
    "deploymentType": "SAG_SAAS",
    "owner": {
        "name": "smith"
    },
    "tags": [
        "dev"
    ],
    "capacity": {
    	"value": 10000
    	"units": "per second"
    },
    "heartBeatSynchInterval": 60
}
```

The `heartBeatSynchInterval` refers to the time interval to be used by the agent for sending the heartbeats to API Control Plane periodically.

### 2. Sending heartbeats

Once the API runtime is registered with API Control Plane, the agent should start sending the status of the Runtime periodically so that its status can be visualized in API Control Plane landscape. Use the below REST API for sending the heartbeat. Please refer [this](../../apis/openapi-specifications/controlplane-metrics.yaml) Open API spec for detailed explanation of the API.

e.g.

```
POST /api/engine/v1/runtimes/heartbeats
{
	"runtimeId": "AmazonAPIGatewayWestUS"
	"active": 1,
	"created": "2022-07-14 14:25:17:116"
}
```

The agent should run a scheduled task which would run periodically at `heartBeatSynchInterval` interval seconds to send the heartbeats. The API Control Plane uses this interval value for measuring the health of the runtime for a period of time. If heartbeats are missed for certain intervals, it will be assumed by Control Plane that the runtime is unhealthy at those intervals.

### 3. Publishing runtime assets

After the agent is started and made a connection to API Control Place, it should retrieve the assets from the API Runtime and send to Control Plane at once for them to be visualized in the API landscape. As of now the Control Plane has support for only API assets and the list will extend in future. Agent should take care of converting the Runtime API assets into Control Plane API format to be sent to Control Plane. Use the below REST API for sending the API asset. Please refer [this](../../apis/openapi-specifications/api-management.yaml) Open API spec for a detailed explanation of the API.

For e.g. creating the asset at first time,

```
POST /api/assetcatalog/v1/runtimes/AmazonAPIGatewayWestUS/apis
{
    "id": "ac379195-32cf-5198-8e7b-38fe21120309",
    "name": "PetStoreAPI",
    "tags": [
    	"Dev"
    ],
    "systemVersion": 1,
    "displayVersion": "v1.0.1",    
    "previousVersion": "ea488aed-2588-4988-990d-8342d0bca78"
    "nextVersion": 5743beab-51c4-4b49-910f-282067db563d"
    "apiType": "SOAP",
    "apiStatus": "INACTIVE",
    "publishInfo": [],
    "noOfPolicies": 5
}
```

In case the asset is already available at Control Plane, *PUT* method should be called to update the asset.

```
PUT /api/assetcatalog/v1/runtimes/AmazonAPIGatewayWestUS/apis/ac379195-32cf-5198-8e7b-38fe21120309
{
    "id": "ac379195-32cf-5198-8e7b-38fe21120309",
    "name": "PetStoreAPI",
    "tags": [
    	"Dev"
    ],
    "systemVersion": 1,
    "displayVersion": "v1.0.1",    
    "previousVersion": "ea488aed-2588-4988-990d-8342d0bca78"
    "nextVersion": 5743beab-51c4-4b49-910f-282067db563d"
    "apiType": "SOAP",
    "apiStatus": "INACTIVE",
    "publishInfo": [],
    "noOfPolicies": 5
}
```

### 4. Keeping runtime assets in sync

After publishing the runtime assets to API Control Plane, the agent has the responsibility of keeping the runtime assets in Control Plane to be in sync with the Runtime for any asset/resource updates in the API runtime. For helping this, the Control Plane provides a REST API for storing the asset sync timestamp as an audit event. After the assets are published or synced to Control Plane, the agent would need to call this API for updating the timestamp for each asset type. The agent would then run a scheduled task to periodically check for any asset updates in the runtime that might happened after that last sync timestamp stored in Control Plane. Control Plane also provides a search REST API to get the audit events for each asset type which contains the last sync timestamp.

#### Storing the asset sync timestamp

Sample request to store the asset sync timestamp in API Control Plane for API asset (as of now only API is the supported runtime asset),

```
POST /api/assetcatalog/v1/runtimes/AmazonAPIGatewayWestUS/audit
{
	"types": [
		"API"
	]
}
```

Please refer [this](../../apis/openapi-specifications/runtime-management.yaml) Open API spec documentation for more details on the API resource.

#### Retrieving the asset sync timestamp

The search REST API provided in Control Plane can be used to retrieve the asset sync timestamp of the asset types. Please refer [this](../../apis/openapi-specifications/search.yaml) Open API spec documentation for more details on the search API resource.

Sample REST API request to retrieve the last sync timestamp of API asset type,

```
POST /api/assetcatalog/v1/search
{
    "type": "AUDITEVENT",
    "query": {
        "conditions": [
            {
                "criterias": [
                    {
                        "field": "objectType",
                        "value": "API",
                        "operation": "equals"
                    }
                ]
            },
            {
                "criterias": [
                    {
                        "field": "actionType",
                        "value": "RUNTIME_DATA_SYNC",
                        "operation": "equals"
                    }
                ]
            }
        ],
        "logicalConnector": "AND"
    },
    "size": 1,
    "sortFields": {
        "created": "DESC"
    }
}
```

The sample response would be as below.

```
{
    "searchResults": [
        {
            "id": "5644651d-584a-4e07-938d-cda36b65a64e",
            "created": 1700204768436,
            "objectType": "API",
            "actionType": "RUNTIME_DATA_SYNC",
            "data": "",
            "objectId": "AmazonAPIGatewayWestUS",
            "message": "Syncing Data from Runtime complete",
            "status": "success"
        }
    ]
}
```

The `created` field is the last sync timestamp of the API asset type.

#### Syncing the assets to Control Plane

The agent would use the asset sync timestamp retrieved from Control Plane to get the updated assets after that time from the API runtime. Then those assets would be converted into the respective Control Plane asset format and synced with Control Plane using the asset management REST APIs provided. Since the only supported asset type is API, please refer [this](../../apis/openapi-specifications/api-management.yaml) Open API spec documentation for more details on the management of API asset in Control Plane.

After this, the asset sync timestamp of all the asset types should be stored again in Control Plane for future assets sync operation.

### 5. Sending API usage metrics

The agent needs to run a scheduled task periodically to send the API usage metrics from runtime. Control Plane exposes a REST API for sending over the API usage metrics to it. These metrics are in Prometheus format now and closed coupled with webMethods API Gateway Prometheus metric format. This would be changed in future to have a generic format to be in line with all the API runtimes specific to both webMethods and 3rd parties. Till that support is available, the agent needs to convert the runtime metrics to Control Plane metrics format and send to it. Please refer [this](../../apis/openapi-specifications/controlplane-metrics.yaml) Open API spec documentation for more details on the metrics API resource.

e.g.

```
POST /api/engine/v1/runtimes/AmazonAPIGatewayWestUS/metrics

sag_apigw_tx_error_count{code="4xx",env="default",error="policy"} 3 1666787050986
sag_apigw_tx_error_count{code="4xx",env="default",error="internal"} 4 1666787050986
sag_apigw_tx_error_count{code="4xx",env="default",error="backend"} 5 1666787050986
sag_apigw_avg_response_time{code="2xx",env="default"} 4288 1666787050986
sag_apigw_avg_response_time{code="4xx",env="default"} 29 1666787050986
sag_apigw_avg_latency{code="2xx",env="default"} 66 1666787050986
sag_apigw_avg_latency{code="4xx",env="default"} 235 1666787050986
sag_apigw_apicalls_total{code="404",env="default"} 3 1666787050986
sag_apigw_apicalls_total{code="200",env="default"} 63 1666787050986
sag_apigw_api_tx_error_count{code="4xx",api_name="Api1",api_version="1.0",env="default",error="policy"} 3 1666787050986
sag_apigw_api_avg_response_time{code="2xx",api_name="Api1",api_version="1.0",env="default"} 4288 1666787050986
sag_apigw_api_avg_response_time{code="4xx",api_name="Api1",api_version="1.0",env="default"} 29 1666787050986
sag_apigw_api_avg_latency{code="2xx",api_name="Api1",api_version="1.0",env="default"} 66 1666787050986
sag_apigw_api_avg_latency{code="4xx",api_name="Api1",api_version="1.0",env="default"} 29 1666787050986
sag_apigw_api_avg_latency{code="3xx",api_name="Api1",api_version="1.0",env="default"} 39 1666787050986
sag_apigw_api_avg_backend_response_time{code="2xx",api_name="Api1",api_version="1.0",env="default"} 39 1666787050986
sag_apigw_api_avg_backend_response_time{code="3xx",api_name="Api1",api_version="1.0",env="default"} 49 1666787050986
sag_apigw_api_avg_latency{code="3xx",api_name="Api2",api_version="2.0",env="default"} 39 1666787050986
sag_apigw_api_avg_latency{code="3xx",api_name="Api2",api_version="2.0",env="default"} 39 1666787050986
sag_apigw_api_tx_count{code="2xx",api_name="JSONFormat",api_version="1.0",env="default"} 20 1666787050986
sag_apigw_api_tx_count{code="3xx",api_name="JSONFormat",api_version="1.0",env="default"} 10 1666787050986
```

Each Prometheus metric would be in the below format,

\<metric name>{\<comma separate list of labels>} \<value> \<timestamp>

The metrics are categorized into 2 types, API-level and Server-level.

#### API-level metrics

The API-level metrics should contain metrics specific to APIs.

| Prometheus metric name                  | Description                                                  | Type    | Labels                                                       |
| --------------------------------------- | ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| sag_apigw_api_tx_count                  | The total number of API invocations per HTTP response code for an API. | Counter | api_name<br/>api_version<br/>code (can have values 3xx, 4xx, 5xx)<br/>env |
| sag_apigw_api_tx_error_count            | This is an error rate related metric. Counts the number of API invocations with a response code greater or equal to 300 and classifies the invocations by its response code with the label 3xx, 4xx or 5xx. | Counter | api_name<br/>api_version<br/>code (can have values 3xx, 4xx, 5xx)<br/>env<br/>error (can have values internal, policy, backend) |
| sag_apigw_api_avg_latency               | This is a latency related metric. Measures the average time spent by the API invocations, which does not include the time spent in backend services, classified by its response code with the label 2xx, 3xx, 4xx or 5xx. | Gauge   | api_name<br/>api_version<br/>code (can have values 3xx, 4xx, 5xx)<br/>env |
| sag_apigw_api_avg_backend_response_time | This is a latency related metric. Measures the average time spent by the backend services while performing an API invocation request, classified by its response code with the label 2xx, 3xx, 4xx, 5xx or connect. | Gauge   | api_name<br/>api_version<br/>code (can have values 3xx, 4xx, 5xx)<br/>env |
| sag_apigw_api_avg_response_time         | This is a latency related metric. Measures the average time spent by the API invocations, classified by its response code with the label 2xx, 3xx, 4xx or 5xx. This includes the time spent in API Gateway and by the backend service. | Gauge   | api_name<br/>api_version<br/>code (can have values 3xx, 4xx, 5xx)<br/>env |

#### Server-level Metrics

The server-level metrics are aggregated values all the API level metrics.

| Prometheus metric name              | Description                                                  | Type    | Labels                                                       |
| ----------------------------------- | ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| sag_apigw_apicalls_total            | The total number of API invocations per HTTP response code.  | Counter | code (can have values 2xx, 3xx, 4xx, 5xx)<br/>env            |
| sag_apigw_tx_error_count            | This is an error rate related metric. Counts the number of API invocations with a response code greater or equal to 300 and classifies the invocations by its response code with the label 3xx, 4xx or 5xx. | Counter | code (can have values 3xx, 4xx, 5xx)<br/>env<br/>error (can have values internal, policy, backend) |
| sag_apigw_avg_latency               | This is a latency related metric. Measures the average time spent by all API invocations, which does not include the time spent in backend services, classified by its response code with the label 2xx, 3xx, 4xx or 5xx. | Gauge   | code (can have values 2xx, 3xx, 4xx, 5xx)<br/>env            |
| sag_apigw_avg_backend_response_time | This is a latency related metric. Measures the average time spent by the backend services for all API invocations, classified by its response code with the label 2xx, 3xx, 4xx, 5xx or connect. | Gauge   | code (can have values 2xx, 3xx, 4xx, 5xx)<br/>env            |
| sag_apigw_avg_response_time         | This is a latency related metric. Measures the average time spent by all API invocations, classified by its response code with the label 2xx, 3xx, 4xx or 5xx. This includes the time spent in API Gateway and by the backend services | Gauge   | code (can have values 2xx, 3xx, 4xx, 5xx)<br/>env            |

### 6. Resending failed API usage metrics

The agent is responsible for sending the failed metrics due to network or communication issues with Control Plane and/or API runtime. In case of Control Plane unavailability, the agent can store the metrics somewhere temporarily and when the Control Plane is up, it can send the metrics using the metrics REST API described above. The other approach is, it can store the last timestamp at which the metrics request was failed and when the Control Plane is up, it can retrieve the metrics accumulated in the runtime after that time and send to Control Plane. These are just a few approaches, however, the agent writer can come up with a more optimized solution to handle this scenario.
