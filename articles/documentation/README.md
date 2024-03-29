# Product documentation

Links provided here will take you to a section of [API Control Plane documentation page](https://documentation.softwareag.com/wco/10.16.0/en/webhelp/wco-webhelp/).

***

Table of contents

1. Administration of the API Control Plane
   - [Health, monitoring, metrics and actions](#health-monitoring-metrics-and-actions)
   - [High Availability](#high-availability)
   - [Logging and tracing](#logging-and-tracing)
   - [Data management and housekeeping](#data-management-and-housekeeping)
2. Configuration of the API Control Plane
   - [Deploying API Control Plane Application](#deploying-api-control-plane) 
   - [Configuring API Gateway to connect to API Control Plane](#configuring-api-gateway-to-connect-to-api-control-plane)
3. Using API Control Plane Application.
   - [Data planes in API Control Plane](#data-planes-in-api-control-plane)
   - [API Catalog in API Control Plane](#api-catalog-in-api-control-plane)
   - [Runtimes in API Control Plane](#runtimes-in-api-control-plane)
   - [Analytical dashboard](#analytical-dashboard)

***

### Health, monitoring, metrics and actions

API Control Plane provides APIs for checking the liveness and readiness of its microservices.
For detailed explanation go [here](https://documentation.softwareag.com/wco/10.16.0/en/webhelp/wco-webhelp/#page/wco-webhelp%2Fco-microservices_application_level.html%23).

###### [Back to Top](#product-documentation)
***

### High Availability

If you're interested in high availability architecture topics, go [here](https://documentation.softwareag.com/wco/10.16.0/en/webhelp/wco-webhelp/#page/wco-webhelp%2F_api_cp_webhelp.1.008.html%23).

###### [Back to Top](#product-documentation)
***

### Logging and tracing

API Control Plane logs are sent to the standard output (stdout) of the containers/pods and can be accessed with standard docker / kubernetes tooling.

For debugging purposes, API Control Plane can be started in debug mode with open telemetry enabled. Instructions to set it up, please refer to **Enabling Open Telemetry using Jaeger** section specified in [here](../../deployment/docker/README.md).

###### [Back to Top](#product-documentation)
***

### Data management and housekeeping

Data housekeeping refers to the process of regularly cleaning, organizing, and maintaining the data that the API Control Plane deals with.
Please go [here](https://documentation.softwareag.com/wco/10.16.0/en/webhelp/wco-webhelp/#page/wco-webhelp%2Fco-data_mgmt.html%23) for a detailed explanation.

###### [Back to Top](#product-documentation)
***

### Deploying API Control Plane

Refer [here](../../README.md#deploying-api-control-plane) to deploy the API Control Plane application.

###### [Back to Top](#product-documentation)
***


### Configuring API Gateway to connect to API Control Plane

> **Note:** To configure webMethods API Gateway to connect to API Control Plane, API Gateway must be in version **10.15.0.8 and above**. 

The instructions to configure API Gateway connectivity with API Control Plane can be found [here](../../deployment/agent/webmethods-api-gateway/README.md).

You can also refer  [here](https://documentation.softwareag.com/wco/10.16.0/en/webhelp/wco-webhelp/#page/wco-webhelp%2Fco-agent.html%23) for a detailed document on connecting API Gateway to API Control Plane.

###### [Back to Top](#product-documentation)
***

### Data planes in API Control Plane

Data planes are the logical groupings of runtimes in the API Management landscape. You can create data planes by grouping runtimes based on the various criteria.
  
For the documentation to create and manage the Data planes please go [here](https://documentation.softwareag.com/wco/10.16.0/en/webhelp/wco-webhelp/#page/wco-webhelp%2F_api_cp_webhelp.1.085.html%23).

###### [Back to Top](#product-documentation)
***

### API Catalog in API Control Plane

API Control Plane provides a single catalog for all APIs deployed across runtimes in the entire API Management landscape. For instructions for how to manage the list of APIs using API Control Plane go [here](https://documentation.softwareag.com/wco/10.16.0/en/webhelp/wco-webhelp/#page/wco-webhelp%2Fco-manage_apis.html%23wwconnect_header).

###### [Back to Top](#product-documentation)
***

### Runtimes in API Control Plane

For instructions to manage the list of runtimes using API Control Plane go [here](https://documentation.softwareag.com/wco/10.16.0/en/webhelp/wco-webhelp/#page/wco-webhelp%2F_api_cp_webhelp.1.068.html%23).

###### [Back to Top](#product-documentation)
***

### Analytical dashboard

Documentation for analytical dashboard and the metrics it shows in API Control Plane go [here](https://documentation.softwareag.com/wco/10.16.0/en/webhelp/wco-webhelp/#page/wco-webhelp%2Fco-dashboard.html%23).

###### [Back to Top](#product-documentation)
***
