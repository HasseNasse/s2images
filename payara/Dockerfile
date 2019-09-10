ARG FROM_IMAGE
FROM ${FROM_IMAGE}

LABEL maintainer="Hassan Nazar <hassan.nazar94@gmail.com>"

ARG VERSION
ENV PAYARA_DIST payara-${VERSION}
EXPOSE 4848 9009 8080 8181

# Initialize the configurable environment variables
ENV HOME_DIR=/opt/payara\
    PAYARA_DIR=/opt/payara/appserver\
    SCRIPT_DIR=/opt/payara/scripts\
    CONFIG_DIR=/opt/payara/config\
    DEPLOY_DIR=/opt/payara/deployments\
    PASSWORD_FILE=/opt/payara/passwordFile\
    # Payara version (5.183+)
    PAYARA_VERSION=${VERSION}\
    # Payara Server Domain options
    DOMAIN_NAME=production\
    ADMIN_USER=admin\
    ADMIN_PASSWORD=admin \
    # Utility environment variables
    JVM_ARGS=\
    DEPLOY_PROPS=\
    POSTBOOT_COMMANDS=/opt/payara/config/post-boot-commands.asadmin\
    PREBOOT_COMMANDS=/opt/payara/config/pre-boot-commands.asadmin

# Create and set the Payara user and working directory owned by the new user
RUN groupadd payara && \
    useradd -b ${HOME_DIR} -M -s /bin/bash -d ${HOME_DIR} payara -g payara && \
    echo payara:payara | chpasswd && \
    mkdir -p ${DEPLOY_DIR} && \
    mkdir -p ${CONFIG_DIR} && \
    mkdir -p ${SCRIPT_DIR} && \
    chown -R payara:payara ${HOME_DIR}
USER payara
WORKDIR ${HOME_DIR}

# Download and unzip the Payara distribution
RUN wget --no-verbose -O payara.zip https://s3-eu-west-1.amazonaws.com/payara.fish/Payara+Downloads/${PAYARA_VERSION}/${PAYARA_DIST}.zip && \
    chown payara:payara payara.zip && \
    unzip -qq payara.zip -d ./ && \
    mv payara*/ appserver && \
    # Configure the password file for configuring Payara
    echo "AS_ADMIN_PASSWORD=\nAS_ADMIN_NEWPASSWORD=${ADMIN_PASSWORD}" > ${HOME_DIR}/tmpfile && \	
    echo "AS_ADMIN_PASSWORD=${ADMIN_PASSWORD}" >> ${PASSWORD_FILE} && \
    # Cleanup unused files
    rm -rf \
    /tmp/tmpFile \
    payara.zip \
    ${PAYARA_DIR}/glassfish/domains/${DOMAIN_NAME}/osgi-cache \
    ${PAYARA_DIR}/glassfish/domains/${DOMAIN_NAME}/logs \
    ${PAYARA_DIR}/glassfish/domains/domain1

# Copy across docker scripts
COPY --chown=payara:payara bin/*.sh ${SCRIPT_DIR}/
RUN chmod +x ${SCRIPT_DIR}/*

CMD ${SCRIPT_DIR}/generate_deploy_commands.sh && exec ${SCRIPT_DIR}/startInForeground.sh