ARG VERSION

FROM caddy:${VERSION}-builder-alpine AS caddybuilder

RUN xcaddy build \
    --with github.com/caddy-dns/cloudflare \
    --with github.com/caddy-dns/alidns \
    --with github.com/abiosoft/caddy-exec \
    --with github.com/caddyserver/forwardproxy \
    --with github.com/mholt/caddy-webdav \
    --with github.com/WingLim/caddy-webhook \
    --with github.com/Javex/caddy-fail2ban@main

FROM ghcr.io/nuln/upx:latest AS upxbuilder

COPY --from=caddybuilder /usr/bin/caddy /caddy
RUN upx --lzma --best --ultra-brute /caddy

FROM caddy:${VERSION}-alpine

ADD 404.html /html/
COPY --from=upxbuilder /caddy /usr/bin/caddy

RUN apk update && apk add --no-cache ca-certificates