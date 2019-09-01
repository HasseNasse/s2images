# java11-centos7
FROM openshift/base-centos7

LABEL maintainer="Hassan Nazar <hassan.nazar94@gmail.com>"

RUN yum update -y && \
    yum install java-1.8.0-openjdk-devel -y && \
    yum clean all -y
# Install & Configure Java 
ENV JAVA_HOME /usr/lib/jvm/java-1.8.0
ENV PATH "$PATH":${JAVA_HOME}/bin
