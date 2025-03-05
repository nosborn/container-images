FROM alpine:3.21

ARG RELEASE_URL

ENV COMPlus_EnableDiagnostics=0
ENV TMPDIR=/run/radarr-temp

RUN \
  apk add -U --no-cache --upgrade curl icu-libs sqlite-libs xmlstarlet && \
  mkdir -p /app && \
  curl -fLsS "${RELEASE_URL}" | tar -C /app -xzf - --exclude=Radarr.Update --strip-components=1

EXPOSE 7878

VOLUME /config
VOLUME /media
VOLUME /run/radarr-temp

ENTRYPOINT ["/app/Radarr", "-data=/config", "-nobrowser"]
