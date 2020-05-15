## PA WAM with PF
Server Profile: N/A

Frequently, we implement PingAccess as an Access Mananger using PingFed as the OIDC Token Provider. Setting this up can take a little time.

This collection will do the wiring between a PF OIDC Client (`PingAccess`) and PA, and will also add:
* Default Web Session
* Default Identity Mapping (JWT)
* Sample App (https://httpbin.org/anything)

**Pre-Requisites**
* PingFederate instance ([PF - Simple](../pf-simple))
* PingAccess software \ license
* Postman
  * [Environment](./postman_vars.json)
* [Use Case: PA as a Proxy](../pa-proxy)
 
**Postman Collection** 
* [Documentation](https://documenter.getpostman.com/view/1239082/Szmk1Frq)
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
      - COLLECTIONS=https://www.getpostman.com/collections/3db252f80d56599a5f46
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
      - 9031:9031
      - 9999:9999

  pingconfig:
    image: pricecs/pingconfigurator:latest
    environment:
      - COLLECTIONS=https://www.getpostman.com/collections/3db252f80d56599a5f46
    volumes: 
    # An environment file should be injected into the image - this file should contain your specfic info and secrets
      - ./postman_vars.json:/usr/src/app/postman_vars.json

secrets:
  devops-secret:
    file: ${HOME}/.pingidentity/devops
```
* run `docker-compose up -d`
---

**Docker Compose - PF & PA (Ping DevOps Key)**
* Install Docker
* Install Docker Compose
* Configure [Ping DevOps](https://github.com/pingidentity/pingidentity-devops-getting-started/blob/master/docs/getStarted.md)
* Create `docker-compose.yaml`
```
version: "3.5"

services:
  pingfederate:
    image: pingidentity/pingfederate:latest
    secrets:
      - devops-secret
    environment:
      - PING_IDENTITY_ACCEPT_EULA=YES
#    volumes:       
#      - ./pingfederate:/opt/out/instance
    ports:
      - 9031:9031
      - 9999:9999

  pingaccess:
    image: pingidentity/pingaccess:latest
    secrets:
      - devops-secret
    environment:
      - PING_IDENTITY_ACCEPT_EULA=YES
#    volumes:       
#      - ./pingaccess:/opt/out/instance
    ports:
      - 443:3000
      - 9000:9000
  pingconfig:
    image: pricecs/pingconfigurator:latest
    environment:
      - COLLECTIONS=https://www.getpostman.com/collections/33ce6e21b3c94585e33e,https://www.getpostman.com/collections/eaa397bd3a35ef3095c1,https://www.getpostman.com/collections/3db252f80d56599a5f46
    volumes: 
    # An environment file should be injected into the image - this file should contain your specfic info and secrets
      - ./postman_vars.json:/usr/src/app/postman_vars.json

secrets:
  devops-secret:
    file: ${HOME}/.pingidentity/devops
```
* run `docker-compose up -d`