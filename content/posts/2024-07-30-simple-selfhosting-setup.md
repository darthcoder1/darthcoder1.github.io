+++
title = 'Simple Self-hosting Setup'
date = 2024-07-30T20:14:43+02:00
draft = false
+++

# Simple Self-hosting Setup

I have been on the journey to take back ownership of my data for a while. There is still a long way to go, but it lead me down the rabbit hole of self hosting all kinds of services. Whether it's [immich](https://immich.app) as a replacement for Google Photos, [paperless-ngx](https://github.com/paperless-ngx/paperless-ngx) for digital archiving of documents or [nextcloud](https://nextcloud.com). It has been quite a fun time to dig around in this field and try to figure out what the best setup might be. How to get it easiest to update, to maintain, to extend.

<!--more-->

Initially I wanted to go with a simple self-hosting solution, like [YUNOhost](https://yunohost.org), [CasaOS](https://casaos.io) and some others. It seemed exactly what I needed in the beginning. Unfortuantely all of them where in different ways quite restriced in the kinds of services they supported. But my main problem with it was, that all modifications to the system are done through UI and are not automatically reproducible. Sure, you can backup the configuration and/or the full setup and recreate it from this backup. This is not very handy though and you cannot easily keep track of different versions of your setup. I enjoy trying out new services, so I have quite regular additions and removals of services. This is not really what these self-hosting solutions optimize for. Often random errros and instabilities creeped in due to the left-over residue of old services piling up. So apparently, this was not really the solution I have been looking for.

My requirements became a lot clearer now though. I want something where I can setup a service with a configuration I can push to git. Get it version controlled. Setting up the system on a new server with any version of the setup should be very easy. It needs to be safe to add and remove services quite frequently without risking the stability of the system.
From there, the direction became clear. A consistent folder structure with a docker-compose file for each service.

An example of the git repository might look like that:

```
my-selfhosting-setup
    - immich
        docker-compose.yml
    - nextcloud
        docker-compose.yml
    - nginx
        docker-compose.yml
    deploy.sh
```

When preparing the docker compose files, I make sure that all data is stored within the directory of the service itself. The `deploy.sh` script deploys this structure as is to the target machine via ssh. Backups of databases or other things are handled within the docker compose file of each service and also stored within this service's directory.

```
/opt/my-selfhosting-setup
    - immich
        - data
            ...
        .env
        docker-compose.yml
        hwaccel.yml
        postgres-backup.sql.gz
    - nextcloud
        - data
            ...
        .env
        docker-compose.yml
    - nginx
        - data
            ...
        - letsencrypt
            ...
        .env
        docker-compose.yml
```

Now the management of the server becomes as easy as invocations of docker commands. Starting, stopping and updating services is straightforward and simple. Removing a services leaves no residue which might have effects on stability. As everything ist just off the shelve docker, I could also use some management frontend quite easily. But I actually prefer working with an IDE and the terminal.
