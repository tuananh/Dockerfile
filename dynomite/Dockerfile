FROM bitnami/minideb:jessie

LABEL maintainer "Tuan Anh Tran <me@tuananh.org>"

RUN apt-get update && apt-get install -y \
    autoconf \
    build-essential \
    dh-autoreconf \
    git \
    libssl-dev \
    libtool \
    python-software-properties \
    redis-server \
    tcl8.5

RUN git clone https://github.com/Netflix/dynomite.git
COPY scripts/startup.sh dynomite/startup.sh
WORKDIR dynomite/

RUN autoreconf -fvi \
    && ./configure --enable-debug=log \
    && CFLAGS="-ggdb3 -O0" ./configure --enable-debug=full \
    && make \
    && make install

# redis port: 22122, stat/admin port: 222222, client port 8102
EXPOSE 8101 22122 22222 8102
# RUN sysctl vm.overcommit_memory=1

ENTRYPOINT ["/dynomite/startup.sh"]