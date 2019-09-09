# s2i-payara-micro
ARG FROM_IMAGE=<substituted>
FROM ${FROM_IMAGE}

LABEL maintainer="Hassan Nazar <hassan.nazar94@gmail.com>"

LABEL io.k8s.description="Source-to-Image for running Payara Micro applications" \
    io.k8s.display-name="s2i-payara-micro" \
    io.openshift.expose-services="8080:http,8181:https" \
    io.openshift.tags="builder,${PAYARA_VERSION},Payara Micro"

# sets io.openshift.s2i.scripts-url label that way, or update that label
COPY ./s2i/bin/ /usr/libexec/s2i

# This default user is created in the openshift/base-centos7 image
USER 1001

CMD ["/usr/libexec/s2i/usage"]
