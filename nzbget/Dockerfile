FROM debian:12.9-slim

ARG DEBIAN_FRONTEND=noninteractive

RUN \
     apt-get update \
  && apt-get install -y --no-install-recommends \
       ca-certificates \
       curl \
       jq \
       python3 \
  && url="$( \
       curl -fsS https://api.github.com/repos/nzbgetcom/nzbget/releases/latest \
         | jq -r '.assets[]|select(.name|endswith("-bin-linux.run"))|.browser_download_url' \
     )" \
  && curl -fLOsS "${url:?}" \
  && sh "$(basename "${url:?}")" --destdir /app \
  && rm -f \
       "$(basename "${url:?}")" \
       /app/cacert.pem \
       /app/nzbget.conf \
       /app/pubkey.pem \
  && mkdir -p /app/scripts \
  && curl -fLOsS --output-dir /app/scripts \
       https://raw.githubusercontent.com/l3uddz/nzbgetScripts/d9d852b4c889dd3636e6434d82445d5fedcbef0a/HashRenamer.py \
  && chmod +x /app/scripts/HashRenamer.py \
  && rm -rf \
       /var/lib/apt/lists/*

EXPOSE 6789
ENTRYPOINT ["/app/nzbget", "--server", "--configfile=/config/nzbget.conf", "--option=OutputMode=log", "--option=WriteLog=none"]
