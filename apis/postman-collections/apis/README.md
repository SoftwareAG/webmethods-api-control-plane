   **How to use this postman collection?**
* Import postman collection apis.json.</br>
  apis.json contains all the external APIs in API Control plane.
  
  ***Folder structure of the collection***
  
  ```
  API Control Plane Postman Collections/
  ├── v1 (contains all the /v1 REST APIs)
  │   ├── API (contains REST APIs to create, read, update, and delete the APIs in API Control Plane)
  │   ├── Dataplanes (contains REST APIs to create, read, update, and delete the data planes in API Control Plane)
  │   ├── Runtimes (contains REST APIs to create, read, update, and delete the runtimes in API Control Plane)
  │   ├── Search (contains search and count query)
  │   └── UserManagement (contains REST APIs to create, read, update, and delete the users and user groups in API Control Plane)
  └── Get microservice build version and API controlplane product version(contains REST APIs to retrieve build version of each microservice and controlplane product version.)
  ```
  
* Update the collection variables as per your requirement.</br>

  | Collection Variable | Description                                                     |
  |---------------------|-----------------------------------------------------------------|
  | username            | username for basic authentication                               |
  | password            | password for basic authentication                               |
  | baseURL             | basic url Example- http://localhost:8080                        |
  | runtimeId           | ID of the runtime for which you want to invoke the REST API     |
  | apiId               | ID of the API for which you want to invoke the REST API         |
  | dataplaneId         | ID of the data plane for which you want to invoke the REST API   |
  | userId              | ID of the user for which you want to invoke the REST API        |
  | groupId             | ID of the group for which you want to invoke the REST API       |

* Provide the corresponding value to the collection variables and invoke the respective REST API.
