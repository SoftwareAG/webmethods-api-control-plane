## ⚠️ Please note that API Control Plane is compatible with API Gateway version 10.15 fix 8 and above.</span>

# webMethods API Control Plane

APIs are everywhere. API Management is where APIs are. It’s all distributed and heterogenous. Management and administration becomes harder. API Management technology should blend in into the rest of the infrastructure and be managed from one place. API Control Plane is a single solution for understanding, managing and controlling the entire API Management landscape, regardless of where it is, on premises or in the cloud. Things in the API Control Plane are organized across:

- Data planes - these are logical groupings of your gateways, portals and other runtimes dealing with APIs.
- Runtimes - there are your API Gateways, developer portals etc.
- APIs - that's self explanatory. These are APIs present in your landscape.

![image](/attachments/apicp_dashboard_page.png)

This repository hosts assets documenting how to deploy and run webMethods API Control Plane, articles and tutorials, public API docs and Postman collections whoing who to use them and more.

## Deploying API Control Plane

We have two ways of deploying a self-hosted version of API Control Plane - using docker compose and using helm.

- For deployment instructions using docker compose go [here](deployment/docker/README.md).
- For deployment instructions using helm on kubernetes go [here](deployment/helm/README.md).

## Public APIs

API Control Plane exposes public APIs to interact with it. API docs as well as postman collection with sample usage can be found [here](apis).

## Articles

We also host various articles related to API Control Plane deployment, use, administration etc. [here](articles).

***

These tools are provided as-is and without warranty or support. They do not constitute part of the Software AG product suite. Users are free to use, fork and modify them, subject to the license agreement. While Software AG welcomes contributions, we cannot guarantee to include every contribution in the master project.
