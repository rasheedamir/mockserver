#
# MockServer Dockerfile
#
# https://github.com/jamesdbloom/mockserver
# http://www.mock-server.com
#

# pull base image
FROM phusion/baseimage:0.9.17

# reference maintainer
MAINTAINER Rasheed Amir <rasheed@aurorasolutions.io>

# make sure the package repository is up to date
RUN echo "deb http://archive.ubuntu.com/ubuntu trusty main universe" > /etc/apt/sources.list
RUN apt-get -y update

# install python-software-properties (so you can do add-apt-repository)
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y -q python-software-properties software-properties-common

# install utilities
RUN apt-get install vim -y && \ 
    apt-get install nano -y && \
    apt-get install git -y && \
    apt-get install sudo -y && \
    apt-get install zip -y && \
    apt-get install bzip2 -y && \
    apt-get install fontconfig -y && \ 
    apt-get install curl -y && \
    apt-get install wget -y && \
    apt-get clean

# install oracle java 8 from PPA
RUN add-apt-repository ppa:webupd8team/java -y
RUN apt-get update
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
RUN apt-get -y install oracle-java8-installer && apt-get clean

# set oracle java as the default java
RUN update-java-alternatives -s java-8-oracle
RUN echo "export JAVA_HOME=/usr/lib/jvm/java-8-oracle" >> ~/.bashrc

# download MockServer
RUN  \
  mkdir -p /opt/mockserver && \
  wget -O /opt/mockserver/mockserver-netty-jar-with-dependencies.jar https://oss.sonatype.org/content/repositories/releases/org/mock-server/mockserver-netty/3.9.17/mockserver-netty-3.9.17-jar-with-dependencies.jar && \
  wget -O /opt/mockserver/run_mockserver.sh https://raw.github.com/jamesdbloom/mockserver/master/docker/run_mockserver.sh && \
  chmod +x /opt/mockserver/run_mockserver.sh

# configure users

# configure the "aurora" and "root" users
RUN echo 'root:aurora' |chpasswd
RUN groupadd aurora && useradd aurora -s /bin/bash -m -g aurora -G aurora && adduser aurora sudo
RUN echo 'aurora:aurora' |chpasswd
RUN cd /home && chown -R aurora:aurora /home/aurora

# set working directory
WORKDIR /opt/mockserver

# expose ports.
EXPOSE 1080 1090

# expose the the SSHD port
EXPOSE 22

# Baseimage-docker disables the SSH server by default. Add the following to your Dockerfile to enable it:
RUN rm -f /etc/service/sshd/down

# Regenerate SSH host keys. baseimage-docker does not contain any, so you
# have to do that yourself. You may also comment out this instruction; the
# init system will auto-generate one during boot.
RUN /etc/my_init.d/00_regen_ssh_host_keys.sh

# default command
CMD /usr/sbin/sshd -D

### example to run the server...
# docker run -d -p 1080:1080 -p 1090:1090 jamesdbloom/mockserver /opt/mockserver/run_mockserver.sh 
# -logLevel INFO -serverPort 1080 -proxyPort 1090 -proxyRemotePort 80 
# -proxyRemoteHost https://wamsdubclus001rest-hs.cloudapp.net