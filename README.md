# webMethods API Control Plane

APIs are everywhere. API Management is where APIs are. It’s all distributed and heterogenous. Management and administration becomes harder. API Management technology should blend in into the rest of the infrastructure and be managed from one place. API Control Plane is a single solution for understanding, managing and controlling the entire API Management landscape, regardless of where it is, on premises. Things in the control plane are organized across:

- Data planes - these are logical groupings of your gateways, portals and other runtimes dealing with APIs.
- Runtimes - there are your API Gateways, developer portals etc.
- APIs - that's self explanatory. These are APIs present in your landscape.

![image](/attachments/apico_dashboard_page.png)

This repository hosts assets documenting how to deploy and run webMethods API Control Plane, articles and tutorials, public API docs and Postman collections whoing who to use them and more.

For deployment instructions using docker compose go [here](install/helm/README.md).

For deployment instructions using helm on kubernetes go [here](install/docker/README.md).

Once you have API Control Plane deployed, you will want to connect your API Gateway runtimes to it. For instructions how to do it do [here](install/agent/README.md).

------------------------------

These tools are provided as-is and without warranty or support. They do not constitute part of the Software AG product suite. Users are free to use, fork and modify them, subject to the license agreement. While Software AG welcomes contributions, we cannot guarantee to include every contribution in the master project.
