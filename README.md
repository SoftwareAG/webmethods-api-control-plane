## ⚠️ Please note that API Control Plane is compatible with API Gateway version 10.15.0.8 and above.</span>

# webMethods API Control Plane

APIs are everywhere. API Management is where APIs are. It’s all distributed and heterogeneous. Management and administration becomes harder. API Management technology should blend in into the rest of the infrastructure and be managed from one place. API Control Plane is a single solution for understanding, managing and controlling the entire API Management landscape, whether it's self-hosted, in private clouds, in a public cloud, or a hybrid combination of any of these. Assets in the API Control Plane are organized across:

- APIs - whereever they are deployed, however many versions they have
- Runtimes - API Gateways from any vendor, custom runtime implementations, or developer portals
- Data planes - Logical groupings of your runtimes. For example, certain cloud gateways, customer facing runtimes, geographical groupings

![image](/attachments/apicp_dashboard_page.png)

This repository hosts assets documenting how to deploy and run webMethods API Control Plane, articles and tutorials, public API docs and Postman collections wooing who to use them and more.

## Deploying API Control Plane

We have two ways of deploying a self-hosted version of API Control Plane - using docker compose and using helm.

- For deployment instructions using docker compose go [here](deployment/docker/README.md).
- For deployment instructions using helm on kubernetes go [here](deployment/helm/README.md).

## Public APIs

API Control Plane exposes public APIs to interact with it. API docs as well as Postman collection with samples can be found [here](apis).

## License for Control Plane Application
### Loading license to Application
- A valid license file is to be mounted.
- The docker composes and/or helm charts file is to be modified to mount the license file.
- The API Control Plane will load the license file from the mounted path configured through the environment variable `APICP_LICENSE_PATH`. This will happen during application start-up.
- The other way to load the license after startup is by using the rest endpoints. Please to the Open API spec for the details.
- The license file may have either one of the two product codes specified below
  - WCOS1: for on-prem instances
  - WCOC1: for cloud instances
- This license information is validated in the API Control Plane for both the license types.

### Application Behaviour without a valid license
- All the PUT, POST and DELETE API calls to the API Control Plane application will be blocked, and 406 HTTP status code 
  will be returned as a response code.

## Agent SDK

The Java based agent SDK can be used to develop a Java application that can fetch runtime details and metrics 
information from Runtime and send to the API Control Plane. A detailed document is available [here](https://documentation.softwareag.com/wco/11.0.0/en/webhelp/wco-webhelp/#page/wco-webhelp%2Fco-agent_sdk.html).

## Articles

We also host various articles related to API Control Plane deployment, use, administration etc. [here](articles).

## Useful links   

📘 Explore the Knowledge Base    
Dive into a wealth of webMethods tutorials and articles in our [Tech Community Knowledge Base](https://tech.forums.softwareag.com/tags/c/knowledge-base/6/webmethods).  

💡 Get Expert Answers    
Stuck or just curious? Ask the webMethods experts directly on our [Forum](https://tech.forums.softwareag.com/tags/c/forum/1/webMethods).  

🚀 Try webMethods    
See webMethods in action with a [Free Trial](https://techcommunity.softwareag.com/en_en/downloads.html).    

✍️ Share Your Feedback    
Your input drives our innovation. If you find a bug, please create an issue in the repository. If you’d like to share your ideas or feedback, please post them [here](https://tech.forums.softwareag.com/c/feedback/2).   

More to discover 
* [Not your grandpa’s API Management | IUG 2023](https://tech.forums.softwareag.com/t/not-your-grandpa-s-api-management-iug-2023/283800)  
* [What is Develop Anywhere, Deploy Anywhere?](https://tech.forums.softwareag.com/t/what-is-develop-anywhere-deploy-anywhere/284756)
* [API Control Plane YouTube playlist](https://www.youtube.com/watch?v=WAgRBN8rDVo&list=PL3HwmrSYjxiPGTgJZSR_B9faFGKEC2txA)
   
***

The tools in this GitHub repository are provided as-is and without warranty or support. They do not constitute part of the Software AG product suite. Users are free to use, fork and modify them, subject to the license agreement. While Software AG welcomes contributions, we cannot guarantee to include every contribution in the master project.
