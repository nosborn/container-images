FROM debian:12.10-slim

ARG DEBIAN_FRONTEND=noninteractive

RUN \
     apt-get update \
  && apt-get install -y --no-install-recommends \
       7zip \
       ca-certificates \
       curl \
       jq \
       python3 \
       python3-pip \
       python3-setuptools \
       python3-venv \
       unrar-free \
       unzip \
  && url="$( \
       curl -fsS https://api.github.com/repos/morpheus65535/bazarr/releases/latest \
         | jq -r '.assets[]|select(.name=="bazarr.zip")|.browser_download_url' \
     )" \
  && curl -fLOsS "${url:?}" \
  && mkdir -p /app \
  && unzip "$(basename "${url:?}")" -d /app \
  && rm "$(basename "${url:?}")" \
  && python3 -m venv /app/venv \
  && /app/venv/bin/python3 -m pip install -r /app/requirements.txt \
  && mkdir -p /app/bin/Linux/x86_64/ffmpeg \
  && curl -fOsS --output-dir /app/bin/Linux/x86_64/ffmpeg \
       https://raw.githubusercontent.com/morpheus65535/bazarr-binaries/refs/heads/master/bin/Linux/x86_64/ffmpeg/ffmpeg \
  && curl -fOsS --output-dir /app/bin/Linux/x86_64/ffmpeg \
       https://raw.githubusercontent.com/morpheus65535/bazarr-binaries/refs/heads/master/bin/Linux/x86_64/ffmpeg/ffprobe \
  && chmod +x /app/bin/Linux/x86_64/ffmpeg/* \
  && rm -rf \
       /var/lib/apt/lists/*

EXPOSE 6767
WORKDIR /app
ENTRYPOINT ["/app/venv/bin/python3", "/app/bazarr.py"]
