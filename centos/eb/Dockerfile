FROM centos:7
LABEL maintainer="Tony Vo <tonyvo@wustl.edu>"
 
## Prerequisites
RUN yum -y update && \
    yum -y --enablerepo=extras install epel-release && \
    yum groupinstall -y "Development tools" && \
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
 
# SET up variables used by the build process
    EB_VER=4.6.0
 
## Install easybuild
# Easybuild does not install as root.
RUN useradd easybuild && \
    mkdir -p /opt/versions && chown easybuild:easybuild /opt/versions
 
WORKDIR /home/easybuild
 
RUN pip install easybuild==$EB_VER
 
## Create a helper script to setup environment
# PYTHONHTTPSVERIFY=0 is for disabling the HTTPS verify
RUN echo "source /opt/lmod/lmod/init/bash" > /home/easybuild/setup.sh && \
  echo "module use /opt/versions/2022.9/$(uname | tr A-Z a-z)/$(uname -p)/intel/cascadelake/modules/all" >> /home/easybuild/setup.sh && \
  echo "module load EasyBuild/${EB_VER}" >> /home/easybuild/setup.sh && \
  echo "export PYTHONHTTPSVERIFY=0" >> /home/easybuild/setup.sh
 
# set default command. Note: it is still user easybuild
CMD source /home/easybuild/setup.sh && bash
# Show below is an example command to install using EasyBuild
## . /home/easybuild/setup.sh && eb goolf-1.7.20.eb --installpath=/opt/versions/2022.9/$(uname | tr A-Z a-z)/$(uname -p)/intel/cascadelake/software --buildpath=/tmp/easybuild --sourcepath=/tmp/easybuild -r && rm -rf /tmp/easybuild
