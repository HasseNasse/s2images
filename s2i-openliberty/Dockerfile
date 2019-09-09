# s2i-openliberty
ARG FROM_IMAGE
FROM $FROM_IMAGE

LABEL maintainer="Hassan Nazar <hassan.nazar94@gmail.com>"

LABEL io.k8s.description="Source-to-Image for running OpenLiberty applications" \
    io.k8s.display-name="s2i-openliberty" \
    io.openshift.expose-services="9080:http,9443:https" \
    io.openshift.tags="builder,${VERSION},OpenLiberty,WLP"

COPY ./s2i/bin/ /usr/libexec/s2i

USER 1001

CMD ["usage"]
