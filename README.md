# Solution - Use Cases
Use Case API sets that can be layered into Ping Server Profiles

This repo provides Postman collections that can be applied to Server Profile stacks, or standalone servers. They do not contain files that need to be deployed into the containers.

---
# Use Cases
API collections that modify Server Profiles to do other things

## PingFederate
* [PF - Simple Config](docs/pf-simple)

## PingAccess
* [PA Proxy - ACME Certificate](docs/pa-proxy)
* [PA WAM with PF](docs/pa-wam-with-pf)

---
# Deployment
**Pre-requisites**  
These collections are designed to be deployed onto Ping Software products.   
How the servers are deployed shouldn't matter.

If a collection requires a Product or IK before you run it, it will be listed in the Use Case documentation.

These use cases can be deployed in several ways:
* Postman --> Manual or Collection Runner
* Docker --> `docker run postman/newman ...`
* Compose \ K8s --> Service definitions (`yaml`)
---