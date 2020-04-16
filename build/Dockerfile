FROM    debian:stretch

MAINTAINER  kawawa <kawawa.y@gmail.com>

ENV     DEBIAN_FRONTEND noninteractive

RUN     apt-get update -y
RUN     apt-get install -y \
        gawk \
        wget \
        git-core \
        diffstat \
        unzip \
        texinfo \
        gcc-multilib \
        build-essential \
        chrpath \
        socat \
        cpio \
        python \
        python3 \
        python3-pip \
        python3-pexpect \
        xz-utils \
        debianutils \
        iputils-ping \
        locales \
        apt-utils \
        sudo \
        curl

# Set up locales 
RUN     sed -i -e 's/# *en_US.UTF-8/en_US.UTF-8/' /etc/locale.gen && \
        locale-gen &&  \
        update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
ENV     LANG en_US.UTF8
ENV     LC_ALL en_US.UTF8

# Clean up APT when done. 
RUN     apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install repo
RUN     curl -Lo /usr/local/bin/repo https://storage.googleapis.com/git-repo-downloads/repo &&\
        chmod a+x /usr/local/bin/repo

# User management
RUN groupadd -g 1001 build && \
useradd -u 1001 -g 1001 -ms /bin/bash build && \
usermod -a -G sudo build && \
usermod -a -G users build

# Run as build user from the installation path
ENV YOCTO_INSTALL_PATH "/opt/yocto"
RUN install -o 1001 -g 1001 -d $YOCTO_INSTALL_PATH USER build WORKDIR ${YOCTO_INSTALL_PATH}

# Replace dash with bash 
#RUN rm /bin/sh && ln -s bash /bin/sh 

# Install FSL community BSP
#RUN mkdir -p ${YOCTO_INSTALL_PATH}/fsl-community-bsp && \
#        cd ${YOCTO_INSTALL_PATH}/fsl-community-bsp && \
#        repo init -u https://github.com/Freescale/fsl-community-bsp-platform -b ${YOCTO_RELEASE} && \
#        repo sync 

# Create a build directory for the FSL community BSP
#RUN mkdir -p ${YOCTO_INSTALL_PATH}/fsl-community-bsp/build


ADD entrypoint /usr/local/bin

# the working directory
WORKDIR /home/build