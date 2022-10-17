FROM centos:7
LABEL maintainer="Tony Vo <tonyvo@wustl.edu>"
 
# Prerequisites for Easybuild and LMOD
RUN yum -y --enablerepo=extras install epel-release && \
  yum groupinstall -y "Development tools" && \
  yum install -y \
    which \
    libibverbs-dev \
    libibverbs-devel \
    lua \
    lua-posix \
    lua-filesystem \
    lua-devel \
    tcl \
    python-pip \
    python-wheel \
    openssl \
    openssl-devel \
    libssl-dev \
    libopenssl-devel \
    shadow-utils \
    squashfs-tools && \
  yum clean -y all
 
# SET up variables used by the build process
ENV LMOD_VER=8.7.12 \
    EB_VER=4.6.0
 
# Install LMOD
WORKDIR /tmp/build
RUN curl -LO http://github.com/TACC/Lmod/archive/${LMOD_VER}.tar.gz && \
    tar xvf ${LMOD_VER}.tar.gz > /dev/null && \
    cd Lmod-${LMOD_VER} && \
    ./configure --prefix=/opt --with-fastTCLInterp=no && \
    make install && \
    ln -s /opt/lmod/lmod/init/profile /etc/profile.d/modules.sh && \
    ln -s /opt/lmod/lmod/init/cshrc /etc/profile.d/modules.csh && \
    rm -r /tmp/build/*
 
# Easybuild does not install as root.
RUN useradd easybuild && \
    mkdir -p /opt/versions && chown easybuild:easybuild /opt/versions
 
WORKDIR /home/easybuild
 
RUN pip install easybuild==$EB_VER
 
# Create a helper script to setup environment
# PYTHONHTTPSVERIFY=0 is for disabling the HTTPS verify
RUN echo "source /opt/lmod/lmod/init/bash" > /home/easybuild/setup.sh && \
  echo "module use /opt/versions/2022.9/$(uname | tr A-Z a-z)/$(uname -p)/intel/cascadelake/modules/all" >> /home/easybuild/setup.sh && \
  echo "module load EasyBuild/${EB_VER}" >> /home/easybuild/setup.sh && \
  echo "export PYTHONHTTPSVERIFY=0" >> /home/easybuild/setup.sh
 
# set default command. Note: it is still user easybuild
CMD source /home/easybuild/setup.sh && bash
# Show below is an example command to install using EasyBuild
## . /home/easybuild/setup.sh && eb goolf-1.7.20.eb --installpath=/opt/versions/2022.9/$(uname | tr A-Z a-z)/$(uname -p)/intel/cascadelake/software --buildpath=/tmp/easybuild --sourcepath=/tmp/easybuild -r && rm -rf /tmp/easybuild
