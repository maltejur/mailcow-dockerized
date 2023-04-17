FROM debian:sid-slim
LABEL maintainer "Andre Peters <andre.peters@servercow.de>"

ARG DEBIAN_FRONTEND=noninteractive
ARG SOGO_DEBIAN_REPOSITORY=http://packages.sogo.nu/nightly/5/debian/
# renovate: datasource=github-releases depName=tianon/gosu versioning=semver-coerced
ARG GOSU_VERSION=1.16
ENV LC_ALL C

# Prerequisites
RUN apt-get update && apt-get install -y --no-install-recommends \
  apt-transport-https \
  ca-certificates \
  gettext \
  gnupg \
  mariadb-client \
  rsync \
  supervisor \
  syslog-ng \
  syslog-ng-core \
  syslog-ng-mod-redis \
  dirmngr \
  netcat-openbsd \
  psmisc \
  wget \
  patch \
  && apt-get install -y --no-install-recommends \
    sogo \
    sogo-activesync \
  && apt-get autoclean \
  && touch /etc/default/locale

COPY ./bootstrap-sogo.sh /bootstrap-sogo.sh
COPY syslog-ng.conf /etc/syslog-ng/syslog-ng.conf
COPY syslog-ng-redis_slave.conf /etc/syslog-ng/syslog-ng-redis_slave.conf
COPY supervisord.conf /etc/supervisor/supervisord.conf
COPY acl.diff /acl.diff
COPY stop-supervisor.sh /usr/local/sbin/stop-supervisor.sh
COPY docker-entrypoint.sh /

RUN chmod +x /bootstrap-sogo.sh \
  /usr/local/sbin/stop-supervisor.sh

ENTRYPOINT ["/docker-entrypoint.sh"]

CMD exec /usr/bin/supervisord -c /etc/supervisor/supervisord.conf