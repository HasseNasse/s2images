# s2i-payara-micro
ARG FROM_IMAGE=<substituted>
FROM ${FROM_IMAGE}

LABEL maintainer="Hassan Nazar <hassan.nazar94@gmail.com>"

ARG VERSION
ENV PAYARA_VERSION $VERSION
ENV PAYARA_JAR payara-micro-${VERSION}.jar
ENV BASE_DIR /opt
ENV LIB_DIR ${BASE_DIR}/lib
ENV DEPLOYMENT_DIR ${BASE_DIR}/deploy

LABEL io.k8s.description="Source-to-Image for running Payara Micro applications" \
    io.k8s.display-name="s2i-payara-micro" \
    io.openshift.expose-services="8080:http,8181:https" \
    io.openshift.tags="builder,${PAYARA_VERSION},Payara Micro"

RUN curl -o ${BASE_DIR}/${PAYARA_JAR} -L https://s3-eu-west-1.amazonaws.com/payara.fish/Payara+Downloads/${PAYARA_VERSION}/${PAYARA_JAR} && \
    mkdir -p ${DEPLOYMENT_DIR} && \
    mkdir -p ${LIB_DIR} && \
    chown -R 1001:0 ${BASE_DIR} &&\
    chmod -R a+rw ${BASE_DIR} 

# sets io.openshift.s2i.scripts-url label that way, or update that label
COPY ./s2i/bin/ /usr/libexec/s2i

# This default user is created in the openshift/base-centos7 image
USER 1001

EXPOSE 8080 8181

CMD ["/usr/libexec/s2i/usage"]
