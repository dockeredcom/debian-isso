# Version 1.0.0

FROM dockeredcom/debian-stretch-gpg:latest

MAINTAINER Yves Vindevogel (yves.vindevogel@dockered.com)

ENV SHELL=/bin/bash

ENV ISSO_HOST="http://localhost:8820/"
ENV ISSO_LISTEN="http://0.0.0.0:8080/"

ARG DEBIAN_FRONTEND=noninteractive

COPY ./isso.conf /etc/isso/isso.conf
COPY ./start.sh /srv/script/start.sh

RUN chmod +x /srv/script/start.sh
RUN mkdir -p /srv/db

RUN apt-get update --yes
RUN apt-get upgrade --yes

RUN apt-get install dialog python-pip sqlite3 build-essential procps --yes
RUN pip install isso

RUN echo "[general]"                        >   /etc/isso/isso.conf
RUN echo "dbpath = /srv/db/comments.db"     >>  /etc/isso/isso.conf
RUN echo "host = $ISSO_HOST"                >>  /etc/isso/isso.conf
RUN echo "log-file = /var/log/isso.log"     >>  /etc/isso/isso.conf
RUN echo ""                                 >>  /etc/isso/isso.conf
RUN echo "[server]"                         >>  /etc/isso/isso.conf
RUN echo "listen = $ISSO_LISTEN"            >>  /etc/isso/isso.conf

RUN apt-get autoremove --yes
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENTRYPOINT ["/srv/script/start.sh"]
