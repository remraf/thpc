# syntax=docker/dockerfile:1.3-labs
FROM centos:7
LABEL maintainer="Tony Vo <tonyvo@wustl.edu>" 
ARG APP_DIR=/opt/thpc
ARG EB_CONFIG=/home/easybuild/.config/easybuild/config.cfg
ARG EB_CONFIGDIR=/home/easybuild/.config/easybuild
ARG EB_DIR=/home/easybuild
ARG EB_INSTALLPATH="/opt/thpc/${EB_PERIOD}/$(uname | tr '[:upper:]' '[:lower:]')/$(uname -p)/intel/cascadelake"
ARG EB_MODULES="$EB_DIR/.local/easybuild/modules/all"
ARG EB_PERIOD=2022.9
ARG EB_PYTHON=python3
ARG EB_VERSION=4.6.1
ARG TMPDIR=/tmp/easybuild 
ARG USERNAME=easybuild
 
## Prerequisites
RUN yum -y update && \
    yum groupinstall -y "Development Tools" && \
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
# Configure easybuild
RUN useradd $USERNAME && \
    mkdir -p $APP_DIR $EB_CONFIGDIR && chown $USERNAME:$USERNAME $APP_DIR $EB_CONFIGDIR && \
    python3 -m pip install --ignore-installed --prefix=$TMPDIR easybuild 
RUN cat <<EOF > /etc/profile.d/z01_eb.sh
export PYTHONHTTPSVERIFY=0
module use $EB_MODULES
module use ${EB_INSTALLPATH}/modules/all
module load EasyBuild/$EB_VERSION
EOF
# Install Easybuild as non-root user
USER easybuild
RUN export PATH=$TMPDIR/bin:$PATH && \
    export PYTHONPATH=$(/bin/ls -rtd -1 $TMPDIR/lib*/python*/site-packages | tail -1):$PYTHONPATH && \
    export EB_PYTHON=$EB_PYTHON && \
    source /etc/profile.d/00-modulepath.sh && \
    source /usr/share/lmod/8.7.7/init/profile && \
    eb EasyBuild-$EB_VERSION.eb -r
RUN INSTALLPATH="/opt/thpc/${EB_PERIOD}/$(uname | tr '[:upper:]' '[:lower:]')/$(uname -p)/intel/cascadelake" && \
cat <<EOF > /home/easybuild/.config/easybuild/config.cfg
[config]
installpath = $INSTALLPATH
EOF
