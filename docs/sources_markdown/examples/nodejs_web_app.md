title
:   Running a Node.js app on CentOS

description
:   Installing and running a Node.js app on CentOS

keywords
:   docker, example, package installation, node, centos

Node.js Web App
===============

The goal of this example is to show you how you can build your own
docker images from a parent image using a `Dockerfile` . We will do that
by making a simple Node.js hello world web application running on
CentOS. You can get the full source code at
<https://github.com/gasi/docker-node-hello>.

Create Node.js app
------------------

First, create a `package.json` file that describes your app and its
dependencies:

~~~~ {.sourceCode .json}
{
  "name": "docker-centos-hello",
  "private": true,
  "version": "0.0.1",
  "description": "Node.js Hello World app on CentOS using docker",
  "author": "Daniel Gasienica <daniel@gasienica.ch>",
  "dependencies": {
    "express": "3.2.4"
  }
}
~~~~

Then, create an `index.js` file that defines a web app using the
[Express.js](http://expressjs.com/) framework:

~~~~ {.sourceCode .javascript}
var express = require('express');

// Constants
var PORT = 8080;

// App
var app = express();
app.get('/', function (req, res) {
  res.send('Hello World\n');
});

app.listen(PORT)
console.log('Running on http://localhost:' + PORT);
~~~~

In the next steps, we’ll look at how you can run this app inside a
CentOS container using docker. First, you’ll need to build a docker
image of your app.

Creating a `Dockerfile`
-----------------------

Create an empty file called `Dockerfile`:

~~~~ {.sourceCode .bash}
touch Dockerfile
~~~~

Open the `Dockerfile` in your favorite text editor and add the following
line that defines the version of docker the image requires to build
(this example uses docker 0.3.4):

~~~~ {.sourceCode .bash}
# DOCKER-VERSION 0.3.4
~~~~

Next, define the parent image you want to use to build your own image on
top of. Here, we’ll use [CentOS](https://index.docker.io/_/centos/)
(tag: `6.4`) available on the [docker index](https://index.docker.io/):

~~~~ {.sourceCode .bash}
FROM    centos:6.4
~~~~

Since we’re building a Node.js app, you’ll have to install Node.js as
well as npm on your CentOS image. Node.js is required to run your app
and npm to install your app’s dependencies defined in `package.json`. To
install the right package for CentOS, we’ll use the instructions from
the [Node.js
wiki](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager#rhelcentosscientific-linux-6):

~~~~ {.sourceCode .bash}
# Enable EPEL for Node.js
RUN     rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
# Install Node.js and npm
RUN     yum install -y npm
~~~~

To bundle your app’s source code inside the docker image, use the `ADD`
command:

~~~~ {.sourceCode .bash}
# Bundle app source
ADD . /src
~~~~

Install your app dependencies using npm:

~~~~ {.sourceCode .bash}
# Install app dependencies
RUN cd /src; npm install
~~~~

Your app binds to port `8080` so you’ll use the `EXPOSE` command to have
it mapped by the docker daemon:

~~~~ {.sourceCode .bash}
EXPOSE  8080
~~~~

Last but not least, define the command to run your app using `CMD` which
defines your runtime, i.e. `node`, and the path to our app, i.e.
`src/index.js` (see the step where we added the source to the
container):

~~~~ {.sourceCode .bash}
CMD ["node", "/src/index.js"]
~~~~

Your `Dockerfile` should now look like this:

~~~~ {.sourceCode .bash}
# DOCKER-VERSION 0.3.4
FROM    centos:6.4

# Enable EPEL for Node.js
RUN     rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
# Install Node.js and npm
RUN     yum install -y npm

# Bundle app source
ADD . /src
# Install app dependencies
RUN cd /src; npm install

EXPOSE  8080
CMD ["node", "/src/index.js"]
~~~~

Building your image
-------------------

Go to the directory that has your `Dockerfile` and run the following
command to build a docker image. The `-t` flag let’s you tag your image
so it’s easier to find later using the `docker images` command:

~~~~ {.sourceCode .bash}
sudo docker build -t <your username>/centos-node-hello .
~~~~

Your image will now be listed by docker:

~~~~ {.sourceCode .bash}
sudo docker images

> # Example
> REPOSITORY                 TAG       ID              CREATED
> centos                     6.4       539c0211cd76    8 weeks ago
> gasi/centos-node-hello     latest    d64d3505b0d2    2 hours ago
~~~~

Run the image
-------------

Running your image with `-d` runs the container in detached mode,
leaving the container running in the background. Run the image you
previously built:

~~~~ {.sourceCode .bash}
sudo docker run -d <your username>/centos-node-hello
~~~~

Print the output of your app:

~~~~ {.sourceCode .bash}
# Get container ID
sudo docker ps

# Print app output
sudo docker logs <container id>

> # Example
> Running on http://localhost:8080
~~~~

Test
----

To test your app, get the the port of your app that docker mapped:

~~~~ {.sourceCode .bash}
docker ps

> # Example
> ID            IMAGE                          COMMAND              ...   PORTS
> ecce33b30ebf  gasi/centos-node-hello:latest  node /src/index.js         49160->8080
~~~~

In the example above, docker mapped the `8080` port of the container to
`49160`.

Now you can call your app using `curl` (install if needed via:
`sudo apt-get install curl`):

~~~~ {.sourceCode .bash}
curl -i localhost:49160

> HTTP/1.1 200 OK
> X-Powered-By: Express
> Content-Type: text/html; charset=utf-8
> Content-Length: 12
> Date: Sun, 02 Jun 2013 03:53:22 GMT
> Connection: keep-alive
>
> Hello World
~~~~

We hope this tutorial helped you get up and running with Node.js and
CentOS on docker. You can get the full source code at
<https://github.com/gasi/docker-node-hello>.

Continue to running\_redis\_service.
