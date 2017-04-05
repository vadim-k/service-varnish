FROM alpine:3.4

MAINTAINER Leonid Makarov <leonid.makarov@blinkreaction.com>

#Upgrade alpine to edge
RUN sed -i -e 's/v3\.4/edge/g' /etc/apk/repositories

RUN apk add --update --no-cache bash varnish automake make varnish-dev python openssl autoconf libtool curl py-docutils file && \
	rm -rf /var/cache/apk/*

RUN apk upgrade --update-cache --available

RUN mkdir /usr/local/src

#
# Install Mhash
#
RUN cd /usr/local/src && \
    curl -sfLO https://downloads.sourceforge.net/project/mhash/mhash/0.9.9.9/mhash-0.9.9.9.tar.gz && \
    tar -xzf mhash-0.9.9.9.tar.gz && \
    cd /usr/local/src/mhash-0.9.9.9 && \
    ./configure --prefix=/usr && \
    make install && \
    cd /usr/local/src && \
    rm -rf mhash*

#
# install libvmod-digest
#

ENV LIBVMOD_DIGEST_VERSION=1.0.1

RUN cd /usr/local/src && \
    curl -sfLO https://github.com/varnish/libvmod-digest/archive/libvmod-digest-${LIBVMOD_DIGEST_VERSION}.tar.gz && \
    tar -xzf libvmod-digest-${LIBVMOD_DIGEST_VERSION}.tar.gz && \
    cd /usr/local/src/libvmod-digest-libvmod-digest-${LIBVMOD_DIGEST_VERSION} && \
    ./autogen.sh && \
    ./configure && \
    make install && \
    cd /usr/local/src && \
    rm -rf libvmod-digest*

COPY default.vcl /opt/default.vcl
COPY startup.sh /opt/startup.sh

ENV VARNISH_PORT 80
ENV VARNISH_BACKEND_PORT 80
ENV VARNISH_CACHE_SIZE 64M
ENV VARNISH_VARNISHLOG_PARAMS '-c -I RespStatus:^50'

EXPOSE 80

CMD ["/opt/startup.sh"]
