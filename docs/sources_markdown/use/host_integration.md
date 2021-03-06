title
:   Host Integration

description
:   How to generate scripts for upstart, systemd, etc.

keywords
:   systemd, upstart, supervisor, docker, documentation, host
    integration

Host Integration
================

You can use your Docker containers with process managers like `upstart`,
`systemd` and `supervisor`.

Introduction
------------

If you want a process manager to manage your containers you will need to
run the docker daemon with the `-r=false` so that docker will not
automatically restart your containers when the host is restarted.

When you have finished setting up your image and are happy with your
running container, you may want to use a process manager to manage it.
When your run `docker start -a` docker will automatically attach to the
process and forward all signals so that the process manager can detect
when a container stops and correctly restart it.

Here are a few sample scripts for systemd and upstart to integrate with
docker.

Sample Upstart Script
---------------------

~~~~ {.sourceCode .bash}
description "Redis container"
author "Me"
start on filesystem and started lxc-net and started docker
stop on runlevel [!2345]
respawn
exec docker start -a 0a7e070b698b
~~~~

Sample systemd Script
---------------------

~~~~ {.sourceCode .bash}
[Unit]
Description=Redis container
Author=Me
After=docker.service

[Service]
Restart=always
ExecStart=/usr/bin/docker start -a 0a7e070b698b
ExecStop=/usr/bin/docker stop -t 2 0a7e070b698b

[Install]
WantedBy=local.target
~~~~
