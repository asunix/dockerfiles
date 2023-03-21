FROM alpine:3.16.4 as builder

RUN set -eux \
        && apk add --update --no-cache \
                coreutils \
                g++  \
                gcc \
                make  \
                musl-dev \
                openssl-dev \
                python3-dev \
                bc \
                libffi-dev \
                libxml2-dev \
                libxslt-dev \
                py3-pip \
                python3 \
                py3-yaml \
        && python3 -c 'import _yaml' \
	&& pip3 install --no-cache-dir --no-compile \
		wheel \
	&& pip3 install --no-cache-dir --no-compile \
		Jinja2 \
		MarkupSafe \
		PyNaCl \
		bcrypt \
		cffi \
		cryptography \
		pycparser \
	&& pip3 install --no-cache-dir --no-compile ansible \
	&& find /usr/lib/ -name '__pycache__' -print0 | xargs -0 -n1 rm -rf \
	&& find /usr/lib/ -name '*.pyc' -print0 | xargs -0 -n1 rm -rf \
	&& pip3 install --no-cache-dir --no-compile \
		junit-xml \
		lxml \
		paramiko \
	&& find /usr/lib/ -name '__pycache__' -print0 | xargs -0 -n1 rm -rf \
	&& find /usr/lib/ -name '*.pyc' -print0 | xargs -0 -n1 rm -rf \
	&& pip3 uninstall --yes \
		setuptools \
		wheel \
	&& find /usr/lib/ -name '__pycache__' -print0 | xargs -0 -n1 rm -rf \
	&& find /usr/lib/ -name '*.pyc' -print0 | xargs -0 -n1 rm -rf

FROM alpine:3.16.4 as production

LABEL "maintainer"="Giovanni Moroni <asunix@gmail.com>"

RUN set -eux \
	&& apk add --no-cache \
		libgcc \
		py3-pip \
		python3 \
		yaml \
		openssh-client \
		sshpass \
	&& find /usr/lib/ -name '__pycache__' -print0 | xargs -0 -n1 rm -rf \
	&& find /usr/lib/ -name '*.pyc' -print0 | xargs -0 -n1 rm -rf \
	&& ln -s /usr/bin/python3 /usr/bin/python

COPY --from=builder /usr/lib/python3.10/site-packages/ /usr/lib/python3.10/site-packages/
COPY --from=builder /usr/bin/ansible* /usr/bin/

RUN set -eux \
	&& python3 -m compileall -j 0 /usr/lib/python3.10

WORKDIR /data

CMD ["/bin/sh"]
