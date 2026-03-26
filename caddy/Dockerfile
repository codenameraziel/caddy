# Usa imagem base do Go para compilar o Caddy com plugin
FROM golang:1.22-alpine AS builder

# Instala dependências necessárias
RUN apk add --no-cache git build-base

# Compila o Caddy com o plugin Cloudflare
RUN go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest && \
    xcaddy build \
      --with github.com/caddy-dns/cloudflare

# Imagem final baseada no Alpine
FROM alpine:latest

# Instala pacotes extras
RUN apk add --no-cache \
    curl \
    vim \
    nano \
    openssl \
    bash \
    tzdata

# Copia binário do Caddy compilado
COPY --from=builder /go/caddy /usr/bin/caddy

# Copia Caddyfile customizado
COPY ./Caddyfile /etc/caddy/Caddyfile

# Define diretórios de dados e configuração
VOLUME /data
VOLUME /config

# Porta padrão HTTPS
EXPOSE 443
# Porta HTTP
EXPOSE 80

# Comando padrão
CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile", "--adapter", "caddyfile"]
