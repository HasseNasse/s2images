# s2i-openliberty
ARG FROM_IMAGE
FROM $FROM_IMAGE

LABEL maintainer="Hassan Nazar <hassan.nazar94@gmail.com>"

LABEL io.k8s.description="Source-to-Image for running OpenLiberty applications" \
    io.k8s.display-name="s2i-openliberty" \
    io.openshift.expose-services="9080:http,9443:https" \
    io.openshift.tags="builder,${VERSION},OpenLiberty,WLP"

ARG VERSION
ENV LIBERTY_VERSION $VERSION
ENV LIBERTY_SERVER_NAME defaultServer
ENV INSTALL_DIR /opt
ENV LIBERTY_DIR ${INSTALL_DIR}/liberty
ENV SHARED_RESOURCES_DIR ${LIBERTY_DIR}/wlp/usr/shared/resources
ENV SERVER_DIR ${LIBERTY_DIR}/wlp/usr/servers/${LIBERTY_SERVER_NAME}
ENV DEPLOYMENT_DIR ${SERVER_DIR}/dropins

# Get OpenLiberty
RUN wget "https://search.maven.org/remotecontent?filepath=io/openliberty/openliberty-runtime/$VERSION/openliberty-runtime-$VERSION.zip" -O ${INSTALL_DIR}/liberty.zip && \
    mkdir -p ${LIBERTY_DIR} && mkdir -p ${SHARED_RESOURCES_DIR} && \
    unzip ${INSTALL_DIR}/liberty.zip -d ${LIBERTY_DIR} && \
    sh ${LIBERTY_DIR}/wlp/bin/server create ${LIBERTY_SERVER_NAME} && \
    rm ${INSTALL_DIR}/liberty.zip && \
    yum clean all -y

RUN chown -R 1001:0 ${INSTALL_DIR} && \
    chmod -R a+rw ${INSTALL_DIR}

COPY ./s2i/bin/ /usr/libexec/s2i

USER 1001

EXPOSE 9080 9443

CMD ["usage"]
