FROM debian:12.9-slim

ENV DEBIAN_FRONTEND=noninteractive
ARG PAR2_VERSION=1.2.0
ARG RELEASE_URL
ARG UNRAR_VERSION=5.2.5

ENV HOME=/config
ENV PYTHONIOENCODING=utf-8

RUN \
  apt-get update && \
  apt-get install -y --no-install-recommends \
    7zip \
    ca-certificates \
    curl \
    python3 \
    python3-venv \
    xz-utils && \
  curl -fLOsS "https://github.com/animetosho/par2cmdline-turbo/releases/download/v${PAR2_VERSION}/par2cmdline-turbo-v${PAR2_VERSION}-linux-amd64.xz" && \
  xz -dv "par2cmdline-turbo-v${PAR2_VERSION}-linux-amd64.xz" && \
  mv "par2cmdline-turbo-v${PAR2_VERSION}-linux-amd64" /usr/local/bin/par2 && \
  chmod 0555 /usr/local/bin/par2 && \
  par2 --version && \
  curl -fOsS "https://www.rarlab.com/rar/unrar_${UNRAR_VERSION}-0.1_amd64.deb" && \
  dpkg -i "unrar_${UNRAR_VERSION}-0.1_amd64.deb" && \
  rm "unrar_${UNRAR_VERSION}-0.1_amd64.deb" && \
  mkdir -p /app && \
  curl -fLsS "${RELEASE_URL}" | tar -C /app -xzf - --strip-components=1 && \
  python3 -m venv /app/venv && \
  /app/venv/bin/python3 -m pip install -r /app/requirements.txt && \
  apt-get clean && \
  rm -rf \
    ${HOME}/.cache \
    /tmp/* \
    /var/lib/apt/lists/* \
    /var/tmp/*

EXPOSE 8080

VOLUME /config/
VOLUME /media/
WORKDIR /app

ENTRYPOINT ["/app/venv/bin/python3", "-OO", "SABnzbd.py", "--config-file=/config"]
