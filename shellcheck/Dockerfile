FROM debian:bullseye-slim

LABEL maintainer "Giovanni Moroni <asunix@gmail.com>"

RUN apt-get update && apt-get install -y \
	file \
	shellcheck \
	--no-install-recommends \
	&& rm -rf /var/lib/apt/lists/*

CMD ["shellcheck"]
