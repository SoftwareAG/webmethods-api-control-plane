# IBM webMethods API Control Plane

APIs are everywhere. API Management is where APIs are. It’s all distributed and heterogeneous. Management and administration becomes harder. API Management technology should blend in into the rest of the infrastructure and be managed from one place. API Control Plane is a single solution for understanding, managing and controlling the entire API Management landscape, whether it's self-hosted, in private clouds, in a public cloud, or a hybrid combination of any of these. Assets in the API Control Plane are organized across:

- APIs - whereever they are deployed, however many versions they have
- Runtimes - API Gateways from any vendor, custom runtime implementations, or developer portals
- Data planes - Logical groupings of your runtimes. For example, certain cloud gateways, customer facing runtimes, geographical groupings

![image](/attachments/apicp_dashboard_page.png)

This repository hosts assets documenting how to deploy and run IBM webMethods API Control Plane as a container, articles and tutorials, public API docs and Postman collections. Please see the official documentation for [cloud](https://docs.webmethods.io/apicontrolplane/welcome/home/#gsc.tab=0) or [self-hosted](https://docs.webmethods.io/on-premises/webmethods-api-control-plane) for how to use the product and system requirements. 

## Deploying API Control Plane

We have two ways of deploying a self-hosted version of API Control Plane - using docker compose and using helm.

- For deployment instructions using Docker Compose, go [here](deployment/docker/README.md).
- For deployment instructions using Helm Charts on Kubernetes, go [here](deployment/helm/README.md).

## Connecting runtimes to the API Control Plane

API Control Plane provides native support for connecting with webMethods API Gateway and an agent SDK to connect with any runtime. We also provide some sample agents for third party API gateways. Please see [here](deployment/agent/) for detailed information about those.

## Public APIs

API Control Plane exposes public REST APIs to interact with it. API docs as well as Postman collection with samples can be found [here](apis).

## License for Control Plane Application
### Loading the license
- You need a valid license to user the API Control Plane.
- The docker composes and/or helm charts file needs to be modified to mount the license file.
- The API Control Plane will load the license file from the mounted path configured through the environment variable `APICP_LICENSE_PATH`. This will happen during application start-up.
- You can use the REST API to load or update the license file after startup. Please see [its OAS](https://github.com/SoftwareAG/webmethods-api-control-plane/blob/main/apis/openapi-specifications/license-management.yaml) for details.
- The license file may have either one of the two product codes specified below
  - WCOS1: for on-prem instances
  - WCOC1: for cloud instances
- This license information is validated in the API Control Plane for both the license types.

### Behavior without a valid license
- All the PUT, POST and DELETE API calls to the API Control Plane application will be blocked, and 406 HTTP status code 
  will be returned as a response code.

## Articles

We also host various articles related to API Control Plane deployment, use, administration etc. [here](articles).

## Useful links   

📘 Official Documentation

For the most up-to-date information about the product, see the official documentation for [cloud](https://docs.webmethods.io/saas/webmethods-api-control-plane#gsc.tab=0) or [self-hosted](https://docs.webmethods.io/on-premises/webmethods-api-control-plane/11.1.0/webhelp/index.html)

💻 Explore the Knowledge Base    

Dive into a wealth of webMethods tutorials and articles in our [Tech Community Knowledge Base](https://tech.forums.softwareag.com/tags/c/knowledge-base/6/webmethods).  

💡 Get Expert Answers    

Stuck or just curious? Ask the webMethods experts directly on our [Forum](https://tech.forums.softwareag.com/tags/c/forum/1/webMethods).  

🚀 Try webMethods    

See webMethods in action with a [Free Trial](https://techcommunity.softwareag.com/en_en/downloads.html).    

✍️ Share Your Feedback    

Your input drives our innovation. If you find a bug, please create an issue in the repository. If you’d like to share your ideas or feedback, please post them [here](https://tech.forums.softwareag.com/c/feedback/2).   

📺 YouTube Playlist

We upload videos on how to use the API Control Plane on our [YouTube playlist](https://www.youtube.com/watch?v=WAgRBN8rDVo&list=PL3HwmrSYjxiPGTgJZSR_B9faFGKEC2txA)
   
***

The tools in this GitHub repository are provided as-is and without warranty or support. They do not constitute part of the Software AG product suite. Users are free to use, fork and modify them, subject to the license agreement. While Software AG welcomes contributions, we cannot guarantee to include every contribution in the master project.
