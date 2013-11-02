title
:   Using Vagrant (Mac, Linux)

description
:   This guide will setup a new virtualbox virtual machine with docker
    installed on your computer.

keywords
:   Docker, Docker documentation, virtualbox, vagrant, git, ssh, putty,
    cygwin

Using Vagrant (Mac, Linux)
==========================

This guide will setup a new virtualbox virtual machine with docker
installed on your computer. This works on most operating systems,
including MacOX, Windows, Linux, FreeBSD and others. If you can install
these and have at least 400MB RAM to spare you should be good.

Install Vagrant and Virtualbox
------------------------------

1.  Install virtualbox from <https://www.virtualbox.org/> (or use your
    package manager)
2.  Install vagrant from <http://www.vagrantup.com/> (or use your
    package manager)
3.  Install git if you had not installed it before, check if it is
    installed by running `git` in a terminal window

Spin it up
----------

1.  Fetch the docker sources (this includes the `Vagrantfile` for
    machine setup).

    ~~~~ {.sourceCode .bash}
    git clone https://github.com/dotcloud/docker.git
    ~~~~

2.  Change directory to docker

    ~~~~ {.sourceCode .bash}
    cd docker
    ~~~~

3.  Run vagrant from the sources directory

    ~~~~ {.sourceCode .bash}
    vagrant up
    ~~~~

    Vagrant will:

    -   Download the 'official' Precise64 base ubuntu virtual machine
        image from vagrantup.com
    -   Boot this image in virtualbox
    -   Follow official ubuntu\_linux installation path

    You now have a Ubuntu Virtual Machine running with docker
    pre-installed.

Connect
-------

To access the VM and use Docker, Run `vagrant ssh` from the same
directory as where you ran `vagrant up`. Vagrant will connect you to the
correct VM.

~~~~ {.sourceCode .bash}
vagrant ssh
~~~~

Run
---

Now you are in the VM, run docker

~~~~ {.sourceCode .bash}
sudo docker
~~~~

Continue with the hello\_world example.
