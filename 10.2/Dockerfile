FROM kubedb/postgres:10.2-v5
ENV POSTGIS_VERSION 2.5.3

RUN set -ex && \
    apk add --no-cache --virtual .fetch-deps \
        ca-certificates \
        file \
        openssl \
        tar && \
    wget -O postgis.tar.gz "https://github.com/postgis/postgis/archive/$POSTGIS_VERSION.tar.gz" && \
    mkdir -p /usr/src/postgis && \
    tar --extract \
        --file postgis.tar.gz \
        --directory /usr/src/postgis \
        --strip-components 1 && \
    rm postgis.tar.gz && \
    apk add --no-cache --virtual .build-deps \
        autoconf \
        automake \
        expat-dev \
        g++ \
        json-c-dev \
        libtool \
        libxml2-dev \
        make \
        perl && \
    apk add -v --no-cache --virtual .postgis-rundeps json-c && \
    apk add --force-overwrite --no-cache --virtual .build-deps-edge \
        --repository http://dl-cdn.alpinelinux.org/alpine/edge/main \
        --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing \
        apk-tools@edge \
        gdal-dev \
        geos-dev \
        proj-dev \
        protobuf-c-dev && \
    cd /usr/src/postgis && \
    ./autogen.sh && \
    ./configure && \
    make && \
    make install && \
    cd / && \
    rm -rf /usr/src/postgis
