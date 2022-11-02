# syntax=docker/dockerfile:1.3-labs
FROM centos:7
LABEL maintainer="Tony Vo <tonyvo@wustl.edu>"
ARG APP=easybuild
ARG APP_DIR=/opt/thpc
ARG EB_DIR=/home/easybuild
ARG EB_PYTHON=python3
ARG EB_VERSION=4.6.1
ARG TMPDIR=/tmp/easybuild 
ARG USERNAME=easybuild
 
## Prerequisites
RUN yum -y update && \
    yum groupinstall -y "Development tools" && \
    yum install -y epel-release && \
    yum install -y \
        libibverbs-dev \
        libibverbs-devel \
        libopenssl-devel \
        libssl-dev \
        Lmod \
        lua \
        lua-posix \
        lua-filesystem \
        lua-devel \
        ncurses \
        ncurses-devel \
        openssl \
        openssl-devel \
        python3 \
        python3-pip \
        python3-wheel \
        shadow-utils \
        squashfs-tools \
        tcl \
        which  && \
    yum clean -y all

## Install easybuild
# Easybuild does not install as root
RUN useradd $USERNAME && \
    mkdir -p $APP_DIR && chown $USERNAME:$USERNAME $APP_DIR && \
    python3 -m pip install --ignore-installed --prefix=$TMPDIR easybuild 
USER easybuild
WORKDIR $EB_DIR
RUN export PATH=$TMPDIR/bin:$PATH && \
    export PYTHONPATH=$(/bin/ls -rtd -1 $TMPDIR/lib*/python*/site-packages | tail -1):$PYTHONPATH && \
    export EB_PYTHON=$EB_PYTHON && \
    source /etc/profile.d/00-modulepath.sh && \
    source /usr/share/lmod/8.7.7/init/profile && \
    eb --prefix $EB_DIR/.local EasyBuild-$EB_VERSION.eb -r
