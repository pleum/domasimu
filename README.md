domasimu
========

domasimu is a command line tool that enables reading and management of dnsimple domains from the command line.

    $ domasimu -l
    example.com
    example.net
    $ domasimu example.com
     A 192.0.2.2
    www A 192.0.2.2
    $ domasimu -u 'example.com mail A - 192.0.2.3 600'
    record written with id 3400485
    $ domasimu -u 'example.com mail A 192.0.2.3 192.0.2.4 600'
    record written with id 3400485
    $ domasimu -d 'example.com mail A 192.0.2.4'
    record deleted with id 3400485

The update -u flag takes a space separated list of
`domain, name, type, oldvalue, newvalue, ttl`. To create a new record use a
`oldvalue` of -. The TTL is always updated.

For example, to add 3 A records for www and then change one of them:

    $ domasimu -u 'example.com www A - 192.0.2.10 600'
    $ domasimu -u 'example.com www A - 192.0.2.11 600'
    $ domasimu -u 'example.com www A - 192.0.2.12 600'
    $ domasimu -u 'example.com www A 192.0.2.11 192.0.2.14 600'
    $ domasimu example.com | grep ^www
    www A 192.0.2.10
    www A 192.0.2.12
    www A 192.0.2.14


Installation
------------

~~$ go get github.com/jrwren/domasimu/...
$ go install github.com/jrwren/domasimu/...~~

Until a PR is merged, building from source is #dicey.

    $ go get github.com/jrwren/domasimu/...
    # ignore the errors
    $ go get github.com/jrwren/dnsimple/
    $ sed -i  's|"github.com/pearkes/dnsimple|"github.com/jrwren/dnsimple|' \
        $(find $GOPATH/src/github.com/jrwren/domasimu -name '*.go')
    $ go install github.com/jrwren/domasimu/...


Configuration
-------------

    $ echo 'user = "youraccount@example.com' > ~/.domasimurc
    $ echo 'token = "YOURTOKENGOESHERE_YESQUOTED"' >> ~/.domasimurc

Alternate Configuration
-----------------------
domasimu will read config from a different file if DOMASIMU_CONF environment variable is set.

    $ DOMASIMU_CONF="alt-domasimurc"
    $ echo 'user = "yourotheraccount@example.com' > $DOMASIMU_CONF
    $ echo 'token = "YOURTOKENGOESHERE_YESQUOTED"' >> $DOMASIMU_CONF
    $ domasimu -l
    moardomains.org

