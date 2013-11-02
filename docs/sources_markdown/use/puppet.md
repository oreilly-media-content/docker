title
:   Puppet Usage

description
:   Installating and using Puppet

keywords
:   puppet, installation, usage, docker, documentation

Using Puppet
============

> **note**
>
> Please note this is a community contributed installation path. The
> only 'official' installation is using the ubuntu\_linux installation
> path. This version may sometimes be out of date.

Requirements
------------

To use this guide you'll need a working installation of Puppet from
[Puppetlabs](https://www.puppetlabs.com) .

The module also currently uses the official PPA so only works with
Ubuntu.

Installation
------------

The module is available on the [Puppet
Forge](https://forge.puppetlabs.com/garethr/docker/) and can be
installed using the built-in module tool.

~~~~ {.sourceCode .bash}
puppet module install garethr/docker
~~~~

It can also be found on
[GitHub](https://www.github.com/garethr/garethr-docker) if you would
rather download the source.

Usage
-----

The module provides a puppet class for installing docker and two defined
types for managing images and containers.

### Installation

~~~~ {.sourceCode .ruby}
include 'docker'
~~~~

### Images

The next step is probably to install a docker image, for this we have a
defined type which can be used like so:

~~~~ {.sourceCode .ruby}
docker::image { 'ubuntu': }
~~~~

This is equivalent to running:

~~~~ {.sourceCode .bash}
docker pull ubuntu
~~~~

Note that it will only if the image of that name does not already exist.
This is downloading a large binary so on first run can take a while. For
that reason this define turns off the default 5 minute timeout for exec.
Note that you can also remove images you no longer need with:

~~~~ {.sourceCode .ruby}
docker::image { 'ubuntu':
  ensure => 'absent',
}
~~~~

### Containers

Now you have an image you can run commands within a container managed by
docker.

~~~~ {.sourceCode .ruby}
docker::run { 'helloworld':
  image   => 'ubuntu',
  command => '/bin/sh -c "while true; do echo hello world; sleep 1; done"',
}
~~~~

This is equivalent to running the following command, but under upstart:

~~~~ {.sourceCode .bash}
docker run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
~~~~

Run also contains a number of optional parameters:

~~~~ {.sourceCode .ruby}
docker::run { 'helloworld':
  image        => 'ubuntu',
  command      => '/bin/sh -c "while true; do echo hello world; sleep 1; done"',
  ports        => ['4444', '4555'],
  volumes      => ['/var/lib/counchdb', '/var/log'],
  volumes_from => '6446ea52fbc9',
  memory_limit => 10485760, # bytes
  username     => 'example',
  hostname     => 'example.com',
  env          => ['FOO=BAR', 'FOO2=BAR2'],
  dns          => ['8.8.8.8', '8.8.4.4'],
}
~~~~

Note that ports, env, dns and volumes can be set with either a single
string or as above with an array of values.
