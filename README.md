# sollie/docker-unifi

Beta versions and release candidates of unifi controller.
Forked from linuxserver/unifi

## Usage

```
docker create \
  --name=unifi \
  -v <path to data>:/config \
  -e PGID=<gid> -e PUID=<uid>  \
  -e 3478:3478/udp \
  -p 8080:8080 \
  -p 8081:8081 \
  -p 8443:8443 \
  -p 8843:8843 \
  -p 8880:8880 \
  linuxserver/unifi
```

## Parameters

`The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side. 
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
http://192.168.x.x:8080 would show you what's running INSIDE the container on port 80.`


* `-p 3478` - port(s) required for Unifi to function
* `-p 8080` - port(s)
* `-p 8081` - port(s)
* `-p 8443` - port(s)
* `-p 8843` - port(s)
* `-p 8880` - port(s)
* `-v /config` - where unifi stores it config files etc, needs 3gb free
* `-e PGID` for GroupID - see below for explanation
* `-e PUID` for UserID - see below for explanation

It is based on xenial with s6 overlay, for shell access whilst the container is running do `docker exec -it unifi /bin/bash`.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" <sup>TM</sup>.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id dockeruser
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application

The webui is at https://ip:8443 , setup with the first run wizard.

To adopt a Unifi Access Point, and get it to show up in the software, take these steps:

```
ssh ubnt@$AP-IP
mca-cli
set-inform http://$address:8080/inform 
```
  
Use `ubnt` as the password to login and `$address` is the IP address of the host you are running this container on and `$AP-IP` is the Access Point IP address.

## Info

* Shell access whilst the container is running: `docker exec -it unifi /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f unifi`


* container version number 

`docker inspect -f '{{ index .Config.Labels "build_version" }}' unifi`

* image version number

`docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/unifi`


## Versions

+ **2019-06-01:** Update to 5.10.24
+ **2019-05-16:** Update to 5.10.23
+ **2019-04-21:** Update to 5.10.21
+ **2019-02-17:** Update to 5.10.17
+ **2019-02-05:** Update to 5.10.12-20644d4901
+ **2018-10-23:** Update to 5.9.29
+ **2018-09-03:** Update to 5.8.28
+ **2018-06-19:** Update to 5.8.23-d5a5bbfda4
+ **2018-03-09:** Update to 5.7.20
+ **2017-12-31:** Update to 5.7.12-f5afb57178
+ **2017-11-08:** Update to 5.6.22-78ce2979bb
+ **2017-11-04:** Update to 5.6.20
+ **2017-10-17:** Update to 5.6.19-17e4cda571
+ **05.10.17:** Update to 5.5.24.
+ **03.08.17:** Update to 5.5.20.
+ **15.07.17:** Update to 5.5.19 and switch to using .deb package, no need to keep up with the unifi repo merry-go-round.
+ **05.07.17:** Change repo to stable. Remove execstack command, no longer required.
+ **18.03.17:** Fix execstack warning.
+ **14.10.16:** Add version layer information.
+ **20.09.16** Bump to pick up ver 5.27.
+ **10.09.16** Add layer badges to README.
+ **28.08.16** Add badges to README.
+ **01.07.16** Switch to lsiobase/xenial for conformity.
+ **25.06.16** Rebase to xenial and use updated repository.
+ **02.11.15** Initial Release.
