FROM erlang:24.3-slim

LABEL maintainer="ohr486"

# for M1 mac
ENV ARCH="aarch64"

ENV ELIXIR_VERSION="v1.13.4"
ENV RUST_VERSION="1.60.0"
ENV NODE_VERSION="18.x"
ENV LANG="ja_JP.UTF-8"

ENV ELIXIR_DOWNLOAD_URL="https://github.com/elixir-lang/elixir/archive/${ELIXIR_VERSION}.tar.gz"
ENV RUST_ARCHIVE="rust-$RUST_VERSION-$ARCH-unknown-linux-gnu.tar.gz"
ENV RUST_DOWNLOAD_URL="https://static.rust-lang.org/dist/$RUST_ARCHIVE"
ENV NODE_DOWNLOAD_URL="https://deb.nodesource.com/setup_$NODE_VERSION"

RUN apt-get update && apt-get install \
      curl gcc make git vim \
      ca-certificates libc6-dev inotify-tools \
      -qqy --no-install-recommends \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# docker
RUN curl -fsSL "https://get.docker.com" | sh

# node.js
RUN curl -sL $NODE_DOWNLOAD_URL | bash - \
    && apt-get install -y nodejs

# elixir
RUN set -xe \
    && mkdir -p /usr/local/src/elixir && cd /usr/local/src/elixir \
    && curl -fSL -o elixir-src.tar.gz $ELIXIR_DOWNLOAD_URL \
    && tar -xzC /usr/local/src/elixir --strip-components=1 -f elixir-src.tar.gz \
    && rm elixir-src.tar.gz \
    && make install clean

# rust
RUN set -ex \
    && mkdir -p /usr/local/src/rust && cd /usr/local/src/rust \
    && curl -fsOSL $RUST_DOWNLOAD_URL \
    && curl -s $RUST_DOWNLOAD_URL.sha256 | sha256sum -c - \
    && tar -C /usr/local/src/rust -xzf $RUST_ARCHIVE --strip-components=1 \
    && rm $RUST_ARCHIVE \
    && ./install.sh

