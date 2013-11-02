title
:   Requirements and Installation on Ubuntu Linux

description
:   Please note this project is currently under heavy development. It
    should not be used in production.

keywords
:   Docker, Docker documentation, requirements, virtualbox, vagrant,
    git, ssh, putty, cygwin, linux

Ubuntu Linux
============

> **warning**
>
> These instructions have changed for 0.6. If you are upgrading from an
> earlier version, you will need to follow them again.

Right now, the officially supported distribution are:

-   ubuntu\_precise
-   ubuntu\_raring

Docker has the following dependencies

-   Linux kernel 3.8 (read more about kernel)
-   AUFS file system support (we are working on BTRFS support as an
    alternative)

Please read ufw, if you plan to use [UFW (Uncomplicated
Firewall)](https://help.ubuntu.com/community/UFW)

Ubuntu Precise 12.04 (LTS) (64-bit)
-----------------------------------

This installation path should work at all times.

### Dependencies

**Linux kernel 3.8**

Due to a bug in LXC, docker works best on the 3.8 kernel. Precise comes
with a 3.2 kernel, so we need to upgrade it. The kernel you'll install
when following these steps comes with AUFS built in. We also include the
generic headers to enable packages that depend on them, like ZFS and the
VirtualBox guest additions. If you didn't install the headers for your
"precise" kernel, then you can skip these headers for the "raring"
kernel. But it is safer to include them if you're not sure.

~~~~ {.sourceCode .bash}
# install the backported kernel
sudo apt-get update
sudo apt-get install linux-image-generic-lts-raring linux-headers-generic-lts-raring

# reboot
sudo reboot
~~~~

### Installation

> **warning**
>
> These instructions have changed for 0.6. If you are upgrading from an
> earlier version, you will need to follow them again.

Docker is available as a Debian package, which makes installation easy.

~~~~ {.sourceCode .bash}
# Add the Docker repository key to your local keychain
# using apt-key finger you can check the fingerprint matches 36A1 D786 9245 C895 0F96 6E92 D857 6A8B A88D 21E9
sudo sh -c "wget -qO- https://get.docker.io/gpg | apt-key add -"

# Add the Docker repository to your apt sources list.
sudo sh -c "echo deb http://get.docker.io/ubuntu docker main\
> /etc/apt/sources.list.d/docker.list"

# Update your sources
sudo apt-get update

# Install, you will see another warning that the package cannot be authenticated. Confirm install.
sudo apt-get install lxc-docker
~~~~

Verify it worked

~~~~ {.sourceCode .bash}
# download the base 'ubuntu' container and run bash inside it while setting up an interactive shell
sudo docker run -i -t ubuntu /bin/bash

# type 'exit' to exit
~~~~

**Done!**, now continue with the hello\_world example.

Ubuntu Raring 13.04 (64 bit)
----------------------------

### Dependencies

**AUFS filesystem support**

Ubuntu Raring already comes with the 3.8 kernel, so we don't need to
install it. However, not all systems have AUFS filesystem support
enabled, so we need to install it.

~~~~ {.sourceCode .bash}
sudo apt-get update
sudo apt-get install linux-image-extra-`uname -r`
~~~~

### Installation

Docker is available as a Debian package, which makes installation easy.

*Please note that these instructions have changed for 0.6. If you are
upgrading from an earlier version, you will need to follow them again.*

~~~~ {.sourceCode .bash}
# Add the Docker repository key to your local keychain
# using apt-key finger you can check the fingerprint matches 36A1 D786 9245 C895 0F96 6E92 D857 6A8B A88D 21E9
sudo sh -c "wget -qO- https://get.docker.io/gpg | apt-key add -"

# Add the Docker repository to your apt sources list.
sudo sh -c "echo deb http://get.docker.io/ubuntu docker main\
> /etc/apt/sources.list.d/docker.list"

# update
sudo apt-get update

# install
sudo apt-get install lxc-docker
~~~~

Verify it worked

~~~~ {.sourceCode .bash}
# download the base 'ubuntu' container
# and run bash inside it while setting up an interactive shell
sudo docker run -i -t ubuntu /bin/bash

# type exit to exit
~~~~

**Done!**, now continue with the hello\_world example.

Docker and UFW
--------------

Docker uses a bridge to manage containers networking, by default UFW
drop all forwarding, a first step is to enable forwarding:

~~~~ {.sourceCode .bash}
sudo nano /etc/default/ufw
----
# Change:
# DEFAULT_FORWARD_POLICY="DROP"
# to
DEFAULT_FORWARD_POLICY="ACCEPT"
~~~~

Then reload UFW:

~~~~ {.sourceCode .bash}
sudo ufw reload
~~~~

UFW's default set of rules denied all incoming, so if you want to be
able to reach your containers from another host, you should allow
incoming connections on the docker port (default 4243):

~~~~ {.sourceCode .bash}
sudo ufw allow 4243/tcp
~~~~
