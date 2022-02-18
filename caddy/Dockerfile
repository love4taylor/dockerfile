FROM caddy:2.4.6-builder AS builder

RUN xcaddy build \
    --with github.com/caddy-dns/cloudflare \
    --with github.com/caddy-dns/dnspod \
    --with github.com/caddy-dns/alidns \
    --with github.com/mholt/caddy-webdav \
    --with github.com/mholt/caddy-dynamicdns \
    --with github.com/mholt/caddy-ratelimit \
    --with github.com/mastercactapus/caddy2-proxyprotocol \
    --with github.com/lolPants/caddy-requestid \
    --with github.com/kirsch33/realip \
    --with github.com/ueffel/caddy-brotli \
    --with github.com/ueffel/caddy-imagefilter/all \
    --with github.com/porech/caddy-maxmind-geolocation \
    --with github.com/sillygod/cdp-cache \
    --with github.com/hairyhenderson/caddy-teapot-module \
    --with github.com/imgk/caddy-trojan

FROM caddy:2.4.6

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
