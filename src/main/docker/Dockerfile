# Version 1.0.0

FROM dockeredcom/debian-stretch-gpg:latest

MAINTAINER Yves Vindevogel (yves.vindevogel@dockered.com)

ENV SHELL=/bin/bash

# At this moment, there are 2 variables exported (only).
# That is what is needed to be able to link a comment section in a JBake site.
# Most likely, I should add some more to have styling, but for version 1.0.0, this is it.

ENV ISSO_HOST="http://localhost:8820/"
ENV ISSO_LISTEN="http://0.0.0.0:8080/"

ARG DEBIAN_FRONTEND=noninteractive

# Copy the start script to the image
COPY ./start.sh /srv/script/start.sh

# ONLY RUN COMMANDS FROM HERE UNTIL THE END

# Make sure the script can be run

RUN chmod +x /srv/script/start.sh

# Make the directory for the database.  This directory should be exported in a volume to have access to the data
# for backups. You should also make a volume of /etc/isso, which will contain the config file.

RUN mkdir -p /srv/db

# Install the needed packages for running ISSO

RUN apt-get update --yes
RUN apt-get upgrade --yes

RUN apt-get install dialog python-pip sqlite3 build-essential --yes
RUN pip install isso

# Create the config based on the 2 parameters and some fixed paths

RUN echo "[general]"                        >   /etc/isso/isso.conf
RUN echo "dbpath = /srv/db/comments.db"     >>  /etc/isso/isso.conf
RUN echo "host = $ISSO_HOST"                >>  /etc/isso/isso.conf
RUN echo "log-file = /var/log/isso.log"     >>  /etc/isso/isso.conf
RUN echo ""                                 >>  /etc/isso/isso.conf
RUN echo "[server]"                         >>  /etc/isso/isso.conf
RUN echo "listen = $ISSO_LISTEN"            >>  /etc/isso/isso.conf

# Clean up
RUN apt-get autoremove --yes
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Make sure the script to start the server runs. This script makes the image stay alive also.

ENTRYPOINT ["/srv/script/start.sh"]
