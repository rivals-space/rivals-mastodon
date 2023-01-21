ARG MASTODON_CONTAINER=ghcr.io/rivals-space/rivals-mastodon:main
FROM $MASTODON_CONTAINER as mastodon
FROM alpine:3.17 as compress_assets

COPY --from=mastodon /opt/mastodon/public /opt/mastodon/public

RUN set -eux; \
    apk add --no-cache \
      brotli \
      gzip \
    ; \
     sh -c 'find /opt/mastodon/public -maxdepth 1 \( -type f -name "*.svg" -o -name "*.css" -o -name "*.js" -o -name "*.html" \) -exec brotli -f -9 -v {} \;' ; \
     sh -c 'find /opt/mastodon/public -maxdepth 1 \( -type f -name "*.svg" -o -name "*.css" -o -name "*.js" -o -name "*.html" \) -exec gzip -f -9 -v -k -- {} \;'

FROM ghcr.io/rivals-space/nginx-unpriv-brotli-zstd:main as nginx

COPY .docker/nginx/config/nginx.conf /etc/nginx/nginx.conf
COPY .docker/nginx/config/default.conf /etc/nginx/conf.d/default.conf
COPY --from=compress_assets --chown=nginx:nginx /opt/mastodon/public /opt/mastodon/public
