FROM alpine:3.21

ARG RELEASE_URL

ENV COMPlus_EnableDiagnostics=0
ENV TMPDIR=/run/raedarr-temp

RUN \
     apk add -U --no-cache --upgrade \
       curl \
       icu-libs \
       sqlite-libs \
       xmlstarlet \
  && mkdir -p /app \
  && curl -fLsS "${RELEASE_URL}" \
       | tar -C /app -xzf - --exclude=Readarr.Update --strip-components=1

EXPOSE 8787
ENTRYPOINT ["/app/Readarr"]
