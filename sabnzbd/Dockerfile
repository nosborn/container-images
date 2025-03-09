FROM debian:12.9 AS build

ARG DEBIAN_FRONTEND=noninteractive
ARG PAR2_VERSION=1.2.0
ARG RELEASE_URL
ARG UNRAR_VERSION=7.1.5

ENV HOME=/config
ENV PYTHONIOENCODING=utf-8

RUN \
     apt-get update && \
  && apt-get install -y --no-install-recommends \
       build-essential \
       ca-certificates \
       curl \
       python3 \
       python3-venv \
       xz-utils \
  && mkdir -p /tmp/par2 \
  && cd /tmp/par2 \
  && curl -fLOsS "https://github.com/animetosho/par2cmdline-turbo/releases/download/v${PAR2_VERSION}/par2cmdline-turbo-v${PAR2_VERSION}-linux-amd64.xz" \
  && xz -dv "par2cmdline-turbo-v${PAR2_VERSION}-linux-amd64.xz" \
  && mv -v "par2cmdline-turbo-v${PAR2_VERSION}-linux-amd64" /usr/local/bin/par2 \
  && mkdir -p /tmp/unrar \
  && cd /tmp/unrar \
  && curl -fsS "https://www.rarlab.com/rar/unrarsrc-${UNRAR_VERSION}.tar.gz" \
       | tar -xzf - --strip-components=1 && \
  && sed -i 's|CXXFLAGS=-march=native |CXXFLAGS=|' makefile \
  && sed -i 's|LDFLAGS=-pthread|LDFLAGS=-pthread -static|' makefile \
  && make \
  && mv -v unrar /usr/local/bin/unrar

FROM debian:12.9-slim

ARG DEBIAN_FRONTEND=noninteractive
ARG RELEASE_URL

ENV HOME=/config
ENV PYTHONIOENCODING=utf-8

COPY --from=build --chmod=555 /usr/local/bin/par2 /usr/local/bin/unrar /usr/local/bin/

RUN \
     apt-get update \
  && apt-get install -y --no-install-recommends \
       7zip \
       ca-certificates \
       curl \
       python3 \
       python3-venv \
       xz-utils \
  && mkdir -p /app \
  && cd /app \
  && curl -fLsS "${RELEASE_URL}" \
       | tar -xzf - --strip-components=1 \
  && python3 -m venv venv \
  && ./venv/bin/python3 -m pip install -r requirements.txt \
  && apt-get clean \
  && rm -rf \
       ${HOME}/.cache \
       /tmp/* \
       /var/lib/apt/lists/* \
       /var/tmp/*

EXPOSE 8080
WORKDIR /app
ENTRYPOINT ["/app/venv/bin/python3", "-OO", "SABnzbd.py"]
