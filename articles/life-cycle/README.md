## All about API Control Plane

This section serves as a comprehensive guide for understanding the life cycle of API Control Plane.

1. <b>What is IBM webMethods API Control Plane?</b>

   API Control Plane is a centralized control and monitoring system that oversees and manages multiple runtimes such as <b>API Gateway</b>, <b>Developer Portal</b>, <b>Microgateway</b>, and <b>other runtimes</b> deployed across multiple
   regions including cloud and on-premises environments. With API Control Plane, you can achieve comprehensive real-time monitoring, leveraging advanced analytics, and customizable dashboards to gain valuable
   insights from your data to make data-driven decisions and improvements in your organization.
   <br><br>
2. <b>What are the deployment variants of API Control Plane?</b>

    - **Cloud**
        - *```SAAS Cloud:```* Software AG hosts and manages API Control Plane on a cloud-based Software as a Service (SaaS) infrastructure. Opting for this fully managed deployment is the recommended choice for ease of
          management and scalability.
        - *```Private Cloud:```* If you prefer to deploy API Control Plane in private cloud environment, you have the flexibility to choose between running the application in docker containers or kubernetes.
    - **On-premises**. You can deploy API Control Plane using docker or helm.
      <br><br>
3. <b>Where is API Control Plane Cloud hosted?</b>

   Software AG provisions API Control Plane on the Software AG Cloud platform exclusively in the *<b>Azure West Europe</b>* region (The Netherlands).
   <br><br>
4. <b>How can you request for a new API Control Plane tenant?</b>

   webMethods.io API Control Plane is available only for enterprise customers. webMethods.io API Control Plane tenants are provisioned to enterprise customers upon request. <b>Contact Software AG support if you wish
   to subscribe to API Control Plane.</b>
   <br><br>
5. <b>What is the process for upgrading an API Control Plane cloud tenant to a new version?</b>

   API Control Plane tenants are automatically upgraded to the latest version by Software AG.
   <br><br>
6. <b>What is the downtime for the upgrade process?</b>

   No downtime is needed.
   <br><br>
7. <b>How are the upgrade notifications communicated to the customers?</b>

   Upgrade notifications are sent through the [Status page](https://status.webmethods.io/) and e-mails. The status page has information on major releases, hotfixes, and patch releases including the latest
   statistics on cloud environments availability, incident history, and planned maintenance events.
   <br><br>
8. <b>What runtimes are supported?</b>

   API Control Plane currently supports connectivity with webMethods API Gateway (version 10.15 Fix 10 and above).  
   <br><br>
9. <b>Can the domain name of API Control Plane tenant be customized?</b>

   This is an upcoming feature. It will be supported from API Control Plane 11.0.5 onwards. Create a support ticket for a custom domain.
   <br><br>
10. <b>How are the certificates managed?</b>

    Software AG Cloud Ops handles the certificate management. Create a support ticket if you have a requirement.
    <br><br>
11. <b>How are the users managed?</b>

    **User groups in API Control Plane**

    The following predefined user groups can access API Control Plane:

    -	API Control Plane administrators
    -	API platform providers
    -	API product managers

    For details on the privileges based on the user groups, see https://docs.webmethods.io/apicontrolplane/administration/chapter3wco#co-user_management
    <br><br>
12. <b>What is the deployment model of API Control Plane?</b>

    API Control Plane can be deployed using High availability solution for protection against single point of failure within a single data center or HADR (High Availability and Disaster Recovery) solution for
    protection from the failure of an entire data center.
    <br><br>
13. <b>How to deploy the on-prem version of API Control Plane?</b>

    API Control Plane can be deployed using:

    - Docker: For details, see https://docs.webmethods.io/on-premises/webmethods-api-control-plane/11.1.0/webhelp/index.html

    - Helm: For details, see https://docs.webmethods.io/on-premises/webmethods-api-control-plane/11.1.0/webhelp/index.html#page/wco-webhelp%2Fta-deploy_standalone_apicp_docker.html
      <br><br>
14. <b>How to connect API Gateway with API Control Plane?</b>

    To connect API Control Plane using API Gateway, see https://docs.webmethods.io/on-premises/webmethods-api-control-plane/11.1.0/webhelp/index.html#page/wco-webhelp%2Fco-connecting-apigw.html

    <br><br>
15. <b>What are the hardware and product configuration guidelines that are required to deploy API Control Plane to run at an optimal scale?</b>

    For details, see https://docs.webmethods.io/on-premises/webmethods-api-control-plane/11.1.0/webhelp/index.html#page/wco-webhelp%2Fre-resourcing_guidelines.html
    <br><br>
16. <b>How to manage data backups/snapshot to ensure data resiliency and disaster recovery?</b>

    For details, see https://docs.webmethods.io/on-premises/webmethods-api-control-plane/11.1.0/webhelp/index.html#page/wco-webhelp%2Fco-snapshot_management.html
    <br><br>
17. <b>Does API Control Plane provide REST endpoints to monitor the health and resource utilization of the microservices and Elastic search?</b>

    Yes. For details about how to monitor microservices health and resource utilization, see https://docs.webmethods.io/on-premises/webmethods-api-control-plane/11.1.0/webhelp/index.html#page/wco-webhelp%2Fco-microservices_application_level.html

    For details about how to monitor Elastic search health and resource utilization, see https://docs.webmethods.io/on-premises/webmethods-api-control-plane/11.1.0/webhelp/index.html#page/wco-webhelp%2Fco-elasticsearch_monitoring.html
    <br><br>
18. <b>List the Prometheus metrics to analyze API Control Plane health.</b>

    For details, see https://docs.webmethods.io/on-premises/webmethods-api-control-plane/11.1.0/webhelp/index.html#page/wco-webhelp%2Fco-collect_microservices_metrics.html
    <br><br>
19. <b>Is Open telemetry supported for tracing?</b>

    Yes. To deploy API Control Plane enabling Open Telemetry using Jaeger UI with Docker, perform *step 5* mentioned in
   https://docs.webmethods.io/on-premises/webmethods-api-control-plane/11.1.0/webhelp/index.html#page/wco-webhelp%2Fco-collect_microservices_metrics.html
    <br><br>
20. <b>How do I report an incident?</b>

    You can report an incident through our Service Portal, [JSM](https://getsupport.softwareag.com/)

## References

* Official On-prem documentation link: https://docs.webmethods.io/on-premises/webmethods-api-control-plane
* Official Cloud documentation link: https://docs.webmethods.io/saas/webmethods-api-control-plane 
