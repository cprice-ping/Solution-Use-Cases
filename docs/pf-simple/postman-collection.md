## Postman Collection
* [Collection Documentation](https://documenter.getpostman.com/view/1239082/SzmjyF3m) 
* [Sample Environment](./postman_vars.json)

### Postman Environment
The collection has a set of default variables defined - to override them, place them in the `postman_vars.json` file.

**Collection Defaults**
| Used By | Variable | Description | Default |
| --- | --- | --- | --- |
| PingFederate | `pfAdminURL` | PingFed Administration URL | https://pingfederate:9999 |
| PingFederate | `pfAdmin` | PingFed API Admin Account | `api-admin` |
| PingFederate | `pfAdminPwd` | PingFed API Admin Password| {{globalPwd}} |
| All | `globalPwd` | Global Password | 2FederateM0re |


**Required in `postman_vars.json`**  
| Used By | Variable | Description | Customer Values |
| --- | --- | --- | --- |
| PingFederate | `pfBaseURL` | PingFed Runtime URL | https://{{your PF public FQDN}}:9031 |
| PingFederate | `pingId` | PingID Properties  | Your PingID Properties file |

---

### Postman Client  

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/33ce6e21b3c94585e33e)

---

### Postman Newman
`newman run https://www.getpostman.com/collections/33ce6e21b3c94585e33e -e postman_vars.json --insecure --ignore-redirects`

---
### Newman - Docker
`docker run -d postman/newman run https://www.getpostman.com/collections/33ce6e21b3c94585e33e -e postman_vars.json --insecure --ignore-redirects -v ./postman_vars.json:/etc/newman/postman_vars.json`

---
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