# nginx-proxy-docker

Proxy nginx-server for docker services.


## New Features!
  - You can specify port on host-machine via ENV variable NGINX_PORT=port. Default value: port=80
  - You can specify server-name of host-machine via ENV variable SERVER_NAME=host-machine-name. Default value:SERVER_NAME=localhost
  - You can specify root path  via ENV variable SAVE_ROOT_PATH={value}. Default value: SAVE_ROOT_PATH=yes
  If SAVE_ROOT_PATH=yes, it means that the root URL will be saved.
  If SAVE_ROOT_PATH=no, it means that the root URL will NOT be saved.

## Installing and Running

#### Requirements
Nginx-proxy-docker runs on any system equipped with docker v.1.10.0+. and docker-compose v. 1.6.0+.

#### Running

```sh
$ cd [path]/nginx-proxy-docker
$ docker-compose up
```

#### Configuring
Example of docker-compose.yml file

```sh
version: '2'
services:
  nginx-proxy:
    image: jwilder/nginx-proxy
    container_name: nginx-proxy
    environment:
      - NGINX_PORT=80
      - SERVER_NAME=localhost
      - SAVE_ROOT_PATH=yes
    ports:
      - "80:80"
    volumes:
      - /var/run/docker.sock:/tmp/docker.sock:ro
      - ./nginx.tmpl:/app/nginx.tmpl

  whoami:
    image: jwilder/whoami
    container_name: whoami
    environment:
      - VIRTUAL_HOST=whoami

  tomcat:
    image: tomcat:8.5
    container_name: tomcat8.5
    environment:
      - VIRTUAL_HOST=tomcat
    volumes:
      - ./tomcat/server.xml:/usr/local/tomcat/conf/server.xml
      - ./tomcat/tomcat-users.xml:/usr/local/tomcat/conf/tomcat-users.xml
```

In this state, you can access the test service via url http://localhost/whoami{/path}
Keep in mind that the service will receive URL WITH the first part. In this case it will be http://localhost/whoami{/path}

