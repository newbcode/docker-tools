FROM alpine:3.12
MAINTAINER Rich Braun "docker@instantlinux.net"
ARG BUILD_DATE
ARG VCS_REF
LABEL org.label-schema.build-date=$BUILD_DATE \
    org.label-schema.license=Apache-2.0 \
    org.label-schema.name=data-sync \
    org.label-schema.vcs-ref=$VCS_REF \
    org.label-schema.vcs-url=https://github.com/instantlinux/docker-tools

ENV PEERNAME= \
    PUBKEY1= \
    PUBKEY2= \
    RRSYNC_ROOT=/ \
    SYNC_INTERVAL=5 \
    SSHKEY1=data-sync-sshkey1 \
    SSHKEY2=data-sync-sshkey2

ARG UNISON_VERSION=2.48.15_p4-r1
ARG RRSYNC_SHA=b745a37909fc10087cc9c901ad7dfda8ad8b6b493097b156b68ba33db4a5a52f
COPY src/ /root/src/
RUN echo '@edge http://dl-cdn.alpinelinux.org/alpine/edge/main' \
      >>/etc/apk/repositories && \
    # TODO get rid of edge repo once openssh 8.4 appears in alpine
    apk add --update --no-cache openssh-client@edge openssh-server@edge perl rsync \
      unison=$UNISON_VERSION && \
    passwd -u root && \
    cd /usr/local/bin && \
    wget -q https://raw.githubusercontent.com/instantlinux/docker-tools/master/ansible/roles/docker_node/files/rrsync && \
    echo "$RRSYNC_SHA  rrsync" | sha256sum -c && \
    chmod 755 rrsync && chmod g-w,o-w /root/src/* && \
    rm -fr /var/log/* && \
    mkdir /var/log/unison /root/.unison

EXPOSE 22
VOLUME /etc/ssh /etc/unison.d /root/.unison /var/data-sync /var/log/unison
ENTRYPOINT ["/root/src/entrypoint.sh"]
