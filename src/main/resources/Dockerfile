FROM centos:7 AS tarantool-base
ARG TARANTOOL_VERSION=2.4
ARG TARANTOOL_SERVER_USER="tarantool"
ARG TARANTOOL_SERVER_UID=1000
ARG TARANTOOL_SERVER_GROUP="tarantool"
ARG TARANTOOL_SERVER_GID=1000
ARG TARANTOOL_WORKDIR="/app"
ARG TARANTOOL_RUNDIR="/tmp/run"
ARG TARANTOOL_DATADIR="/tmp/data"
ENV TARANTOOL_WORKDIR=$TARANTOOL_WORKDIR
ENV TARANTOOL_RUNDIR=$TARANTOOL_RUNDIR
ENV TARANTOOL_DATADIR=$TARANTOOL_DATADIR
RUN curl -L https://tarantool.io/installer.sh | VER=$TARANTOOL_VERSION /bin/bash -s -- --repo-only && \
    yum -y install cmake make gcc git unzip tarantool tarantool-devel cartridge-cli && \
    yum clean all
RUN groupadd -g $TARANTOOL_SERVER_GID $TARANTOOL_SERVER_GROUP && \
    useradd -u $TARANTOOL_SERVER_UID -g $TARANTOOL_SERVER_GID -m -s /bin/bash $TARANTOOL_SERVER_USER \
    || true
USER $TARANTOOL_SERVER_USER:$TARANTOOL_SERVER_GROUP
RUN cartridge version

FROM tarantool-base AS cartridge-base
WORKDIR $TARANTOOL_WORKDIR
CMD cartridge build && cartridge start --run-dir=$TARANTOOL_RUNDIR --data-dir=$TARANTOOL_DATADIR
