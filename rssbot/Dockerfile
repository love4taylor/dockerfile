FROM --platform=$BUILDPLATFORM rust:1.53.0-buster AS build

ARG TARGETARCH

RUN apt-get update && apt-get install -y build-essential curl git musl-tools

RUN git clone --depth 1 https://github.com/iovxw/rssbot.git /home/rssbot

RUN rustup default nightly && \
    case "$TARGETARCH" in \
    "386") \
        RUST_TARGET="i686-unknown-linux-musl" \
        MUSL="i686-linux-musl" \
        ;; \
    "amd64") \
        RUST_TARGET="x86_64-unknown-linux-musl" \
        MUSL="x86_64-linux-musl" \
        ;; \
    "arm64") \
        RUST_TARGET="aarch64-unknown-linux-musl" \
        MUSL="aarch64-linux-musl" \
        ;; \
    *) \
        echo "Doesn't support $TARGETARCH architecture" \
        exit 1 \
        ;; \
    esac && \
    wget -qO- "https://musl.cc/$MUSL-cross.tgz" | tar -xzC /root/ && \
    CC=/root/$MUSL-cross/bin/$MUSL-gcc && \
    rustup target add $RUST_TARGET && \
    cd /home/rssbot && \
    RUSTFLAGS="-C linker=$CC" CC=$CC cargo build --target $RUST_TARGET --release && \
    mv target/$RUST_TARGET/release/rssbot target/release/

FROM alpine:latest

RUN apk --no-cache add ca-certificates

COPY --from=build \
    /home/rssbot/target/release/rssbot \
    /usr/local/bin/

ENV DATAFILE="/rssbot/rssdata.json"
ENV TELEGRAM_BOT_TOKEN=""
ENV MAX_FEED_SIZE="2097152"
ENV MAX_INTERVAL="43200"
ENV MIN_INTERVAL="300"

VOLUME /rssbot

ENTRYPOINT /usr/local/bin/rssbot --max-feed-size $MAX_FEED_SIZE --max-interval $MAX_INTERVAL --min-interval $MIN_INTERVAL --database $DATAFILE $TELEGRAM_BOT_TOKEN
