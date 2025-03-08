FROM alpine:3.21

ENV HOME=/config
ENV QTWEBENGINE_DISABLE_SANDBOX=1
ENV TITLE=Calibre

RUN \
  apk add --no-cache \
    curl && \
  apk add --no-cache --repository=http://dl-cdn.alpinelinux.org/alpine/edge/community/ \
    qt6-qtbase-private-dev && \
  apk add --no-cache --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing/ \
    calibre

EXPOSE 8080
ENTRYPOINT ["/usr/bin/calibre-server"]
