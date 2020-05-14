## PingFederate SimplePCV Configuration
**Pre-Requisites**
* New PingFederate instance
* PingFed license file
* PingID Tenant & Properties File
* Postman
  * [Environment](./postman_vars.json)

**Configuration**  
This API collection takes the vanilla PF instance and adds a (slightly) stripped down version of [Workforce360](https://github.com/pingidentity/Workforce360).  

It's based on a SimplePCV and no external datastore.

**Removed Elements**
* Software Components
  * ActiveDirectory
  * PingDirectory
  * PingDataSync
* PingFederate Configuration
  * AD Datastore \ PCV \ Realm
  * PD Datastore \ PCV
  * Kerberos Adapter \ AuthN Policy branch

## Deployment
**Postman Environment**
* Modify Postman Environment
  * [Sample](./postman_vars.json)
  * Add your PF hostname \ IP
  * Copy and Paste `pingid.properties` contents into `pingId`
---

**Software Only**
* Install PF server
* Execute the [Postman Collection](./postman-collection.md)
---

**Docker**
* Install Docker
* Run PingFed in a container
```
docker run -e "PING_IDENTITY_ACCEPT_EULA=YES" -v ${PWD}/pingfederate.lic:/opt/in/instance/server/default/conf/pingfederate.lic pingidentity/pingfederate
```
---
**Docker Compose (License file)**
* Install Docker
* Install Docker Compose
* Create `docker-compose.yaml`
  * **Note:** This yaml will persist the PF configuration
```
version: "3.5"

services:
  pingfederate:
    image: pingidentity/pingfederate:latest
    environment:
      - PING_IDENTITY_ACCEPT_EULA=YES
    volumes:
      - ./pingfederate.lic:/opt/in/instance/server/default/conf/pingfederate.lic       
      - ./pingfederate:/opt/out/instance

  pingconfig:
    image: pricecs/pingconfigurator:latest
    environment:
      - COLLECTIONS=https://www.getpostman.com/collections/33ce6e21b3c94585e33e
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
  pingfederate:
    image: pingidentity/pingfederate:latest
    secrets:
      - devops-secret
    environment:
      - PING_IDENTITY_ACCEPT_EULA=YES
    volumes:       
      - ./pingfederate:/opt/out/instance
    ports:
      - 9031:9031
      - 9999:9999

  pingconfig:
    image: pricecs/pingconfigurator:latest
    environment:
      - COLLECTIONS=https://www.getpostman.com/collections/33ce6e21b3c94585e33e
    volumes: 
    # An environment file should be injected into the image - this file should contain your specfic info and secrets
      - ./postman_vars.json:/usr/src/app/postman_vars.json

secrets:
  devops-secret:
    file: ${HOME}/.pingidentity/devops
```
* run `docker-compose up -d`
---