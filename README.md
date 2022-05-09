Website: https://aspia.org

GitHub: https://github.com/dchapyshev/aspia

Docker image GitHub: https://github.com/l0ser140/aspia-docker
# Docker:
## router
```
docker run -d \
    --name=aspia-router \
    -p 8060:8060 \
    -v /opt/aspia/data:/var/lib/aspia:rw \
    -v /opt/aspia/config:/etc/aspia:rw \
    --restart unless-stopped \
    l0ser140/aspia-router:latest
```
## relay
```
docker run -d \
    --name=aspia-relay \
    -p 8070:8070 \
    -v /opt/aspia/config:/etc/aspia:rw \
    -e ROUTER_ADDRESS=host.docker.internal \
    -e EXTERNAL_ADDRESS=<Server external IP> \
    --restart unless-stopped \
    l0ser140/aspia-relay:latest
```

# Tips
First time start router before relay to generate pulic key into `/etc/aspia/router.pub` overwise relay will not find it.

Default router credentials is **admin:admin** 


# Docker-compose minimal example:
```
version: "3.9"
services:
  aspia-router:
    image: l0ser140/aspia-router:latest
    restart: unless-stopped
    ports:
      - "8060:8060"
    volumes:
      - /opt/aspia/data:/var/lib/aspia:rw
      - /opt/aspia/config:/etc/aspia:rw
  aspia-relay:
    image: l0ser140/aspia-relay:latest
    restart: unless-stopped
    ports:
      - "8070:8070"
    volumes:
      - /opt/aspia/config:/etc/aspia:rw
    environment:
      - ROUTER_ADDRESS=aspia-router
      - EXTERNAL_ADDRESS=<Server external IP>
    depends_on:
      - aspia-router
```

# Supported environment variables:
## router
 - **PORT** default 8060
 - **CLIENT_WHITELIST** whitelist of IP adresses for Client connections. You can specify list of ip, networks *(10.10.10.0/24)* or ip ranges *(192.168.2.10-192.168.2.10)* with semicolon delimiter. Networks and ranges will be revealed into list of IP, so avoid large ranges and wide masks.
 - **HOST_WHITELIST** Same as CLIENT_WHITELIST but for Host connections.
 - **ADMIN_WHITELIST** Same as CLIENT_WHITELIST but for Admin connections.
 - **RELAY_WHITELIST** Same as CLIENT_WHITELIST but for Relay connections. I highly recommend to specify this witelist.
## relay
 - **PUBLIC_KEY** public key of router, you can pass `/etc/aspia/router.pub` file or specify a variable.
 - **ROUTER_ADDRESS** address of router.
 - **ROUTER_PORT** default 8060
 - **EXTERNAL_ADDRESS** address of relay accessible from Hosts and Clients.
 - **PORT** default 8070
