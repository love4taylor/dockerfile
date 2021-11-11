FROM node:12-alpine3.14
RUN set -ex \
 && apk add --no-cache --virtual .build-deps \
      autoconf \
      automake \
      build-base \
      c-ares-dev \
      libev-dev \
      libtool \
      libsodium-dev \
      linux-headers \
      mbedtls-dev \
      pcre-dev \
      curl \
      git \
      unzip \
 && cd /tmp \
 && curl -fLs -o x86_64-unknown-linux-musl.zip https://nightly.link/shadowsocks/shadowsocks-rust/workflows/build-nightly-release/master/x86_64-unknown-linux-musl.zip \
 && unzip x86_64-unknown-linux-musl.zip \
 && tar Jxf shadowsocks*.tar.xz \
 && mv ssmanager /usr/bin/ \
 && git clone --recursive https://github.com/shadowsocks/shadowsocks-libev \
 && cd shadowsocks-libev \
 && ./autogen.sh \
 && ./configure --prefix=/usr --disable-documentation \
 && make install \
 && apk del .build-deps \
 && apk add --no-cache \
      rng-tools \
      $(scanelf --needed --nobanner /usr/bin/ss-* \
      | awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
      | sort -u) \
 && rm -rf /tmp/*
RUN apk --no-cache add tzdata iproute2 \
 && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
 && echo "Asia/Shanghai" > /etc/timezone \
 && npm i -g shadowsocks-manager --unsafe-perm \
 && sed -i "s@'-m', method, '-U', '--manager-address'@'-m', method, '-U', '--tcp-no-delay', '--manager-address'@g" /usr/local/lib/node_modules/shadowsocks-manager/init/runShadowsocks.js \
 && sed -i "s@'-v', '-m', method, '-u', '--manager-address'@'--reuse-port', '-m', method, '-u', '--manager-address'@g" /usr/local/lib/node_modules/shadowsocks-manager/init/runShadowsocks.js

CMD ["ssmgr"]
