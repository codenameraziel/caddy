# Etapa de build: compila o Caddy com plugin Cloudflare
FROM golang:1.22-alpine AS builder

# Etapa de build: compila o Caddy com plugin Cloudflare
FROM golang:1.22-alpine AS builder

# Instala dependências
RUN apk add --no-cache git build-base

# Instala xcaddy
RUN go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest

# Compila o Caddy com plugin Cloudflare
RUN /go/bin/xcaddy build \
    --with github.com/caddy-dns/cloudflare

# Etapa final: imagem leve com Caddy pronto
FROM alpine:latest

# Instala pacotes extras
RUN apk add --no-cache \
    curl \
    vim \
    nano \
    openssl \
    bash \
    tzdata

# Copia binário compilado
COPY --from=builder /go/caddy /usr/bin/caddy

# Copia Caddyfile customizado
COPY ./Caddyfile /etc/caddy/Caddyfile

# Define diretórios de dados e configuração
VOLUME /data
VOLUME /config

EXPOSE 80
EXPOSE 443

CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile", "--adapter", "caddyfile"]
