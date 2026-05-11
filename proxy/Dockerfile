FROM nginx:1.25-alpine

COPY nginx.conf /etc/nginx/conf.d/default.conf

# Self-signed TLS so nginx can listen on 443 with no mounts; replace these files (same paths) in production.
RUN apk add --no-cache openssl && \
    mkdir -p /etc/nginx/certs && \
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/nginx/certs/server.key \
    -out /etc/nginx/certs/server.crt \
    -subj "/CN=localhost"
