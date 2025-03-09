FROM debian:12.9-slim

ARG DEBIAN_FRONTEND=noninteractive
ARG RELEASE_URL # not used

RUN \
     apt-get update \
  && apt-get install -y --no-install-recommends \
       ca-certificates \
       curl \
  && cd /tmp \
  && curl -fLOsS https://nzbget.net/download/nzbget-latest-bin-linux.run \
  && sh nzbget-latest-bin-linux.run --destdir /app \
  && apt-get clean \
  && rm -rf \
       /app/cacert.pem \
       /app/nzbget.conf \
       /app/pubkey.pem \
       /tmp/* \
       /var/lib/apt/lists/*

EXPOSE 6789
ENTRYPOINT ["/app/nzbget", "--configfile=/config/nzbget.conf", "--server"]
