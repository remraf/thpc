# syntax=docker/dockerfile:1.3-labs
FROM centos:7
LABEL maintainer="Tony Vo <tonyvo@wustl.edu>"
ARG EB_VERSION=4.6.1
 
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
