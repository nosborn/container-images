FROM alpine:3.21

ENV COMPlus_EnableDiagnostics=0
ENV TMPDIR=/run/prowlarr-temp

RUN \
     apk add -U --no-cache --upgrade \
       curl \
       icu-libs \
       jq \
       sqlite-libs \
       xmlstarlet \
  && mkdir -p /app \
  && url="$( \
       curl -fsS https://api.github.com/repos/Prowlarr/Prowlarr/releases/latest \
         | jq -r '.assets[]|select(.name|endswith(".linux-musl-core-x64.tar.gz"))|.browser_download_url' \
     )" \
  && curl -fLsS "${url:?}" \
       | tar -C /app -xzf - --exclude=Prowlar.Update --no-same-owner --strip-components=1

EXPOSE 9696
ENTRYPOINT ["/app/Prowlarr"]
