title
:   PostgreSQL service How-To

description
:   Running and installing a PostgreSQL service

keywords
:   docker, example, package installation, postgresql

PostgreSQL Service
==================

> **note**
>
> A shorter version of [this blog
> post](http://zaiste.net/2013/08/docker_postgresql_how_to/).

> **note**
>
> As of version 0.5.2, docker requires root privileges to run. You have
> to either manually adjust your system configuration (permissions on
> /var/run/docker.sock or sudo config), or prefix docker with sudo.
> Check [this
> thread](https://groups.google.com/forum/?fromgroups#!topic/docker-club/P3xDLqmLp0E)
> for details.

Installing PostgreSQL on Docker
-------------------------------

For clarity I won't be showing commands output.

Run an interactive shell in Docker container.

~~~~ {.sourceCode .bash}
sudo docker run -i -t ubuntu /bin/bash
~~~~

Update its dependencies.

~~~~ {.sourceCode .bash}
apt-get update
~~~~

Install `python-software-properties`.

~~~~ {.sourceCode .bash}
apt-get -y install python-software-properties
apt-get -y install software-properties-common
~~~~

Add Pitti's PostgreSQL repository. It contains the most recent stable
release of PostgreSQL i.e. `9.2`.

~~~~ {.sourceCode .bash}
add-apt-repository ppa:pitti/postgresql
apt-get update
~~~~

Finally, install PostgreSQL 9.2

~~~~ {.sourceCode .bash}
apt-get -y install postgresql-9.2 postgresql-client-9.2 postgresql-contrib-9.2
~~~~

Now, create a PostgreSQL superuser role that can create databases and
other roles. Following Vagrant's convention the role will be named
docker with docker password assigned to it.

~~~~ {.sourceCode .bash}
su postgres -c "createuser -P -d -r -s docker"
~~~~

Create a test database also named `docker` owned by previously created
`docker` role.

~~~~ {.sourceCode .bash}
su postgres -c "createdb -O docker docker"
~~~~

Adjust PostgreSQL configuration so that remote connections to the
database are possible. Make sure that inside
`/etc/postgresql/9.2/main/pg_hba.conf` you have following line (you will
need to install an editor, e.g. `apt-get install vim`):

~~~~ {.sourceCode .bash}
host    all             all             0.0.0.0/0               md5
~~~~

Additionaly, inside `/etc/postgresql/9.2/main/postgresql.conf` uncomment
`listen_addresses` so it is as follows:

~~~~ {.sourceCode .bash}
listen_addresses='*'
~~~~

> **note**
>
> This PostgreSQL setup is for development only purposes. Refer to
> PostgreSQL documentation how to fine-tune these settings so that it is
> enough secure.

Exit.

~~~~ {.sourceCode .bash}
exit
~~~~

Create an image and assign it a name. `<container_id>` is in the Bash
prompt; you can also locate it using `docker ps -a`.

~~~~ {.sourceCode .bash}
docker commit <container_id> <your username>/postgresql
~~~~

Finally, run PostgreSQL server via `docker`.

~~~~ {.sourceCode .bash}
CONTAINER=$(sudo docker run -d -p 5432 \
  -t <your username>/postgresql \
  /bin/su postgres -c '/usr/lib/postgresql/9.2/bin/postgres \
    -D /var/lib/postgresql/9.2/main \
    -c config_file=/etc/postgresql/9.2/main/postgresql.conf')
~~~~

Connect the PostgreSQL server using `psql` (You will need postgres
installed on the machine. For ubuntu, use something like
`sudo apt-get install postgresql`).

~~~~ {.sourceCode .bash}
CONTAINER_IP=$(sudo docker inspect $CONTAINER | grep IPAddress | awk '{ print $2 }' | tr -d ',"')
psql -h $CONTAINER_IP -p 5432 -d docker -U docker -W
~~~~

As before, create roles or databases if needed.

~~~~ {.sourceCode .bash}
psql (9.2.4)
Type "help" for help.

docker=# CREATE DATABASE foo OWNER=docker;
CREATE DATABASE
~~~~

Additionally, publish your newly created image on Docker Index.

~~~~ {.sourceCode .bash}
sudo docker login
Username: <your username>
[...]
~~~~

~~~~ {.sourceCode .bash}
sudo docker push <your username>/postgresql
~~~~

PostgreSQL service auto-launch
------------------------------

Running our image seems complicated. We have to specify the whole
command with `docker run`. Let's simplify it so the service starts
automatically when the container starts.

~~~~ {.sourceCode .bash}
sudo docker commit -run='{"Cmd": \
  ["/bin/su", "postgres", "-c", "/usr/lib/postgresql/9.2/bin/postgres -D \
  /var/lib/postgresql/9.2/main -c \
  config_file=/etc/postgresql/9.2/main/postgresql.conf"], "PortSpecs": ["5432"]}' \
  <container_id> <your username>/postgresql
~~~~

From now on, just type `docker run <your username>/postgresql` and
PostgreSQL should automatically start.
