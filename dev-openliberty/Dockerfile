ARG FROM_IMAGE
FROM ${FROM_IMAGE}

LABEL maintainer="Hassan Nazar <hassan.nazar94@gmail.com>"

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
    sh ${LIBERTY_DIR}/wlp/bin/server create ${LIBERTY_SERVER_NAME} && mkdir -p ${DEPLOYMENT_DIR} && \
    rm ${INSTALL_DIR}/liberty.zip && \
    yum clean all -y

WORKDIR ${SERVER_DIR}

EXPOSE 9080 9443

CMD ${LIBERTY_DIR}/wlp/bin/server run defaultServer