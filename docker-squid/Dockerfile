FROM alpine

LABEL maintainer="Giulio Calzolari <giuliocalzolari@users.noreply.github.com>" \
  org.label-schema.name="Squid" \
  org.label-schema.description="AWS UTM Squid docker image based on Alpine Linux." \
  org.label-schema.schema-version="1.0"
ENV CONFD_VERSION=0.16.0

# HEALTHCHECK --interval=30m --timeout=1s \
#   CMD squidclient -h localhost cache_object://localhost/counters || exit 1

# Install packages
RUN echo "http://nl.alpinelinux.org/alpine/edge/testing/" >> /etc/apk/repositories && \
    apk add --no-cache --update bash squid

ADD https://github.com/kelseyhightower/confd/releases/download/v${CONFD_VERSION}/confd-${CONFD_VERSION}-linux-amd64 /usr/bin/confd

RUN chmod +x /usr/bin/confd
# Redirect squid access logs to stdout
RUN ln -sf /dev/stdout /var/log/squid/access.log

# Copy confd configuration
COPY confd /etc/confd

# Set entrypoint and default command arguments
COPY entrypoint.sh /usr/bin/entrypoint.sh
ENTRYPOINT ["/usr/bin/entrypoint.sh"]
CMD ["squid","-f","/etc/squid/squid.conf","-NYCd","1"]
