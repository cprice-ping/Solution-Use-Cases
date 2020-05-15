## PA with ACME-Managed cert for PF / PD
Server Profile: N/A

Often theres a need to have certificates that are signed by a public CA. Things like Delegator or PingCentral use the OIDC Issuer for both backchannel calls (Metadata and token validation) and frontchannel (Application rendering and API calls). Not all products allow insecure TLS connections.   

PingAccess v6 included a feature to generate KeyPairs and have them managed with the ACME protocol and the Let's Encrypt service. This capability makes PA a nice way to include a proxy to a configuration to enable trusted certificates in front of other services.

**Pre-Requisites**
* PingFederate instance ([PF - Simple](../pf-simple))
* PingAccess software \ license
* Postman
  * [Environment](./postman_vars.json)
 
**Postman Collection**
 * [Documentation](https://documenter.getpostman.com/view/1239082/SWT5jLpF)
---

## Deployment
**Postman Environment**
* Modify Postman Environment
  * [Sample](./postman_vars.json)
  * Add your PA hostname \ IP
---

**Software Only**
* Install PingAccess
* Execute the [Postman Collection](./postman-collection.md)
---

**Docker**
* Install Docker
* Run PingAccess in a container
```
docker run -e "PING_IDENTITY_ACCEPT_EULA=YES" -v ${PWD}/pingaccess.lic:/opt/in/instance/conf/pingaccess.lic pingidentity/pingaccess
```
* Execute the [Postman Collection](./postman-collection.md)
---
**Docker Compose (License file)**
* Install Docker
* Install Docker Compose
* Create `docker-compose.yaml`
  * **Note:** This yaml will persist the configuration
```
version: "3.5"

services:
  pingaccess:
    image: pingidentity/pingaccess:latest
    environment:
      - PING_IDENTITY_ACCEPT_EULA=YES
    volumes:
      - ./pingaccess.lic:/opt/in/instance/conf/pingaccess.lic       
      - ./pingaccess:/opt/out/instance

  pingconfig:
    image: pricecs/pingconfigurator:latest
    environment:
      - COLLECTIONS=https://www.getpostman.com/collections/eaa397bd3a35ef3095c1
    volumes: 
    # An environment file should be injected into the image - this file should contain your specfic info and secrets
      - ./postman_vars.json:/usr/src/app/postman_vars.json
```
* run `docker-compose up -d`
---

**Docker Compose (Ping DevOps Key)**
* Install Docker
* Install Docker Compose
* Configure [Ping DevOps](https://github.com/pingidentity/pingidentity-devops-getting-started/blob/master/docs/getStarted.md)
* Create `docker-compose.yaml`
```
version: "3.5"

services:
  pingaccess:
    image: pingidentity/pingaccess:latest
    secrets:
      - devops-secret
    environment:
      - PING_IDENTITY_ACCEPT_EULA=YES
    volumes:       
      - ./pingaccess:/opt/out/instance
    ports:
      - 443:3000
      - 9000:9000

  pingconfig:
    image: pricecs/pingconfigurator:latest
    environment:
      - COLLECTIONS=https://www.getpostman.com/collections/eaa397bd3a35ef3095c1
    volumes: 
    # An environment file should be injected into the image - this file should contain your specfic info and secrets
      - ./postman_vars.json:/usr/src/app/postman_vars.json

secrets:
  devops-secret:
    file: ${HOME}/.pingidentity/devops
```
* run `docker-compose up -d`
---