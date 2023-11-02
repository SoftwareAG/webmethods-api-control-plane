This folder contains postman collection to create a new admin user, platform provider, and product manager. This folder also contains postman collection for some searchAPI related use-cases.

**How to use this postman collection?**

* Import the postman collection that you are interested in.
  administrator_creation.json collection can be used to create a new administrator.
  platform_provider_creation.json can be used to create a new Platform Provider.
  product_manager_creation.json can be used to create a new Product Manager.
  search.json contains some searchAPI related use-cases.
  
* Update the collection variables as per your Requirement.

  | Collection Variable | Description                                                  |
  | ------------------- | ------------------------------------------------------------ |
  | username            | username for basic authentication                            |
  | password            | password for basic authentication                            |
  | baseURL             | basic url Eg- http://localhost:8080                          |
  | uuid                | uuid of the newly created user. No need to update this variable. This can be ignored. |
  
* Provide the corresponding value to the collection variables and run the collection using postman runner.
