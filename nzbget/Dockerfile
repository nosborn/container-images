FROM debian:12.9-slim

ARG DEBIAN_FRONTEND=noninteractive

RUN \
     apt-get update \
  && apt-get install -y --no-install-recommends \
       ca-certificates \
       curl \
       jq \
       python3 \
  && cd /tmp \
  && url="$(curl -fsS https://api.github.com/repos/nzbgetcom/nzbget/releases/latest | jq -r '.assets[]|select(.name|endswith("-bin-linux.run"))|.browser_download_url')" \
  && curl -fLOsS "${url:?}" \
  && sh "$(basename "${url:?}")" --destdir /app \
  && rm -f "$(basename "${url:?}")" \
  && mkdir -p /app/scripts \
  && cd /app/scripts \
  && curl -fLOsS https://raw.githubusercontent.com/l3uddz/nzbgetScripts/d9d852b4c889dd3636e6434d82445d5fedcbef0a/HashRenamer.py \
  && chmod +x HashRenamer.py \
  && rm -rf \
       /app/cacert.pem \
       /app/nzbget.conf \
       /app/pubkey.pem \
       /tmp/* \
       /var/lib/apt/lists/*

EXPOSE 6789
ENTRYPOINT ["/app/nzbget", "--server", "--configfile=/config/nzbget.conf", "--option=OutputMode=log", "--option=WriteLog=none"]
