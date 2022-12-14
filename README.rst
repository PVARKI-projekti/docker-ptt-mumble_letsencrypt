==============================
Mumble server with letsencrypt
==============================

Build::

    docker build --progress plain --target server -t mumbleserver:latest .


First run::

    docker run -d -p 80:80 -p 64738:64738/tcp -p 64738:64738/udp \
      --name mumbleserver \
      -e CERTBOT_EMAIL="contact@example.com" \
      -e SERVER_DOMAIN=something.example.com \
      -e SERVER_PASSWORD="some good passphrase" \
      -e SUPERUSER_PASSWORD="even better passphrase" \
      mumbleserver:latest

If you have handled certificates some other way you can add "-e NO_CERTBOT=1", just make sure
to mount the certificate and key in the cerbot expected location for SERVER_DOMAIN.

If you just want murmurd to autogenerate its own self-signed certificate pass "-e NO_TLS=1"

If you want more or less users than the default of 100 pass "-e SERVER_USERS=200"

Coming back later::

  docker start mumbleserver


Ping
----

We also have a image for pinging a server

build::

    docker build --progress plain --ssh default --target ping  --tag mumbleping:latest  -f Dockerfile_ping .

Run::

    docker run --rm -it -e SERVER_DOMAIN=omething.example.com mumbleping:latest

Development
-----------

pre-commit
^^^^^^^^^^

We use pre-commit to keep things clean, you need some basic dependencies (run in this directory)::

    pip3 install --user pre-commit detect-secrets
    pre-commit install

As usual using virtualenvs is generally recommended but in this case not strictly mandatory.

Before committing check your work with::

    pre-commit run --all-files ; echo $?

This saves you annoyance of rewriting commit messages when one of the checks fail.
