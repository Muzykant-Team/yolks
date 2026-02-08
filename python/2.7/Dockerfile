FROM        --platform=$TARGETOS/$TARGETARCH python:2.7-slim
LABEL       author="Michael Parker" maintainer="parker@pterodactyl.io"

RUN         sed -i 's/deb.debian.org/archive.debian.org/g' /etc/apt/sources.list \
            && sed -i 's|security.debian.org|archive.debian.org|g' /etc/apt/sources.list \
            && sed -i '/stretch-updates/d' /etc/apt/sources.list \
            && apt update \
            && apt -y install git gcc g++ ca-certificates dnsutils curl iproute2 ffmpeg procps tini \
            && useradd -m -d /home/container container

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL SIGINT

COPY        --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh

ENTRYPOINT    ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
