# syntax=docker/dockerfile:1.3-labs
FROM ubuntu:20.04
LABEL maintainer="Tony Vo <tonyvo@wustl.edu>" 
ARG APP_DIR=/opt/thpc \
    APP_PERIOD=2022.9 \
    EB_CONFIG=/home/easybuild/.config/easybuild/config.cfg \
    EB_CONFIGDIR=/home/easybuild/.config/easybuild \
    EB_DIR=/home/easybuild \
    EB_INSTALLPATH="/opt/thpc/${APP_PERIOD}/$(uname | tr '[:upper:]' '[:lower:]')/$(uname -p)/intel/cascadelake/easybuild" \
    EB_MODULES="$EB_DIR/.local/easybuild/modules/all" \
    EB_PYTHON=python3 \
    EB_TMPDIR=/tmp/easybuild \
    EB_VERSION=4.6.1 \
    SPACK_VERSION=0.19.0 \
    USERNAME=easybuild 
 
## Prerequisites
RUN apt update && \
    apt upgrade && \
    apt install -y lmod && \
    apt install -y \
        # Base image
        bc \
        bison \
        build-essential \
        bzip2 \
        ca-certificates \
        coreutils \
        curl \
        doxygen \
        emacs \
        flex \
        fuse \
        gawk \
        gdb \
        gfortran \
        git \
        ksh \
        less \
        libboost-all-dev \
        libibverbs-dev \
        libncurses6 \
        libncurses-dev \
        libncursesw6 \
        libpam-dev \
        libssl-dev \
        libtcl8.6 \
        lsb-release \
        mosh \
        openjdk-8-jdk \
        openjdk-8-jre \
        openssl \
        p7zip-full \
        p7zip-rar \
        patchelf \
        perl \
        rsync \
        ruby \
        samba-client \
        screen \
        sqlite \
        squashfs-tools \
        # EasyBuild & Spack dependencies
        liblua5.3-0 \
        liblua5.3-dev \
        lua5.3 \
        lua-bit32 \
        lua-filesystem \
        lua-posix \
        lua-posix-dev \
        python2 \
        python2-dev \
        python3 \
        python3-distutils \
        python3-pip \
        python3-venv \
        python-wheel-common \
        tcl \
        tcl8.6 \
        tcl8.6-dev \
        tcl-dev \
        tzdata \
        unzip \
        zip && \        
    apt clean all && \
    cat <<EOF >> /root/.profile
# Source global definitions
if [[ -f /etc/profile ]]
then
    source /etc/profile
fi
EOF

## Install spack
WORKDIR /opt
RUN curl -LO https://github.com/spack/spack/releases/download/v${SPACK_VERSION}/spack-${SPACK_VERSION}.tar.gz && \
    tar -xvzf spack-${SPACK_VERSION}.tar.gz && \
    rm -rf spack-${SPACK_VERSION}.tar.gz && \
    ln -s spack-${SPACK_VERSION} spack && \
    ln -s /opt/spack/share/spack/setup-env.sh /etc/profile.d/z03_spack-env.sh && \
    cat <<EOF > /etc/profile.d/z02_spack.sh
export SPACK_INSTALLPATH="/opt/versions/${APP_VERSION}/$(uname | tr '[:upper:]' '[:lower:]')/$(uname -p)/intel/cascadelake/spack"
export SPACK_ROOT=/opt/spack
module use /opt/versions/2022.9/$(uname | tr A-Z a-z)/$(uname -p)/intel/cascadelake/spack/lmod/modules/Core
EOF
COPY /spack/config.yaml /opt/spack/etc/spack/config.yaml
COPY /spack/modules.yaml /etc/spack/modules.yaml

