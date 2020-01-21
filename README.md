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
Often theres a need to have certificates that are signed by a public CA. Things like Delegator or PingCentral use the OIDC Issuer for both backchannel calls (Metadata and token validation) and frontchannel (Application rendering and API calls). Not all products allow insecure TLS connections.   

PingAccess v6 included a feature to generate KeyPairs and have them managed with the ACME protocol and the Let's Encrypt service. This capability makes PA a nice way to include a proxy to a configuration to enable trusted certificates in front of other services.
 
[Documentation](https://documenter.getpostman.com/view/1239082/SWT5jLpF)

### Deployment
**Postman Collection**  

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/eaa397bd3a35ef3095c1)

**Postman Newman**  
`newman run https://www.getpostman.com/collections/eaa397bd3a35ef3095c1 -e postman_vars.json --insecure --nofollow-redirects`

**Newman - Docker**  
`docker run postman/newman run https://www.getpostman.com/collections/eaa397bd3a35ef3095c1 -e postman_vars.json --insecure --nofollow-redirects -v ./postman_vars.json:/etc/newman/postman_vars.json`

**YAML**
```
pa-config-proxy:
    image: postman/newman
    command: run https://www.getpostman.com/collections/eaa397bd3a35ef3095c1 -e postman-pa.json --insecure --ignore-redirects
    volumes:
        # An environment file should be injected into the image - this file should contain your specfic info and secrets
        - ./postman-pa.json:/etc/newman/postman-pa.json
```
---