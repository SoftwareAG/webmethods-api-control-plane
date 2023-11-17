# Writing Runtime agent for connecting to API Control Plane
This article describes the details about writing an agent for connecting a Runtime to API Control Plane.

## Introduction

The article explains the details about writing an agent for API runtimes to connect to API Control Plane.

## Required agent functionalities

For an agent to connect to API Control Plane successfully and be part of API Control Plane landscape, the following functionalities should be supported by it.

- The agent should register the API runtime to API Control Plane at runtime startup.
- The agent should send the API runtime status as heartbeats to API Control Plane to know about its status.
- The agent should publish the assets/resources from the API runtime to API Control Plane in a compatible/required format.
- The agent should be responsible for keeping its assets in API Control Plane to be in sync.
- The agent should send the API usage metrics to API Control Plane in a compatible/required format.
- The agent should be responsible for resending the metrics which are lost when communication to API Control Plane or API runtime is failed due to server unavailability or license expiration.

## Steps to write the agent implementation

Follow the below steps to write the agent to make the API runtime to be part of API Control Plane landscape. API Control Plane provides REST APIs for its assets management and receiving metrics. The agent needs to make use of those APIs for accomplishing all the required functionalities. Refer the Open API documentation present [here](../../apis/openapi-specifications) for detailed information.

### 1. Registering the API runtime

As a first step, the agent should register the API runtime to API Control Plane for making it as member in the API Control Plane runtimes landscape. Use the below REST API for registering the API runtime. Please refer [this](../../apis/openapi-specifications/runtime-management.yaml) Open API spec for detailed explanation of each property.

For e.g.

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

The `heartBeatSynchInterval` refers to the time interval to be used by the agent for sending the heartbeat to API Control Plane periodically.

### 2. Sending heartbeats

Once the API runtime is registered with API Control Plane, the agent should start sending the status of the Runtime periodically so that its status can be visualized in Control Plane landscape. Use the below REST API for sending the heartbeat. Please refer [this](../../apis/openapi-specifications/controlplane-metrics.yaml) Open API spec for detailed explanation of each property.

For e.g.

```
POST /api/engine/v1/runtimes/heartbeats
{
	"runtimeId": "AmazonAPIGatewayWestUS"
	"active": 1,
	"created": "2022-07-14 14:25:17:116"
}
```

The agent should run a scheduled task which would run periodically at `heartBeatSynchInterval` interval seconds to send the heartbeats.

### 3. Publishing runtime assets

After the agent is started an made a connection to API Control Place, it should retrieve the assets from the API Runtime and send to Control Plane at once for them to be visualized in the API landscape. As of now the Control Plane has support for only API assets. Agent should take care of converting the Runtime API assets into Control Plane API format to be sent to Control Plane. Use the below REST API for sending the API asset. Please refer [this](../../apis/openapi-specifications/api-management.yaml) Open API spec for detailed explanation of each property.

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

In case the asset is already available at Control Plane, PUT method should be called to update the asset.

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

After publishing the runtime assets in Control Plane, the agent has the responsibility of keeping the runtime assets in sync with Control Plane for any future asset/resource updates in the API runtime. For helping this, the Control Plane has a REST API for storing the asset sync timestamp as an audit event. After the assets are published or synced to Control Plane, the agent would need to call this API for updating the timestamp for each asset type. The agent would then run a scheduled task to periodically check for any asset updates in the runtime that might happened after that last sync timestamps stored in Control Plane. There is a REST API in Control Plane to get the audit events for each asset type which contains the last sync timestamp.

#### Storing the asset sync timestamp

For e.g. to store the asset sync timestamp in API Control Plane for API asset (as of now only API is the supported runtime asset),

```json
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

Example REST API request to retrieve the last sync timestamp of API asset type,

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

The agent would use the asset sync timestamp retrieved from Control Plane to get the updated assets from the API runtime after that time. Then those assets would be converted into the respective Control Plane asset format and synced with Control Plane using the asset management REST APIs provided. Since the only supported asset type is API, please refer [this](../../apis/openapi-specifications/api-management.yaml) Open API spec documentation for more details on the management of API asset in Control Plane.

After this, the asset sync timestamp of all the asset types should be stored again in Control Plane for future assets sync operation.

### 5. Sending API usage metrics



### 6. Resending failed API usage metrics
