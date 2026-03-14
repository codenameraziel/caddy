FROM caddy:latest

RUN apk add --no-cache curl vim nano openssl bash tzdata

COPY ./file /etc/caddy/Caddyfile

VOLUME /data
VOLUME /config

EXPOSE 80
EXPOSE 443

CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile", "--adapter", "caddyfile"]
