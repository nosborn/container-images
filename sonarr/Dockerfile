FROM alpine:3.21

ENV COMPlus_EnableDiagnostics=0
ENV TMPDIR=/run/sonarr-temp

RUN \
     apk add -U --no-cache --upgrade \
       curl \
       icu-libs \
       jq \
       sqlite-libs \
       xmlstarlet \
  && mkdir -p /app \
  && url="$( \
       curl -fsS https://api.github.com/repos/Sonarr/Sonarr/releases/latest \
         | jq -r '.assets[]|select(.name|endswith(".linux-musl-x64.tar.gz"))|.browser_download_url' \
     )" \
  && curl -fLsS "${url:?}" \
       | tar -C /app -xzf - --exclude=Sonarr.Update --no-same-owner --strip-components=1

EXPOSE 8989
ENTRYPOINT ["/app/Sonarr"]
