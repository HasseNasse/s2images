ARG FROM_IMAGE
FROM ${FROM_IMAGE}

LABEL maintainer="Hassan Nazar <hassan.nazar94@gmail.com>"

ARG VERSION
ENV PAYARA_CONFIG /tmp/src/payara
ENV PAYARA_VERSION $VERSION
ENV PAYARA_JAR payara-micro-${VERSION}.jar
ENV BASE_DIR /opt
ENV LIB_DIR ${BASE_DIR}/lib
ENV DEPLOYMENT_DIR ${BASE_DIR}/deploy

RUN curl -o ${BASE_DIR}/${PAYARA_JAR} -L https://s3-eu-west-1.amazonaws.com/payara.fish/Payara+Downloads/${PAYARA_VERSION}/${PAYARA_JAR} && \
    mkdir -p ${DEPLOYMENT_DIR} && mkdir -p ${PAYARA_CONFIG} && \
    touch ${PAYARA_CONFIG}/asadmin-preboot && \
    touch ${PAYARA_CONFIG}/asadmin-postboot && \
    touch ${PAYARA_CONFIG}/asadmin-postdeploy && \
    mkdir -p ${LIB_DIR} && \
    chown -R 1001:0 ${BASE_DIR} &&\
    chmod -R a+rw ${BASE_DIR} && \
    chown -R 1001:0 /tmp &&\
    chmod -R a+rw /tmp    

EXPOSE 8080 8181

CMD java -jar ${BASE_DIR}/${PAYARA_JAR} \
    --deploymentDir ${DEPLOYMENT_DIR} \
    --addLibs ${LIB_DIR} \
    --prebootcommandfile ${PAYARA_CONFIG}/asadmin-preboot \
    --postbootcommandfile ${PAYARA_CONFIG}/asadmin-postboot \
    --postdeploycommandfile ${PAYARA_CONFIG}/asadmin-postdeploy --autoBindSsl