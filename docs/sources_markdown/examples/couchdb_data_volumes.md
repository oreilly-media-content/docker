title
:   Sharing data between 2 couchdb databases

description
:   Sharing data between 2 couchdb databases

keywords
:   docker, example, package installation, networking, couchdb, data
    volumes

CouchDB Service
===============

Here's an example of using data volumes to share the same data between 2
CouchDB containers. This could be used for hot upgrades, testing
different versions of CouchDB on the same data, etc.

Create first database
---------------------

Note that we're marking `/var/lib/couchdb` as a data volume.

~~~~ {.sourceCode .bash}
COUCH1=$(sudo docker run -d -v /var/lib/couchdb shykes/couchdb:2013-05-03)
~~~~

Add data to the first database
------------------------------

We're assuming your docker host is reachable at localhost. If not,
replace localhost with the public IP of your docker host.

~~~~ {.sourceCode .bash}
HOST=localhost
URL="http://$HOST:$(sudo docker port $COUCH1 5984)/_utils/"
echo "Navigate to $URL in your browser, and use the couch interface to add data"
~~~~

Create second database
----------------------

This time, we're requesting shared access to \$COUCH1's volumes.

~~~~ {.sourceCode .bash}
COUCH2=$(sudo docker run -d -volumes-from $COUCH1 shykes/couchdb:2013-05-03)
~~~~

Browse data on the second database
----------------------------------

~~~~ {.sourceCode .bash}
HOST=localhost
URL="http://$HOST:$(sudo docker port $COUCH2 5984)/_utils/"
echo "Navigate to $URL in your browser. You should see the same data as in the first database"'!'
~~~~

Congratulations, you are running 2 Couchdb containers, completely
isolated from each other *except* for their data.
