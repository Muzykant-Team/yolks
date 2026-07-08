# Etap 1: Pobranie najnowszego statycznego FFmpeg 8.1
FROM        mwader/static-ffmpeg:8.1 AS ffmpeg

# Etap 2: Główny obraz Python
FROM        --platform=$TARGETOS/$TARGETARCH python:3.15-rc-slim-trixie
LABEL       author="Michael Parker" maintainer="parker@pterodactyl.io"

# Kopiujemy binarki FFmpeg bezpośrednio do ścieżki systemowej
COPY        --from=ffmpeg /ffmpeg /usr/local/bin/
COPY        --from=ffmpeg /ffprobe /usr/local/bin/

RUN         apt update \
            && apt -y install git gcc g++ ca-certificates dnsutils curl iproute2 procps tini \
            && useradd -m -d /home/container container \
            && apt clean \
            && rm -rf /var/lib/apt/lists/*

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL SIGINT

COPY        --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh

ENTRYPOINT    ["/usr/bin/tini", "-g", "--"]
CMD          ["/entrypoint.sh"]