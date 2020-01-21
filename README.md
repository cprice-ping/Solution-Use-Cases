# Solution - Use Cases
Use Case API sets that can be layered into Ping Server Profiles

This repo provides Postman collections that can be applied to Server Profile stacks, or standalone servers. They do not contain files that need to be deployed into the containers.

---
# Deployment
These use cases can be deployed in several ways:
* Postman --> Manual or Collection Runner
* Docker --> `docker run postman/newman ...`
* Compose / K8s --> Service definition

---
# Use Cases
API collections that modify Server Profiles to do other things
## PA with ACME-Managed cert for PF / PD
Often theres a need to have certificates that are signed by a public CA. Things like Delegator or PingCentral use the OIDC Issuer for both backchannel calls (Metadata and token validation) and frontchannel (Application rendering and API calls). not all products allow insecure TLS connections.  

PingAccess v6 included a feature to generate KeyPairs and have them managed with the ACME protocol and the Let's Encrypt service. This capability makes PA a nice way to include a proxy to a configuration to enable trusted certificates in fron of other services.
