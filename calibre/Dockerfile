FROM alpine:3.21

ENV HOME=/config
ENV QTWEBENGINE_DISABLE_SANDBOX=1
ENV TITLE=Calibre

RUN \
     apk add --no-cache \
       curl \
       jq \
       unzip \
  && apk add --no-cache --repository=http://dl-cdn.alpinelinux.org/alpine/edge/community/ \
       qt6-qtbase-private-dev \
  && apk add --no-cache --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing/ \
       calibre \
  && url=https://plugins.calibre-ebook.com/291290.zip \
  && curl -fLOsS "${url:?}" \
  && calibre-customize --add-plugin "$(basename "${url:?}")" \
  && rm "$(basename "${url:?}")" \
  && url="$( \
       curl -fsS https://api.github.com/repos/noDRM/DeDRM_tools/releases/latest \
         | jq -r '.assets[]|select(.name|endswith(".zip"))|.browser_download_url' \
     )" \
  && curl -fLOsS "${url:?}" \
  && unzip "$(basename "${url:?}")" DeDRM_plugin.zip \
  && rm "$(basename "${url}")" \
  && calibre-customize --add-plugin DeDRM_plugin.zip \
  && rm DeDRM_plugin.zip

EXPOSE 8080
ENTRYPOINT ["/usr/bin/calibre-server"]
