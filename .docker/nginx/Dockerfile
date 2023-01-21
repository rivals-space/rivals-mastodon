ARG MASTODON_CONTAINER=ghcr.io/rivals-space/rivals-mastodon:main
FROM $MASTODON_CONTAINER as mastodon
FROM ghcr.io/rivals-space/nginx-unpriv-brotli-zstd:main as nginx

COPY .docker/nginx/config/nginx.conf /etc/nginx/nginx.conf
COPY .docker/nginx/config/default.conf /etc/nginx/conf.d/default.conf
COPY --from=mastodon --chown=nginx:nginx /opt/mastodon/public /opt/mastodon/public
