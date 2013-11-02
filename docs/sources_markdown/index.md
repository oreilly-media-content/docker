title
:   Docker Documentation

description
:   An overview of the Docker Documentation

keywords
:   containers, lxc, concepts, explanation

Introduction
============

`docker`, the Linux Container Runtime, runs Unix processes with strong
guarantees of isolation across servers. Your software runs repeatably
everywhere because its container\_def includes any dependencies.

`docker` runs three ways:

-   as a daemon to manage LXC containers on your Linux host
    \<kernel\> (`sudo docker -d`)
-   as a CLI \<cli\> which talks to the daemon's [REST
    API](api/docker_remote_api) (`docker run ...`)
-   as a client of Repositories \<working\_with\_the\_repository\> that
    let you share what you've built (`docker pull, docker commit`).

Each use of `docker` is documented here. The features of Docker are
currently in active development, so this documentation will change
frequently.

For an overview of Docker, please see the
[Introduction](http://www.docker.io). When you're ready to start working
with Docker, we have a [quick
start](http://www.docker.io/gettingstarted) and a more in-depth guide to
ubuntu\_linux and other installation\_list paths including prebuilt
binaries, Vagrant-created VMs, Rackspace and Amazon instances.

Enough reading! Try it out! \<running\_examples\>
