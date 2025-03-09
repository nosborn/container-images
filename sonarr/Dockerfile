FROM alpine:3.21

ARG RELEASE_URL

ENV COMPlus_EnableDiagnostics=0
ENV TMPDIR=/run/sonarr-temp

RUN \
     apk add -U --no-cache --upgrade \
       curl \
       icu-libs \
       sqlite-libs \
       xmlstarlet \
  && mkdir -p /app \
  && curl -fLsS "${RELEASE_URL}" \
       | tar -C /app -xzf - --exclude=Sonarr.Update --strip-components=1

EXPOSE 8989
ENTRYPOINT ["/app/Sonarr"]
