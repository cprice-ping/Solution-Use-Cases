## PA with ACME-Managed cert for PF / PD
Server Profile: N/A

Often theres a need to have certificates that are signed by a public CA. Things like Delegator or PingCentral use the OIDC Issuer for both backchannel calls (Metadata and token validation) and frontchannel (Application rendering and API calls). Not all products allow insecure TLS connections.   

PingAccess v6 included a feature to generate KeyPairs and have them managed with the ACME protocol and the Let's Encrypt service. This capability makes PA a nice way to include a proxy to a configuration to enable trusted certificates in front of other services.
 
[Documentation](https://documenter.getpostman.com/view/1239082/SWT5jLpF)

### Deployment
[Sample Environment File](pa_add_proxy_env_sample.json)

**Postman Collection**  

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/eaa397bd3a35ef3095c1)

**Postman Newman**  
`newman run https://www.getpostman.com/collections/eaa397bd3a35ef3095c1 -e postman_vars.json --insecure --ignore-redirects`

**Newman - Docker**  
`docker run -d postman/newman run https://www.getpostman.com/collections/eaa397bd3a35ef3095c1 -e postman_vars.json --insecure --ignore-redirects -v ./pa_add_proxy.json:/etc/newman/postman_vars.json`

**YAML**
```
pa-config-proxy:
    image: postman/newman
    command: run https://www.getpostman.com/collections/eaa397bd3a35ef3095c1 -e postman_vars.json --insecure --ignore-redirects
    volumes:
        # An environment file should be injected into the image - this file should contain your specfic info and secrets
        - ./pa_add_proxy.json:/etc/newman/postman_vars.json
    networks:
      - pingnet-internal
```
**Note:** The `networks` need to match something the target service is using in the stack
