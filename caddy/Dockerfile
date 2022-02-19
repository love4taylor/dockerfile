FROM caddy:2.4.6-builder AS builder

RUN xcaddy build \
    --with github.com/caddy-dns/cloudflare \
    --with github.com/caddy-dns/dnspod \
    --with github.com/caddy-dns/alidns \
    --with github.com/mholt/caddy-webdav \
    --with github.com/mholt/caddy-ratelimit \
    --with github.com/mastercactapus/caddy2-proxyprotocol \
    --with github.com/kirsch33/realip \
    --with github.com/ueffel/caddy-imagefilter/all \
    --with github.com/porech/caddy-maxmind-geolocation \
    --with github.com/sillygod/cdp-cache \
    --with github.com/imgk/caddy-trojan \
    --with github.com/greenpau/caddy-git \
    --with github.com/greenpau/caddy-trace

FROM debian:bullseye-slim

COPY --from=builder /usr/bin/caddy /usr/bin/caddy

RUN set -eux \
 && mkdir -p \
    /config/caddy \
    /data/caddy \
    /etc/caddy \
    /usr/share/caddy \
 && apt update \
 && apt install -y libcap2-bin curl \
 && curl -o /etc/caddy/Caddyfile "https://github.com/caddyserver/dist/raw/master/config/Caddyfile" \
 && curl -o /usr/share/caddy/index.html "https://github.com/caddyserver/dist/raw/master/welcome/index.html" \
 && rm -rf /var/lib/apt/lists/* \
 && setcap cap_net_bind_service=ep /usr/bin/caddy \
 && chown -R www-data:www-data /config /data /etc/caddy /usr/share/caddy

ENV XDG_CONFIG_HOME /config
ENV XDG_DATA_HOME /data

VOLUME /config
VOLUME /data

EXPOSE 80
EXPOSE 443
EXPOSE 2019

WORKDIR /srv

USER www-data

CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile", "--adapter", "caddyfile"]
