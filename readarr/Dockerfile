FROM alpine:3.21

ENV COMPlus_EnableDiagnostics=0
ENV TMPDIR=/run/raedarr-temp

RUN \
     apk add -U --no-cache --upgrade \
       curl \
       icu-libs \
       jq \
       sqlite-libs \
       xmlstarlet \
  && mkdir -p /app \
  && url="$( \
       curl -fsS https://api.github.com/repos/Readarr/Readarr/releases \
         | jq -r '.[0]|.assets[]|select(.name|endswith(".linux-musl-core-x64.tar.gz"))|.browser_download_url' \
     )" \
  && curl -fLsS "${url:?}" \
       | tar -C /app -xzf - --exclude=Readarr.Update --no-same-owner --strip-components=1

EXPOSE 8787
ENTRYPOINT ["/app/Readarr"]
