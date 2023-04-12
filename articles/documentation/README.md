# Product documentation

Links provided here will take you to a :construction: work-in-progress :construction: product documentation.

***

Table of contents

1. [Health, monitoring, metrics and actions](#health-monitoring-metrics-and-actions)
2. [High Availability](#high-availability)
3. [Logging and tracing](#logging-and-tracing)
4. [Data management and housekeeping](#data-management-and-housekeeping)
5. [Configuring API Gateway to connect to API Control Plane](#configuring-api-gateway-to-connect-to-api-control-plane)
6. [Data planes in API Control Plane](#data-planes-in-api-control-plane)
7. [API Catalog in API Control Plane](#api-catalog-in-api-control-plane)
8. [Runtimes in API Control Plane](#runtimes-in-api-control-plane)
9. [Analytical dashboard](#analytical-dashboard)

***

### Health, monitoring, metrics and actions

API Control Plane provides APIs for checking the liveness and readiness of its microservices.
For detailed explanation go [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2F_api_cp_webhelp.1.23.html%23).

###### [Back to Top](#product-documentation)
***

### High Availability

If you're interested in high availability architecture topics, go [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2Fco-deployment.html%23).

###### [Back to Top](#product-documentation)
***

### Logging and tracing

API Control Plane logs are sent to the standard output (stdout) of the containers/pods and can be accessed with stardard docker / kubernetes tooling.

For debugging purposes, API Control Plane can be started in debug mode with open telemetry enabled. Instructions to set it up are available [here](deployment/docker/README.md#1-enabling-open-telemetry-using-jaeger).

###### [Back to Top](#product-documentation)
***

### Data management and housekeeping

Data housekeeping refers to the process of regularly cleaning, organizing, and maintaining the data that the API Control Plane deals with.
Please go [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2F_api_cp_webhelp.1.19.html%23) for a detailed explanation.

###### [Back to Top](#product-documentation)
***

### Configuring API Gateway to connect to API Control Plane

For instructions to configure API Gateway Agent connectivity with API Control Plane go [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2Fco-agent.html%23).

###### [Back to Top](#product-documentation)
***

### Data planes in API Control Plane

Data planes are the logical groupings of runtimes in the API Management landscape. You can create data planes by grouping runtimes based on the variosu criteria.
  
For the documentation to create and manage the Data planes please go [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2F_api_cp_webhelp.1.46.html%23).

###### [Back to Top](#product-documentation)
***

### API Catalog in API Control Plane

API Control Plane provides a single catalog for all APIs deployed acroos runtimes in the entire API Management landscape. For instructions for how to manage the list of APIs using API Control Plane go [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2F_api_cp_webhelp.1.50.html%23).

###### [Back to Top](#product-documentation)
***

### Runtimes in API Control Plane

For instructions to manage the list of runtimes using API Control Plane go [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2F_api_cp_webhelp.1.37.html%23).

###### [Back to Top](#product-documentation)
***

### Analytical dashboard

Documentation for analytical dashboard and the metrics it shows in API Control Plane go [here](https://documentation.softwareag.com/webmethods/api_control_plane/wco10-15/webhelp/wco-webhelp/index.html#page/wco-webhelp%2F_api_cp_webhelp.1.52.html%23).

###### [Back to Top](#product-documentation)
***
