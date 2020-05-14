## Postman Collection
* [Collection Documentation](https://documenter.getpostman.com/view/1239082/SzmjyF3m) 
* [Sample Environment](./postman_vars.json)

**Postman Client**  

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/33ce6e21b3c94585e33e)

**Postman Newman**  
`newman run https://www.getpostman.com/collections/33ce6e21b3c94585e33e -e postman_vars.json --insecure --ignore-redirects`

**Newman - Docker**  
`docker run -d postman/newman run https://www.getpostman.com/collections/33ce6e21b3c94585e33e -e postman_vars.json --insecure --ignore-redirects -v ./postman_vars.json:/etc/newman/postman_vars.json`

**YAML**
```
  pingconfig:
    image: pricecs/pingconfigurator:latest
    environment:
      - COLLECTIONS=https://www.getpostman.com/collections/33ce6e21b3c94585e33e
    volumes: 
    # An environment file should be injected into the image - this file should contain your specfic info and secrets
      - ./postman_vars.json:/usr/src/app/postman_vars.json
```