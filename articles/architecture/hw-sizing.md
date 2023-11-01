# Hardware sizing

API Control Plane product consists of the following microservices:

- Asset Catalog
- Engine
- Ingress
- UI

## Product components

The sample helm deployment scripts are configured to have 2 pods per microservice. 

Following microservices pod will have the following configurations:
- Asset Catalog
- Engine
- UI
  - Min RAM: 512 MB
  - Max RAM: 512 MB
  - CPU: 0.5

Ingress microservice pod will have the following configuration
  - Min RAM: 512 MB
  - Max RAM: 1024 MB
  - CPU: 0.5

The sample docker compose scripts are configure to use 1 replica per microservice.

## Data storage

API Control Plane also uses elasticsearch for data storage. The sample helm deployment scripts and docker compose deployment scipts are configured to have 1 elasticsearch instance. This instance is used by Asset Catalog, Ingress & Engine.

Each elasticsearch node will have the following configuration.

- Min RAM: 2 GB
- Max RAM: 2 GB
- CPU: 1
- Volume: 300 GB for retaining 45 days of data - [100 KB * 1(per minute) * 60(per hour) * 24(per day) * 20(20 API GW pods) * 2(replicas) - comes to ~260GB]
