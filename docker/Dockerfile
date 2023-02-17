FROM fluent/fluentd:v1.14-1

USER root
WORKDIR /home/fluent
ENV BUILDDEPS="sudo make gcc g++ libc-dev ruby-dev libffi-dev"

# RUN apt-get update \
#     && apt-get upgrade -y \
    # && apt-get install -y --no-install-recommends $BUILDDEPS \
    # && echo 'gem: --no-document' >> /etc/gemrc \
    # && gem install fluent-plugin-systemd \
    # && gem install fluent-plugin-papertrail \
    # && gem install fluent-plugin-loggly-syslog \
    # && gem install fluent-plugin-rewrite-tag-filter \
    # && gem install fluent-plugin-kubernetes_metadata_input \
    # && gem install fluent-plugin-kubernetes_metadata_filter \
    # && gem install fluent-plugin-detect-exceptions \
    # && SUDO_FORCE_REMOVE=yes apt-get purge -y --auto-remove \
    #               -o APT::AutoRemove::RecommendsImportant=false \
    #               $BUILDDEPS \
    # && rm -rf /var/lib/apt/lists/* \
    # && gem sources --clear-all \
    # && rm -rf /tmp/* /var/tmp/* /usr/lib/ruby/gems/*/cache/*.gem

RUN set -eux; \
    apk add --no-cache --virtual .build-deps \
            build-base linux-headers ruby-dev; \
    gem install fluent-plugin-systemd \
                fluent-plugin-papertrail \
                fluent-plugin-loggly-syslog \
                fluent-plugin-rewrite-tag-filter \
                fluent-plugin-kubernetes_metadata_input \
                fluent-plugin-kubernetes_metadata_filter \
                fluent-plugin-detect-exceptions; \
    apk del .build-deps

# Copy configuration files
COPY ./conf/fluent.conf /fluentd/etc/
COPY ./conf/systemd.conf /fluentd/etc/
COPY ./conf/kubernetes.conf /fluentd/etc/

# Copy plugins
COPY plugins /fluentd/plugins/

# Environment variables
ENV FLUENTD_OPT=""
ENV FLUENTD_CONF="fluent.conf"

# Overriding entrypoint, otherwise parent image causes to run as user fluent
ENTRYPOINT []

# Run Fluentd
CMD exec fluentd -c /fluentd/etc/$FLUENTD_CONF -p /fluentd/plugins $FLUENTD_OPT
