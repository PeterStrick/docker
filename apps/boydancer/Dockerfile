FROM --platform=$TARGETOS/$TARGETARCH node:20-bookworm-slim

LABEL author="Peter Strick" maintainer="uwupeter@rawrr.dev"

RUN apt update
RUN apt -y install ffmpeg iproute2 git sqlite3 libsqlite3-dev python3 python3-dev python3-pip ca-certificates dnsutils tzdata zip tar curl build-essential libtool iputils-ping libnss3 tini
RUN python3 -m pip install yt-dlp --break-system-packages
RUN python3 -m pip install yt-dlp-proxy==1.0.6 --break-system-packages
RUN python3 -m pip install votify mutagen --break-system-packages
RUN useradd -m -d /home/container container

RUN npm install npm@latest typescript ts-node @types/node --location=global

# install pnpm
RUN npm install -g corepack
RUN corepack enable
RUN corepack prepare pnpm@latest --activate

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

STOPSIGNAL SIGINT

COPY --chown=container:container ./entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]
