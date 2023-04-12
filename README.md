# webMethods API Control Plane

APIs are everywhere. API Management is where APIs are. It’s all distributed and heterogenous. Management and administration becomes harder. API Management technology should blend in into the rest of the infrastructure and be managed from one place. API Control Plane is a single solution for understanding, managing and controlling the entire API Management landscape, regardless of where it is, on premises. Things in the control plane are organized across:

- Data planes - these are logical groupings of your gateways, portals and other runtimes dealing with APIs.
- Runtimes - there are your API Gateways, developer portals etc.
- APIs - that's self explanatory. These are APIs present in your landscape.

![image](/attachments/apicp_dashboard_page.png)

This repository hosts assets documenting how to deploy and run webMethods API Control Plane, articles and tutorials, public API docs and Postman collections whoing who to use them and more.

## Deployment
### Hardware and product configurations
API Control Plane consists of the following microservices.
- Asset Catalog
- Engine
- Ingress
- UI

As a standard tier, we will have two pods per microservice. Each microservice pod will have the following configurations.
 - **Max RAM:** 512 MB
 - **Min RAM:** 512 MB
 - **CPU:** 0.5

##### **Elasticsearch configuration**
We will have a 3 node self-hosted Elasticsearch cluster which is Reused by Asset Catalog, Ingress (UMC) & Engine. 
Each ES node will have the following configuration.
 - **Max RAM:** 2 GB
 - **Min RAM:** 2 GB
 - **CPU:** 1
 - **Volume:** 300 GB for retaining 45 days of data - [100KB * 1(per minute) * 60(per hour) * 24(per day) * 20(20 API GW pods) * 2(replicas) - comes to ~260GB]


### Health, monitoring, metrics and actions
API Control plane provides API's to check the liveness and readiness of microservices.
Please refer this link for a detailed explanation [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2F_api_cp_webhelp.1.23.html%23).
### High Availability
Please refer the high availability architecture section for a detailed explanation [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2Fco-deployment.html%23).
### logging and tracing
- The logs are printed to the standard output of the containers/pods, the log information can be read through this.
- To perform debugging, API Control plane can be started in debug mode with Open telemetry enabled. 
  Instructions to set up is available [here](deployment/docker/README.md#1-enabling-open-telemetry-using-jaeger).

### Data management and housekeeping
Data housekeeping refers to the process of regularly cleaning, organizing, and maintaining the data that the API Control Plane receives.
Please refer this link for a detailed explanation [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2F_api_cp_webhelp.1.19.html%23).

## Using API Control Plane
### Control Plane setup
- We have two modes of deployment for API Control Plane
  - API Control Plane deployment with docker compose
  - API Control Plane deployment with helm
- For deployment instructions using docker compose go [here](deployment/docker/README.md).
- For deployment instructions using helm on kubernetes go [here](deployment/helm/README.md).

### Preparing API Gateway for connectivity
For instructions to configure API Gateway Agent connectivity with API Control Plane refer  [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2Fco-agent.html%23).

### Setting up Control plane
- Data planes are the logical groupings of the runtimes in the landscape. You can create data planes by grouping the runtimes based on the selection criteria. 
  For the documentation to create and manage the Data Planes please refer [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2F_api_cp_webhelp.1.46.html%23).

### Seeing the APIs and monitoring data in control plane
- For instructions to manage the list of API's using API Control Plane refer [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2F_api_cp_webhelp.1.50.html%23).
- For instructions to manage the list of runtimes using API Control Plane refer [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2F_api_cp_webhelp.1.37.html%23).
- For instructions to monitor different metrics using API Control Plane refer [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2F_api_cp_webhelp.1.52.html%23).
## Public APIs

API Control Plane exposes public APIs to interact with it. API docs as well as postman collection with sample usage can be found [here](apis/README.md).

## Articles

We also host various articles related to API Control Plane deployment, use, administration etc. [here](articles/README.md).

***

These tools are provided as-is and without warranty or support. They do not constitute part of the Software AG product suite. Users are free to use, fork and modify them, subject to the license agreement. While Software AG welcomes contributions, we cannot guarantee to include every contribution in the master project.
