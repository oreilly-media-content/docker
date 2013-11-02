title
:   Hello world example

description
:   A simple hello world example with Docker

keywords
:   docker, example, hello world

Hello World
===========

Running the Examples
--------------------

All the examples assume your machine is running the docker daemon. To
run the docker daemon in the background, simply type:

~~~~ {.sourceCode .bash}
sudo docker -d &
~~~~

Now you can run docker in client mode: by default all commands will be
forwarded to the `docker` daemon via a protected Unix socket, so you
must run as root.

~~~~ {.sourceCode .bash}
sudo docker help
~~~~

* * * * *

Hello World
-----------

This is the most basic example available for using Docker.

Download the base image (named "ubuntu"):

~~~~ {.sourceCode .bash}
# Download an ubuntu image
sudo docker pull ubuntu
~~~~

Alternatively to the *ubuntu* image, you can select *busybox*, a bare
minimal Linux system. The images are retrieved from the Docker
repository.

~~~~ {.sourceCode .bash}
#run a simple echo command, that will echo hello world back to the console over standard out.
sudo docker run ubuntu /bin/echo hello world
~~~~

**Explanation:**

-   **"sudo"** execute the following commands as user *root*
-   **"docker run"** run a command in a new container
-   **"ubuntu"** is the image we want to run the command inside of.
-   **"/bin/echo"** is the command we want to run in the container
-   **"hello world"** is the input for the echo command

**Video:**

See the example in action

<div style="margin-top:10px;">
  <iframe width="560" height="350" src="http://ascii.io/a/2603/raw" frameborder="0"></iframe>
</div>

* * * * *

Hello World Daemon
------------------

And now for the most boring daemon ever written!

This example assumes you have Docker installed and the Ubuntu image
already imported with `docker pull ubuntu`. We will use the Ubuntu image
to run a simple hello world daemon that will just print hello world to
standard out every second. It will continue to do this until we stop it.

**Steps:**

~~~~ {.sourceCode .bash}
CONTAINER_ID=$(sudo docker run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done")
~~~~

We are going to run a simple hello world daemon in a new container made
from the *ubuntu* image.

-   **"docker run -d "** run a command in a new container. We pass "-d"
    so it runs as a daemon.
-   **"ubuntu"** is the image we want to run the command inside of.
-   **"/bin/sh -c"** is the command we want to run in the container
-   **"while true; do echo hello world; sleep 1; done"** is the mini
    script we want to run, that will just print hello world once a
    second until we stop it.
-   **\$CONTAINER\_ID** the output of the run command will return a
    container id, we can use in future commands to see what is going on
    with this process.

~~~~ {.sourceCode .bash}
sudo docker logs $CONTAINER_ID
~~~~

Check the logs make sure it is working correctly.

-   **"docker logs**" This will return the logs for a container
-   **\$CONTAINER\_ID** The Id of the container we want the logs for.

~~~~ {.sourceCode .bash}
sudo docker attach $CONTAINER_ID
~~~~

Attach to the container to see the results in realtime.

-   **"docker attach**" This will allow us to attach to a background
    process to see what is going on.
-   **\$CONTAINER\_ID** The Id of the container we want to attach too.

Exit from the container attachment by pressing Control-C.

~~~~ {.sourceCode .bash}
sudo docker ps
~~~~

Check the process list to make sure it is running.

-   **"docker ps"** this shows all running process managed by docker

~~~~ {.sourceCode .bash}
sudo docker stop $CONTAINER_ID
~~~~

Stop the container, since we don't need it anymore.

-   **"docker stop"** This stops a container
-   **\$CONTAINER\_ID** The Id of the container we want to stop.

~~~~ {.sourceCode .bash}
sudo docker ps
~~~~

Make sure it is really stopped.

**Video:**

See the example in action

<div style="margin-top:10px;">
  <iframe width="560" height="350" src="http://ascii.io/a/2562/raw" frameborder="0"></iframe>
</div>
The next example in the series is a python\_web\_app example, or you
could skip to any of the other examples:

-   python\_web\_app
-   nodejs\_web\_app
-   running\_redis\_service
-   running\_ssh\_service
-   running\_couchdb\_service
-   postgresql\_service
-   mongodb\_image

