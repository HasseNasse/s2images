# openliberty-centos7
FROM openshift/base-centos7

# TODO: Put the maintainer name in the image metadata
LABEL maintainer="Hassan Nazar <hassan.nazar94@gmail.com>"

# TODO: Rename the builder environment variable to inform users about application you provide them
ENV BUILDER_VERSION 1.0

# TODO: Set labels used in OpenShift to describe the builder image
LABEL io.k8s.description="Source-to-Image for running OpenLiberty applications" \
    io.k8s.display-name="s2i-openliberty" \
    io.openshift.expose-services="9080:http,9443:https" \
    io.openshift.tags="builder,19.0.0.8,OpenLiberty,WLP"

WORKDIR /opt/app-root/src
RUN yum install java-11-openjdk-devel -y
RUN which java && java -version

# Get OpenLiberty
RUN wget https://search.maven.org/remotecontent?filepath=io/openliberty/openliberty-runtime/19.0.0.8/openliberty-runtime-19.0.0.8.zip -O /opt/app-root/liberty.zip
RUN unzip /opt/app-root/liberty.zip
RUN sh wlp/bin/server create
RUN rm /opt/app-root/liberty.zip

# get Maven
RUN mkdir /opt/app-root/maven
RUN wget https://www-us.apache.org/dist/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz
RUN tar xf /opt/app-root/src/apache-maven-3.6.0-bin.tar.gz
RUN cp -r /opt/app-root/src/apache-maven-3.6.0/* /opt/app-root/maven
RUN rm -rf /opt/app-root/src/apache-maven-3.6.0-bin.tar.gz /opt/app-root/src/apache-maven-3.6.0/
RUN chown -R  1001:1001 /opt/app-root/maven
RUN mkdir -p /opt/app-root/src/wlp/usr/shared/resources/

#Clean up
RUN yum clean all -y

# TODO: Copy the S2I scripts to /usr/libexec/s2i, since openshift/base-centos7 image
# sets io.openshift.s2i.scripts-url label that way, or update that label
COPY ./s2i/bin/ /usr/libexec/s2i

# TODO: Drop the root user and make the content of /opt/app-root owned by user 1001
RUN chmod -R 775 /opt/app-root/src
RUN chmod -R 777 /opt/app-root/src/wlp/usr/servers/defaultServer
RUN chown -R 1001:1001 /opt/app-root/src

# This default user is created in the openshift/base-centos7 image
USER 1001

# TODO: Set the default port for applications built using this image
EXPOSE 9080 9443

# TODO: Set the default CMD for the image
CMD ["usage"]
