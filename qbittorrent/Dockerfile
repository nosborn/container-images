FROM alpine:3.21

ENV HOME=/config
ENV XDG_CONFIG_HOME=/config
ENV XDG_DATA_HOME=/config

RUN \
     apk add --no-cache \
       curl \
       p7zip \
       qbittorrent-nox

EXPOSE 6881 6881/udp 8080
ENTRYPOINT ["/usr/bin/qbittorrent-nox", "--webui-port=8080"]
