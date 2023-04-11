# webMethods API Control Plane

APIs are everywhere. API Management is where APIs are. It’s all distributed and heterogenous. Management and administration becomes harder. API Management technology should blend in into the rest of the infrastructure and be managed from one place. API Control Plane is a single solution for understanding, managing and controlling the entire API Management landscape, regardless of where it is, on premises. Things in the control plane are organized across:

- Data planes - these are logical groupings of your gateways, portals and other runtimes dealing with APIs.
- Runtimes - there are your API Gateways, developer portals etc.
- APIs - that's self explanatory. These are APIs present in your landscape.

![image](/attachments/apicp_dashboard_page.png)

This repository hosts assets documenting how to deploy and run webMethods API Control Plane, articles and tutorials, public API docs and Postman collections whoing who to use them and more.

## Deployment
### HW and product configurations
API Control Plane consists of the following microservices.
- Asset Catalog
- Engine
- Ingress
- UI

As a standard tier, we will have two pods per microservice. The configuration for each microservice' pod will the following.
**Max RAM:** 512 MB
**Min RAM:** 512 MB
**CPU:** 0.5

**Elasticsearch configuration**
We will have a self-hosted Elasticsearch clusters with 3 nodes (Reused by Asset Catalog, Ingress (UMC) & Engine). Each ES node will have the following configuration.
**Max RAM:** 2 GB
**Min RAM:** 2 GB
**CPU:** 1
**Volume:** 300 GB (100KB * 1(per minute) * 60(per hour) * 24(per day) * 20(20 API GW pods) * 2(replicas)) - for retaining 45 days of data - comes to ~260GB.

The application level configuration for different microservices are available [here](deployment/docker/README.md).

### Health, monitoring, metrics and actions
Please refer this link for a detailed explanation [here](deployment/docker/README.md).
### High Availability
Please refer this link for a detailed explanation [here](deployment/docker/README.md).
### logging and tracing
- The logs are printed to the standard output by the containers/pods, the log information can be read through this.
- To perform debugging, API Control plane can be started in debug mode with Open telemetry enabled. Instructions to set up is available [here](deployment/docker/README.md#1-enabling-open-telemetry-using-jaeger).

### Data management and housekeeping
Please refer this link for a detailed explanation [here](deployment/docker/README.md).

## Using API Control Plane
### Control Plane setup
- We have two modes of deployment for API Control Plane
  - API Control Plane deployment with docker compose
  - API Control Plane deployment with helm
- For deployment instructions using docker compose go [here](deployment/docker/README.md).
- For deployment instructions using helm on kubernetes go [here](deployment/helm/README.md).

### Preparing API Gateway for connectivity
For instructions on how to configure API Gateway Agent connectivity with API Control Plane refer  [here](deployment/docker/README.md).

### Setting up Control plane
- Data planes are the logical groupings of the runtimes in the landscape. You can create data planes by grouping the runtimes based on the selection criteria. For creating data planes go [here](deployment/docker/README.md).

### Seeing the APIs and monitoring data in control plane
- To manage the list of API's using API Control Plane refer [here](deployment/docker/README.md).
- To manage the list of runtimes using API Control Plane refer [here](deployment/docker/README.md).
- To monitor different metrics using API Control Plane refer [here](deployment/docker/README.md).
## Public APIs

API Control Plane exposes public APIs to interact with it. API docs as well as postman collection with sample usage can be found [here](apis/README.md).

## Articles

We also host various articles related to API Control Plane deployment, use, administration etc. [here](articles/README.md).

***

These tools are provided as-is and without warranty or support. They do not constitute part of the Software AG product suite. Users are free to use, fork and modify them, subject to the license agreement. While Software AG welcomes contributions, we cannot guarantee to include every contribution in the master project.
