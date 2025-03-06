FROM debian:12.9

ARG RELEASE_URL

ENV HOME=/config
ENV TITLE=Calibre
ENV QTWEBENGINE_DISABLE_SANDBOX=1

RUN \
  apt-get update && \
  apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    dbus \
    fcitx-rime \
    fonts-wqy-microhei \
    libnss3 \
    libopengl0 \
    libqpdf29 \
    libxcb-cursor0 \
    libxcb-icccm4 \
    libxcb-image0 \
    libxcb-keysyms1 \
    libxcb-randr0 \
    libxcb-render-util0 \
    libxcb-xinerama0 \
    libxkbcommon-x11-0 \
    poppler-utils \
    python3 \
    python3-xdg \
    ttf-wqy-zenhei \
    wget \
    xz-utils && \
  apt-get install -y \
    speech-dispatcher && \
  mkdir -p /opt/calibre && \
  curl -fLsS "${RELEASE_URL}" | tar xJof - -C /opt/calibre && \
  /opt/calibre/calibre_postinstall && \
  apt-get clean && \
  dbus-uuidgen >/etc/machine-id && \
  rm -rf \
    /tmp/* \
    /var/lib/apt/lists/* \
    /var/tmp/*

EXPOSE 8080

VOLUME /config/
VOLUME /config/.cache
VOLUME /media

ENTRYPOINT ["/usr/bin/calibre-server", "--enable-local-write", "/media/Books"]
