FROM alpine:latest

LABEL maintainer "Giovanni Moroni <asunix@gmail.com>"

RUN apk add --no-cache \
	apache2-utils

ENTRYPOINT [ "htpasswd" ]
